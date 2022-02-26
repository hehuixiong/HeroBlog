---
title: JavaScript中命名约定的最佳实践
date: 2022-02-24 18:00:00
cover: https://cdn.ithhx.cn/img-blog/20220226142235.png
top_img: https://cdn.ithhx.cn/img-blog/20220226142235.png
categories:
  - JavaScript
tags:
  - JavaScript
---

## 前言

在`前端开发`过程中，遵循标准的命名约定可以提高代码的可读性。下面就来看看 `JavaScript` 中命名约定的最佳实践。

***

### 1. 变量的命名约定

> JavaScript 变量名称是区分大小写的，大写和小写字母是不同的。比如：

```javascript
let DogName = 'Scooby-Doo';
let dogName = 'Droopy';
let DOGNAME = 'Odie';
console.log(DogName);   // "Scooby-Doo"
console.log(dogName);   // "Droopy"
console.log(DOGNAME);   // "Odie"
```

> 但是，最推荐的声明 JavaScript 变量的方法是使用驼峰式变量名。我们可以对JavaScript 所有类型的变量使用驼峰式命名约定，这样就不会相同命名的变量。

```javascript
// bad
let dogname = 'Droopy'; 
// bad
let dog_name = 'Droopy'; 
// bad
let DOGNAME = 'Droopy'; 
// bad
let DOG_NAME = 'Droopy'; 
// good
let dogName = 'Droopy';
```

> 变量的名称应该是不言自明的，并描述了储存的值。例如，如果需要一个变量来储存狗的名字，应该使用 dogName 而不是 Name，因为 dogNam 更有意义：

```javascript
// bad
let d = 'Droopy';
// bad
let name = 'Droopy';
// good
let dogName = 'Droopy';
```

### 2. 布尔值的命名约定

> 当定义布尔类型的变量时，应该使用is或者has作为变量的前缀。例如，如果需要一个变量来检查狗是否有主人，应该使用 hasOwner 作为变量名：

```javascript
// bad
let bark = false;
// good
let isBark = false;

// bad
let ideal = true;
// good
let areIdeal = true;

// bad
let owner = true;
// good
let hasOwner = true;
```

### 3. 函数的命名约定

> JavaScript 中函数的名称也是区分大小写的。因为在声明函数时，推荐使用驼峰式方法来命名函数。
> 除此之外，推荐使用描述性名词和动词来作为前缀。例如，如果声明一个函数来获取名称，则函数名字应该是 getName：

```javascript
// bad
function name(dogName, ownerName) { 
  return '${dogName} ${ownerName}';
}

// good
function getName(dogName, ownerName) { 
  return '${dogName} ${ownerName}';
}
```

### 4. 常量的命名约定

> JavaScript 中的常量和变量是一样的，都区分大小写，在定义常量时，推荐使用大写，因为它们是不变的变量。

```javascript
const LEG = 4;
const TAIL = 1;
const MOVABLE = LEG + TAIL;
```

> 如果变量声明名称中包含多个单词，就应该使用 UPPER_SNAKE_CASE。

```javascript
const DAYS_UNTIL_TOMORROW = 1;
```

### 5. 类的命名约定

> JavaScript 中类的命名约定规则与函数非常相似，推荐使用描述性的名称来描述类的功能。
> 函数名和类名之间的主要区别在于类名要使用大写开头：

```javascript
class DogCartoon { 
  constructor(dogName, ownerName) { 
    this.dogName = dogName; 
    this.ownerName = ownerName; 
  }
}

const cartoon = new DogCartoon('Scooby-Doo', 'Shaggy');
```

### 6. 组件的命名规则

> JavaScript 组件广泛应用于React、Vue等前端框架中。组件的命名建议与类保持一致，使用开头大写的驼峰式命名法：

```javascript
// bad
function dogCartoon(roles) { 
  return ( 
    <div> 
      <span> Dog Name: { roles.dogName } </span> 
      <span> Owner Name: { roles.ownerName } </span> 
    </div> 
  );
} 

// good
function DogCartoon(roles) { 
  return ( 
    <div> 
      <span> Dog Name: { roles.dogName } </span> 
      <span> Owner Name: { roles.ownerName } </span> 
    </div> 
  );
}
```

> 由于组件的命名开头字母是大写，因此在使用时，就很容易和HTML、属性值等区分开来：

```javascript
<div> 
  <DogCartoon 
    roles={{ dogName: 'Scooby-Doo', ownerName: 'Shaggy' }} 
  />
</div>
```

### 7. 方法的命名约定

> 这里说的方法指的是类中方法，在 JavaScript 中，类的方法和函数的结构是非常类似的，因此，命名约定规则也是一样的。
> 推荐需要使用驼峰式方法来声明 JavaScript 方法，并使用动词作为前缀，使方法名称更有意义：

```javascript
class DogCartoon {
  constructor(dogName, ownerName) { 
    this.dogName = dogName; 
    this.ownerName = ownerName; 
  }

  getName() { 
    return '${this.dogName} ${this.ownerName}'; 
  }
}

const cartoon= new DogCartoon('Scooby-Doo', 'Shaggy');

console.log(cartoon.getName());   // "Scooby-Doo Shaggy"
```

### 8. 私有函数的命名约定

> 下划线 (_) 在 MySQL 和 PHP 等语言中广泛用于定义变量、函数和方法。但在 JavaScript 中，下划线用于表示私有变量或函数。
> 例如，有一个私有函数名 toonName，则可以通过添加下划线作为前缀 (_toonName) 来将其表示为私有函数。

```javascript
class DogCartoon { 
  constructor(dogName, ownerName) { 
    this.dogName = dogName; 
    this.ownerName = ownerName; 
    this.name = _toonName(dogName, ownerName); 
  } 
  _toonName(dogName, ownerName) { 
    return `${dogName} ${ownerName}`; 
  } 
}

const cartoon = new DodCartoon('Scooby-Doo', 'Shaggy'); 

// good
const name = cartoon.name;
console.log(name);   // "Scooby-Doo Shaggy" 

// bad
name =cartoon._toonName(cartoon.dogName, cartoon.ownerName);
console.log(name);   // "Scooby-Doo Shaggy"
```

### 9. 全局变量的命名约定

> 对于 JavaScript 全局变量，没有特定的命名标准。建议对可变全局变量使用驼峰式大小写的方式，对不可变全局对象使用大写。

### 10. 文件名的命名约定

> 大多数 Web 服务器（Apache、Unix）在处理文件时都区分大小写。例如，flower.jpg 和 Flower.jpg 是不一样的。
> 但是，如果从不区分大小写的服务器切换到区分大小写的服务器，即使是一个小错误也可能导致网站崩溃。
> 因此，尽管它们是支持区分大小写的，建议在所有服务器中还是使用小写来命名文件。

***