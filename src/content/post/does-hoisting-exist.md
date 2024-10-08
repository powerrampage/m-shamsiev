---
title: 'Javascript-da hoisting mavjudmi?'
description: "Hoisting jargonining paydo bo'lish sabablari va uning haqiqatga qanchalik yaqinligi, aslida qanday ishlashi va nega bu termin ko'p dasturchilar orasida tarqalgani bo'yicha."
publishDate: '27 July 2024'
tags:
  ['javascript', 'hoisting', 'ecmascript', 'specification', 'jargon', 'uzbek', 'execution context']
---

## Hoisting Nima ?

Hoisting - qaysidir manbalarga ko'ra: ko'tarish, o'zgaruvchilar(var, let, const), funksiyalar, class'lar...ni biz e'lon qilmasimizdan oldin tepaga ko'tarish va ishalatish.
WTF? magic 👀

## Specification'ga Muvofiq Hoisting

ECMAScript bu standart. ECMAScript JavaScript specification'dir, har qanday javascript engine, ecmascript standartga bo‘ysunadi. Ko'cha tilida aytilganda: ECMAScript - mashina bo'lsa, javascript engine'lar mashina turlari. Ko'rinishi, ichidagi implementation'lar har xil bo'lishi mumkin, lekin hammasi standartga bo‘ysunadi. Gazni bosganda haydaydi, tormozni bosa to'xtaydi, ...

`Har bir engine standartga bo‘ysunadi. Specification'da yozilgan barcha narsalar yagona manba va haqiqatdir`

[ECMAScript specification](https://tc39.es/ecma262/)'da **hoisting** termin mavjud emas, boshqa ecma specification version'larda ham bo'lmagan. Faqat **HoistableDeclaration** bor, lekin bu umman hoistinga aloqador emas. [**HoistableDeclaration**](https://tc39.es/ecma262/multipage/ecmascript-language-statements-and-declarations.html#prod-HoistableDeclaration) - bu qayta ishlanishi mumkin bo'lgan funksiya va generator deklaratsiyasini o'z ichiga olgan deklaratsiya toifasi.

Demak **hoisting**ni **jargon** desak ham bo'ladi. Jargon nima ?

_Jargon (fransuzcha: jargon safsata) — biror ijtimoiy guruhning oʻziga xos leksikasi._
[wikipedia](https://uz.wikipedia.org/wiki/Jargon).
Ya'ni odamlar tomonidan o'ylab topilgan va haqiqatga yaqin bo'lmagan termin. [Mif](https://uz.wikipedia.org/wiki/Mif) desa ham bo'ladi.

## Tahlil Qilamiz

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

### Aslida Javascript 3 Fazali:

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

- **Realm** - bizning kodimiz ishlaydigan izolyatsiya qilingan muhit, masalan, brauzerlarda yangi tab ochilganda yangi realm yaratiladi.

- **LexicalEnvironment** - **var** o'zgaruvchilardan `tashqari` hamma narsa uchun bog'lanishlarni o'z ichiga olgan muhitdir (`let`, `const`, `functions`, ...).

- **VariableEnvironment** - faqat **var** bilan e'lon qilingan o'zgaruvchilar uchun bog'lanishlarni saqlaydigan muhitdir (only `var`).

**LexicalEnvironment** va **VariableEnvironment** o'zining **EnvironmentRecord** mavjud.
[**EnvironmentRecord**](https://tc39.es/ecma262/#sec-environment-records) - bu ECMAScript kodining lexical nesting structure'ga asoslangan identifikatorlarning ma'lum o'zgaruvchilar va functionlar bilan bog'lanishini aniqlash uchun ishlatiladigan specification turi. Odatda **Environment Record** _FunctionDeclaration_, _BlockStatement_ yoki _TryStatementning Catch_ bandi kabi ECMAScript kodining o‘ziga xos syntax tuzilishi bilan bog‘lanadi. Har safar bunday kod baholanganda, ushbu kod tomonidan yaratilgan identifikator bog'lanishlarini yozish uchun yangi **Environment Record** yaratiladi.

Environment Record subclasses:

- [Declarative Environment Record](https://tc39.es/ecma262/#sec-declarative-environment-records)

- [Object Environment Record](https://tc39.es/ecma262/#sec-object-environment-records)

- [Global Environment Record](https://tc39.es/ecma262/#sec-global-environment-records)

Example:

```js
var foo = 123
let baz = 'Hello, Saturn!'

function qwx(msg) {
	return msg + foo
}

qwx(baz)
```

Visualize:

```yml
GlobalExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      DeclarativeEnvironmentRecord: {
        baz: <uninitialized>,
        qwx: < function qwx(msg) {
          return msg + foo;
        } >,
      }, // (let, const, function, ...) bilan e'lon qilinganlar joylashadi.
      ObjectEnvironmentRecord: {
        window: <ref. to Global obj.>,
        this: <ref. to window obj.>,
      },
      OuterEnv : <null>, // ref. ota (env. record.) (bu erda null, chunki global execution bo'lgani uchun, ota execution context mavjud emas)
    },
  },
  VariableEnvironment: {
    EnvironmentRecord: {
      DeclarativeEnvironmentRecord: {
        foo: undefined,
      }, // tepada aytilganidek bu muhit (`var`) uchun.
    },
  },
}
```

Yuqoridagi kod parchasi yordamida `Creation phase`:

1. JS engine creation bosqichga kiradi.
2. `window object` uchun `Global object` uchun bindinglarni yaratadi.
3. **VariableEnvironment**ga `foo` identifikatorini yaratadi va uni `undefined` qiymat bilan initialize qiladi.
4. **LexicalEnvironment**ga `baz` identifikatorini yaratadi va uni `<uninitialized>` qilib qoyadi (initialized bo'lmaydi).
5. **LexicalEnvironment**ga `qwx` identifikatorini yaratadi va function declaration linkni store qiladi.
6. **Global Execution Context** uchun `Creation Phase` tugaydi va `Execution Phase` boshlanadi.
7. `qwx` chaqiriladi. Yangi `Execution Context` create bo'lib `Execution Context Stack` (call stack)ga push bo'ladi.
   - Yangi `Execution Context` yaratilsa, va shu uchun `Creation Phase` boshlanadi.
   - **LexicalEnvironment**ga `msg` paramter joylashadi va `Creation Phase` tugaydi va `Execution Phase` boshlanadi.
   - `msg + foo` amal bajariladi. `foo` variable `qwx` *function execution context*da mavjud emasligi sababi, **[[OuterEnv]]** dan qidirishni boshlaydi, ya'ni tashqi environment'dan `foo` variable'ni qidiradi, shuyerni _"scope chain"_ deyishadi. Closure'lar ham shu **[[OuterEnv]]** orqali variable'larga access qiladi.
   - Javob return bo'ladi. `Execution Phase` tugaydi. `qwx` execution context *call stack*dan pop bo'ladi.
8. **Execution Context Stack** (call stack) empty bo'lib qoladi.

Xulosa:
**Variable**lar va **Function**lar qanday qaysi paytda qayerga reference'i saqlashini ko'rib chiqdik. Agar chuqurroq bilishni xohlasayiz, [specification](https://tc39.es/ecma262)ni o'qib chiqishni maslahat beraman, asl javascript ishlashini bilish uchun (men ham to'liq o'qimapman hali).

## Hoisting jargonining paydo bo'lish sabablari va bu termin ko'p dasturchilar orasida tarqalgani

Aniq fatklar mavud emas. Aytishlaricha: tilni to'liq o'rganmasdan, ichiga deep kirmasdan yokida ecmascript specificationi to'liq o'rganib chiqmasdan. Ba'zi bir doc, book, blog va tutorial yozuvchilar masalan ([_learn.javscript.ru_](https://learn.javascript.ru/), [_javascript.info_](https://javascript.info/), _JavaScript books: The Good Parts_, _The Definitive Guide_, ...), o'zilari 100% aniqlik kiritmasdan. O'zlaricha "JS shunaqa ishlar ekanda" dep o'zlari tasavvur qiladigan holatda "nom" o'ylab topishgan. Aniq emas bu terminlar qaysi resource'dan tarqalib ketgan. Lekin shu "o'ylab topilgan terminlar" ecma standarda yoqligi sababli jargon hisoblanadi.

#### Lekin hamma shu jargonni ishlatsa nimasi yomon?

Birinchi o'rinda **Narrow Mental Model**ni yaratadi. Hoisting juda ko'p ishonish tushunishni cheklashi mumkin. Agar developer'lar faqat "hoisting" nuqtai nazaridan o'ylashsa, ular JavaScript-ning boshqa muhim jihatlarini, masalan, "closure ishlashi" yoki "declaration vaqti" va "initialization vaqti" o'rtasidagi farqni o'tkazib yuborishlari mumkin.
Bu jargon so'z haqiqatga yaqin ham emas. Endi tilni o'rganadigan dasturchilar qiynaladi bu terminlarni tushunishga.

Qisqacha aytganda, bu termin "noto'g'ri" emas, lekin unga haddan tashqari ishonish JavaScript-ni execution modelini yanada "nuanced" tushunishni cheklashi mumkin.

<Image 
  src="/images/posts/does-hoisting-exist/stick-term-hoisting.png"
  alt='Why Did the Term Stick?'
  loading='lazy'
/>
