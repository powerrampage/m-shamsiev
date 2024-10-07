---
title: 'Javascript-da hoisting mavjudmi?'
description: "Hoisting jargonining paydo bo'lish sabablari va uning haqiqatga qanchalik yaqinligi, aslida qanday ishlashi va nega bu termin ko'p dasturchilar orasida tarqalgani bo'yicha."
publishDate: '27 July 2024'
tags: ['javascript', 'hoisting', 'ecmascript', 'specification', 'jargon', 'uzbek']
draft: true
---

## Hoisting Nima ?

Hoisting - qaysidir manbalarga ko'ra: ko'tarish, o'zgaruvchilar(var, let, const), funksiyalar, class'lar...ni biz e'lon qilmasimizdan oldin ishlata olish.
WTF? magic 👀

## Specification'ga Muvofiq Hoisting

ECMAScript bu standart. ECMAScript JavaScript specification'dir, har qanday javascript engine, ecmascript standartga bo‘ysunadi. Ko'cha tilida aytilganda: ECMAScript - mashina bo'lsa, javascript engine'lar mashina turlari. Ko'rinishi, ichidagi implementation'lar har xil bo'lishi mumkin, lekin hammasi standartga bo‘ysunadi. Gazni bosganda haydaydi, tormozni bosa to'xtaydi, ...

`Har bir engine standartga bo‘ysunadi. Specification'da yozilgan barcha narsalar yagona manba va haqiqatdir`

[ECMAScript specification](https://tc39.es/ecma262/)'da **hoisting** termin mavjud emas, boshqa ecma specification version'larda ham bo'lmagan. Faqat **HoistableDeclaration** bor, lekin bu umman hoistinga aloqador emas. [**HoistableDeclaration**](https://tc39.es/ecma262/multipage/ecmascript-language-statements-and-declarations.html#prod-HoistableDeclaration) - bu qayta ishlanishi mumkin bo'lgan funksiya va generator deklaratsiyasini o'z ichiga olgan deklaratsiya toifasi.

Demak **hoisting**ni **jargon** desak ham bo'ladi. Jargon nima ?

_Jargon (fransuzcha: jargon safsata) — biror ijtimoiy guruhning oʻziga xos leksikasi._
[wikipedia](https://uz.wikipedia.org/wiki/Jargon).
Ya'ni odamlar tomonidan o'ylab topilgan va haqiqatga yaqin bo'lmagan termin. [Mif](https://uz.wikipedia.org/wiki/Mif) desa ham bo'ladi.

## Tahlil qilamiz

```js
function foo() {
	console.log(baz) // undefined
	var baz = 123
}
foo()
```

`foo` funksiya chaqirilsa. `baz` hoisting bo'ladi, tepaga ko'tariladi va assign bo'lmasidan oldin access qilganda `undefined` bo'ladi. `let` va `const` larda access qilib bo'lmaydi, sababi Reference Error beradi.

Ok!

```js
function foo() {
	let baz = 123
	let baz = 321
}
```

Ushbu kod-da, `foo` funksiya ichida 2-ta variable `let` bilan e'lon qilingan. `foo` funksiya chaqirilmagan bo'lsa ham, bu kod error beradi. Hali variable'larni e'lon qilmasdan, hoisting'ga uchramasdan. Nimaga error beradi ? Hamma jim... 🗿 Hop bunga nima deysiz:

```js
function foo() {
	const id = 123
	id = 100

	xsadjknasxiodjas
	return sdapdmasdspam
}
```

`foo` chaqirilmasa bu kod error bermaydi. Oops! really ? `foo`ni ichida constant'ni o'zgartirsam ham, yaratilmagan variable'larni ishlatsam ham xato bermayapti.
Nega bunday ? Bu savollarga javob topish uchun, oxiri specification'ga murojaat qilishga to'g'ri keladi (haqiqat manbasi :D).

### Aslida Javascript 3 fazali:

1. Static Semantics
2. Runtime Semantics
3. EvaluateBody

#### Static Semantics

Statik semantika - qoidalarni tavsiflaydi va kodni ishga tushirmasdan aniqlash mumkin bo'lgan xususiyatlarni tekshiradi. Ushbu qoidalar ishlash vaqtida emas, balki kompilyatsiya vaqtida (yoki tahlil qilish bosqichida) tekshiriladi. Statik semantika kod tilning syntax va tizimli qoidalariga rioya qilishini ta'minlaydi, lekin kodni bajarmaydi.

- Variable e'lon qilish qoidalari (masalan variableni bitta scope'da ikki marta e'lon qilib bo'lmasligi)
- Scope qoidalari (masalan lexical scoping va variable'larni ko'rinishi)
- Type checking

#### Runtime Semantics

Runtime Semantics - kod bajarilganda uning harakatini tavsiflaydi. Bu semantika til konstruksiyalari qanday ishlashini, iboralar qanday baholanishini, boshqaruv oqimini va bajarilish jarayonida dastur holati qanday o‘zgarishini belgilaydi.

- Expression'larni baholash (masalan, arifmetik amallar, funksiya chaqiruvlari).
- Boshqaruv tuzilmalarining xatti-harakatlari (masalan, looplar, conditionalar).
- Dinamik xususiyatlarni boshqarish (masalan, dynamic typing, closures).
- ob'ektlar va ularning xususiyatlarini manipulation qilish.

#### Static Semantics & Runtime Semantics

Xulosa qilib aytganda, Static Semantics kodning syntax va tizimli ravishda to'g'riligini ta'minlaydigan kompilyatsiya vaqti qoidalari va tekshiruvlarini o'z ichiga oladi, Runtime Semantics esa amaldagi bajarilish xatti-harakati va dastur ishlayotgan paytda yuzaga keladigan dinamik holat o'zgarishlarini o'z ichiga oladi.

### Let da 2 marta e'lon qilingan variable'lar

[<Image 
  src="/images/posts/does-hoisting-exist/double-let.png"
  alt='Double let specification screenshot'
  className="border-2 rounded border-[#52e052]"
  loading='lazy'
/>](https://tc39.es/ecma262/multipage/ecmascript-language-statements-and-declarations.html#sec-let-and-const-declarations-static-semantics-early-errors)

Static Semantics sharti bilan: BindingList ning BoundNames tarkibida `let` bo'lsa, bu sintaksis xato hisoblanadi. Va birinchi fazada error beradi. [Resource](https://tc39.es/ecma262/multipage/ecmascript-language-statements-and-declarations.html#sec-let-and-const-declarations-static-semantics-early-errors)ni topdikmi ?

### Let va Const Deklaratsiyalari

[<Image 
  src="/images/posts/does-hoisting-exist/let-and-const-declarations.png"
  alt='Let and Const Declarations'
  className="border-2 rounded border-[#52e052]"
  loading='lazy'
/>](https://tc39.es/ecma262/multipage/ecmascript-language-statements-and-declarations.html#sec-let-and-const-declarations)

> **_NOTE:_** `let` and `const` declarations define variables that are scoped to the running execution context's LexicalEnvironment.
> The variables are created when their containing Environment Record is instantiated but may not be accessed in any way until the variable's LexicalBinding is evaluated.
> A variable defined by a LexicalBinding with an _Initializer_ is assigned the value of its _Initializer_'s _AssignmentExpression_ when the LexicalBinding is evaluated, not when the variable is created.
> If a LexicalBinding in a `let` declaration does not have an _Initializer_ the variable is assigned the value `undefined` when the LexicalBinding is evaluated.

`Let va Const o'zgaruvchilar ularni o'z ichiga olgan Environment Record yaratilganda so'ng yaratiladi, lekin o'zgaruvchining LexicalBinding evaluated(baholanmaguncha) ularga hech qanday tarzda access qilish mumkin emas.`

`let` va `const`lar uchun javob topildi, hoisting haqida sado ham chiqqani yoq.

### Var Deklaratsiyasi

[<Image 
  src="/images/posts/does-hoisting-exist/about-var.png"
  alt='Var Statement'
  className="border-2 rounded border-[#52e052]"
  loading='lazy'
/>](https://tc39.es/ecma262/multipage/ecmascript-language-statements-and-declarations.html#sec-variable-statement)

> **_NOTE:_** A `var` statement declares variables that are scoped to the running execution context's _VariableEnvironment_. Var variables are created when their containing Environment Record is instantiated and are initialized to `undefined` when created. Within the scope of any _VariableEnvironment_ a common _BindingIdentifier_ may appear in more than one _VariableDeclaration_ but those declarations collectively define only one variable. A variable defined by a _VariableDeclaration_ with an Initializer is assigned the value of its Initializer's _AssignmentExpression_ when the _VariableDeclaration_ is executed, not when the variable is created.

`Var o'zgaruvchilari o'z ichiga olgan Environment Record yaratilganda so'ng yaratiladi va yaratilganda undefined'ga initialized qilinadi`

## Execution Context

Execution Context juda ham katta mavzu, tushuntirish uchun alohida post yaratish kerak! Asosiy va hoistinga tegishli concept'larni aytib o'taman.

Execution Context fazalari:

1. Creation phase (Yaratilish bosqichi)
2. Execution phase (Bajarish bosqichi)

Sodda tilda:
Agar function call qilinsa, **Execution context** create bo'lib, **Execution Context Stack** (Call Stack) ga push bo'ladi. Har bir execution contextni o'zining component'lari mavjud. Bulardan (**Realm**, **LexicalEnvironment**, **VariableEnvironment**, ...)
