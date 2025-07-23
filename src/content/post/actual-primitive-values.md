---
title: 'Primitive Qiymatlarning Asl Ko’rinishi'
description: 'Aksariyat dasturchilar primitive qiymatlar, qiymati bo’yicha ko’chiriladi deb o’ylashadi, ammo bu mutlaqo to’g’ri emas. Ushbu maqolada JavaScript aslida qanday qilib xotirada primitive qiymatlarni saqlashi va ularga reference qilishiga bag’ishlangan'
publishDate: '30 March 2025'
tags: ['javascript', 'v8', 'primitive value', 'uzbek']
language: "uz-UZ"
---

## Ba'zi manbalardagi tavsiflar va Misconceptionlar

### Hammaga mashhur bo'lgan javascript.info

[<Picture 
  src="/images/posts/actual-primitive-values/javascript-info.png"
  alt='Javascript.info primitive qiymatlar haqida'
  loading='lazy'
/>](https://javascript.info/object-copy)

> **_TARJIMASI:_** Objectlarning primitivelardan asosiy farqlaridan biri shundaki, objectlar "by reference" saqlanadi va ko'chiriladi, primitive qiymatlar: string, number, boolean va boshqalar har doim "butun qiymat sifatida" ko'chiriladi. (“as a whole value”).

### Freecodecamp bloglaridan birida

[<Image 
  src="/images/posts/actual-primitive-values/freecodecamp-primitive-value.png"
  alt='Freecodecamp primitive qiymatlar haqida'
  className="border-2 rounded border-[#0a0a23]"
  loading='lazy'
/>](https://www.freecodecamp.org/news/javascript-assigning-values-vs-assigning-references)

> **_TARJIMASI:_** Primitive qiymatlar to'g'ridan-to'g'ri o'zgaruvchida saqlanadigan ma'lumotlardir. O'zgaruvchiga primitive qiymat berganimizda, bu qiymatning nusxasi yaratiladi va xotirada saqlanadi. O'zgaruvchiga kiritilgan har qanday o'zgarishlar asl qiymatga ta'sir qilmaydi.

### MDN Primitive qiymatlar haqida

[<Image 
  src="/images/posts/actual-primitive-values/mdn-primitive-values.png"
  alt='MDN primitive qiymatlar haqida'
  loading='lazy'
/>](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Data_structures#primitive_values)

Qayerda va qaysi holatda saqlanishini yozilmagan, lekin **object wrapper** haqida ma'lumot beryapti. **Object wrapper** va **boxing/unboxing** haqida to'liqroq ma'lumot berman.

## Asl primitive qiymatlar

Tepadagi manbalarga ko‘ra primitive qiymatlar - **copied as value**, yaʼni qaysidir qiymatni boshqa bir o‘zgaruvchiga berganimizda, o‘sha qiymatni nusxasi yaratilib va xotirada saqlanadi yoki o‘zgaruvchiga tenglashtirilarkan. Tasavvur qiling siz dasturlash tilini yaratmoqchisiz, asosiy aniqlab oladigan ishiz, qanday qilib xotirani boshqarish. Xotira juda qimmat turadi. O'zgaruvchilar, referencelar, katta objectlar, xotirani tozalash, faol/faol emas obyektlar va bundan tashqari juda ko'p xotirani boshqarish usullari. Agar primitive qiymatlar **"copied as value"** bo'lganda, eheee oddiy narsalar bilan xotirani to'ldirib qoyar edik.

#### Specification primitive qiymatlar haqida

[<Image 
  src="/images/posts/actual-primitive-values/primitive-value-term-spec.png"
  alt='Ecma specification primitive qiymatlar haqida'
  className="border-2 rounded border-[#52e052]"
  loading='lazy'
/>](https://tc39.es/ecma262/multipage/overview.html#sec-primitive-value)

_"Tilni amalga oshirishning eng past darajasida (lowest level) to'g'ridan-to'g'ri ifodalangan"_ iborasi primitive qiymatlar JavaScript engine tomonidan eng fundamental tarzda saqlanishi va boshqarilishini anglatadi. Primitive qiymatlar yangi o'zgaruvchilarga tayinlanganda ko'chirilgandek ko'rinsa-da, ular hali ham pastroq darajadagi JavaScript engine tomonidan reference qilinishi mumkin.

### JavaScript primitive qiymatlarni ichki tomondan qanday boshqaradi

[<Image 
  src="/images/posts/actual-primitive-values/reference-like.png"
  alt='Ecma specification reference like haqida'
  className="border-2 rounded border-[#52e052]"
  loading='lazy'
/>](https://tc39.es/ecma262/multipage/notational-conventions.html#sec-algorithm-conventions)

`Let x be someValue` dan foydalanib qiymat belgilash nusxa ko'chirish o'rniga mos yozuvlarga o'xshash alias yaratadi. Bu shuni anglatadiki, `x` va `someValue` bir xil asosiy value'ga refer qiladi.

Majburiy nusxa ko'chirish uchun spec'da aniq ko'rsatilgan: `Let x be a copy of someValue`.

Primitive qiymatlar o'zgarmas bo'lsa-da, JavaScript har doim yangi nusxalarni yaratish o'rniga ularga samarali reference qilish orqali saqlashni optimallashtirishi mumkin.

**"copied as value"** degan keng tarqalgan tushunchani shubha ostiga qo'yadi - aslida ular ko'pincha quyi darajadagi referancelar kabi saqlanadi.

```js
let foo = '...long str...' // bir-xil qiymat
let baz = '...long str...' // bir-xil qiymat
let bar = foo // bir-xil qiymat
```

Agar ba'zi resourcelarda ko'ra: _"copied as a whole value"_ bo'lganda, memory 3x joy olar edi. Aslida `...long str...` uchun memoryda bir marta reference yaratiladi, boshqa variable'lar o'sha referencega point bo'ladi.

```js
// pseudocode:
 0x0000003D00098CCD <--> "...long str..."

 foo -> 0x0000003D00098CCD
 baz -> 0x0000003D00098CCD
 bar -> 0x0000003D00098CCD
// hammasida bir-xil reference, xotiradan yutamiz 🔥
```

## Demo

### V8 engine-dagi d8 da debugg qilib ko'ramiz

<Image 
  src="/images/posts/actual-primitive-values/demo-1.png"
  alt='demo 1'
  loading='lazy'
/>

Rasmda 6-ta o'zgaruvchi mavjud. `foo` va `me` ichidagi content bir-xil, boshqa o'zgaruvchilarga tenglashtiriyapmiz, faqat `name` o'zgaruvchida "Person" content turipti. Bu holatda bizada 2 ta reference yaratishilishi kerak.

1. `asdfghjkl....`
2. `Person`

<Image 
  src="/images/posts/actual-primitive-values/demo-1-result.png"
  alt='demo 1 result'
  loading='lazy'
/>

Kutilganidek: 2 ta address yaratildi, va shu addresslarga o'zgaruvchilar reference qildi.

```js
// pseudocode:
foo = 0x011e00098cc5 // bir-xil, 0 reference String<[509]>
me = 0x011e00098cc5 // bir-xil, 0 reference String<[509]>
bar = 0x011e00098cc5 // bir-xil, 0 reference String<[509]>
bazz = 0x011e00098cc5 // bir-xil, 0 reference String<[509]>
big = 0x011e00098cc5 // bir-xil, 0 reference String<[509]>
name = 0x011e00098f01 // 1 reference String<[6]>
// String<[n]> - n so'z uzunligi
```

### Devtools Memory

<Image
  src="/images/posts/actual-primitive-values/demo-2.png"
  alt='demo 2'
  loading='lazy'
/>

<Image 
    src="/images/posts/actual-primitive-values/demo-devtools-1.png"
    alt='demo devtools 1'
    loading='lazy'
  />
<Image 
    src="/images/posts/actual-primitive-values/demo-devtools-2.png"
    alt='demo devtools 2'
    loading='lazy'
  />

## Isbotlar

### v8 pointer compression

[<Image 
  src="/images/posts/actual-primitive-values/v8-pointer-compression.png"
  alt='JavaScript values in V8 are represented as objects and allocated on the V8 heap, no matter if they are objects, arrays, numbers or strings. This allows us to represent any value as a pointer to an object'
  className="border-2 rounded border-[#0a0a23]"
  loading='lazy'
/>](https://v8.dev/blog/pointer-compression#value-tagging-in-v8)

### Mozilla Centralning Primitive (JS:Value)

[<Image
src="/images/posts/actual-primitive-values/mozilla-central-source.png"
alt="JS::Value is an object, the fraction field's least significant end holds the address of a JSObject; if a string, the address of a JSString; and so on"
className="border-2 rounded border-[#0a0a23]"
loading='lazy'
/>](https://searchfox.org/mozilla-central/source/js/public/Value.h#91)

## Conclusion

Engine code run qilganda, undagi barcha so'zlar xotirada yoziladi. Oddiy misol `addEventListener` so'zi xotirada saqlandi, qachon `const some = "addEventListener"` o'zgaruvchiga tenglashtirsak, oldin xotirada o'sha so'z bo'lsa, o'sha addressga point bo'ladi, re-create qilishni hojati yoq.

Ba'zi holatlarda Agent boshqacha yo'l tutishi mumkin. Masalan v8 har doim ham valuelarga reference yaratib o'tirmaydi. Aytaylik `for(let i = 0; i < 10000; i++)` bu yerda `i` qiymati uchun V8 reference yaratmaydi. Buning o'rniga, u oddiy raqamni ishlatishi yoki hatto register darajasida optimizatsiya qilishi mumkin. Agar yaratganida xotirada `0` dan `10000` reference yaratishga to'g'ri kelar edi, optimization uchun bu ishni qilmaydi. **SMI** (small integer)larda ham reference yaratilmaydi:

```js
for (let i = 0; i < 1000; ++i) {
	// tez 🚀 (SMI bo'lgani uchun, reference yaratilmaydi)
}

for (let i = 0.1; i < 1000.1; ++i) {
	// sekin 🐌 (SMI bo'lmagani uchun, har bittasi uchun reference yartiladi)
}
```

[<Image 
  src="/images/posts/actual-primitive-values/v8-smi-vs-heapnumber.png"
  alt='Smi vs HeapNumber'
  className="border-2 rounded border-[#0a0a23]"
  loading='lazy'
/>](https://v8.dev/blog/react-cliff#smi-heapnumber-mutableheapnumber)

LEKIN BU HOLATNI SPECIFICATION TALAB QILMAYDI! Enginelar performance-ni oshirish maqsadida optimization qiladi.
