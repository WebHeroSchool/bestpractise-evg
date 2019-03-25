## BEST JS PRACTICES

*Данный гайд содержит лучшие практики кодинга на JavaScript.*
Источник: [w3schools.com](https://www.w3schools.com/js/js_best_practices.asp)

### (1) Избегать глобальных переменных
Это относится ко всем типам данных, к переменным, объектам и функциям. По возможности лучше использовать **замыкание**. 
Если пренебрегать данным правилом, может оказаться, что данные окажутся перезаписанными другими сценариями из этого же или другого скрипта.

```js
function user() {
    var name = 'Unknown';
    return {
        getName: function() {
            return name;
        },
        setName: function(newName) {
            name = newName;
        }
    }
}

var testUser = user();
testUser.getName(); // Unknown
testUser.setName(‘John Smith’); // Изменяем значение приватной переменной
testUser.getName(); // John Smith
```
[source](https://getinstance.info/articles/javascript/closures-in-javascript/)


### (2) Всегда объявлять локальные переменные
Все переменные внутри локальной scope должны быть объявлены с помощью ключевого слова `let` или `const`, иначе они попадут в глобальную scope.

```js
function foo(x) {
    let bar = x + 1;
    console.log(bar);
}
```


### (3) Выносить переменные в начало скрипта
Данная практика считается хорошей, поскольку она позволяет:
+ видеть более **чистый код**;
+ **не искать** переменные по всему скрипту;
+ проще избежать проблем с **непредвиденными глобальными** переменными;
+ снизить вероятность **повторных объявлений** переменных.

```js
// Declare at the beginning
var firstName, lastName, price, discount, fullPrice;

// Use later
firstName = "John";
lastName = "Doe";

price = 19.90;
discount = 0.10;

fullPrice = price * 100 / discount;
```


### (4) Инициализировать переменные при объявлении
Данная практика является хорошей, поскольку она позволяет:
- видеть более **чистый код**;
- хранить все **инициализированные** переменные в одном месте;
- предупредить появление переменных со значением **undefined**;
- инициализированные переменные создают идею их последующего использования, определяют **тип данных**;

```js
// Declare and initiate at the beginning
var firstName = "",
lastName = "",
price = 0,
discount = 0,
fullPrice = 0,
myArray = [],
myObject = {};
```


### (5) Никогда не объявлять число, строку или булевское значение как объект!
Данные типы являются примитивными, и объявление их через `new` замедляет скорость обработки данных и производит множество неприятных побочных эффектов.

```js
var x = "John";             
var y = new String("John");
(x === y) // is false because x is a string and y is an object.
```
или даже хуже
```js
var x = new String("John");             
var y = new String("John");
(x == y) // is false because you cannot compare objects.
```


### (6) Не создавать новый объект с помощью синтаксиса `new Object`!
Данная практика тоже считается плохой, поскольку данный способ создания менее эффективен.

```js
var x1 = {};           // new object
var x2 = "";           // new primitive string
var x3 = 0;            // new primitive number
var x4 = false;        // new primitive boolean
var x5 = [];           // new array object
var x6 = /()/;         // new regexp object
var x7 = function(){}; // new function object
```


### (7) Остерегаться неявного преобразования данных

```js
var x = "Hello";     // typeof x is a string
x = 5;               // changes typeof x to a number
```

Данная практика способна порождать ошибки в операциях. 

```js
var x = 5 + 7;       // x.valueOf() is 12,  typeof x is a number
var x = 5 + "7";     // x.valueOf() is 57,  typeof x is a string
var x = "5" + 7;     // x.valueOf() is 57,  typeof x is a string
var x = 5 - 7;       // x.valueOf() is -2,  typeof x is a number
var x = 5 - "7";     // x.valueOf() is -2,  typeof x is a number
var x = "5" - 7;     // x.valueOf() is -2,  typeof x is a number
var x = 5 - "x";     // x.valueOf() is NaN, typeof x is a number
```

Однажды объявленная переменная должна хранить один и тот же тип данных. При операциях, для того чтобы получить число, иной раз стоит явно преобразовать переменную к числу:

```js
let foo = +bar % 5;
```


### (8) Использовать оператор сравнения '==='
Использование данного оператора позволяет избежать нежелательного преобразования типов данных. Если нам необходимо удостовериться, что переменные действительно идентичны друг другу, стоит использовать строгое равенство.

```js
0 == "";        // true
1 == "1";       // true
1 == true;      // true

0 === "";       // false
1 === "1";      // false
1 === true;     // false
```


### (9) Использовать значения параметров по умолчанию
Если функция вызвана без аргумента, недостающий аргумент принимает значение `undefined`. Такие значения могут сломать код, поэтому хорошей практикой является присвоение аргументам дефолтных значений.

```js
function myFunction(x, y) {
  if (y === undefined) {
    y = 0;
  }
}
```

ES2015 позволяет вставлять дефолтные значения прямо в параметры при объявлении функции:

```js
function (a=1, b=1) { // function code }
```


### (10) Всегда завершать синтаксис `switch` значением по умолчанию
Необходимо всегда завершать утверждения `switch` с помощью `default`. Даже если кажется, что в нем нет необходимости. Это позволяет иметь обходной сценарий на случай ошибки в коде.

```js
switch (new Date().getDay()) {
  case 0:
    day = "Sunday";
    break;
  case 1:
    day = "Monday";
    break;
  case 2:
    day = "Tuesday";
    break;
  case 3:
    day = "Wednesday";
    break;
  case 4:
    day = "Thursday";
    break;
  case 5:
    day = "Friday";
    break;
  case 6:
    day = "Saturday";
    break;
  default:
    day = "Unknown";
}
```