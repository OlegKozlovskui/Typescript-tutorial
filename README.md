# Typescript tutorial

- [Data types](#data-types)
- [Functions](#functions)
- [Class](#class)
- [Interface](#interface)
- [Modificators](#modificators)
- [Generic](#generic)
- [Decorators](#decorators)
- [Namespace](#namespace)

## Data types
У **TypeScript** доступні наступні типи даних:

- **Boolean**: логічне значення true або false
- **Number**: числове значення
- **String**: рядки
- **Array**: масиви
Масиви визначаються за допомогою формули [] і також є строго типізований. Тобто якщо спочатку масив містить рядки, то в майбутньому він зможе працювати тільки з рядками.
```javascript
let list: number[] = [10, 20, 30];
let colors: string[] = ["red", "green", "blue"];
console.log(list[0]);
console.log(colors[1]);
```
```javascript
let names: Array<string> = ["Tom", "Bob", "Alice"];
```
- **Tuple**: кортежі
**Tuples** також, як і масиви, представляють набір елементів, для яких вже заздалегідь відомий тип.
```javascript
let userInfo: [string, number];
userInfo = ["Tom", 28];
```
- **Enum**: перерахування
Тип **enum** призначений для опису набору числових даних за допомогою строкових констант. Так, оголосимо наступне перерахування:
```javascript
enum Season { Winter, Spring, Summer, Autumn };
let current: Season = Season.Summer;
console.log(current);
current = Season.Autumn;
```
Тут створюється змінна current, яка має тип Season. При цьому консоль виведе нам число 2. Так як всі елементи перерахування представляють числові значення. За замовчуванням такі:
```javascript
enum Season { Winter=0, Spring=1, Summer=2, Autumn=3 };
```
- **Any**: довільний тип
**Any** описує дані, тип яких може бути невідомий на момент написання програми.
- **Null і undefined**: відповідність нульових значень і невизначеним в javascript
- **Void**: відсутність певного типу

Більшість з цих типів відповідають примітивним типам з JavaScript.

- **Комплексні об'єкти** 
```javascript
let person = {name:"Tom", age:23};
```
#### Обєднання
**Об'єднання** або union не є власне типом даних, але вони дозволяють визначити змінну, яка може зберігати значення двох або більше типів:
```javascript
let names : string[] | string;
names = "Tom";
console.log(names); // Tom
names = ["Alice", "Bob"];
console.log(names[1]);  // Bob
```
#### Type assertion
**Type assertion** представляє модель перетворення значення змінної до певного типу. Зазвичай в деяких ситуаціях одна змінна може представляти якийсь широкий тип, наприклад, any, який за фактом допускає значення різних типів. Однак при цьому нам треба використовувати змінну як значення строго певного типу. І в цьому випадку ми можемо привести до цього типу.

Є дві форми приведення. Перша форма полягає в використанні кутових дужок:
```javascript
let someAnyValue: any = "hello world!";
let strLength: number = (<string>someAnyValue).length;
console.log(strLength); // 12
 
let someUnionValue: string | number = "hello work";
strLength = (<string>someUnionValue).length;
console.log(strLength); // 10
```
Друга форма полягає в застосуванні оператора **as**:
```javascript
let someAnyValue: any = "hello world!";
let strLength: number = (someAnyValue as string).length;
console.log(strLength); // 12
 
let someUnionValue: string | number = "hello work";
strLength = (someUnionValue as string).length;
console.log(strLength); // 10
```
## Functions
**TypeScript** також визначає функцію за допомогою ключового слова function, але при цьому додає додаткові можливості по роботі з функціями. Зокрема, тепер ми можемо визначити тип переданих параметрів і тип значення, що повертається. Типове визначення функції в TypeScript:
```javascript
function add(a: number, b: number): number {
    return a + b;
}
```
Функція може мати параметри, які вказуються після назви функції в дужках через кому. Через двокрапку після імені параметра вказується його тип.

Якщо функція нічого не повертає, то вказується тип **void**
```javascript
function add(a: number, b: number): void {
    console.log(a + b);
}
```
##### Необов'язкові параметри і параметри за замовчуванням
Щоб мати можливість передавати різну кількість значень в функцію, в **TS** деякі параметри можна оголосити як необов'язкові. Необов'язкові параметри повинні бути позначені знаком**?**. Причому необов'язкові параметри повинні йти після обов'язкових:
```javascript
function getName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
```

#### Тип функції
```javascript
let operation: (x: number, y: number) => number;
operation = function(x: number, y: number): number {
    return x + y;
};
console.log(operation(10, 20)); // 30
operation = function (x: number, y: number): number {
    return x * y;
};
console.log(operation(10, 20)); // 200
```
Тут визначена змінна operation, яка має тип (x: number, y: number) => number;, тобто фактично являє деяку функцію, яка приймає два параметри типу number і повертає значення також типу number. Однак на момент визначення змінної невідомо, яку саме функцію вона буде представляти.

#### Функції зворотного виклику
Тип функції можна використовувати як тип змінної, але він також може застосовуватися для визначення типу параметра іншої функції:
```javascript
function mathOp(x: number, y: number, operation: (a: number, b: number) => number): number{
 
    let result = operation(x, y);
    return result;
}
let operationFunc: (x: number, y: number) => number;
operationFunc = function (a: number, b: number): number {
    return a + b;
}
console.log(mathOp(10, 20, operationFunc)); // 30 
 
operationFunc = function (a: number, b: number): number {
    return a * b;
}
console.log(mathOp(10, 20, operationFunc)); // 200 
```
#### Arrow functions
```javascript
let sum = (x: number, y: number) => x + y;
```

## Class
Крім звичайних функцій класи мають спеціальні функції - конструктори, які визначаються за допомогою ключового слова **constructor**. Конструктори виконують початкову ініціалізацію об'єкта.

```javascript
class User {
    id: number;
    name: string;
    constructor(userId: number, userName: string) {
        this.id = userId;
        this.name = userName;
    }
    getInfo(): string {
        return "id:" + this.id + " name:" + this.name;
    }
}
```
У TypeScript наслідування реалізується за допомогою ключового слова **extends**
```javascript
class User {
    name: string;
    constructor(userName: string) {
        this.name = userName;
    }
	
    getInfo(): void {
        console.log("Имя: " + this.name);
    }
	
    getClassName(): string {
        return "User";
    }
}
 
class Employee extends User {
    company: string;
    constructor(employeeCompany: string, userName: string) {
        super(userName);
        this.company = employeeCompany;
    }
 
    getInfo(): void {
        super.getInfo();
        console.log( this.company);
    }
    getClassName(): string {
        return "Employee";
    }
}
```
## Interface
Інтерфейс визначає властивості і методи, які об'єкт повинен реалізувати. Іншими словами, інтерфейс - це визначення кастомниx типів даних, але без реалізації.

```javascript
interface IUser {
    id: number;
    name: string;
    getFullName(surname: string): string;
}
let employee: IUser = {
    id: 1, 
    name: "Alice",
    getFullName : function (surname: string): string {
        return this.name + " " + surname;
    }
}

```
#### Интерфейси классів
Інтерфейси можуть бути реалізовані не тільки об'єктами, але і класами. Для цього використовується ключове слово **implements**:
```javascript
interface IUser {
    id: number;
    name: string;
    getFullName(surname: string): string;
}
 
class User implements IUser{
    id: number;
    name: string;
    age: number;
	
    constructor(userId: number, userName: string, userAge: number) {
        this.id = userId;
        this.name = userName;
        this.age = userAge;
    }
    getFullName(surname: string): string {
        return this.name + " " + surname;
    }
}
```
#### Наслідування інтерфейсів
Інтерфейси, як і класи, можуть наслідуватись:
```javascript
interface IMovable {
    speed: number;
    move(): void;
}

interface ICar extends IMovable {
    fill(): void;
}
```

## Modificators
- **public**
Якщо до властивостей і функцій класів не застосовується модифікатор, то такі властивості і функції розцінюються як ніби вони визначені з модифікатором **public**.

- **private**
Якщо ж до властивостей і методів застосовується модифікатор **private**, то до них неможна буде звернутися ззовні при створенні об'єкта даного класу.

- **protected**
Модифікатор **protected** багато в чому аналогічний **private** - властивості і методи з даними модифікатором не видно , але до них можна звернутися з класів-спадкоємців.

Методи доступу визначаються як звичайні методи, тільки перед ними ставляться ключові слова **get/set**. Set-метод контролюється установку значення, а get-метод повертає значення.

## Generic
**TypeScript** є строго універсальна мова, проте іноді треба побудувати функціонал так, щоб він міг використовувати дані будь-яких типів. У деяких випадках ми могли б використовувати тип **any**:
```javascript
function getId(id: any): any {
    return id;
}
```
Однак в цьому випадку ми не можемо використовувати результат функції як об'єкт того типу, який переданий в функцію. Для нас це тип any. Якби замість числа 5 в функцію передавався б об'єкт будь-якого класу, і нам потім треба було б використовувати цей об'єкт, наприклад, викликати у нього функції, то це було б проблематично. І щоб конкретизувати повертається тип, ми можемо використовувати узагальнення:
```javascript
function getId<T>(id: T): T {
    return id;
}
```
За допомогою виразу **T** ми вказуємо, що функція getId типізовані певним типом **T**. При виконанні функції замість **T** буде підставлятися конкретний тип. Причому на етапі компіляції конкретний тип ніхто не знає. І повертати функція буде об'єкт цього типу.

## Decorators
**Decorators** - це спеціальна можливість тайпскріпта по добавленню метадати для класів і функцій. Основне правило що декоратор приймає як параметр котструктор того класу до якого він прикліплений.
```javascript
function addShowAbility(constructorFn: Function) {
	constructorFn.prototype.showHtml = function() {
		const pre = document.createElement('pre');
		pre.innerHTML = JSON.stringify(this);
		document.body.appendChild(pre);
	}
}

@addShowAbility
class User {
	constructor(public name: string, public age: number) {}
}
```

## Namespace
**Namespace** - це певна локальна сутність яка містить певний набір функціоналу.
Namespace містять групу класів, інтерфейсів, функцій, інших просторів імен, які можуть використовуватися в деякому загальному контексті.

```javascript
namespace UserNamespace {
 
    export interface IUser {
        displayInfo();
    }
 
    export class User implements IUser {
        private _id: number;
        private _name: string;
        constructor(id: number, name: string) {
            this._id = id;
            this._name = name;
        }
        displayInfo() {
            console.log("id: " + this._id + "; name: " + this._name);
        }
    }
     
    let defaultUser: IUser = new User(2, "Tom");
}
```
В даному випадку модуль називається UserNamespace і містить інтерфейс, клас і об'єкт defaultUser. Щоб типи і об'єкти, визначені в модулі, було видно ззовні, вони визначаються з ключовим словом **export**.
У програмі можна кілька разів оголосити модуль з одним і тим же ім'ям, в цьому випадку всі ці визначення потім зіллються в один модуль.
```javascript
namespace UserNamespace {
    export class User{ }
}
 
namespace UserNamespace {
    export class Employee extends User { }
    export class Manager extends User { }
}
```