# 数值的扩展

## 二进制和八进制表示法

ES6 提供了二进制和八进制数值的新的写法，分别用前缀`0b`（或`0B`）和`0o`（或`0O`）表示。

```javascript
0b111110111 === 503 // true
0o767 === 503 // true
```

从 ES5 开始，在严格模式之中，八进制就不再允许使用前缀`0`表示，ES6 进一步明确，要使用前缀`0o`表示。

```javascript
// 非严格模式
(function(){
  console.log(0o11 === 011);
})() // true

// 严格模式
(function(){
  'use strict';
  console.log(0o11 === 011);
})() // Uncaught SyntaxError: Octal literals are not allowed in strict mode.
```

如果要将`0b`和`0o`前缀的字符串数值转为十进制，要使用`Number`方法。

```javascript
Number('0b111')  // 7
Number('0o10')  // 8
```

## 数值分隔符

欧美语言中，较长的数值允许每三位添加一个分隔符（通常是一个逗号），增加数值的可读性。比如，`1000`可以写作`1,000`。

[ES2021](https://github.com/tc39/proposal-numeric-separator)，允许 JavaScript 的数值使用下划线（`_`）作为分隔符。

```javascript
let budget = 1_000_000_000_000;
budget === 10 ** 12 // true
```

这个数值分隔符没有指定间隔的位数，也就是说，可以每三位添加一个分隔符，也可以每一位、每两位、每四位添加一个。

```javascript
123_00 === 12_300 // true

12345_00 === 123_4500 // true
12345_00 === 1_234_500 // true
```

小数和科学计数法也可以使用数值分隔符。

```javascript
// 小数
0.000_001

// 科学计数法
1e10_000
```

数值分隔符有几个使用注意点。

- 不能放在数值的最前面（leading）或最后面（trailing）。
- 不能两个或两个以上的分隔符连在一起。
- 小数点的前后不能有分隔符。
- 科学计数法里面，表示指数的`e`或`E`前后不能有分隔符。

下面的写法都会报错。

```javascript
// 全部报错
3_.141
3._141
1_e12
1e_12
123__456
_1464301
1464301_
```

除了十进制，其他进制的数值也可以使用分隔符。

```javascript
// 二进制
0b1010_0001_1000_0101
// 十六进制
0xA0_B0_C0
```

可以看到，数值分隔符可以按字节顺序分隔数值，这在操作二进制位时，非常有用。

注意，分隔符不能紧跟着进制的前缀`0b`、`0B`、`0o`、`0O`、`0x`、`0X`。

```javascript
// 报错
0_b111111000
0b_111111000
```

数值分隔符只是一种书写便利，对于 JavaScript 内部数值的存储和输出，并没有影响。

```javascript
let num = 12_345;

num // 12345
num.toString() // 12345
```

上面示例中，变量`num`的值为`12_345`，但是内部存储和输出的时候，都不会有数值分隔符。

下面三个将字符串转成数值的函数，不支持数值分隔符。主要原因是语言的设计者认为，数值分隔符主要是为了编码时书写数值的方便，而不是为了处理外部输入的数据。

- Number()
- parseInt()
- parseFloat()

```javascript
Number('123_456') // NaN
parseInt('123_456') // 123
```

## Number.isFinite(), Number.isNaN()

ES6 在`Number`对象上，新提供了`Number.isFinite()`和`Number.isNaN()`两个方法。

`Number.isFinite()`用来检查一个数值是否为有限的（finite），即不是`Infinity`。

```javascript
Number.isFinite(15); // true
Number.isFinite(0.8); // true
Number.isFinite(NaN); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite('foo'); // false
Number.isFinite('15'); // false
Number.isFinite(true); // false
```

注意，如果参数类型不是数值，`Number.isFinite`一律返回`false`。

`Number.isNaN()`用来检查一个值是否为`NaN`。

```javascript
Number.isNaN(NaN) // true
Number.isNaN(15) // false
Number.isNaN('15') // false
Number.isNaN(true) // false
Number.isNaN(9/NaN) // true
Number.isNaN('true' / 0) // true
Number.isNaN('true' / 'true') // true
```

如果参数类型不是`NaN`，`Number.isNaN`一律返回`false`。

它们与传统的全局方法`isFinite()`和`isNaN()`的区别在于，传统方法先调用`Number()`将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，`Number.isFinite()`对于非数值一律返回`false`, `Number.isNaN()`只有对于`NaN`才返回`true`，非`NaN`一律返回`false`。

```javascript
isFinite(25) // true
isFinite("25") // true
Number.isFinite(25) // true
Number.isFinite("25") // false

isNaN(NaN) // true
isNaN("NaN") // true
Number.isNaN(NaN) // true
Number.isNaN("NaN") // false
Number.isNaN(1) // false
```

## Number.parseInt(), Number.parseFloat()

ES6 将全局方法`parseInt()`和`parseFloat()`，移植到`Number`对象上面，行为完全保持不变。

```javascript
// ES5的写法
parseInt('12.34') // 12
parseFloat('123.45#') // 123.45

// ES6的写法
Number.parseInt('12.34') // 12
Number.parseFloat('123.45#') // 123.45
```

这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

```javascript
Number.parseInt === parseInt // true
Number.parseFloat === parseFloat // true
```

## Number.isInteger()

`Number.isInteger()`用来判断一个数值是否为整数。

```javascript
Number.isInteger(25) // true
Number.isInteger(25.1) // false
```

JavaScript 内部，整数和浮点数采用的是同样的储存方法，所以 25  和 25.0 被视为同一个值。

```javascript
Number.isInteger(25) // true
Number.isInteger(25.0) // true
```

如果参数不是数值，`Number.isInteger`返回`false`。

```javascript
Number.isInteger() // false
Number.isInteger(null) // false
Number.isInteger('15') // false
Number.isInteger(true) // false
```

注意，由于 JavaScript 采用 IEEE 754 标准，数值存储为64位双精度格式，数值精度最多可以达到 53 个二进制位（1 个隐藏位与 52 个有效位）。如果数值的精度超过这个限度，第54位及后面的位就会被丢弃，这种情况下，`Number.isInteger`可能会误判。

```javascript
Number.isInteger(3.0000000000000002) // true
```

上面代码中，`Number.isInteger`的参数明明不是整数，但是会返回`true`。原因就是这个小数的精度达到了小数点后16个十进制位，转成二进制位超过了53个二进制位，导致最后的那个`2`被丢弃了。

类似的情况还有，如果一个数值的绝对值小于`Number.MIN_VALUE`（5E-324），即小于 JavaScript 能够分辨的最小值，会被自动转为 0。这时，`Number.isInteger`也会误判。

```javascript
Number.isInteger(5E-324) // false
Number.isInteger(5E-325) // true
```

上面代码中，`5E-325`由于值太小，会被自动转为0，因此返回`true`。

总之，如果对数据精度的要求较高，不建议使用`Number.isInteger()`判断一个数值是否为整数。

## Number.EPSILON

ES6 在`Number`对象上面，新增一个极小的常量`Number.EPSILON`。根据规格，它表示 1 与大于 1 的最小浮点数之间的差。

对于 64 位浮点数来说，大于 1 的最小浮点数相当于二进制的`1.00..001`，小数点后面有连续 51 个零。这个值减去 1 之后，就等于 2 的 -52 次方。

```javascript
Number.EPSILON === Math.pow(2, -52)
// true
Number.EPSILON
// 2.220446049250313e-16
Number.EPSILON.toFixed(20)
// "0.00000000000000022204"
```

`Number.EPSILON`实际上是 JavaScript 能够表示的最小精度。误差如果小于这个值，就可以认为已经没有意义了，即不存在误差了。

引入一个这么小的量的目的，在于为浮点数计算，设置一个误差范围。我们知道浮点数计算是不精确的。

```javascript
0.1 + 0.2
// 0.30000000000000004

0.1 + 0.2 - 0.3
// 5.551115123125783e-17

5.551115123125783e-17.toFixed(20)
// '0.00000000000000005551'
```

上面代码解释了，为什么比较`0.1 + 0.2`与`0.3`得到的结果是`false`。

```javascript
0.1 + 0.2 === 0.3 // false
```

`Number.EPSILON`可以用来设置“能够接受的误差范围”。比如，误差范围设为 2 的-50 次方（即`Number.EPSILON * Math.pow(2, 2)`），即如果两个浮点数的差小于这个值，我们就认为这两个浮点数相等。

```javascript
5.551115123125783e-17 < Number.EPSILON * Math.pow(2, 2)
// true
```

因此，`Number.EPSILON`的实质是一个可以接受的最小误差范围。

```javascript
function withinErrorMargin (left, right) {
  return Math.abs(left - right) < Number.EPSILON * Math.pow(2, 2);
}

0.1 + 0.2 === 0.3 // false
withinErrorMargin(0.1 + 0.2, 0.3) // true

1.1 + 1.3 === 2.4 // false
withinErrorMargin(1.1 + 1.3, 2.4) // true
```

上面的代码为浮点数运算，部署了一个误差检查函数。

## 安全整数和 Number.isSafeInteger()

JavaScript 能够准确表示的整数范围在`-2^53`到`2^53`之间（不含两个端点），超过这个范围，无法精确表示这个值。

```javascript
Math.pow(2, 53) // 9007199254740992

9007199254740992  // 9007199254740992
9007199254740993  // 9007199254740992

Math.pow(2, 53) === Math.pow(2, 53) + 1
// true
```

上面代码中，超出 2 的 53 次方之后，一个数就不精确了。

ES6 引入了`Number.MAX_SAFE_INTEGER`和`Number.MIN_SAFE_INTEGER`这两个常量，用来表示这个范围的上下限。

```javascript
Number.MAX_SAFE_INTEGER === Math.pow(2, 53) - 1
// true
Number.MAX_SAFE_INTEGER === 9007199254740991
// true

Number.MIN_SAFE_INTEGER === -Number.MAX_SAFE_INTEGER
// true
Number.MIN_SAFE_INTEGER === -9007199254740991
// true
```

上面代码中，可以看到 JavaScript 能够精确表示的极限。

`Number.isSafeInteger()`则是用来判断一个整数是否落在这个范围之内。

```javascript
Number.isSafeInteger('a') // false
Number.isSafeInteger(null) // false
Number.isSafeInteger(NaN) // false
Number.isSafeInteger(Infinity) // false
Number.isSafeInteger(-Infinity) // false

Number.isSafeInteger(3) // true
Number.isSafeInteger(1.2) // false
Number.isSafeInteger(9007199254740990) // true
Number.isSafeInteger(9007199254740992) // false

Number.isSafeInteger(Number.MIN_SAFE_INTEGER - 1) // false
Number.isSafeInteger(Number.MIN_SAFE_INTEGER) // true
Number.isSafeInteger(Number.MAX_SAFE_INTEGER) // true
Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1) // false
```

这个函数的实现很简单，就是跟安全整数的两个边界值比较一下。

```javascript
Number.isSafeInteger = function (n) {
  return (typeof n === 'number' &&
    Math.round(n) === n &&
    Number.MIN_SAFE_INTEGER <= n &&
    n <= Number.MAX_SAFE_INTEGER);
}
```

实际使用这个函数时，需要注意。验证运算结果是否落在安全整数的范围内，不要只验证运算结果，而要同时验证参与运算的每个值。

```javascript
Number.isSafeInteger(9007199254740993)
// false
Number.isSafeInteger(990)
// true
Number.isSafeInteger(9007199254740993 - 990)
// true
9007199254740993 - 990
// 返回结果 9007199254740002
// 正确答案应该是 9007199254740003
```

上面代码中，`9007199254740993`不是一个安全整数，但是`Number.isSafeInteger`会返回结果，显示计算结果是安全的。这是因为，这个数超出了精度范围，导致在计算机内部，以`9007199254740992`的形式储存。

```javascript
9007199254740993 === 9007199254740992
// true
```

所以，如果只验证运算结果是否为安全整数，很可能得到错误结果。下面的函数可以同时验证两个运算数和运算结果。

```javascript
function trusty (left, right, result) {
  if (
    Number.isSafeInteger(left) &&
    Number.isSafeInteger(right) &&
    Number.isSafeInteger(result)
  ) {
    return result;
  }
  throw new RangeError('Operation cannot be trusted!');
}

trusty(9007199254740993, 990, 9007199254740993 - 990)
// RangeError: Operation cannot be trusted!

trusty(1, 2, 3)
// 3
```

## Math 对象的扩展

ES6 在 Math 对象上新增了 17 个与数学相关的方法。所有这些方法都是静态方法，只能在 Math 对象上调用。

### Math.trunc()

`Math.trunc`方法用于去除一个数的小数部分，返回整数部分。

```javascript
Math.trunc(4.1) // 4
Math.trunc(4.9) // 4
Math.trunc(-4.1) // -4
Math.trunc(-4.9) // -4
Math.trunc(-0.1234) // -0
```

对于非数值，`Math.trunc`内部使用`Number`方法将其先转为数值。

```javascript
Math.trunc('123.456') // 123
Math.trunc(true) //1
Math.trunc(false) // 0
Math.trunc(null) // 0
```

对于空值和无法截取整数的值，返回`NaN`。

```javascript
Math.trunc(NaN);      // NaN
Math.trunc('foo');    // NaN
Math.trunc();         // NaN
Math.trunc(undefined) // NaN
```

对于没有部署这个方法的环境，可以用下面的代码模拟。

```javascript
Math.trunc = Math.trunc || function(x) {
  return x < 0 ? Math.ceil(x) : Math.floor(x);
};
```

### Math.sign()

`Math.sign`方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

它会返回五种值。

- 参数为正数，返回`+1`；
- 参数为负数，返回`-1`；
- 参数为 0，返回`0`；
- 参数为-0，返回`-0`;
- 其他值，返回`NaN`。

```javascript
Math.sign(-5) // -1
Math.sign(5) // +1
Math.sign(0) // +0
Math.sign(-0) // -0
Math.sign(NaN) // NaN
```

如果参数是非数值，会自动转为数值。对于那些无法转为数值的值，会返回`NaN`。

```javascript
Math.sign('')  // 0
Math.sign(true)  // +1
Math.sign(false)  // 0
Math.sign(null)  // 0
Math.sign('9')  // +1
Math.sign('foo')  // NaN
Math.sign()  // NaN
Math.sign(undefined)  // NaN
```

对于没有部署这个方法的环境，可以用下面的代码模拟。

```javascript
Math.sign = Math.sign || function(x) {
  x = +x; // convert to a number
  if (x === 0 || isNaN(x)) {
    return x;
  }
  return x > 0 ? 1 : -1;
};
```

### Math.cbrt()

`Math.cbrt()`方法用于计算一个数的立方根。

```javascript
Math.cbrt(-1) // -1
Math.cbrt(0)  // 0
Math.cbrt(1)  // 1
Math.cbrt(2)  // 1.2599210498948732
```

对于非数值，`Math.cbrt()`方法内部也是先使用`Number()`方法将其转为数值。

```javascript
Math.cbrt('8') // 2
Math.cbrt('hello') // NaN
```

对于没有部署这个方法的环境，可以用下面的代码模拟。

```javascript
Math.cbrt = Math.cbrt || function(x) {
  var y = Math.pow(Math.abs(x), 1/3);
  return x < 0 ? -y : y;
};
```

### Math.clz32()

`Math.clz32()`方法将参数转为 32 位无符号整数的形式，然后返回这个 32 位值里面有多少个前导 0。

```javascript
Math.clz32(0) // 32
Math.clz32(1) // 31
Math.clz32(1000) // 22
Math.clz32(0b01000000000000000000000000000000) // 1
Math.clz32(0b00100000000000000000000000000000) // 2
```

上面代码中，0 的二进制形式全为 0，所以有 32 个前导 0；1 的二进制形式是`0b1`，只占 1 位，所以 32 位之中有 31 个前导 0；1000 的二进制形式是`0b1111101000`，一共有 10 位，所以 32 位之中有 22 个前导 0。

`clz32`这个函数名就来自”count leading zero bits in 32-bit binary representation of a number“（计算一个数的 32 位二进制形式的前导 0 的个数）的缩写。

左移运算符（`<<`）与`Math.clz32`方法直接相关。

```javascript
Math.clz32(0) // 32
Math.clz32(1) // 31
Math.clz32(1 << 1) // 30
Math.clz32(1 << 2) // 29
Math.clz32(1 << 29) // 2
```

对于小数，`Math.clz32`方法只考虑整数部分。

```javascript
Math.clz32(3.2) // 30
Math.clz32(3.9) // 30
```

对于空值或其他类型的值，`Math.clz32`方法会将它们先转为数值，然后再计算。

```javascript
Math.clz32() // 32
Math.clz32(NaN) // 32
Math.clz32(Infinity) // 32
Math.clz32(null) // 32
Math.clz32('foo') // 32
Math.clz32([]) // 32
Math.clz32({}) // 32
Math.clz32(true) // 31
```

### Math.imul()

`Math.imul`方法返回两个数以32位带符号整数形式相乘的结果，返回的也是一个32位的带符号整数。

```javascript
Math.imul(2, 4)   // 8
Math.imul(-1, 8)  // -8
Math.imul(-2, -2) // 4
```

如果只考虑最后 32 位，大多数情况下，`Math.imul(a, b)`与`a * b`的结果是相同的，即该方法等同于`(a * b)|0`的效果（超过 32 位的部分溢出）。之所以需要部署这个方法，是因为 JavaScript 有精度限制，超过 2 的 53 次方的值无法精确表示。这就是说，对于那些很大的数的乘法，低位数值往往都是不精确的，`Math.imul`方法可以返回正确的低位数值。

```javascript
(0x7fffffff * 0x7fffffff)|0 // 0
```

上面这个乘法算式，返回结果为 0。但是由于这两个二进制数的最低位都是 1，所以这个结果肯定是不正确的，因为根据二进制乘法，计算结果的二进制最低位应该也是 1。这个错误就是因为它们的乘积超过了 2 的 53 次方，JavaScript 无法保存额外的精度，就把低位的值都变成了 0。`Math.imul`方法可以返回正确的值 1。

```javascript
Math.imul(0x7fffffff, 0x7fffffff) // 1
```

### Math.fround()

`Math.fround`方法返回一个数的32位单精度浮点数形式。

对于32位单精度格式来说，数值精度是24个二进制位（1 位隐藏位与 23 位有效位），所以对于 -2<sup>24</sup> 至 2<sup>24</sup> 之间的整数（不含两个端点），返回结果与参数本身一致。

```javascript
Math.fround(0)   // 0
Math.fround(1)   // 1
Math.fround(2 ** 24 - 1)   // 16777215
```

如果参数的绝对值大于 2<sup>24</sup>，返回的结果便开始丢失精度。

```javascript
Math.fround(2 ** 24)       // 16777216
Math.fround(2 ** 24 + 1)   // 16777216
```

`Math.fround`方法的主要作用，是将64位双精度浮点数转为32位单精度浮点数。如果小数的精度超过24个二进制位，返回值就会不同于原值，否则返回值不变（即与64位双精度值一致）。

```javascript
// 未丢失有效精度
Math.fround(1.125) // 1.125
Math.fround(7.25)  // 7.25

// 丢失精度
Math.fround(0.3)   // 0.30000001192092896
Math.fround(0.7)   // 0.699999988079071
Math.fround(1.0000000123) // 1
```

对于 `NaN` 和 `Infinity`，此方法返回原值。对于其它类型的非数值，`Math.fround` 方法会先将其转为数值，再返回单精度浮点数。

```javascript
Math.fround(NaN)      // NaN
Math.fround(Infinity) // Infinity

Math.fround('5')      // 5
Math.fround(true)     // 1
Math.fround(null)     // 0
Math.fround([])       // 0
Math.fround({})       // NaN
```

对于没有部署这个方法的环境，可以用下面的代码模拟。

```javascript
Math.fround = Math.fround || function (x) {
  return new Float32Array([x])[0];
};
```

### Math.hypot()

`Math.hypot`方法返回所有参数的平方和的平方根。

```javascript
Math.hypot(3, 4);        // 5
Math.hypot(3, 4, 5);     // 7.0710678118654755
Math.hypot();            // 0
Math.hypot(NaN);         // NaN
Math.hypot(3, 4, 'foo'); // NaN
Math.hypot(3, 4, '5');   // 7.0710678118654755
Math.hypot(-3);          // 3
```

上面代码中，3 的平方加上 4 的平方，等于 5 的平方。

如果参数不是数值，`Math.hypot`方法会将其转为数值。只要有一个参数无法转为数值，就会返回 NaN。

### 对数方法

ES6 新增了 4 个对数相关方法。

**（1） Math.expm1()**

`Math.expm1(x)`返回 e<sup>x</sup> - 1，即`Math.exp(x) - 1`。

```javascript
Math.expm1(-1) // -0.6321205588285577
Math.expm1(0)  // 0
Math.expm1(1)  // 1.718281828459045
```

对于没有部署这个方法的环境，可以用下面的代码模拟。

```javascript
Math.expm1 = Math.expm1 || function(x) {
  return Math.exp(x) - 1;
};
```

**（2）Math.log1p()**

`Math.log1p(x)`方法返回`1 + x`的自然对数，即`Math.log(1 + x)`。如果`x`小于-1，返回`NaN`。

```javascript
Math.log1p(1)  // 0.6931471805599453
Math.log1p(0)  // 0
Math.log1p(-1) // -Infinity
Math.log1p(-2) // NaN
```

对于没有部署这个方法的环境，可以用下面的代码模拟。

```javascript
Math.log1p = Math.log1p || function(x) {
  return Math.log(1 + x);
};
```

**（3）Math.log10()**

`Math.log10(x)`返回以 10 为底的`x`的对数。如果`x`小于 0，则返回 NaN。

```javascript
Math.log10(2)      // 0.3010299956639812
Math.log10(1)      // 0
Math.log10(0)      // -Infinity
Math.log10(-2)     // NaN
Math.log10(100000) // 5
```

对于没有部署这个方法的环境，可以用下面的代码模拟。

```javascript
Math.log10 = Math.log10 || function(x) {
  return Math.log(x) / Math.LN10;
};
```

**（4）Math.log2()**

`Math.log2(x)`返回以 2 为底的`x`的对数。如果`x`小于 0，则返回 NaN。

```javascript
Math.log2(3)       // 1.584962500721156
Math.log2(2)       // 1
Math.log2(1)       // 0
Math.log2(0)       // -Infinity
Math.log2(-2)      // NaN
Math.log2(1024)    // 10
Math.log2(1 << 29) // 29
```

对于没有部署这个方法的环境，可以用下面的代码模拟。

```javascript
Math.log2 = Math.log2 || function(x) {
  return Math.log(x) / Math.LN2;
};
```

### 双曲函数方法

ES6 新增了 6 个双曲函数方法。

- `Math.sinh(x)` 返回`x`的双曲正弦（hyperbolic sine）
- `Math.cosh(x)` 返回`x`的双曲余弦（hyperbolic cosine）
- `Math.tanh(x)` 返回`x`的双曲正切（hyperbolic tangent）
- `Math.asinh(x)` 返回`x`的反双曲正弦（inverse hyperbolic sine）
- `Math.acosh(x)` 返回`x`的反双曲余弦（inverse hyperbolic cosine）
- `Math.atanh(x)` 返回`x`的反双曲正切（inverse hyperbolic tangent）

## BigInt 数据类型

### 简介

JavaScript 所有数字都保存成 64 位浮点数，这给数值的表示带来了两大限制。一是数值的精度只能到 53 个二进制位（相当于 16 个十进制位），大于这个范围的整数，JavaScript 是无法精确表示，这使得 JavaScript 不适合进行科学和金融方面的精确计算。二是大于或等于2的1024次方的数值，JavaScript 无法表示，会返回`Infinity`。

```javascript
// 超过 53 个二进制位的数值，无法保持精度
Math.pow(2, 53) === Math.pow(2, 53) + 1 // true

// 超过 2 的 1024 次方的数值，无法表示
Math.pow(2, 1024) // Infinity
```

[ES2020](https://github.com/tc39/proposal-bigint) 引入了一种新的数据类型 BigInt（大整数），来解决这个问题，这是 ECMAScript 的第八种数据类型。BigInt 只用来表示整数，没有位数的限制，任何位数的整数都可以精确表示。

```javascript
const a = 2172141653n;
const b = 15346349309n;

// BigInt 可以保持精度
a * b // 33334444555566667777n

// 普通整数无法保持精度
Number(a) * Number(b) // 33334444555566670000
```

为了与 Number 类型区别，BigInt 类型的数据必须添加后缀`n`。

```javascript
1234 // 普通整数
1234n // BigInt

// BigInt 的运算
1n + 2n // 3n
```

BigInt 同样可以使用各种进制表示，都要加上后缀`n`。

```javascript
0b1101n // 二进制
0o777n // 八进制
0xFFn // 十六进制
```

BigInt 与普通整数是两种值，它们之间并不相等。

```javascript
42n === 42 // false
```

`typeof`运算符对于 BigInt 类型的数据返回`bigint`。

```javascript
typeof 123n // 'bigint'
```

BigInt 可以使用负号（`-`），但是不能使用正号（`+`），因为会与 asm.js 冲突。

```javascript
-42n // 正确
+42n // 报错
```

JavaScript 以前不能计算70的阶乘（即`70!`），因为超出了可以表示的精度。

```javascript
let p = 1;
for (let i = 1; i <= 70; i++) {
  p *= i;
}
console.log(p); // 1.197857166996989e+100
```

现在支持大整数了，就可以算了，浏览器的开发者工具运行下面代码，就OK。

```javascript
let p = 1n;
for (let i = 1n; i <= 70n; i++) {
  p *= i;
}
console.log(p); // 11978571...00000000n
```

### BigInt 函数

JavaScript 原生提供`BigInt`函数，可以用它生成 BigInt 类型的数值。转换规则基本与`Number()`一致，将其他类型的值转为 BigInt。

```javascript
BigInt(123) // 123n
BigInt('123') // 123n
BigInt(false) // 0n
BigInt(true) // 1n
```

`BigInt()`函数必须有参数，而且参数必须可以正常转为数值，下面的用法都会报错。

```javascript
new BigInt() // TypeError
BigInt(undefined) //TypeError
BigInt(null) // TypeError
BigInt('123n') // SyntaxError
BigInt('abc') // SyntaxError
```

上面代码中，尤其值得注意字符串`123n`无法解析成 Number 类型，所以会报错。

参数如果是小数，也会报错。

```javascript
BigInt(1.5) // RangeError
BigInt('1.5') // SyntaxError
```

BigInt 继承了 Object 对象的两个实例方法。

- `BigInt.prototype.toString()`
- `BigInt.prototype.valueOf()`

它还继承了 Number 对象的一个实例方法。

- `BigInt.prototype.toLocaleString()`

此外，还提供了三个静态方法。

- `BigInt.asUintN(width, BigInt)`： 给定的 BigInt 转为 0 到 2<sup>width</sup> - 1 之间对应的值。
- `BigInt.asIntN(width, BigInt)`：给定的 BigInt 转为 -2<sup>width - 1</sup> 到 2<sup>width - 1</sup> - 1 之间对应的值。
- `BigInt.parseInt(string[, radix])`：近似于`Number.parseInt()`，将一个字符串转换成指定进制的 BigInt。

```javascript
const max = 2n ** (64n - 1n) - 1n;

BigInt.asIntN(64, max)
// 9223372036854775807n
BigInt.asIntN(64, max + 1n)
// -9223372036854775808n
BigInt.asUintN(64, max + 1n)
// 9223372036854775808n
```

上面代码中，`max`是64位带符号的 BigInt 所能表示的最大值。如果对这个值加`1n`，`BigInt.asIntN()`将会返回一个负值，因为这时新增的一位将被解释为符号位。而`BigInt.asUintN()`方法由于不存在符号位，所以可以正确返回结果。

如果`BigInt.asIntN()`和`BigInt.asUintN()`指定的位数，小于数值本身的位数，那么头部的位将被舍弃。

```javascript
const max = 2n ** (64n - 1n) - 1n;

BigInt.asIntN(32, max) // -1n
BigInt.asUintN(32, max) // 4294967295n
```

上面代码中，`max`是一个64位的 BigInt，如果转为32位，前面的32位都会被舍弃。

下面是`BigInt.parseInt()`的例子。

```javascript
// Number.parseInt() 与 BigInt.parseInt() 的对比
Number.parseInt('9007199254740993', 10)
// 9007199254740992
BigInt.parseInt('9007199254740993', 10)
// 9007199254740993n
```

上面代码中，由于有效数字超出了最大限度，`Number.parseInt`方法返回的结果是不精确的，而`BigInt.parseInt`方法正确返回了对应的 BigInt。

对于二进制数组，BigInt 新增了两个类型`BigUint64Array`和`BigInt64Array`，这两种数据类型返回的都是64位 BigInt。`DataView`对象的实例方法`DataView.prototype.getBigInt64()`和`DataView.prototype.getBigUint64()`，返回的也是 BigInt。

### 转换规则

可以使用`Boolean()`、`Number()`和`String()`这三个方法，将 BigInt 可以转为布尔值、数值和字符串类型。

```javascript
Boolean(0n) // false
Boolean(1n) // true
Number(1n)  // 1
String(1n)  // "1"
```

上面代码中，注意最后一个例子，转为字符串时后缀`n`会消失。

另外，取反运算符（`!`）也可以将 BigInt 转为布尔值。

```javascript
!0n // true
!1n // false
```

### 数学运算

数学运算方面，BigInt 类型的`+`、`-`、`*`和`**`这四个二元运算符，与 Number 类型的行为一致。除法运算`/`会舍去小数部分，返回一个整数。

```javascript
9n / 5n
// 1n
```

几乎所有的数值运算符都可以用在 BigInt，但是有两个例外。

- 不带符号的右移位运算符`>>>`
- 一元的求正运算符`+`

上面两个运算符用在 BigInt 会报错。前者是因为`>>>`运算符是不带符号的，但是 BigInt 总是带有符号的，导致该运算无意义，完全等同于右移运算符`>>`。后者是因为一元运算符`+`在 asm.js 里面总是返回 Number 类型，为了不破坏 asm.js 就规定`+1n`会报错。

BigInt 不能与普通数值进行混合运算。

```javascript
1n + 1.3 // 报错
```

上面代码报错是因为无论返回的是 BigInt 或 Number，都会导致丢失精度信息。比如`(2n**53n + 1n) + 0.5`这个表达式，如果返回 BigInt 类型，`0.5`这个小数部分会丢失；如果返回 Number 类型，有效精度只能保持 53 位，导致精度下降。

同样的原因，如果一个标准库函数的参数预期是 Number 类型，但是得到的是一个 BigInt，就会报错。

```javascript
// 错误的写法
Math.sqrt(4n) // 报错

// 正确的写法
Math.sqrt(Number(4n)) // 2
```

上面代码中，`Math.sqrt`的参数预期是 Number 类型，如果是 BigInt 就会报错，必须先用`Number`方法转一下类型，才能进行计算。

asm.js 里面，`|0`跟在一个数值的后面会返回一个32位整数。根据不能与 Number 类型混合运算的规则，BigInt 如果与`|0`进行运算会报错。

```javascript
1n | 0 // 报错
```

### 其他运算

BigInt 对应的布尔值，与 Number 类型一致，即`0n`会转为`false`，其他值转为`true`。

```javascript
if (0n) {
  console.log('if');
} else {
  console.log('else');
}
// else
```

上面代码中，`0n`对应`false`，所以会进入`else`子句。

比较运算符（比如`>`）和相等运算符（`==`）允许 BigInt 与其他类型的值混合计算，因为这样做不会损失精度。

```javascript
0n < 1 // true
0n < true // true
0n == 0 // true
0n == false // true
0n === 0 // false
```

BigInt 与字符串混合运算时，会先转为字符串，再进行运算。

```javascript
'' + 123n // "123"
```
# 函数的扩展

## 函数参数的默认值

### 基本用法

ES6 之前，不能直接为函数的参数指定默认值，只能采用变通的方法。

```javascript
function log(x, y) {
  y = y || 'World';
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello World
```

上面代码检查函数`log`的参数`y`有没有赋值，如果没有，则指定默认值为`World`。这种写法的缺点在于，如果参数`y`赋值了，但是对应的布尔值为`false`，则该赋值不起作用。就像上面代码的最后一行，参数`y`等于空字符，结果被改为默认值。

为了避免这个问题，通常需要先判断一下参数`y`是否被赋值，如果没有，再等于默认值。

```javascript
if (typeof y === 'undefined') {
  y = 'World';
}
```

ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。

```javascript
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```

可以看到，ES6 的写法比 ES5 简洁许多，而且非常自然。下面是另一个例子。

```javascript
function Point(x = 0, y = 0) {
  this.x = x;
  this.y = y;
}

const p = new Point();
p // { x: 0, y: 0 }
```

除了简洁，ES6 的写法还有两个好处：首先，阅读代码的人，可以立刻意识到哪些参数是可以省略的，不用查看函数体或文档；其次，有利于将来的代码优化，即使未来的版本在对外接口中，彻底拿掉这个参数，也不会导致以前的代码无法运行。

参数变量是默认声明的，所以不能用`let`或`const`再次声明。

```javascript
function foo(x = 5) {
  let x = 1; // error
  const x = 2; // error
}
```

上面代码中，参数变量`x`是默认声明的，在函数体中，不能用`let`或`const`再次声明，否则会报错。

使用参数默认值时，函数不能有同名参数。

```javascript
// 不报错
function foo(x, x, y) {
  // ...
}

// 报错
function foo(x, x, y = 1) {
  // ...
}
// SyntaxError: Duplicate parameter name not allowed in this context
```

另外，一个容易忽略的地方是，参数默认值不是传值的，而是每次都重新计算默认值表达式的值。也就是说，参数默认值是惰性求值的。

```javascript
let x = 99;
function foo(p = x + 1) {
  console.log(p);
}

foo() // 100

x = 100;
foo() // 101
```

上面代码中，参数`p`的默认值是`x + 1`。这时，每次调用函数`foo`，都会重新计算`x + 1`，而不是默认`p`等于 100。

### 与解构赋值默认值结合使用

参数默认值可以与解构赋值的默认值，结合起来使用。

```javascript
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined 5
foo({x: 1}) // 1 5
foo({x: 1, y: 2}) // 1 2
foo() // TypeError: Cannot read property 'x' of undefined
```

上面代码只使用了对象的解构赋值默认值，没有使用函数参数的默认值。只有当函数`foo`的参数是一个对象时，变量`x`和`y`才会通过解构赋值生成。如果函数`foo`调用时没提供参数，变量`x`和`y`就不会生成，从而报错。通过提供函数参数的默认值，就可以避免这种情况。

```javascript
function foo({x, y = 5} = {}) {
  console.log(x, y);
}

foo() // undefined 5
```

上面代码指定，如果没有提供参数，函数`foo`的参数默认为一个空对象。

下面是另一个解构赋值默认值的例子。

```javascript
function fetch(url, { body = '', method = 'GET', headers = {} }) {
  console.log(method);
}

fetch('http://example.com', {})
// "GET"

fetch('http://example.com')
// 报错
```

上面代码中，如果函数`fetch`的第二个参数是一个对象，就可以为它的三个属性设置默认值。这种写法不能省略第二个参数，如果结合函数参数的默认值，就可以省略第二个参数。这时，就出现了双重默认值。

```javascript
function fetch(url, { body = '', method = 'GET', headers = {} } = {}) {
  console.log(method);
}

fetch('http://example.com')
// "GET"
```

上面代码中，函数`fetch`没有第二个参数时，函数参数的默认值就会生效，然后才是解构赋值的默认值生效，变量`method`才会取到默认值`GET`。

作为练习，请问下面两种写法有什么差别？

```javascript
// 写法一
function m1({x = 0, y = 0} = {}) {
  return [x, y];
}

// 写法二
function m2({x, y} = { x: 0, y: 0 }) {
  return [x, y];
}
```

上面两种写法都对函数的参数设定了默认值，区别是写法一函数参数的默认值是空对象，但是设置了对象解构赋值的默认值；写法二函数参数的默认值是一个有具体属性的对象，但是没有设置对象解构赋值的默认值。

```javascript
// 函数没有参数的情况
m1() // [0, 0]
m2() // [0, 0]

// x 和 y 都有值的情况
m1({x: 3, y: 8}) // [3, 8]
m2({x: 3, y: 8}) // [3, 8]

// x 有值，y 无值的情况
m1({x: 3}) // [3, 0]
m2({x: 3}) // [3, undefined]

// x 和 y 都无值的情况
m1({}) // [0, 0];
m2({}) // [undefined, undefined]

m1({z: 3}) // [0, 0]
m2({z: 3}) // [undefined, undefined]
```

### 参数默认值的位置

通常情况下，定义了默认值的参数，应该是函数的尾参数。因为这样比较容易看出来，到底省略了哪些参数。如果非尾部的参数设置默认值，实际上这个参数是没法省略的。

```javascript
// 例一
function f(x = 1, y) {
  return [x, y];
}

f() // [1, undefined]
f(2) // [2, undefined]
f(, 1) // 报错
f(undefined, 1) // [1, 1]

// 例二
function f(x, y = 5, z) {
  return [x, y, z];
}

f() // [undefined, 5, undefined]
f(1) // [1, 5, undefined]
f(1, ,2) // 报错
f(1, undefined, 2) // [1, 5, 2]
```

上面代码中，有默认值的参数都不是尾参数。这时，无法只省略该参数，而不省略它后面的参数，除非显式输入`undefined`。

如果传入`undefined`，将触发该参数等于默认值，`null`则没有这个效果。

```javascript
function foo(x = 5, y = 6) {
  console.log(x, y);
}

foo(undefined, null)
// 5 null
```

上面代码中，`x`参数对应`undefined`，结果触发了默认值，`y`参数等于`null`，就没有触发默认值。

### 函数的 length 属性

指定了默认值以后，函数的`length`属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值后，`length`属性将失真。

```javascript
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```

上面代码中，`length`属性的返回值，等于函数的参数个数减去指定了默认值的参数个数。比如，上面最后一个函数，定义了 3 个参数，其中有一个参数`c`指定了默认值，因此`length`属性等于`3`减去`1`，最后得到`2`。

这是因为`length`属性的含义是，该函数预期传入的参数个数。某个参数指定默认值以后，预期传入的参数个数就不包括这个参数了。同理，后文的 rest 参数也不会计入`length`属性。

```javascript
(function(...args) {}).length // 0
```

如果设置了默认值的参数不是尾参数，那么`length`属性也不再计入后面的参数了。

```javascript
(function (a = 0, b, c) {}).length // 0
(function (a, b = 1, c) {}).length // 1
```

### 作用域

一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域（context）。等到初始化结束，这个作用域就会消失。这种语法行为，在不设置参数默认值时，是不会出现的。

```javascript
var x = 1;

function f(x, y = x) {
  console.log(y);
}

f(2) // 2
```

上面代码中，参数`y`的默认值等于变量`x`。调用函数`f`时，参数形成一个单独的作用域。在这个作用域里面，默认值变量`x`指向第一个参数`x`，而不是全局变量`x`，所以输出是`2`。

再看下面的例子。

```javascript
let x = 1;

function f(y = x) {
  let x = 2;
  console.log(y);
}

f() // 1
```

上面代码中，函数`f`调用时，参数`y = x`形成一个单独的作用域。这个作用域里面，变量`x`本身没有定义，所以指向外层的全局变量`x`。函数调用时，函数体内部的局部变量`x`影响不到默认值变量`x`。

如果此时，全局变量`x`不存在，就会报错。

```javascript
function f(y = x) {
  let x = 2;
  console.log(y);
}

f() // ReferenceError: x is not defined
```

下面这样写，也会报错。

```javascript
var x = 1;

function foo(x = x) {
  // ...
}

foo() // ReferenceError: Cannot access 'x' before initialization
```

上面代码中，参数`x = x`形成一个单独作用域。实际执行的是`let x = x`，由于暂时性死区的原因，这行代码会报错。

如果参数的默认值是一个函数，该函数的作用域也遵守这个规则。请看下面的例子。

```javascript
let foo = 'outer';

function bar(func = () => foo) {
  let foo = 'inner';
  console.log(func());
}

bar(); // outer
```

上面代码中，函数`bar`的参数`func`的默认值是一个匿名函数，返回值为变量`foo`。函数参数形成的单独作用域里面，并没有定义变量`foo`，所以`foo`指向外层的全局变量`foo`，因此输出`outer`。

如果写成下面这样，就会报错。

```javascript
function bar(func = () => foo) {
  let foo = 'inner';
  console.log(func());
}

bar() // ReferenceError: foo is not defined
```

上面代码中，匿名函数里面的`foo`指向函数外层，但是函数外层并没有声明变量`foo`，所以就报错了。

下面是一个更复杂的例子。

```javascript
var x = 1;
function foo(x, y = function() { x = 2; }) {
  var x = 3;
  y();
  console.log(x);
}

foo() // 3
x // 1
```

上面代码中，函数`foo`的参数形成一个单独作用域。这个作用域里面，首先声明了变量`x`，然后声明了变量`y`，`y`的默认值是一个匿名函数。这个匿名函数内部的变量`x`，指向同一个作用域的第一个参数`x`。函数`foo`内部又声明了一个内部变量`x`，该变量与第一个参数`x`由于不是同一个作用域，所以不是同一个变量，因此执行`y`后，内部变量`x`和外部全局变量`x`的值都没变。

如果将`var x = 3`的`var`去除，函数`foo`的内部变量`x`就指向第一个参数`x`，与匿名函数内部的`x`是一致的，所以最后输出的就是`2`，而外层的全局变量`x`依然不受影响。

```javascript
var x = 1;
function foo(x, y = function() { x = 2; }) {
  x = 3;
  y();
  console.log(x);
}

foo() // 2
x // 1
```

### 应用

利用参数默认值，可以指定某一个参数不得省略，如果省略就抛出一个错误。

```javascript
function throwIfMissing() {
  throw new Error('Missing parameter');
}

function foo(mustBeProvided = throwIfMissing()) {
  return mustBeProvided;
}

foo()
// Error: Missing parameter
```

上面代码的`foo`函数，如果调用的时候没有参数，就会调用默认值`throwIfMissing`函数，从而抛出一个错误。

从上面代码还可以看到，参数`mustBeProvided`的默认值等于`throwIfMissing`函数的运行结果（注意函数名`throwIfMissing`之后有一对圆括号），这表明参数的默认值不是在定义时执行，而是在运行时执行。如果参数已经赋值，默认值中的函数就不会运行。

另外，可以将参数默认值设为`undefined`，表明这个参数是可以省略的。

```javascript
function foo(optional = undefined) { ··· }
```

## rest 参数

ES6 引入 rest 参数（形式为`...变量名`），用于获取函数的多余参数，这样就不需要使用`arguments`对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

```javascript
function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }

  return sum;
}

add(2, 5, 3) // 10
```

上面代码的`add`函数是一个求和函数，利用 rest 参数，可以向该函数传入任意数目的参数。

下面是一个 rest 参数代替`arguments`变量的例子。

```javascript
// arguments变量的写法
function sortNumbers() {
  return Array.from(arguments).sort();
}

// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
```

上面代码的两种写法，比较后可以发现，rest 参数的写法更自然也更简洁。

`arguments`对象不是数组，而是一个类似数组的对象。所以为了使用数组的方法，必须使用`Array.from`先将其转为数组。rest 参数就不存在这个问题，它就是一个真正的数组，数组特有的方法都可以使用。下面是一个利用 rest 参数改写数组`push`方法的例子。

```javascript
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
    console.log(item);
  });
}

var a = [];
push(a, 1, 2, 3)
```

注意，rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。

```javascript
// 报错
function f(a, ...b, c) {
  // ...
}
```

函数的`length`属性，不包括 rest 参数。

```javascript
(function(a) {}).length  // 1
(function(...a) {}).length  // 0
(function(a, ...b) {}).length  // 1
```

## 严格模式

从 ES5 开始，函数内部可以设定为严格模式。

```javascript
function doSomething(a, b) {
  'use strict';
  // code
}
```

ES2016 做了一点修改，规定只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。

```javascript
// 报错
function doSomething(a, b = a) {
  'use strict';
  // code
}

// 报错
const doSomething = function ({a, b}) {
  'use strict';
  // code
};

// 报错
const doSomething = (...a) => {
  'use strict';
  // code
};

const obj = {
  // 报错
  doSomething({a, b}) {
    'use strict';
    // code
  }
};
```

这样规定的原因是，函数内部的严格模式，同时适用于函数体和函数参数。但是，函数执行的时候，先执行函数参数，然后再执行函数体。这样就有一个不合理的地方，只有从函数体之中，才能知道参数是否应该以严格模式执行，但是参数却应该先于函数体执行。

```javascript
// 报错
function doSomething(value = 070) {
  'use strict';
  return value;
}
```

上面代码中，参数`value`的默认值是八进制数`070`，但是严格模式下不能用前缀`0`表示八进制，所以应该报错。但是实际上，JavaScript 引擎会先成功执行`value = 070`，然后进入函数体内部，发现需要用严格模式执行，这时才会报错。

虽然可以先解析函数体代码，再执行参数代码，但是这样无疑就增加了复杂性。因此，标准索性禁止了这种用法，只要参数使用了默认值、解构赋值、或者扩展运算符，就不能显式指定严格模式。

两种方法可以规避这种限制。第一种是设定全局性的严格模式，这是合法的。

```javascript
'use strict';

function doSomething(a, b = a) {
  // code
}
```

第二种是把函数包在一个无参数的立即执行函数里面。

```javascript
const doSomething = (function () {
  'use strict';
  return function(value = 42) {
    return value;
  };
}());
```

## name 属性

函数的`name`属性，返回该函数的函数名。

```javascript
function foo() {}
foo.name // "foo"
```

这个属性早就被浏览器广泛支持，但是直到 ES6，才将其写入了标准。

需要注意的是，ES6 对这个属性的行为做出了一些修改。如果将一个匿名函数赋值给一个变量，ES5 的`name`属性，会返回空字符串，而 ES6 的`name`属性会返回实际的函数名。

```javascript
var f = function () {};

// ES5
f.name // ""

// ES6
f.name // "f"
```

上面代码中，变量`f`等于一个匿名函数，ES5 和 ES6 的`name`属性返回的值不一样。

如果将一个具名函数赋值给一个变量，则 ES5 和 ES6 的`name`属性都返回这个具名函数原本的名字。

```javascript
const bar = function baz() {};

// ES5
bar.name // "baz"

// ES6
bar.name // "baz"
```

`Function`构造函数返回的函数实例，`name`属性的值为`anonymous`。

```javascript
(new Function).name // "anonymous"
```

`bind`返回的函数，`name`属性值会加上`bound`前缀。

```javascript
function foo() {};
foo.bind({}).name // "bound foo"

(function(){}).bind({}).name // "bound "
```

## 箭头函数

### 基本用法

ES6 允许使用“箭头”（`=>`）定义函数。

```javascript
var f = v => v;

// 等同于
var f = function (v) {
  return v;
};
```

如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。

```javascript
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```

如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用`return`语句返回。

```javascript
var sum = (num1, num2) => { return num1 + num2; }
```

由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。

```javascript
// 报错
let getTempItem = id => { id: id, name: "Temp" };

// 不报错
let getTempItem = id => ({ id: id, name: "Temp" });
```

下面是一种特殊情况，虽然可以运行，但会得到错误的结果。

```javascript
let foo = () => { a: 1 };
foo() // undefined
```

上面代码中，原始意图是返回一个对象`{ a: 1 }`，但是由于引擎认为大括号是代码块，所以执行了一行语句`a: 1`。这时，`a`可以被解释为语句的标签，因此实际执行的语句是`1;`，然后函数就结束了，没有返回值。

如果箭头函数只有一行语句，且不需要返回值，可以采用下面的写法，就不用写大括号了。

```javascript
let fn = () => void doesNotReturn();
```

箭头函数可以与变量解构结合使用。

```javascript
const full = ({ first, last }) => first + ' ' + last;

// 等同于
function full(person) {
  return person.first + ' ' + person.last;
}
```

箭头函数使得表达更加简洁。

```javascript
const isEven = n => n % 2 === 0;
const square = n => n * n;
```

上面代码只用了两行，就定义了两个简单的工具函数。如果不用箭头函数，可能就要占用多行，而且还不如现在这样写醒目。

箭头函数的一个用处是简化回调函数。

```javascript
// 普通函数写法
[1,2,3].map(function (x) {
  return x * x;
});

// 箭头函数写法
[1,2,3].map(x => x * x);
```

另一个例子是

```javascript
// 普通函数写法
var result = values.sort(function (a, b) {
  return a - b;
});

// 箭头函数写法
var result = values.sort((a, b) => a - b);
```

下面是 rest 参数与箭头函数结合的例子。

```javascript
const numbers = (...nums) => nums;

numbers(1, 2, 3, 4, 5)
// [1,2,3,4,5]

const headAndTail = (head, ...tail) => [head, tail];

headAndTail(1, 2, 3, 4, 5)
// [1,[2,3,4,5]]
```

### 使用注意点

箭头函数有几个使用注意点。

（1）箭头函数没有自己的`this`对象（详见下文）。

（2）不可以当作构造函数，也就是说，不可以对箭头函数使用`new`命令，否则会抛出一个错误。

（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。

上面四点中，最重要的是第一点。对于普通函数来说，内部的`this`指向函数运行时所在的对象，但是这一点对箭头函数不成立。它没有自己的`this`对象，内部的`this`就是定义时上层作用域中的`this`。也就是说，箭头函数内部的`this`指向是固定的，相比之下，普通函数的`this`指向是可变的。

```javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });
// id: 42
```

上面代码中，`setTimeout()`的参数是一个箭头函数，这个箭头函数的定义生效是在`foo`函数生成时，而它的真正执行要等到 100 毫秒后。如果是普通函数，执行时`this`应该指向全局对象`window`，这时应该输出`21`。但是，箭头函数导致`this`总是指向函数定义生效时所在的对象（本例是`{id: 42}`），所以打印出来的是`42`。

下面例子是回调函数分别为箭头函数和普通函数，对比它们内部的`this`指向。

```javascript
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);
  // 普通函数
  setInterval(function () {
    this.s2++;
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100);
setTimeout(() => console.log('s2: ', timer.s2), 3100);
// s1: 3
// s2: 0
```

上面代码中，`Timer`函数内部设置了两个定时器，分别使用了箭头函数和普通函数。前者的`this`绑定定义时所在的作用域（即`Timer`函数），后者的`this`指向运行时所在的作用域（即全局对象）。所以，3100 毫秒之后，`timer.s1`被更新了 3 次，而`timer.s2`一次都没更新。

箭头函数实际上可以让`this`指向固定化，绑定`this`使得它不再可变，这种特性很有利于封装回调函数。下面是一个例子，DOM 事件的回调函数封装在一个对象里面。

```javascript
var handler = {
  id: '123456',

  init: function() {
    document.addEventListener('click',
      event => this.doSomething(event.type), false);
  },

  doSomething: function(type) {
    console.log('Handling ' + type  + ' for ' + this.id);
  }
};
```

上面代码的`init()`方法中，使用了箭头函数，这导致这个箭头函数里面的`this`，总是指向`handler`对象。如果回调函数是普通函数，那么运行`this.doSomething()`这一行会报错，因为此时`this`指向`document`对象。

总之，箭头函数根本没有自己的`this`，导致内部的`this`就是外层代码块的`this`。正是因为它没有`this`，所以也就不能用作构造函数。

下面是 Babel 转箭头函数产生的 ES5 代码，就能清楚地说明`this`的指向。

```javascript
// ES6
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

// ES5
function foo() {
  var _this = this;

  setTimeout(function () {
    console.log('id:', _this.id);
  }, 100);
}
```

上面代码中，转换后的 ES5 版本清楚地说明了，箭头函数里面根本没有自己的`this`，而是引用外层的`this`。

请问下面的代码之中，`this`的指向有几个？

```javascript
function foo() {
  return () => {
    return () => {
      return () => {
        console.log('id:', this.id);
      };
    };
  };
}

var f = foo.call({id: 1});

var t1 = f.call({id: 2})()(); // id: 1
var t2 = f().call({id: 3})(); // id: 1
var t3 = f()().call({id: 4}); // id: 1
```

答案是`this`的指向只有一个，就是函数`foo`的`this`，这是因为所有的内层函数都是箭头函数，都没有自己的`this`，它们的`this`其实都是最外层`foo`函数的`this`。所以不管怎么嵌套，`t1`、`t2`、`t3`都输出同样的结果。如果这个例子的所有内层函数都写成普通函数，那么每个函数的`this`都指向运行时所在的不同对象。

除了`this`，以下三个变量在箭头函数之中也是不存在的，指向外层函数的对应变量：`arguments`、`super`、`new.target`。

```javascript
function foo() {
  setTimeout(() => {
    console.log('args:', arguments);
  }, 100);
}

foo(2, 4, 6, 8)
// args: [2, 4, 6, 8]
```

上面代码中，箭头函数内部的变量`arguments`，其实是函数`foo`的`arguments`变量。

另外，由于箭头函数没有自己的`this`，所以当然也就不能用`call()`、`apply()`、`bind()`这些方法去改变`this`的指向。

```javascript
(function() {
  return [
    (() => this.x).bind({ x: 'inner' })()
  ];
}).call({ x: 'outer' });
// ['outer']
```

上面代码中，箭头函数没有自己的`this`，所以`bind`方法无效，内部的`this`指向外部的`this`。

长期以来，JavaScript 语言的`this`对象一直是一个令人头痛的问题，在对象方法中使用`this`，必须非常小心。箭头函数”绑定”`this`，很大程度上解决了这个困扰。

### 不适用场合

由于箭头函数使得`this`从“动态”变成“静态”，下面两个场合不应该使用箭头函数。

第一个场合是定义对象的方法，且该方法内部包括`this`。

```javascript
const cat = {
  lives: 9,
  jumps: () => {
    this.lives--;
  }
}
```

上面代码中，`cat.jumps()`方法是一个箭头函数，这是错误的。调用`cat.jumps()`时，如果是普通函数，该方法内部的`this`指向`cat`；如果写成上面那样的箭头函数，使得`this`指向全局对象，因此不会得到预期结果。这是因为对象不构成单独的作用域，导致`jumps`箭头函数定义时的作用域就是全局作用域。

再看一个例子。

```javascript
globalThis.s = 21;

const obj = {
  s: 42,
  m: () => console.log(this.s)
};

obj.m() // 21
```

上面例子中，`obj.m()`使用箭头函数定义。JavaScript 引擎的处理方法是，先在全局空间生成这个箭头函数，然后赋值给`obj.m`，这导致箭头函数内部的`this`指向全局对象，所以`obj.m()`输出的是全局空间的`21`，而不是对象内部的`42`。上面的代码实际上等同于下面的代码。

```javascript
globalThis.s = 21;
globalThis.m = () => console.log(this.s);

const obj = {
  s: 42,
  m: globalThis.m
};

obj.m() // 21
```

由于上面这个原因，对象的属性建议使用传统的写法定义，不要用箭头函数定义。

第二个场合是需要动态`this`的时候，也不应使用箭头函数。

```javascript
var button = document.getElementById('press');
button.addEventListener('click', () => {
  this.classList.toggle('on');
});
```

上面代码运行时，点击按钮会报错，因为`button`的监听函数是一个箭头函数，导致里面的`this`就是全局对象。如果改成普通函数，`this`就会动态指向被点击的按钮对象。

另外，如果函数体很复杂，有许多行，或者函数内部有大量的读写操作，不单纯是为了计算值，这时也不应该使用箭头函数，而是要使用普通函数，这样可以提高代码可读性。

### 嵌套的箭头函数

箭头函数内部，还可以再使用箭头函数。下面是一个 ES5 语法的多重嵌套函数。

```javascript
function insert(value) {
  return {into: function (array) {
    return {after: function (afterValue) {
      array.splice(array.indexOf(afterValue) + 1, 0, value);
      return array;
    }};
  }};
}

insert(2).into([1, 3]).after(1); //[1, 2, 3]
```

上面这个函数，可以使用箭头函数改写。

```javascript
let insert = (value) => ({into: (array) => ({after: (afterValue) => {
  array.splice(array.indexOf(afterValue) + 1, 0, value);
  return array;
}})});

insert(2).into([1, 3]).after(1); //[1, 2, 3]
```

下面是一个部署管道机制（pipeline）的例子，即前一个函数的输出是后一个函数的输入。

```javascript
const pipeline = (...funcs) =>
  val => funcs.reduce((a, b) => b(a), val);

const plus1 = a => a + 1;
const mult2 = a => a * 2;
const addThenMult = pipeline(plus1, mult2);

addThenMult(5)
// 12
```

如果觉得上面的写法可读性比较差，也可以采用下面的写法。

```javascript
const plus1 = a => a + 1;
const mult2 = a => a * 2;

mult2(plus1(5))
// 12
```

箭头函数还有一个功能，就是可以很方便地改写 λ 演算。

```javascript
// λ演算的写法
fix = λf.(λx.f(λv.x(x)(v)))(λx.f(λv.x(x)(v)))

// ES6的写法
var fix = f => (x => f(v => x(x)(v)))
               (x => f(v => x(x)(v)));
```

上面两种写法，几乎是一一对应的。由于 λ 演算对于计算机科学非常重要，这使得我们可以用 ES6 作为替代工具，探索计算机科学。

## 尾调用优化

### 什么是尾调用？

尾调用（Tail Call）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某个函数的最后一步是调用另一个函数。

```javascript
function f(x){
  return g(x);
}
```

上面代码中，函数`f`的最后一步是调用函数`g`，这就叫尾调用。

以下三种情况，都不属于尾调用。

```javascript
// 情况一
function f(x){
  let y = g(x);
  return y;
}

// 情况二
function f(x){
  return g(x) + 1;
}

// 情况三
function f(x){
  g(x);
}
```

上面代码中，情况一是调用函数`g`之后，还有赋值操作，所以不属于尾调用，即使语义完全一样。情况二也属于调用后还有操作，即使写在一行内。情况三等同于下面的代码。

```javascript
function f(x){
  g(x);
  return undefined;
}
```

尾调用不一定出现在函数尾部，只要是最后一步操作即可。

```javascript
function f(x) {
  if (x > 0) {
    return m(x)
  }
  return n(x);
}
```

上面代码中，函数`m`和`n`都属于尾调用，因为它们都是函数`f`的最后一步操作。

### 尾调用优化

尾调用之所以与其他调用不同，就在于它的特殊的调用位置。

我们知道，函数调用会在内存形成一个“调用记录”，又称“调用帧”（call frame），保存调用位置和内部变量等信息。如果在函数`A`的内部调用函数`B`，那么在`A`的调用帧上方，还会形成一个`B`的调用帧。等到`B`运行结束，将结果返回到`A`，`B`的调用帧才会消失。如果函数`B`内部还调用函数`C`，那就还有一个`C`的调用帧，以此类推。所有的调用帧，就形成一个“调用栈”（call stack）。

尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用帧，因为调用位置、内部变量等信息都不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了。

```javascript
function f() {
  let m = 1;
  let n = 2;
  return g(m + n);
}
f();

// 等同于
function f() {
  return g(3);
}
f();

// 等同于
g(3);
```

上面代码中，如果函数`g`不是尾调用，函数`f`就需要保存内部变量`m`和`n`的值、`g`的调用位置等信息。但由于调用`g`之后，函数`f`就结束了，所以执行到最后一步，完全可以删除`f(x)`的调用帧，只保留`g(3)`的调用帧。

这就叫做“尾调用优化”（Tail call optimization），即只保留内层函数的调用帧。如果所有函数都是尾调用，那么完全可以做到每次执行时，调用帧只有一项，这将大大节省内存。这就是“尾调用优化”的意义。

注意，只有不再用到外层函数的内部变量，内层函数的调用帧才会取代外层函数的调用帧，否则就无法进行“尾调用优化”。

```javascript
function addOne(a){
  var one = 1;
  function inner(b){
    return b + one;
  }
  return inner(a);
}
```

上面的函数不会进行尾调用优化，因为内层函数`inner`用到了外层函数`addOne`的内部变量`one`。

注意，目前只有 Safari 浏览器支持尾调用优化，Chrome 和 Firefox 都不支持。

### 尾递归

函数调用自身，称为递归。如果尾调用自身，就称为尾递归。

递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。

```javascript
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}

factorial(5) // 120
```

上面代码是一个阶乘函数，计算`n`的阶乘，最多需要保存`n`个调用记录，复杂度 O(n) 。

如果改写成尾递归，只保留一个调用记录，复杂度 O(1) 。

```javascript
function factorial(n, total) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}

factorial(5, 1) // 120
```

还有一个比较著名的例子，就是计算 Fibonacci 数列，也能充分说明尾递归优化的重要性。

非尾递归的 Fibonacci 数列实现如下。

```javascript
function Fibonacci (n) {
  if ( n <= 1 ) {return 1};

  return Fibonacci(n - 1) + Fibonacci(n - 2);
}

Fibonacci(10) // 89
Fibonacci(100) // 超时
Fibonacci(500) // 超时
```

尾递归优化过的 Fibonacci 数列实现如下。

```javascript
function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {
  if( n <= 1 ) {return ac2};

  return Fibonacci2 (n - 1, ac2, ac1 + ac2);
}

Fibonacci2(100) // 573147844013817200000
Fibonacci2(1000) // 7.0330367711422765e+208
Fibonacci2(10000) // Infinity
```

由此可见，“尾调用优化”对递归操作意义重大，所以一些函数式编程语言将其写入了语言规格。ES6 亦是如此，第一次明确规定，所有 ECMAScript 的实现，都必须部署“尾调用优化”。这就是说，ES6 中只要使用尾递归，就不会发生栈溢出（或者层层递归造成的超时），相对节省内存。

### 递归函数的改写

尾递归的实现，往往需要改写递归函数，确保最后一步只调用自身。做到这一点的方法，就是把所有用到的内部变量改写成函数的参数。比如上面的例子，阶乘函数 factorial 需要用到一个中间变量`total`，那就把这个中间变量改写成函数的参数。这样做的缺点就是不太直观，第一眼很难看出来，为什么计算`5`的阶乘，需要传入两个参数`5`和`1`？

两个方法可以解决这个问题。方法一是在尾递归函数之外，再提供一个正常形式的函数。

```javascript
function tailFactorial(n, total) {
  if (n === 1) return total;
  return tailFactorial(n - 1, n * total);
}

function factorial(n) {
  return tailFactorial(n, 1);
}

factorial(5) // 120
```

上面代码通过一个正常形式的阶乘函数`factorial`，调用尾递归函数`tailFactorial`，看起来就正常多了。

函数式编程有一个概念，叫做柯里化（currying），意思是将多参数的函数转换成单参数的形式。这里也可以使用柯里化。

```javascript
function currying(fn, n) {
  return function (m) {
    return fn.call(this, m, n);
  };
}

function tailFactorial(n, total) {
  if (n === 1) return total;
  return tailFactorial(n - 1, n * total);
}

const factorial = currying(tailFactorial, 1);

factorial(5) // 120
```

上面代码通过柯里化，将尾递归函数`tailFactorial`变为只接受一个参数的`factorial`。

第二种方法就简单多了，就是采用 ES6 的函数默认值。

```javascript
function factorial(n, total = 1) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}

factorial(5) // 120
```

上面代码中，参数`total`有默认值`1`，所以调用时不用提供这个值。

总结一下，递归本质上是一种循环操作。纯粹的函数式编程语言没有循环操作命令，所有的循环都用递归实现，这就是为什么尾递归对这些语言极其重要。对于其他支持“尾调用优化”的语言（比如 Lua，ES6），只需要知道循环可以用递归代替，而一旦使用递归，就最好使用尾递归。

### 严格模式

ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。

这是因为在正常模式下，函数内部有两个变量，可以跟踪函数的调用栈。

- `func.arguments`：返回调用时函数的参数。
- `func.caller`：返回调用当前函数的那个函数。

尾调用优化发生时，函数的调用栈会改写，因此上面两个变量就会失真。严格模式禁用这两个变量，所以尾调用模式仅在严格模式下生效。

```javascript
function restricted() {
  'use strict';
  restricted.caller;    // 报错
  restricted.arguments; // 报错
}
restricted();
```

### 尾递归优化的实现

尾递归优化只在严格模式下生效，那么正常模式下，或者那些不支持该功能的环境中，有没有办法也使用尾递归优化呢？回答是可以的，就是自己实现尾递归优化。

它的原理非常简单。尾递归之所以需要优化，原因是调用栈太多，造成溢出，那么只要减少调用栈，就不会溢出。怎么做可以减少调用栈呢？就是采用“循环”换掉“递归”。

下面是一个正常的递归函数。

```javascript
function sum(x, y) {
  if (y > 0) {
    return sum(x + 1, y - 1);
  } else {
    return x;
  }
}

sum(1, 100000)
// Uncaught RangeError: Maximum call stack size exceeded(…)
```

上面代码中，`sum`是一个递归函数，参数`x`是需要累加的值，参数`y`控制递归次数。一旦指定`sum`递归 100000 次，就会报错，提示超出调用栈的最大次数。

蹦床函数（trampoline）可以将递归执行转为循环执行。

```javascript
function trampoline(f) {
  while (f && f instanceof Function) {
    f = f();
  }
  return f;
}
```

上面就是蹦床函数的一个实现，它接受一个函数`f`作为参数。只要`f`执行后返回一个函数，就继续执行。注意，这里是返回一个函数，然后执行该函数，而不是函数里面调用函数，这样就避免了递归执行，从而就消除了调用栈过大的问题。

然后，要做的就是将原来的递归函数，改写为每一步返回另一个函数。

```javascript
function sum(x, y) {
  if (y > 0) {
    return sum.bind(null, x + 1, y - 1);
  } else {
    return x;
  }
}
```

上面代码中，`sum`函数的每次执行，都会返回自身的另一个版本。

现在，使用蹦床函数执行`sum`，就不会发生调用栈溢出。

```javascript
trampoline(sum(1, 100000))
// 100001
```

蹦床函数并不是真正的尾递归优化，下面的实现才是。

```javascript
function tco(f) {
  var value;
  var active = false;
  var accumulated = [];

  return function accumulator() {
    accumulated.push(arguments);
    if (!active) {
      active = true;
      while (accumulated.length) {
        value = f.apply(this, accumulated.shift());
      }
      active = false;
      return value;
    }
  };
}

var sum = tco(function(x, y) {
  if (y > 0) {
    return sum(x + 1, y - 1)
  }
  else {
    return x
  }
});

sum(1, 100000)
// 100001
```

上面代码中，`tco`函数是尾递归优化的实现，它的奥妙就在于状态变量`active`。默认情况下，这个变量是不激活的。一旦进入尾递归优化的过程，这个变量就激活了。然后，每一轮递归`sum`返回的都是`undefined`，所以就避免了递归执行；而`accumulated`数组存放每一轮`sum`执行的参数，总是有值的，这就保证了`accumulator`函数内部的`while`循环总是会执行。这样就很巧妙地将“递归”改成了“循环”，而后一轮的参数会取代前一轮的参数，保证了调用栈只有一层。

## 函数参数的尾逗号

ES2017 [允许](https://github.com/jeffmo/es-trailing-function-commas)函数的最后一个参数有尾逗号（trailing comma）。

此前，函数定义和调用时，都不允许最后一个参数后面出现逗号。

```javascript
function clownsEverywhere(
  param1,
  param2
) { /* ... */ }

clownsEverywhere(
  'foo',
  'bar'
);
```

上面代码中，如果在`param2`或`bar`后面加一个逗号，就会报错。

如果像上面这样，将参数写成多行（即每个参数占据一行），以后修改代码的时候，想为函数`clownsEverywhere`添加第三个参数，或者调整参数的次序，就势必要在原来最后一个参数后面添加一个逗号。这对于版本管理系统来说，就会显示添加逗号的那一行也发生了变动。这看上去有点冗余，因此新的语法允许定义和调用时，尾部直接有一个逗号。

```javascript
function clownsEverywhere(
  param1,
  param2,
) { /* ... */ }

clownsEverywhere(
  'foo',
  'bar',
);
```

这样的规定也使得，函数参数与数组和对象的尾逗号规则，保持一致了。

## Function.prototype.toString()

[ES2019](https://github.com/tc39/Function-prototype-toString-revision) 对函数实例的`toString()`方法做出了修改。

`toString()`方法返回函数代码本身，以前会省略注释和空格。

```javascript
function /* foo comment */ foo () {}

foo.toString()
// function foo() {}
```

上面代码中，函数`foo`的原始代码包含注释，函数名`foo`和圆括号之间有空格，但是`toString()`方法都把它们省略了。

修改后的`toString()`方法，明确要求返回一模一样的原始代码。

```javascript
function /* foo comment */ foo () {}

foo.toString()
// "function /* foo comment */ foo () {}"
```

## catch 命令的参数省略

JavaScript 语言的`try...catch`结构，以前明确要求`catch`命令后面必须跟参数，接受`try`代码块抛出的错误对象。

```javascript
try {
  // ...
} catch (err) {
  // 处理错误
}
```

上面代码中，`catch`命令后面带有参数`err`。

很多时候，`catch`代码块可能用不到这个参数。但是，为了保证语法正确，还是必须写。[ES2019](https://github.com/tc39/proposal-optional-catch-binding) 做出了改变，允许`catch`语句省略参数。

```javascript
try {
  // ...
} catch {
  // ...
}
```
# 数组的扩展

## 扩展运算符

### 含义

扩展运算符（spread）是三个点（`...`）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。

```javascript
console.log(...[1, 2, 3])
// 1 2 3

console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5

[...document.querySelectorAll('div')]
// [<div>, <div>, <div>]
```

该运算符主要用于函数调用。

```javascript
function push(array, ...items) {
  array.push(...items);
}

function add(x, y) {
  return x + y;
}

const numbers = [4, 38];
add(...numbers) // 42
```

上面代码中，`array.push(...items)`和`add(...numbers)`这两行，都是函数的调用，它们都使用了扩展运算符。该运算符将一个数组，变为参数序列。

扩展运算符与正常的函数参数可以结合使用，非常灵活。

```javascript
function f(v, w, x, y, z) { }
const args = [0, 1];
f(-1, ...args, 2, ...[3]);
```

扩展运算符后面还可以放置表达式。

```javascript
const arr = [
  ...(x > 0 ? ['a'] : []),
  'b',
];
```

如果扩展运算符后面是一个空数组，则不产生任何效果。

```javascript
[...[], 1]
// [1]
```

注意，只有函数调用时，扩展运算符才可以放在圆括号中，否则会报错。

```javascript
(...[1, 2])
// Uncaught SyntaxError: Unexpected number

console.log((...[1, 2]))
// Uncaught SyntaxError: Unexpected number

console.log(...[1, 2])
// 1 2
```

上面三种情况，扩展运算符都放在圆括号里面，但是前两种情况会报错，因为扩展运算符所在的括号不是函数调用。

### 替代函数的 apply 方法

由于扩展运算符可以展开数组，所以不再需要`apply`方法，将数组转为函数的参数了。

```javascript
// ES5 的写法
function f(x, y, z) {
  // ...
}
var args = [0, 1, 2];
f.apply(null, args);

// ES6的写法
function f(x, y, z) {
  // ...
}
let args = [0, 1, 2];
f(...args);
```

下面是扩展运算符取代`apply`方法的一个实际的例子，应用`Math.max`方法，简化求出一个数组最大元素的写法。

```javascript
// ES5 的写法
Math.max.apply(null, [14, 3, 77])

// ES6 的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);
```

上面代码中，由于 JavaScript 不提供求数组最大元素的函数，所以只能套用`Math.max`函数，将数组转为一个参数序列，然后求最大值。有了扩展运算符以后，就可以直接用`Math.max`了。

另一个例子是通过`push`函数，将一个数组添加到另一个数组的尾部。

```javascript
// ES5的 写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);

// ES6 的写法
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
arr1.push(...arr2);
```

上面代码的 ES5 写法中，`push`方法的参数不能是数组，所以只好通过`apply`方法变通使用`push`方法。有了扩展运算符，就可以直接将数组传入`push`方法。

下面是另外一个例子。

```javascript
// ES5
new (Date.bind.apply(Date, [null, 2015, 1, 1]))
// ES6
new Date(...[2015, 1, 1]);
```

### 扩展运算符的应用

**（1）复制数组**

数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。

```javascript
const a1 = [1, 2];
const a2 = a1;

a2[0] = 2;
a1 // [2, 2]
```

上面代码中，`a2`并不是`a1`的克隆，而是指向同一份数据的另一个指针。修改`a2`，会直接导致`a1`的变化。

ES5 只能用变通方法来复制数组。

```javascript
const a1 = [1, 2];
const a2 = a1.concat();

a2[0] = 2;
a1 // [1, 2]
```

上面代码中，`a1`会返回原数组的克隆，再修改`a2`就不会对`a1`产生影响。

扩展运算符提供了复制数组的简便写法。

```javascript
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```

上面的两种写法，`a2`都是`a1`的克隆。

**（2）合并数组**

扩展运算符提供了数组合并的新写法。

```javascript
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3]
// [ 'a', 'b', 'c', 'd', 'e' ]
```

不过，这两种方法都是浅拷贝，使用的时候需要注意。

```javascript
const a1 = [{ foo: 1 }];
const a2 = [{ bar: 2 }];

const a3 = a1.concat(a2);
const a4 = [...a1, ...a2];

a3[0] === a1[0] // true
a4[0] === a1[0] // true
```

上面代码中，`a3`和`a4`是用两种不同方法合并而成的新数组，但是它们的成员都是对原数组成员的引用，这就是浅拷贝。如果修改了引用指向的值，会同步反映到新数组。

**（3）与解构赋值结合**

扩展运算符可以与解构赋值结合起来，用于生成数组。

```javascript
// ES5
a = list[0], rest = list.slice(1)
// ES6
[a, ...rest] = list
```

下面是另外一些例子。

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
```

如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。

```javascript
const [...butLast, last] = [1, 2, 3, 4, 5];
// 报错

const [first, ...middle, last] = [1, 2, 3, 4, 5];
// 报错
```

**（4）字符串**

扩展运算符还可以将字符串转为真正的数组。

```javascript
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```

上面的写法，有一个重要的好处，那就是能够正确识别四个字节的 Unicode 字符。

```javascript
'x\uD83D\uDE80y'.length // 4
[...'x\uD83D\uDE80y'].length // 3
```

上面代码的第一种写法，JavaScript 会将四个字节的 Unicode 字符，识别为 2 个字符，采用扩展运算符就没有这个问题。因此，正确返回字符串长度的函数，可以像下面这样写。

```javascript
function length(str) {
  return [...str].length;
}

length('x\uD83D\uDE80y') // 3
```

凡是涉及到操作四个字节的 Unicode 字符的函数，都有这个问题。因此，最好都用扩展运算符改写。

```javascript
let str = 'x\uD83D\uDE80y';

str.split('').reverse().join('')
// 'y\uDE80\uD83Dx'

[...str].reverse().join('')
// 'y\uD83D\uDE80x'
```

上面代码中，如果不用扩展运算符，字符串的`reverse`操作就不正确。

**（5）实现了 Iterator 接口的对象**

任何定义了遍历器（Iterator）接口的对象（参阅 Iterator 一章），都可以用扩展运算符转为真正的数组。

```javascript
let nodeList = document.querySelectorAll('div');
let array = [...nodeList];
```

上面代码中，`querySelectorAll`方法返回的是一个`NodeList`对象。它不是数组，而是一个类似数组的对象。这时，扩展运算符可以将其转为真正的数组，原因就在于`NodeList`对象实现了 Iterator 。

```javascript
Number.prototype[Symbol.iterator] = function*() {
  let i = 0;
  let num = this.valueOf();
  while (i < num) {
    yield i++;
  }
}

console.log([...5]) // [0, 1, 2, 3, 4]
```

上面代码中，先定义了`Number`对象的遍历器接口，扩展运算符将`5`自动转成`Number`实例以后，就会调用这个接口，就会返回自定义的结果。

对于那些没有部署 Iterator 接口的类似数组的对象，扩展运算符就无法将其转为真正的数组。

```javascript
let arrayLike = {
  '0': 'a',
  '1': 'b',
  '2': 'c',
  length: 3
};

// TypeError: Cannot spread non-iterable object.
let arr = [...arrayLike];
```

上面代码中，`arrayLike`是一个类似数组的对象，但是没有部署 Iterator 接口，扩展运算符就会报错。这时，可以改为使用`Array.from`方法将`arrayLike`转为真正的数组。

**（6）Map 和 Set 结构，Generator 函数**

扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。

```javascript
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]
```

Generator 函数运行后，返回一个遍历器对象，因此也可以使用扩展运算符。

```javascript
const go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]
```

上面代码中，变量`go`是一个 Generator 函数，执行后返回的是一个遍历器对象，对这个遍历器对象执行扩展运算符，就会将内部遍历得到的值，转为一个数组。

如果对没有 Iterator 接口的对象，使用扩展运算符，将会报错。

```javascript
const obj = {a: 1, b: 2};
let arr = [...obj]; // TypeError: Cannot spread non-iterable object
```

## Array.from()

`Array.from`方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。

下面是一个类似数组的对象，`Array.from`将它转为真正的数组。

```javascript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

实际应用中，常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的`arguments`对象。`Array.from`都可以将它们转为真正的数组。

```javascript
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).filter(p => {
  return p.textContent.length > 100;
});

// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
```

上面代码中，`querySelectorAll`方法返回的是一个类似数组的对象，可以将这个对象转为真正的数组，再使用`filter`方法。

只要是部署了 Iterator 接口的数据结构，`Array.from`都能将其转为数组。

```javascript
Array.from('hello')
// ['h', 'e', 'l', 'l', 'o']

let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']
```

上面代码中，字符串和 Set 结构都具有 Iterator 接口，因此可以被`Array.from`转为真正的数组。

如果参数是一个真正的数组，`Array.from`会返回一个一模一样的新数组。

```javascript
Array.from([1, 2, 3])
// [1, 2, 3]
```

值得提醒的是，扩展运算符（`...`）也可以将某些数据结构转为数组。

```javascript
// arguments对象
function foo() {
  const args = [...arguments];
}

// NodeList对象
[...document.querySelectorAll('div')]
```

扩展运算符背后调用的是遍历器接口（`Symbol.iterator`），如果一个对象没有部署这个接口，就无法转换。`Array.from`方法还支持类似数组的对象。所谓类似数组的对象，本质特征只有一点，即必须有`length`属性。因此，任何有`length`属性的对象，都可以通过`Array.from`方法转为数组，而此时扩展运算符就无法转换。

```javascript
Array.from({ length: 3 });
// [ undefined, undefined, undefined ]
```

上面代码中，`Array.from`返回了一个具有三个成员的数组，每个位置的值都是`undefined`。扩展运算符转换不了这个对象。

对于还没有部署该方法的浏览器，可以用`Array.prototype.slice`方法替代。

```javascript
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```

`Array.from`还可以接受第二个参数，作用类似于数组的`map`方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

```javascript
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]
```

下面的例子是取出一组 DOM 节点的文本内容。

```javascript
let spans = document.querySelectorAll('span.name');

// map()
let names1 = Array.prototype.map.call(spans, s => s.textContent);

// Array.from()
let names2 = Array.from(spans, s => s.textContent)
```

下面的例子将数组中布尔值为`false`的成员转为`0`。

```javascript
Array.from([1, , 2, , 3], (n) => n || 0)
// [1, 0, 2, 0, 3]
```

另一个例子是返回各种数据的类型。

```javascript
function typesOf () {
  return Array.from(arguments, value => typeof value)
}
typesOf(null, [], NaN)
// ['object', 'object', 'number']
```

如果`map`函数里面用到了`this`关键字，还可以传入`Array.from`的第三个参数，用来绑定`this`。

`Array.from()`可以将各种值转为真正的数组，并且还提供`map`功能。这实际上意味着，只要有一个原始的数据结构，你就可以先对它的值进行处理，然后转成规范的数组结构，进而就可以使用数量众多的数组方法。

```javascript
Array.from({ length: 2 }, () => 'jack')
// ['jack', 'jack']
```

上面代码中，`Array.from`的第一个参数指定了第二个参数运行的次数。这种特性可以让该方法的用法变得非常灵活。

`Array.from()`的另一个应用是，将字符串转为数组，然后返回字符串的长度。因为它能正确处理各种 Unicode 字符，可以避免 JavaScript 将大于`\uFFFF`的 Unicode 字符，算作两个字符的 bug。

```javascript
function countSymbols(string) {
  return Array.from(string).length;
}
```

## Array.of()

`Array.of()`方法用于将一组值，转换为数组。

```javascript
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```

这个方法的主要目的，是弥补数组构造函数`Array()`的不足。因为参数个数的不同，会导致`Array()`的行为有差异。

```javascript
Array() // []
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]
```

上面代码中，`Array()`方法没有参数、一个参数、三个参数时，返回的结果都不一样。只有当参数个数不少于 2 个时，`Array()`才会返回由参数组成的新数组。参数只有一个正整数时，实际上是指定数组的长度。

`Array.of()`基本上可以用来替代`Array()`或`new Array()`，并且不存在由于参数不同而导致的重载。它的行为非常统一。

```javascript
Array.of() // []
Array.of(undefined) // [undefined]
Array.of(1) // [1]
Array.of(1, 2) // [1, 2]
```

`Array.of()`总是返回参数值组成的数组。如果没有参数，就返回一个空数组。

`Array.of()`方法可以用下面的代码模拟实现。

```javascript
function ArrayOf(){
  return [].slice.call(arguments);
}
```

## 实例方法：copyWithin()

数组实例的`copyWithin()`方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。

```javascript
Array.prototype.copyWithin(target, start = 0, end = this.length)
```

它接受三个参数。

- target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
- start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
- end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。

这三个参数都应该是数值，如果不是，会自动转为数值。

```javascript
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]
```

上面代码表示将从 3 号位直到数组结束的成员（4 和 5），复制到从 0 号位开始的位置，结果覆盖了原来的 1 和 2。

下面是更多例子。

```javascript
// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1)
// [4, 2, 3, 4, 5]

// 将3号位复制到0号位
[].copyWithin.call({length: 5, 3: 1}, 0, 3)
// {0: 1, 3: 1, length: 5}

// 将2号位到数组结束，复制到0号位
let i32a = new Int32Array([1, 2, 3, 4, 5]);
i32a.copyWithin(0, 2);
// Int32Array [3, 4, 5, 4, 5]

// 对于没有部署 TypedArray 的 copyWithin 方法的平台
// 需要采用下面的写法
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
// Int32Array [4, 2, 3, 4, 5]
```

## 实例方法：find() 和 findIndex()

数组实例的`find`方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为`true`的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`。

```javascript
[1, 4, -5, 10].find((n) => n < 0)
// -5
```

上面代码找出数组中第一个小于 0 的成员。

```javascript
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
```

上面代码中，`find`方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。

数组实例的`findIndex`方法的用法与`find`方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回`-1`。

```javascript
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```

这两个方法都可以接受第二个参数，用来绑定回调函数的`this`对象。

```javascript
function f(v){
  return v > this.age;
}
let person = {name: 'John', age: 20};
[10, 12, 26, 15].find(f, person);    // 26
```

上面的代码中，`find`函数接收了第二个参数`person`对象，回调函数中的`this`对象指向`person`对象。

另外，这两个方法都可以发现`NaN`，弥补了数组的`indexOf`方法的不足。

```javascript
[NaN].indexOf(NaN)
// -1

[NaN].findIndex(y => Object.is(NaN, y))
// 0
```

上面代码中，`indexOf`方法无法识别数组的`NaN`成员，但是`findIndex`方法可以借助`Object.is`方法做到。

## 实例方法：fill()

`fill`方法使用给定值，填充一个数组。

```javascript
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]
```

上面代码表明，`fill`方法用于空数组的初始化非常方便。数组中已有的元素，会被全部抹去。

`fill`方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。

```javascript
['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
```

上面代码表示，`fill`方法从 1 号位开始，向原数组填充 7，到 2 号位之前结束。

注意，如果填充的类型为对象，那么被赋值的是同一个内存地址的对象，而不是深拷贝对象。

```javascript
let arr = new Array(3).fill({name: "Mike"});
arr[0].name = "Ben";
arr
// [{name: "Ben"}, {name: "Ben"}, {name: "Ben"}]

let arr = new Array(3).fill([]);
arr[0].push(5);
arr
// [[5], [5], [5]]
```

## 实例方法：entries()，keys() 和 values()

ES6 提供三个新的方法——`entries()`，`keys()`和`values()`——用于遍历数组。它们都返回一个遍历器对象（详见《Iterator》一章），可以用`for...of`循环进行遍历，唯一的区别是`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历。

```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

如果不使用`for...of`循环，可以手动调用遍历器对象的`next`方法，进行遍历。

```javascript
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']
```

## 实例方法：includes()

`Array.prototype.includes`方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的`includes`方法类似。ES2016 引入了该方法。

```javascript
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
```

该方法的第二个参数表示搜索的起始位置，默认为`0`。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为`-4`，但数组长度为`3`），则会重置为从`0`开始。

```javascript
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
```

没有该方法之前，我们通常使用数组的`indexOf`方法，检查是否包含某个值。

```javascript
if (arr.indexOf(el) !== -1) {
  // ...
}
```

`indexOf`方法有两个缺点，一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于`-1`，表达起来不够直观。二是，它内部使用严格相等运算符（`===`）进行判断，这会导致对`NaN`的误判。

```javascript
[NaN].indexOf(NaN)
// -1
```

`includes`使用的是不一样的判断算法，就没有这个问题。

```javascript
[NaN].includes(NaN)
// true
```

下面代码用来检查当前环境是否支持该方法，如果不支持，部署一个简易的替代版本。

```javascript
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(['foo', 'bar'], 'baz'); // => false
```

另外，Map 和 Set 数据结构有一个`has`方法，需要注意与`includes`区分。

- Map 结构的`has`方法，是用来查找键名的，比如`Map.prototype.has(key)`、`WeakMap.prototype.has(key)`、`Reflect.has(target, propertyKey)`。
- Set 结构的`has`方法，是用来查找值的，比如`Set.prototype.has(value)`、`WeakSet.prototype.has(value)`。

## 实例方法：flat()，flatMap()

数组的成员有时还是数组，`Array.prototype.flat()`用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。

```javascript
[1, 2, [3, 4]].flat()
// [1, 2, 3, 4]
```

上面代码中，原数组的成员里面有一个数组，`flat()`方法将子数组的成员取出来，添加在原来的位置。

`flat()`默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将`flat()`方法的参数写成一个整数，表示想要拉平的层数，默认为1。

```javascript
[1, 2, [3, [4, 5]]].flat()
// [1, 2, 3, [4, 5]]

[1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]
```

上面代码中，`flat()`的参数为2，表示要“拉平”两层的嵌套数组。

如果不管有多少层嵌套，都要转成一维数组，可以用`Infinity`关键字作为参数。

```javascript
[1, [2, [3]]].flat(Infinity)
// [1, 2, 3]
```

如果原数组有空位，`flat()`方法会跳过空位。

```javascript
[1, 2, , 4, 5].flat()
// [1, 2, 4, 5]
```

`flatMap()`方法对原数组的每个成员执行一个函数（相当于执行`Array.prototype.map()`），然后对返回值组成的数组执行`flat()`方法。该方法返回一个新数组，不改变原数组。

```javascript
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]
```

`flatMap()`只能展开一层数组。

```javascript
// 相当于 [[[2]], [[4]], [[6]], [[8]]].flat()
[1, 2, 3, 4].flatMap(x => [[x * 2]])
// [[2], [4], [6], [8]]
```

上面代码中，遍历函数返回的是一个双层的数组，但是默认只能展开一层，因此`flatMap()`返回的还是一个嵌套数组。

`flatMap()`方法的参数是一个遍历函数，该函数可以接受三个参数，分别是当前数组成员、当前数组成员的位置（从零开始）、原数组。

```javascript
arr.flatMap(function callback(currentValue[, index[, array]]) {
  // ...
}[, thisArg])
```

`flatMap()`方法还可以有第二个参数，用来绑定遍历函数里面的`this`。

## 实例方法：at()

长久以来，JavaScript 不支持数组的负索引，如果要引用数组的最后一个成员，不能写成`arr[-1]`，只能使用`arr[arr.length - 1]`。

这是因为方括号运算符`[]`在 JavaScript 语言里面，不仅用于数组，还用于对象。对于对象来说，方括号里面就是键名，比如`obj[1]`引用的是键名为字符串`1`的键，同理`obj[-1]`引用的是键名为字符串`-1`的键。由于 JavaScript 的数组是特殊的对象，所以方括号里面的负数无法再有其他语义了，也就是说，不可能添加新语法来支持负索引。

为了解决这个问题，现在有一个[提案](https://github.com/tc39/proposal-relative-indexing-method/)，为数组实例增加了`at()`方法，接受一个整数作为参数，返回对应位置的成员，支持负索引。这个方法不仅可用于数组，也可用于字符串和类型数组（TypedArray）。

```javascript
const arr = [5, 12, 8, 130, 44];
arr.at(2) // 8
arr.at(-2) // 130
```

如果参数位置超出了数组范围，`at()`返回`undefined`。

## 数组的空位

数组的空位指的是，数组的某一个位置没有任何值，比如`Array()`构造函数返回的数组都是空位。

```javascript
Array(3) // [, , ,]
```

上面代码中，`Array(3)`返回一个具有 3 个空位的数组。

注意，空位不是`undefined`，某一个位置的值等于`undefined`，依然是有值的。空位是没有任何值，`in`运算符可以说明这一点。

```javascript
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```

上面代码说明，第一个数组的 0 号位置是有值的，第二个数组的 0 号位置没有值。

ES5 对空位的处理，已经很不一致了，大多数情况下会忽略空位。

- `forEach()`, `filter()`, `reduce()`, `every()` 和`some()`都会跳过空位。
- `map()`会跳过空位，但会保留这个值
- `join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。

```javascript
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1

// filter方法
['a',,'b'].filter(x => true) // ['a','b']

// every方法
[,'a'].every(x => x==='a') // true

// reduce方法
[1,,2].reduce((x,y) => x+y) // 3

// some方法
[,'a'].some(x => x !== 'a') // false

// map方法
[,'a'].map(x => 1) // [,1]

// join方法
[,'a',undefined,null].join('#') // "#a##"

// toString方法
[,'a',undefined,null].toString() // ",a,,"
```

ES6 则是明确将空位转为`undefined`。

`Array.from()`方法会将数组的空位，转为`undefined`，也就是说，这个方法不会忽略空位。

```javascript
Array.from(['a',,'b'])
// [ "a", undefined, "b" ]
```

扩展运算符（`...`）也会将空位转为`undefined`。

```javascript
[...['a',,'b']]
// [ "a", undefined, "b" ]
```

`copyWithin()`会连空位一起拷贝。

```javascript
[,'a','b',,].copyWithin(2,0) // [,"a",,"a"]
```

`fill()`会将空位视为正常的数组位置。

```javascript
new Array(3).fill('a') // ["a","a","a"]
```

`for...of`循环也会遍历空位。

```javascript
let arr = [, ,];
for (let i of arr) {
  console.log(1);
}
// 1
// 1
```

上面代码中，数组`arr`有两个空位，`for...of`并没有忽略它们。如果改成`map()`方法遍历，空位是会跳过的。

`entries()`、`keys()`、`values()`、`find()`和`findIndex()`会将空位处理成`undefined`。

```javascript
// entries()
[...[,'a'].entries()] // [[0,undefined], [1,"a"]]

// keys()
[...[,'a'].keys()] // [0,1]

// values()
[...[,'a'].values()] // [undefined,"a"]

// find()
[,'a'].find(x => true) // undefined

// findIndex()
[,'a'].findIndex(x => true) // 0
```

由于空位的处理规则非常不统一，所以建议避免出现空位。

## Array.prototype.sort() 的排序稳定性

排序稳定性（stable sorting）是排序算法的重要属性，指的是排序关键字相同的项目，排序前后的顺序不变。

```javascript
const arr = [
  'peach',
  'straw',
  'apple',
  'spork'
];

const stableSorting = (s1, s2) => {
  if (s1[0] < s2[0]) return -1;
  return 1;
};

arr.sort(stableSorting)
// ["apple", "peach", "straw", "spork"]
```

上面代码对数组`arr`按照首字母进行排序。排序结果中，`straw`在`spork`的前面，跟原始顺序一致，所以排序算法`stableSorting`是稳定排序。

```javascript
const unstableSorting = (s1, s2) => {
  if (s1[0] <= s2[0]) return -1;
  return 1;
};

arr.sort(unstableSorting)
// ["apple", "peach", "spork", "straw"]
```

上面代码中，排序结果是`spork`在`straw`前面，跟原始顺序相反，所以排序算法`unstableSorting`是不稳定的。

常见的排序算法之中，插入排序、合并排序、冒泡排序等都是稳定的，堆排序、快速排序等是不稳定的。不稳定排序的主要缺点是，多重排序时可能会产生问题。假设有一个姓和名的列表，要求按照“姓氏为主要关键字，名字为次要关键字”进行排序。开发者可能会先按名字排序，再按姓氏进行排序。如果排序算法是稳定的，这样就可以达到“先姓氏，后名字”的排序效果。如果是不稳定的，就不行。

早先的 ECMAScript 没有规定，`Array.prototype.sort()`的默认排序算法是否稳定，留给浏览器自己决定，这导致某些实现是不稳定的。[ES2019](https://github.com/tc39/ecma262/pull/1340) 明确规定，`Array.prototype.sort()`的默认排序算法必须稳定。这个规定已经做到了，现在 JavaScript 各个主要实现的默认排序算法都是稳定的。

# 对象的扩展

对象（object）是 JavaScript 最重要的数据结构。ES6 对它进行了重大升级，本章介绍数据结构本身的改变，下一章介绍`Object`对象的新增方法。

## 属性的简洁表示法

ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。

```javascript
const foo = 'bar';
const baz = {foo};
baz // {foo: "bar"}

// 等同于
const baz = {foo: foo};
```

上面代码中，变量`foo`直接写在大括号里面。这时，属性名就是变量名, 属性值就是变量值。下面是另一个例子。

```javascript
function f(x, y) {
  return {x, y};
}

// 等同于

function f(x, y) {
  return {x: x, y: y};
}

f(1, 2) // Object {x: 1, y: 2}
```

除了属性简写，方法也可以简写。

```javascript
const o = {
  method() {
    return "Hello!";
  }
};

// 等同于

const o = {
  method: function() {
    return "Hello!";
  }
};
```

下面是一个实际的例子。

```javascript
let birth = '2000/01/01';

const Person = {

  name: '张三',

  //等同于birth: birth
  birth,

  // 等同于hello: function ()...
  hello() { console.log('我的名字是', this.name); }

};
```

这种写法用于函数的返回值，将会非常方便。

```javascript
function getPoint() {
  const x = 1;
  const y = 10;
  return {x, y};
}

getPoint()
// {x:1, y:10}
```

CommonJS 模块输出一组变量，就非常合适使用简洁写法。

```javascript
let ms = {};

function getItem (key) {
  return key in ms ? ms[key] : null;
}

function setItem (key, value) {
  ms[key] = value;
}

function clear () {
  ms = {};
}

module.exports = { getItem, setItem, clear };
// 等同于
module.exports = {
  getItem: getItem,
  setItem: setItem,
  clear: clear
};
```

属性的赋值器（setter）和取值器（getter），事实上也是采用这种写法。

```javascript
const cart = {
  _wheels: 4,

  get wheels () {
    return this._wheels;
  },

  set wheels (value) {
    if (value < this._wheels) {
      throw new Error('数值太小了！');
    }
    this._wheels = value;
  }
}
```

简洁写法在打印对象时也很有用。

```javascript
let user = {
  name: 'test'
};

let foo = {
  bar: 'baz'
};

console.log(user, foo)
// {name: "test"} {bar: "baz"}
console.log({user, foo})
// {user: {name: "test"}, foo: {bar: "baz"}}
```

上面代码中，`console.log`直接输出`user`和`foo`两个对象时，就是两组键值对，可能会混淆。把它们放在大括号里面输出，就变成了对象的简洁表示法，每组键值对前面会打印对象名，这样就比较清晰了。

注意，简写的对象方法不能用作构造函数，会报错。

```javascript
const obj = {
  f() {
    this.foo = 'bar';
  }
};

new obj.f() // 报错
```

上面代码中，`f`是一个简写的对象方法，所以`obj.f`不能当作构造函数使用。

## 属性名表达式

JavaScript 定义对象的属性，有两种方法。

```javascript
// 方法一
obj.foo = true;

// 方法二
obj['a' + 'bc'] = 123;
```

上面代码的方法一是直接用标识符作为属性名，方法二是用表达式作为属性名，这时要将表达式放在方括号之内。

但是，如果使用字面量方式定义对象（使用大括号），在 ES5 中只能使用方法一（标识符）定义属性。

```javascript
var obj = {
  foo: true,
  abc: 123
};
```

ES6 允许字面量定义对象时，用方法二（表达式）作为对象的属性名，即把表达式放在方括号内。

```javascript
let propKey = 'foo';

let obj = {
  [propKey]: true,
  ['a' + 'bc']: 123
};
```

下面是另一个例子。

```javascript
let lastWord = 'last word';

const a = {
  'first word': 'hello',
  [lastWord]: 'world'
};

a['first word'] // "hello"
a[lastWord] // "world"
a['last word'] // "world"
```

表达式还可以用于定义方法名。

```javascript
let obj = {
  ['h' + 'ello']() {
    return 'hi';
  }
};

obj.hello() // hi
```

注意，属性名表达式与简洁表示法，不能同时使用，会报错。

```javascript
// 报错
const foo = 'bar';
const bar = 'abc';
const baz = { [foo] };

// 正确
const foo = 'bar';
const baz = { [foo]: 'abc'};
```

注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串`[object Object]`，这一点要特别小心。

```javascript
const keyA = {a: 1};
const keyB = {b: 2};

const myObject = {
  [keyA]: 'valueA',
  [keyB]: 'valueB'
};

myObject // Object {[object Object]: "valueB"}
```

上面代码中，`[keyA]`和`[keyB]`得到的都是`[object Object]`，所以`[keyB]`会把`[keyA]`覆盖掉，而`myObject`最后只有一个`[object Object]`属性。

## 方法的 name 属性

函数的`name`属性，返回函数名。对象方法也是函数，因此也有`name`属性。

```javascript
const person = {
  sayName() {
    console.log('hello!');
  },
};

person.sayName.name   // "sayName"
```

上面代码中，方法的`name`属性返回函数名（即方法名）。

如果对象的方法使用了取值函数（`getter`）和存值函数（`setter`），则`name`属性不是在该方法上面，而是该方法的属性的描述对象的`get`和`set`属性上面，返回值是方法名前加上`get`和`set`。

```javascript
const obj = {
  get foo() {},
  set foo(x) {}
};

obj.foo.name
// TypeError: Cannot read property 'name' of undefined

const descriptor = Object.getOwnPropertyDescriptor(obj, 'foo');

descriptor.get.name // "get foo"
descriptor.set.name // "set foo"
```

有两种特殊情况：`bind`方法创造的函数，`name`属性返回`bound`加上原函数的名字；`Function`构造函数创造的函数，`name`属性返回`anonymous`。

```javascript
(new Function()).name // "anonymous"

var doSomething = function() {
  // ...
};
doSomething.bind().name // "bound doSomething"
```

如果对象的方法是一个 Symbol 值，那么`name`属性返回的是这个 Symbol 值的描述。

```javascript
const key1 = Symbol('description');
const key2 = Symbol();
let obj = {
  [key1]() {},
  [key2]() {},
};
obj[key1].name // "[description]"
obj[key2].name // ""
```

上面代码中，`key1`对应的 Symbol 值有描述，`key2`没有。

## 属性的可枚举性和遍历

### 可枚举性

对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为。`Object.getOwnPropertyDescriptor`方法可以获取该属性的描述对象。

```javascript
let obj = { foo: 123 };
Object.getOwnPropertyDescriptor(obj, 'foo')
//  {
//    value: 123,
//    writable: true,
//    enumerable: true,
//    configurable: true
//  }
```

描述对象的`enumerable`属性，称为“可枚举性”，如果该属性为`false`，就表示某些操作会忽略当前属性。

目前，有四个操作会忽略`enumerable`为`false`的属性。

- `for...in`循环：只遍历对象自身的和继承的可枚举的属性。
- `Object.keys()`：返回对象自身的所有可枚举的属性的键名。
- `JSON.stringify()`：只串行化对象自身的可枚举的属性。
- `Object.assign()`： 忽略`enumerable`为`false`的属性，只拷贝对象自身的可枚举的属性。

这四个操作之中，前三个是 ES5 就有的，最后一个`Object.assign()`是 ES6 新增的。其中，只有`for...in`会返回继承的属性，其他三个方法都会忽略继承的属性，只处理对象自身的属性。实际上，引入“可枚举”（`enumerable`）这个概念的最初目的，就是让某些属性可以规避掉`for...in`操作，不然所有内部属性和方法都会被遍历到。比如，对象原型的`toString`方法，以及数组的`length`属性，就通过“可枚举性”，从而避免被`for...in`遍历到。

```javascript
Object.getOwnPropertyDescriptor(Object.prototype, 'toString').enumerable
// false

Object.getOwnPropertyDescriptor([], 'length').enumerable
// false
```

上面代码中，`toString`和`length`属性的`enumerable`都是`false`，因此`for...in`不会遍历到这两个继承自原型的属性。

另外，ES6 规定，所有 Class 的原型的方法都是不可枚举的。

```javascript
Object.getOwnPropertyDescriptor(class {foo() {}}.prototype, 'foo').enumerable
// false
```

总的来说，操作中引入继承的属性会让问题复杂化，大多数时候，我们只关心对象自身的属性。所以，尽量不要用`for...in`循环，而用`Object.keys()`代替。

### 属性的遍历

ES6 一共有 5 种方法可以遍历对象的属性。

**（1）for...in**

`for...in`循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。

**（2）Object.keys(obj)**

`Object.keys`返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。

**（3）Object.getOwnPropertyNames(obj)**

`Object.getOwnPropertyNames`返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。

**（4）Object.getOwnPropertySymbols(obj)**

`Object.getOwnPropertySymbols`返回一个数组，包含对象自身的所有 Symbol 属性的键名。

**（5）Reflect.ownKeys(obj)**

`Reflect.ownKeys`返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。

以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。

- 首先遍历所有数值键，按照数值升序排列。
- 其次遍历所有字符串键，按照加入时间升序排列。
- 最后遍历所有 Symbol 键，按照加入时间升序排列。

```javascript
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
```

上面代码中，`Reflect.ownKeys`方法返回一个数组，包含了参数对象的所有属性。这个数组的属性次序是这样的，首先是数值属性`2`和`10`，其次是字符串属性`b`和`a`，最后是 Symbol 属性。

## super 关键字

我们知道，`this`关键字总是指向函数所在的当前对象，ES6 又新增了另一个类似的关键字`super`，指向当前对象的原型对象。

```javascript
const proto = {
  foo: 'hello'
};

const obj = {
  foo: 'world',
  find() {
    return super.foo;
  }
};

Object.setPrototypeOf(obj, proto);
obj.find() // "hello"
```

上面代码中，对象`obj.find()`方法之中，通过`super.foo`引用了原型对象`proto`的`foo`属性。

注意，`super`关键字表示原型对象时，只能用在对象的方法之中，用在其他地方都会报错。

```javascript
// 报错
const obj = {
  foo: super.foo
}

// 报错
const obj = {
  foo: () => super.foo
}

// 报错
const obj = {
  foo: function () {
    return super.foo
  }
}
```

上面三种`super`的用法都会报错，因为对于 JavaScript 引擎来说，这里的`super`都没有用在对象的方法之中。第一种写法是`super`用在属性里面，第二种和第三种写法是`super`用在一个函数里面，然后赋值给`foo`属性。目前，只有对象方法的简写法可以让 JavaScript 引擎确认，定义的是对象的方法。

JavaScript 引擎内部，`super.foo`等同于`Object.getPrototypeOf(this).foo`（属性）或`Object.getPrototypeOf(this).foo.call(this)`（方法）。

```javascript
const proto = {
  x: 'hello',
  foo() {
    console.log(this.x);
  },
};

const obj = {
  x: 'world',
  foo() {
    super.foo();
  }
}

Object.setPrototypeOf(obj, proto);

obj.foo() // "world"
```

上面代码中，`super.foo`指向原型对象`proto`的`foo`方法，但是绑定的`this`却还是当前对象`obj`，因此输出的就是`world`。

## 对象的扩展运算符

《数组的扩展》一章中，已经介绍过扩展运算符（`...`）。ES2018 将这个运算符[引入](https://github.com/sebmarkbage/ecmascript-rest-spread)了对象。

### 解构赋值

对象的解构赋值用于从一个对象取值，相当于将目标对象自身的所有可遍历的（enumerable）、但尚未被读取的属性，分配到指定的对象上面。所有的键和它们的值，都会拷贝到新对象上面。

```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }
```

上面代码中，变量`z`是解构赋值所在的对象。它获取等号右边的所有尚未读取的键（`a`和`b`），将它们连同值一起拷贝过来。

由于解构赋值要求等号右边是一个对象，所以如果等号右边是`undefined`或`null`，就会报错，因为它们无法转为对象。

```javascript
let { ...z } = null; // 运行时错误
let { ...z } = undefined; // 运行时错误
```

解构赋值必须是最后一个参数，否则会报错。

```javascript
let { ...x, y, z } = someObject; // 句法错误
let { x, ...y, ...z } = someObject; // 句法错误
```

上面代码中，解构赋值不是最后一个参数，所以会报错。

注意，解构赋值的拷贝是浅拷贝，即如果一个键的值是复合类型的值（数组、对象、函数）、那么解构赋值拷贝的是这个值的引用，而不是这个值的副本。

```javascript
let obj = { a: { b: 1 } };
let { ...x } = obj;
obj.a.b = 2;
x.a.b // 2
```

上面代码中，`x`是解构赋值所在的对象，拷贝了对象`obj`的`a`属性。`a`属性引用了一个对象，修改这个对象的值，会影响到解构赋值对它的引用。

另外，扩展运算符的解构赋值，不能复制继承自原型对象的属性。

```javascript
let o1 = { a: 1 };
let o2 = { b: 2 };
o2.__proto__ = o1;
let { ...o3 } = o2;
o3 // { b: 2 }
o3.a // undefined
```

上面代码中，对象`o3`复制了`o2`，但是只复制了`o2`自身的属性，没有复制它的原型对象`o1`的属性。

下面是另一个例子。

```javascript
const o = Object.create({ x: 1, y: 2 });
o.z = 3;

let { x, ...newObj } = o;
let { y, z } = newObj;
x // 1
y // undefined
z // 3
```

上面代码中，变量`x`是单纯的解构赋值，所以可以读取对象`o`继承的属性；变量`y`和`z`是扩展运算符的解构赋值，只能读取对象`o`自身的属性，所以变量`z`可以赋值成功，变量`y`取不到值。ES6 规定，变量声明语句之中，如果使用解构赋值，扩展运算符后面必须是一个变量名，而不能是一个解构赋值表达式，所以上面代码引入了中间变量`newObj`，如果写成下面这样会报错。

```javascript
let { x, ...{ y, z } } = o;
// SyntaxError: ... must be followed by an identifier in declaration contexts
```

解构赋值的一个用处，是扩展某个函数的参数，引入其他操作。

```javascript
function baseFunction({ a, b }) {
  // ...
}
function wrapperFunction({ x, y, ...restConfig }) {
  // 使用 x 和 y 参数进行操作
  // 其余参数传给原始函数
  return baseFunction(restConfig);
}
```

上面代码中，原始函数`baseFunction`接受`a`和`b`作为参数，函数`wrapperFunction`在`baseFunction`的基础上进行了扩展，能够接受多余的参数，并且保留原始函数的行为。

### 扩展运算符

对象的扩展运算符（`...`）用于取出参数对象的所有可遍历属性，拷贝到当前对象之中。

```javascript
let z = { a: 3, b: 4 };
let n = { ...z };
n // { a: 3, b: 4 }
```

由于数组是特殊的对象，所以对象的扩展运算符也可以用于数组。

```javascript
let foo = { ...['a', 'b', 'c'] };
foo
// {0: "a", 1: "b", 2: "c"}
```

如果扩展运算符后面是一个空对象，则没有任何效果。

```javascript
{...{}, a: 1}
// { a: 1 }
```

如果扩展运算符后面不是对象，则会自动将其转为对象。

```javascript
// 等同于 {...Object(1)}
{...1} // {}
```

上面代码中，扩展运算符后面是整数`1`，会自动转为数值的包装对象`Number{1}`。由于该对象没有自身属性，所以返回一个空对象。

下面的例子都是类似的道理。

```javascript
// 等同于 {...Object(true)}
{...true} // {}

// 等同于 {...Object(undefined)}
{...undefined} // {}

// 等同于 {...Object(null)}
{...null} // {}
```

但是，如果扩展运算符后面是字符串，它会自动转成一个类似数组的对象，因此返回的不是空对象。

```javascript
{...'hello'}
// {0: "h", 1: "e", 2: "l", 3: "l", 4: "o"}
```

对象的扩展运算符等同于使用`Object.assign()`方法。

```javascript
let aClone = { ...a };
// 等同于
let aClone = Object.assign({}, a);
```

上面的例子只是拷贝了对象实例的属性，如果想完整克隆一个对象，还拷贝对象原型的属性，可以采用下面的写法。

```javascript
// 写法一
const clone1 = {
  __proto__: Object.getPrototypeOf(obj),
  ...obj
};

// 写法二
const clone2 = Object.assign(
  Object.create(Object.getPrototypeOf(obj)),
  obj
);

// 写法三
const clone3 = Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
)
```

上面代码中，写法一的`__proto__`属性在非浏览器的环境不一定部署，因此推荐使用写法二和写法三。

扩展运算符可以用于合并两个对象。

```javascript
let ab = { ...a, ...b };
// 等同于
let ab = Object.assign({}, a, b);
```

如果用户自定义的属性，放在扩展运算符后面，则扩展运算符内部的同名属性会被覆盖掉。

```javascript
let aWithOverrides = { ...a, x: 1, y: 2 };
// 等同于
let aWithOverrides = { ...a, ...{ x: 1, y: 2 } };
// 等同于
let x = 1, y = 2, aWithOverrides = { ...a, x, y };
// 等同于
let aWithOverrides = Object.assign({}, a, { x: 1, y: 2 });
```

上面代码中，`a`对象的`x`属性和`y`属性，拷贝到新对象后会被覆盖掉。

这用来修改现有对象部分的属性就很方便了。

```javascript
let newVersion = {
  ...previousVersion,
  name: 'New Name' // Override the name property
};
```

上面代码中，`newVersion`对象自定义了`name`属性，其他属性全部复制自`previousVersion`对象。

如果把自定义属性放在扩展运算符前面，就变成了设置新对象的默认属性值。

```javascript
let aWithDefaults = { x: 1, y: 2, ...a };
// 等同于
let aWithDefaults = Object.assign({}, { x: 1, y: 2 }, a);
// 等同于
let aWithDefaults = Object.assign({ x: 1, y: 2 }, a);
```

与数组的扩展运算符一样，对象的扩展运算符后面可以跟表达式。

```javascript
const obj = {
  ...(x > 1 ? {a: 1} : {}),
  b: 2,
};
```

扩展运算符的参数对象之中，如果有取值函数`get`，这个函数是会执行的。

```javascript
let a = {
  get x() {
    throw new Error('not throw yet');
  }
}

let aWithXGetter = { ...a }; // 报错
```

上面例子中，取值函数`get`在扩展`a`对象时会自动执行，导致报错。

## AggregateError 错误对象

ES2021 标准之中，为了配合新增的`Promise.any()`方法（详见《Promise 对象》一章），还引入一个新的错误对象`AggregateError`，也放在这一章介绍。

AggregateError 在一个错误对象里面，封装了多个错误。如果某个单一操作，同时引发了多个错误，需要同时抛出这些错误，那么就可以抛出一个 AggregateError 错误对象，把各种错误都放在这个对象里面。

AggregateError 本身是一个构造函数，用来生成 AggregateError 实例对象。

```javascript
AggregateError(errors[, message])
```

`AggregateError()`构造函数可以接受两个参数。

- errors：数组，它的每个成员都是一个错误对象。该参数是必须的。
- message：字符串，表示 AggregateError 抛出时的提示信息。该参数是可选的。

```javascript
const error = new AggregateError([
  new Error('ERROR_11112'),
  new TypeError('First name must be a string'),
  new RangeError('Transaction value must be at least 1'),
  new URIError('User profile link must be https'),
], 'Transaction cannot be processed')
```

上面示例中，`AggregateError()`的第一个参数数组里面，一共有四个错误实例。第二个参数字符串则是这四个错误的一个整体的提示。

`AggregateError`的实例对象有三个属性。

- name：错误名称，默认为“AggregateError”。
- message：错误的提示信息。
- errors：数组，每个成员都是一个错误对象。

下面是一个示例。

```javascript
try {
  throw new AggregateError([
    new Error("some error"),
  ], 'Hello');
} catch (e) {
  console.log(e instanceof AggregateError); // true
  console.log(e.message);                   // "Hello"
  console.log(e.name);                      // "AggregateError"
  console.log(e.errors);                    // [ Error: "some error" ]
}
```
# 对象的新增方法

本章介绍 Object 对象的新增方法。

## Object.is()

ES5 比较两个值是否相等，只有两个运算符：相等运算符（`==`）和严格相等运算符（`===`）。它们都有缺点，前者会自动转换数据类型，后者的`NaN`不等于自身，以及`+0`等于`-0`。JavaScript 缺乏一种运算，在所有环境中，只要两个值是一样的，它们就应该相等。

ES6 提出“Same-value equality”（同值相等）算法，用来解决这个问题。`Object.is`就是部署这个算法的新方法。它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

```javascript
Object.is('foo', 'foo')
// true
Object.is({}, {})
// false
```

不同之处只有两个：一是`+0`不等于`-0`，二是`NaN`等于自身。

```javascript
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

ES5 可以通过下面的代码，部署`Object.is`。

```javascript
Object.defineProperty(Object, 'is', {
  value: function(x, y) {
    if (x === y) {
      // 针对+0 不等于 -0的情况
      return x !== 0 || 1 / x === 1 / y;
    }
    // 针对NaN的情况
    return x !== x && y !== y;
  },
  configurable: true,
  enumerable: false,
  writable: true
});
```

## Object.assign()

### 基本用法

`Object.assign()`方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

```javascript
const target = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

`Object.assign()`方法的第一个参数是目标对象，后面的参数都是源对象。

注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

```javascript
const target = { a: 1, b: 1 };

const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

如果只有一个参数，`Object.assign()`会直接返回该参数。

```javascript
const obj = {a: 1};
Object.assign(obj) === obj // true
```

如果该参数不是对象，则会先转成对象，然后返回。

```javascript
typeof Object.assign(2) // "object"
```

由于`undefined`和`null`无法转成对象，所以如果它们作为参数，就会报错。

```javascript
Object.assign(undefined) // 报错
Object.assign(null) // 报错
```

如果非对象参数出现在源对象的位置（即非首参数），那么处理规则有所不同。首先，这些参数都会转成对象，如果无法转成对象，就会跳过。这意味着，如果`undefined`和`null`不在首参数，就不会报错。

```javascript
let obj = {a: 1};
Object.assign(obj, undefined) === obj // true
Object.assign(obj, null) === obj // true
```

其他类型的值（即数值、字符串和布尔值）不在首参数，也不会报错。但是，除了字符串会以数组形式，拷贝入目标对象，其他值都不会产生效果。

```javascript
const v1 = 'abc';
const v2 = true;
const v3 = 10;

const obj = Object.assign({}, v1, v2, v3);
console.log(obj); // { "0": "a", "1": "b", "2": "c" }
```

上面代码中，`v1`、`v2`、`v3`分别是字符串、布尔值和数值，结果只有字符串合入目标对象（以字符数组的形式），数值和布尔值都会被忽略。这是因为只有字符串的包装对象，会产生可枚举属性。

```javascript
Object(true) // {[[PrimitiveValue]]: true}
Object(10)  //  {[[PrimitiveValue]]: 10}
Object('abc') // {0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"}
```

上面代码中，布尔值、数值、字符串分别转成对应的包装对象，可以看到它们的原始值都在包装对象的内部属性`[[PrimitiveValue]]`上面，这个属性是不会被`Object.assign()`拷贝的。只有字符串的包装对象，会产生可枚举的实义属性，那些属性则会被拷贝。

`Object.assign()`拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（`enumerable: false`）。

```javascript
Object.assign({b: 'c'},
  Object.defineProperty({}, 'invisible', {
    enumerable: false,
    value: 'hello'
  })
)
// { b: 'c' }
```

上面代码中，`Object.assign()`要拷贝的对象只有一个不可枚举属性`invisible`，这个属性并没有被拷贝进去。

属性名为 Symbol 值的属性，也会被`Object.assign()`拷贝。

```javascript
Object.assign({ a: 'b' }, { [Symbol('c')]: 'd' })
// { a: 'b', Symbol(c): 'd' }
```

### 注意点

**（1）浅拷贝**

`Object.assign()`方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

```javascript
const obj1 = {a: {b: 1}};
const obj2 = Object.assign({}, obj1);

obj1.a.b = 2;
obj2.a.b // 2
```

上面代码中，源对象`obj1`的`a`属性的值是一个对象，`Object.assign()`拷贝得到的是这个对象的引用。这个对象的任何变化，都会反映到目标对象上面。

**（2）同名属性的替换**

对于这种嵌套的对象，一旦遇到同名属性，`Object.assign()`的处理方法是替换，而不是添加。

```javascript
const target = { a: { b: 'c', d: 'e' } }
const source = { a: { b: 'hello' } }
Object.assign(target, source)
// { a: { b: 'hello' } }
```

上面代码中，`target`对象的`a`属性被`source`对象的`a`属性整个替换掉了，而不会得到`{ a: { b: 'hello', d: 'e' } }`的结果。这通常不是开发者想要的，需要特别小心。

一些函数库提供`Object.assign()`的定制版本（比如 Lodash 的`_.defaultsDeep()`方法），可以得到深拷贝的合并。

**（3）数组的处理**

`Object.assign()`可以用来处理数组，但是会把数组视为对象。

```javascript
Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]
```

上面代码中，`Object.assign()`把数组视为属性名为 0、1、2 的对象，因此源数组的 0 号属性`4`覆盖了目标数组的 0 号属性`1`。

**（4）取值函数的处理**

`Object.assign()`只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。

```javascript
const source = {
  get foo() { return 1 }
};
const target = {};

Object.assign(target, source)
// { foo: 1 }
```

上面代码中，`source`对象的`foo`属性是一个取值函数，`Object.assign()`不会复制这个取值函数，只会拿到值以后，将这个值复制过去。

### 常见用途

`Object.assign()`方法有很多用处。

**（1）为对象添加属性**

```javascript
class Point {
  constructor(x, y) {
    Object.assign(this, {x, y});
  }
}
```

上面方法通过`Object.assign()`方法，将`x`属性和`y`属性添加到`Point`类的对象实例。

**（2）为对象添加方法**

```javascript
Object.assign(SomeClass.prototype, {
  someMethod(arg1, arg2) {
    ···
  },
  anotherMethod() {
    ···
  }
});

// 等同于下面的写法
SomeClass.prototype.someMethod = function (arg1, arg2) {
  ···
};
SomeClass.prototype.anotherMethod = function () {
  ···
};
```

上面代码使用了对象属性的简洁表示法，直接将两个函数放在大括号中，再使用`assign()`方法添加到`SomeClass.prototype`之中。

**（3）克隆对象**

```javascript
function clone(origin) {
  return Object.assign({}, origin);
}
```

上面代码将原始对象拷贝到一个空对象，就得到了原始对象的克隆。

不过，采用这种方法克隆，只能克隆原始对象自身的值，不能克隆它继承的值。如果想要保持继承链，可以采用下面的代码。

```javascript
function clone(origin) {
  let originProto = Object.getPrototypeOf(origin);
  return Object.assign(Object.create(originProto), origin);
}
```

**（4）合并多个对象**

将多个对象合并到某个对象。

```javascript
const merge =
  (target, ...sources) => Object.assign(target, ...sources);
```

如果希望合并后返回一个新对象，可以改写上面函数，对一个空对象合并。

```javascript
const merge =
  (...sources) => Object.assign({}, ...sources);
```

**（5）为属性指定默认值**

```javascript
const DEFAULTS = {
  logLevel: 0,
  outputFormat: 'html'
};

function processContent(options) {
  options = Object.assign({}, DEFAULTS, options);
  console.log(options);
  // ...
}
```

上面代码中，`DEFAULTS`对象是默认值，`options`对象是用户提供的参数。`Object.assign()`方法将`DEFAULTS`和`options`合并成一个新对象，如果两者有同名属性，则`options`的属性值会覆盖`DEFAULTS`的属性值。

注意，由于存在浅拷贝的问题，`DEFAULTS`对象和`options`对象的所有属性的值，最好都是简单类型，不要指向另一个对象。否则，`DEFAULTS`对象的该属性很可能不起作用。

```javascript
const DEFAULTS = {
  url: {
    host: 'example.com',
    port: 7070
  },
};

processContent({ url: {port: 8000} })
// {
//   url: {port: 8000}
// }
```

上面代码的原意是将`url.port`改成 8000，`url.host`不变。实际结果却是`options.url`覆盖掉`DEFAULTS.url`，所以`url.host`就不存在了。

## Object.getOwnPropertyDescriptors()

ES5 的`Object.getOwnPropertyDescriptor()`方法会返回某个对象属性的描述对象（descriptor）。ES2017 引入了`Object.getOwnPropertyDescriptors()`方法，返回指定对象所有自身属性（非继承属性）的描述对象。

```javascript
const obj = {
  foo: 123,
  get bar() { return 'abc' }
};

Object.getOwnPropertyDescriptors(obj)
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: get bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } }
```

上面代码中，`Object.getOwnPropertyDescriptors()`方法返回一个对象，所有原对象的属性名都是该对象的属性名，对应的属性值就是该属性的描述对象。

该方法的实现非常容易。

```javascript
function getOwnPropertyDescriptors(obj) {
  const result = {};
  for (let key of Reflect.ownKeys(obj)) {
    result[key] = Object.getOwnPropertyDescriptor(obj, key);
  }
  return result;
}
```

该方法的引入目的，主要是为了解决`Object.assign()`无法正确拷贝`get`属性和`set`属性的问题。

```javascript
const source = {
  set foo(value) {
    console.log(value);
  }
};

const target1 = {};
Object.assign(target1, source);

Object.getOwnPropertyDescriptor(target1, 'foo')
// { value: undefined,
//   writable: true,
//   enumerable: true,
//   configurable: true }
```

上面代码中，`source`对象的`foo`属性的值是一个赋值函数，`Object.assign`方法将这个属性拷贝给`target1`对象，结果该属性的值变成了`undefined`。这是因为`Object.assign`方法总是拷贝一个属性的值，而不会拷贝它背后的赋值方法或取值方法。

这时，`Object.getOwnPropertyDescriptors()`方法配合`Object.defineProperties()`方法，就可以实现正确拷贝。

```javascript
const source = {
  set foo(value) {
    console.log(value);
  }
};

const target2 = {};
Object.defineProperties(target2, Object.getOwnPropertyDescriptors(source));
Object.getOwnPropertyDescriptor(target2, 'foo')
// { get: undefined,
//   set: [Function: set foo],
//   enumerable: true,
//   configurable: true }
```

上面代码中，两个对象合并的逻辑可以写成一个函数。

```javascript
const shallowMerge = (target, source) => Object.defineProperties(
  target,
  Object.getOwnPropertyDescriptors(source)
);
```

`Object.getOwnPropertyDescriptors()`方法的另一个用处，是配合`Object.create()`方法，将对象属性克隆到一个新对象。这属于浅拷贝。

```javascript
const clone = Object.create(Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj));

// 或者

const shallowClone = (obj) => Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
);
```

上面代码会克隆对象`obj`。

另外，`Object.getOwnPropertyDescriptors()`方法可以实现一个对象继承另一个对象。以前，继承另一个对象，常常写成下面这样。

```javascript
const obj = {
  __proto__: prot,
  foo: 123,
};
```

ES6 规定`__proto__`只有浏览器要部署，其他环境不用部署。如果去除`__proto__`，上面代码就要改成下面这样。

```javascript
const obj = Object.create(prot);
obj.foo = 123;

// 或者

const obj = Object.assign(
  Object.create(prot),
  {
    foo: 123,
  }
);
```

有了`Object.getOwnPropertyDescriptors()`，我们就有了另一种写法。

```javascript
const obj = Object.create(
  prot,
  Object.getOwnPropertyDescriptors({
    foo: 123,
  })
);
```

`Object.getOwnPropertyDescriptors()`也可以用来实现 Mixin（混入）模式。

```javascript
let mix = (object) => ({
  with: (...mixins) => mixins.reduce(
    (c, mixin) => Object.create(
      c, Object.getOwnPropertyDescriptors(mixin)
    ), object)
});

// multiple mixins example
let a = {a: 'a'};
let b = {b: 'b'};
let c = {c: 'c'};
let d = mix(c).with(a, b);

d.c // "c"
d.b // "b"
d.a // "a"
```

上面代码返回一个新的对象`d`，代表了对象`a`和`b`被混入了对象`c`的操作。

出于完整性的考虑，`Object.getOwnPropertyDescriptors()`进入标准以后，以后还会新增`Reflect.getOwnPropertyDescriptors()`方法。

## `__proto__`属性，Object.setPrototypeOf()，Object.getPrototypeOf()

JavaScript 语言的对象继承是通过原型链实现的。ES6 提供了更多原型对象的操作方法。

### `__proto__`属性

`__proto__`属性（前后各两个下划线），用来读取或设置当前对象的原型对象（prototype）。目前，所有浏览器（包括 IE11）都部署了这个属性。

```javascript
// es5 的写法
const obj = {
  method: function() { ... }
};
obj.__proto__ = someOtherObj;

// es6 的写法
var obj = Object.create(someOtherObj);
obj.method = function() { ... };
```

该属性没有写入 ES6 的正文，而是写入了附录，原因是`__proto__`前后的双下划线，说明它本质上是一个内部属性，而不是一个正式的对外的 API，只是由于浏览器广泛支持，才被加入了 ES6。标准明确规定，只有浏览器必须部署这个属性，其他运行环境不一定需要部署，而且新的代码最好认为这个属性是不存在的。因此，无论从语义的角度，还是从兼容性的角度，都不要使用这个属性，而是使用下面的`Object.setPrototypeOf()`（写操作）、`Object.getPrototypeOf()`（读操作）、`Object.create()`（生成操作）代替。

实现上，`__proto__`调用的是`Object.prototype.__proto__`，具体实现如下。

```javascript
Object.defineProperty(Object.prototype, '__proto__', {
  get() {
    let _thisObj = Object(this);
    return Object.getPrototypeOf(_thisObj);
  },
  set(proto) {
    if (this === undefined || this === null) {
      throw new TypeError();
    }
    if (!isObject(this)) {
      return undefined;
    }
    if (!isObject(proto)) {
      return undefined;
    }
    let status = Reflect.setPrototypeOf(this, proto);
    if (!status) {
      throw new TypeError();
    }
  },
});

function isObject(value) {
  return Object(value) === value;
}
```

如果一个对象本身部署了`__proto__`属性，该属性的值就是对象的原型。

```javascript
Object.getPrototypeOf({ __proto__: null })
// null
```

### Object.setPrototypeOf()

`Object.setPrototypeOf`方法的作用与`__proto__`相同，用来设置一个对象的原型对象（prototype），返回参数对象本身。它是 ES6 正式推荐的设置原型对象的方法。

```javascript
// 格式
Object.setPrototypeOf(object, prototype)

// 用法
const o = Object.setPrototypeOf({}, null);
```

该方法等同于下面的函数。

```javascript
function setPrototypeOf(obj, proto) {
  obj.__proto__ = proto;
  return obj;
}
```

下面是一个例子。

```javascript
let proto = {};
let obj = { x: 10 };
Object.setPrototypeOf(obj, proto);

proto.y = 20;
proto.z = 40;

obj.x // 10
obj.y // 20
obj.z // 40
```

上面代码将`proto`对象设为`obj`对象的原型，所以从`obj`对象可以读取`proto`对象的属性。

如果第一个参数不是对象，会自动转为对象。但是由于返回的还是第一个参数，所以这个操作不会产生任何效果。

```javascript
Object.setPrototypeOf(1, {}) === 1 // true
Object.setPrototypeOf('foo', {}) === 'foo' // true
Object.setPrototypeOf(true, {}) === true // true
```

由于`undefined`和`null`无法转为对象，所以如果第一个参数是`undefined`或`null`，就会报错。

```javascript
Object.setPrototypeOf(undefined, {})
// TypeError: Object.setPrototypeOf called on null or undefined

Object.setPrototypeOf(null, {})
// TypeError: Object.setPrototypeOf called on null or undefined
```

### Object.getPrototypeOf()

该方法与`Object.setPrototypeOf`方法配套，用于读取一个对象的原型对象。

```javascript
Object.getPrototypeOf(obj);
```

下面是一个例子。

```javascript
function Rectangle() {
  // ...
}

const rec = new Rectangle();

Object.getPrototypeOf(rec) === Rectangle.prototype
// true

Object.setPrototypeOf(rec, Object.prototype);
Object.getPrototypeOf(rec) === Rectangle.prototype
// false
```

如果参数不是对象，会被自动转为对象。

```javascript
// 等同于 Object.getPrototypeOf(Number(1))
Object.getPrototypeOf(1)
// Number {[[PrimitiveValue]]: 0}

// 等同于 Object.getPrototypeOf(String('foo'))
Object.getPrototypeOf('foo')
// String {length: 0, [[PrimitiveValue]]: ""}

// 等同于 Object.getPrototypeOf(Boolean(true))
Object.getPrototypeOf(true)
// Boolean {[[PrimitiveValue]]: false}

Object.getPrototypeOf(1) === Number.prototype // true
Object.getPrototypeOf('foo') === String.prototype // true
Object.getPrototypeOf(true) === Boolean.prototype // true
```

如果参数是`undefined`或`null`，它们无法转为对象，所以会报错。

```javascript
Object.getPrototypeOf(null)
// TypeError: Cannot convert undefined or null to object

Object.getPrototypeOf(undefined)
// TypeError: Cannot convert undefined or null to object
```

## Object.keys()，Object.values()，Object.entries()

### Object.keys()

ES5 引入了`Object.keys`方法，返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。

```javascript
var obj = { foo: 'bar', baz: 42 };
Object.keys(obj)
// ["foo", "baz"]
```

ES2017 [引入](https://github.com/tc39/proposal-object-values-entries)了跟`Object.keys`配套的`Object.values`和`Object.entries`，作为遍历一个对象的补充手段，供`for...of`循环使用。

```javascript
let {keys, values, entries} = Object;
let obj = { a: 1, b: 2, c: 3 };

for (let key of keys(obj)) {
  console.log(key); // 'a', 'b', 'c'
}

for (let value of values(obj)) {
  console.log(value); // 1, 2, 3
}

for (let [key, value] of entries(obj)) {
  console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
}
```

### Object.values()

`Object.values`方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。

```javascript
const obj = { foo: 'bar', baz: 42 };
Object.values(obj)
// ["bar", 42]
```

返回数组的成员顺序，与本章的《属性的遍历》部分介绍的排列规则一致。

```javascript
const obj = { 100: 'a', 2: 'b', 7: 'c' };
Object.values(obj)
// ["b", "c", "a"]
```

上面代码中，属性名为数值的属性，是按照数值大小，从小到大遍历的，因此返回的顺序是`b`、`c`、`a`。

`Object.values`只返回对象自身的可遍历属性。

```javascript
const obj = Object.create({}, {p: {value: 42}});
Object.values(obj) // []
```

上面代码中，`Object.create`方法的第二个参数添加的对象属性（属性`p`），如果不显式声明，默认是不可遍历的，因为`p`的属性描述对象的`enumerable`默认是`false`，`Object.values`不会返回这个属性。只要把`enumerable`改成`true`，`Object.values`就会返回属性`p`的值。

```javascript
const obj = Object.create({}, {p:
  {
    value: 42,
    enumerable: true
  }
});
Object.values(obj) // [42]
```

`Object.values`会过滤属性名为 Symbol 值的属性。

```javascript
Object.values({ [Symbol()]: 123, foo: 'abc' });
// ['abc']
```

如果`Object.values`方法的参数是一个字符串，会返回各个字符组成的一个数组。

```javascript
Object.values('foo')
// ['f', 'o', 'o']
```

上面代码中，字符串会先转成一个类似数组的对象。字符串的每个字符，就是该对象的一个属性。因此，`Object.values`返回每个属性的键值，就是各个字符组成的一个数组。

如果参数不是对象，`Object.values`会先将其转为对象。由于数值和布尔值的包装对象，都不会为实例添加非继承的属性。所以，`Object.values`会返回空数组。

```javascript
Object.values(42) // []
Object.values(true) // []
```

### Object.entries()

`Object.entries()`方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。

```javascript
const obj = { foo: 'bar', baz: 42 };
Object.entries(obj)
// [ ["foo", "bar"], ["baz", 42] ]
```

除了返回值不一样，该方法的行为与`Object.values`基本一致。

如果原对象的属性名是一个 Symbol 值，该属性会被忽略。

```javascript
Object.entries({ [Symbol()]: 123, foo: 'abc' });
// [ [ 'foo', 'abc' ] ]
```

上面代码中，原对象有两个属性，`Object.entries`只输出属性名非 Symbol 值的属性。将来可能会有`Reflect.ownEntries()`方法，返回对象自身的所有属性。

`Object.entries`的基本用途是遍历对象的属性。

```javascript
let obj = { one: 1, two: 2 };
for (let [k, v] of Object.entries(obj)) {
  console.log(
    `${JSON.stringify(k)}: ${JSON.stringify(v)}`
  );
}
// "one": 1
// "two": 2
```

`Object.entries`方法的另一个用处是，将对象转为真正的`Map`结构。

```javascript
const obj = { foo: 'bar', baz: 42 };
const map = new Map(Object.entries(obj));
map // Map { foo: "bar", baz: 42 }
```

自己实现`Object.entries`方法，非常简单。

```javascript
// Generator函数的版本
function* entries(obj) {
  for (let key of Object.keys(obj)) {
    yield [key, obj[key]];
  }
}

// 非Generator函数的版本
function entries(obj) {
  let arr = [];
  for (let key of Object.keys(obj)) {
    arr.push([key, obj[key]]);
  }
  return arr;
}
```

## Object.fromEntries()

`Object.fromEntries()`方法是`Object.entries()`的逆操作，用于将一个键值对数组转为对象。

```javascript
Object.fromEntries([
  ['foo', 'bar'],
  ['baz', 42]
])
// { foo: "bar", baz: 42 }
```

该方法的主要目的，是将键值对的数据结构还原为对象，因此特别适合将 Map 结构转为对象。

```javascript
// 例一
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);

Object.fromEntries(entries)
// { foo: "bar", baz: 42 }

// 例二
const map = new Map().set('foo', true).set('bar', false);
Object.fromEntries(map)
// { foo: true, bar: false }
```

该方法的一个用处是配合`URLSearchParams`对象，将查询字符串转为对象。

```javascript
Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'))
// { foo: "bar", baz: "qux" }
```
# 运算符的扩展

本章介绍 ES6 后续标准添加的一些运算符。

## 指数运算符

ES2016 新增了一个指数运算符（`**`）。

```javascript
2 ** 2 // 4
2 ** 3 // 8
```

这个运算符的一个特点是右结合，而不是常见的左结合。多个指数运算符连用时，是从最右边开始计算的。

```javascript
// 相当于 2 ** (3 ** 2)
2 ** 3 ** 2
// 512
```

上面代码中，首先计算的是第二个指数运算符，而不是第一个。

指数运算符可以与等号结合，形成一个新的赋值运算符（`**=`）。

```javascript
let a = 1.5;
a **= 2;
// 等同于 a = a * a;

let b = 4;
b **= 3;
// 等同于 b = b * b * b;
```

## 链判断运算符

编程实务中，如果读取对象内部的某个属性，往往需要判断一下，属性的上层对象是否存在。比如，读取`message.body.user.firstName`这个属性，安全的写法是写成下面这样。

```javascript
// 错误的写法
const  firstName = message.body.user.firstName || 'default';

// 正确的写法
const firstName = (message
  && message.body
  && message.body.user
  && message.body.user.firstName) || 'default';
```

上面例子中，`firstName`属性在对象的第四层，所以需要判断四次，每一层是否有值。

三元运算符`?:`也常用于判断对象是否存在。

```javascript
const fooInput = myForm.querySelector('input[name=foo]')
const fooValue = fooInput ? fooInput.value : undefined
```

上面例子中，必须先判断`fooInput`是否存在，才能读取`fooInput.value`。

这样的层层判断非常麻烦，因此 [ES2020](https://github.com/tc39/proposal-optional-chaining) 引入了“链判断运算符”（optional chaining operator）`?.`，简化上面的写法。

```javascript
const firstName = message?.body?.user?.firstName || 'default';
const fooValue = myForm.querySelector('input[name=foo]')?.value
```

上面代码使用了`?.`运算符，直接在链式调用的时候判断，左侧的对象是否为`null`或`undefined`。如果是的，就不再往下运算，而是返回`undefined`。

下面是判断对象方法是否存在，如果存在就立即执行的例子。

```javascript
iterator.return?.()
```

上面代码中，`iterator.return`如果有定义，就会调用该方法，否则`iterator.return`直接返回`undefined`，不再执行`?.`后面的部分。

对于那些可能没有实现的方法，这个运算符尤其有用。

```javascript
if (myForm.checkValidity?.() === false) {
  // 表单校验失败
  return;
}
```

上面代码中，老式浏览器的表单对象可能没有`checkValidity()`这个方法，这时`?.`运算符就会返回`undefined`，判断语句就变成了`undefined === false`，所以就会跳过下面的代码。

链判断运算符`?.`有三种写法。

- `obj?.prop` // 对象属性是否存在
- `obj?.[expr]` // 同上
- `func?.(...args)` // 函数或对象方法是否存在

下面是`obj?.[expr]`用法的一个例子。

```bash
let hex = "#C0FFEE".match(/#([A-Z]+)/i)?.[1];
```

上面例子中，字符串的`match()`方法，如果没有发现匹配会返回`null`，如果发现匹配会返回一个数组，`?.`运算符起到了判断作用。

下面是`?.`运算符常见形式，以及不使用该运算符时的等价形式。

```javascript
a?.b
// 等同于
a == null ? undefined : a.b

a?.[x]
// 等同于
a == null ? undefined : a[x]

a?.b()
// 等同于
a == null ? undefined : a.b()

a?.()
// 等同于
a == null ? undefined : a()
```

上面代码中，特别注意后两种形式，如果`a?.b()`和`a?.()`。如果`a?.b()`里面的`a.b`有值，但不是函数，不可调用，那么`a?.b()`是会报错的。`a?.()`也是如此，如果`a`不是`null`或`undefined`，但也不是函数，那么`a?.()`会报错。

使用这个运算符，有几个注意点。

（1）短路机制

本质上，`?.`运算符相当于一种短路机制，只要不满足条件，就不再往下执行。

```javascript
a?.[++x]
// 等同于
a == null ? undefined : a[++x]
```

上面代码中，如果`a`是`undefined`或`null`，那么`x`不会进行递增运算。也就是说，链判断运算符一旦为真，右侧的表达式就不再求值。

（2）括号的影响

如果属性链有圆括号，链判断运算符对圆括号外部没有影响，只对圆括号内部有影响。

```javascript
(a?.b).c
// 等价于
(a == null ? undefined : a.b).c
```

上面代码中，`?.`对圆括号外部没有影响，不管`a`对象是否存在，圆括号后面的`.c`总是会执行。

一般来说，使用`?.`运算符的场合，不应该使用圆括号。

（3）报错场合

以下写法是禁止的，会报错。

```javascript
// 构造函数
new a?.()
new a?.b()

// 链判断运算符的右侧有模板字符串
a?.`{b}`
a?.b`{c}`

// 链判断运算符的左侧是 super
super?.()
super?.foo

// 链运算符用于赋值运算符左侧
a?.b = c
```

（4）右侧不得为十进制数值

为了保证兼容以前的代码，允许`foo?.3:0`被解析成`foo ? .3 : 0`，因此规定如果`?.`后面紧跟一个十进制数字，那么`?.`不再被看成是一个完整的运算符，而会按照三元运算符进行处理，也就是说，那个小数点会归属于后面的十进制数字，形成一个小数。

## Null 判断运算符

读取对象属性的时候，如果某个属性的值是`null`或`undefined`，有时候需要为它们指定默认值。常见做法是通过`||`运算符指定默认值。

```javascript
const headerText = response.settings.headerText || 'Hello, world!';
const animationDuration = response.settings.animationDuration || 300;
const showSplashScreen = response.settings.showSplashScreen || true;
```

上面的三行代码都通过`||`运算符指定默认值，但是这样写是错的。开发者的原意是，只要属性的值为`null`或`undefined`，默认值就会生效，但是属性的值如果为空字符串或`false`或`0`，默认值也会生效。

为了避免这种情况，[ES2020](https://github.com/tc39/proposal-nullish-coalescing) 引入了一个新的 Null 判断运算符`??`。它的行为类似`||`，但是只有运算符左侧的值为`null`或`undefined`时，才会返回右侧的值。

```javascript
const headerText = response.settings.headerText ?? 'Hello, world!';
const animationDuration = response.settings.animationDuration ?? 300;
const showSplashScreen = response.settings.showSplashScreen ?? true;
```

上面代码中，默认值只有在左侧属性值为`null`或`undefined`时，才会生效。

这个运算符的一个目的，就是跟链判断运算符`?.`配合使用，为`null`或`undefined`的值设置默认值。

```javascript
const animationDuration = response.settings?.animationDuration ?? 300;
```

上面代码中，如果`response.settings`是`null`或`undefined`，或者`response.settings.animationDuration`是`null`或`undefined`，就会返回默认值300。也就是说，这一行代码包括了两级属性的判断。

这个运算符很适合判断函数参数是否赋值。

```javascript
function Component(props) {
  const enable = props.enabled ?? true;
  // …
}
```

上面代码判断`props`参数的`enabled`属性是否赋值，基本等同于下面的写法。

```javascript
function Component(props) {
  const {
    enabled: enable = true,
  } = props;
  // …
}
```

`??`本质上是逻辑运算，它与其他两个逻辑运算符`&&`和`||`有一个优先级问题，它们之间的优先级到底孰高孰低。优先级的不同，往往会导致逻辑运算的结果不同。

现在的规则是，如果多个逻辑运算符一起使用，必须用括号表明优先级，否则会报错。

```javascript
// 报错
lhs && middle ?? rhs
lhs ?? middle && rhs
lhs || middle ?? rhs
lhs ?? middle || rhs
```

上面四个表达式都会报错，必须加入表明优先级的括号。

```javascript
(lhs && middle) ?? rhs;
lhs && (middle ?? rhs);

(lhs ?? middle) && rhs;
lhs ?? (middle && rhs);

(lhs || middle) ?? rhs;
lhs || (middle ?? rhs);

(lhs ?? middle) || rhs;
lhs ?? (middle || rhs);
```

## 逻辑赋值运算符

ES2021 引入了三个新的[逻辑赋值运算符](https://github.com/tc39/proposal-logical-assignment)（logical assignment operators），将逻辑运算符与赋值运算符进行结合。

```javascript
// 或赋值运算符
x ||= y
// 等同于
x || (x = y)

// 与赋值运算符
x &&= y
// 等同于
x && (x = y)

// Null 赋值运算符
x ??= y
// 等同于
x ?? (x = y)
```

这三个运算符`||=`、`&&=`、`??=`相当于先进行逻辑运算，然后根据运算结果，再视情况进行赋值运算。

它们的一个用途是，为变量或属性设置默认值。

```javascript
// 老的写法
user.id = user.id || 1;

// 新的写法
user.id ||= 1;
```

上面示例中，`user.id`属性如果不存在，则设为`1`，新的写法比老的写法更紧凑一些。

下面是另一个例子。

```javascript
function example(opts) {
  opts.foo = opts.foo ?? 'bar';
  opts.baz ?? (opts.baz = 'qux');
}
```

上面示例中，参数对象`opts`如果不存在属性`foo`和属性`baz`，则为这两个属性设置默认值。有了“Null 赋值运算符”以后，就可以统一写成下面这样。

```javascript
function example(opts) {
  opts.foo ??= 'bar';
  opts.baz ??= 'qux';
}
```
# Symbol

## 概述

ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入`Symbol`的原因。

ES6 引入了一种新的原始数据类型`Symbol`，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：`undefined`、`null`、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

Symbol 值通过`Symbol`函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

```javascript
let s = Symbol();

typeof s
// "symbol"
```

上面代码中，变量`s`就是一个独一无二的值。`typeof`运算符的结果，表明变量`s`是 Symbol 数据类型，而不是字符串之类的其他类型。

注意，`Symbol`函数前不能使用`new`命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。

`Symbol`函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

```javascript
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```

上面代码中，`s1`和`s2`是两个 Symbol 值。如果不加参数，它们在控制台的输出都是`Symbol()`，不利于区分。有了参数以后，就等于为它们加上了描述，输出的时候就能够分清，到底是哪一个值。

如果 Symbol 的参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后才生成一个 Symbol 值。

```javascript
const obj = {
  toString() {
    return 'abc';
  }
};
const sym = Symbol(obj);
sym // Symbol(abc)
```

注意，`Symbol`函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的`Symbol`函数的返回值是不相等的。

```javascript
// 没有参数的情况
let s1 = Symbol();
let s2 = Symbol();

s1 === s2 // false

// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');

s1 === s2 // false
```

上面代码中，`s1`和`s2`都是`Symbol`函数的返回值，而且参数相同，但是它们是不相等的。

Symbol 值不能与其他类型的值进行运算，会报错。

```javascript
let sym = Symbol('My symbol');

"your symbol is " + sym
// TypeError: can't convert symbol to string
`your symbol is ${sym}`
// TypeError: can't convert symbol to string
```

但是，Symbol 值可以显式转为字符串。

```javascript
let sym = Symbol('My symbol');

String(sym) // 'Symbol(My symbol)'
sym.toString() // 'Symbol(My symbol)'
```

另外，Symbol 值也可以转为布尔值，但是不能转为数值。

```javascript
let sym = Symbol();
Boolean(sym) // true
!sym  // false

if (sym) {
  // ...
}

Number(sym) // TypeError
sym + 2 // TypeError
```

## Symbol.prototype.description

创建 Symbol 的时候，可以添加一个描述。

```javascript
const sym = Symbol('foo');
```

上面代码中，`sym`的描述就是字符串`foo`。

但是，读取这个描述需要将 Symbol 显式转为字符串，即下面的写法。

```javascript
const sym = Symbol('foo');

String(sym) // "Symbol(foo)"
sym.toString() // "Symbol(foo)"
```

上面的用法不是很方便。[ES2019](https://github.com/tc39/proposal-Symbol-description) 提供了一个实例属性`description`，直接返回 Symbol 的描述。

```javascript
const sym = Symbol('foo');

sym.description // "foo"
```

## 作为属性名的 Symbol

由于每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。这对于一个对象由多个模块构成的情况非常有用，能防止某一个键被不小心改写或覆盖。

```javascript
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

上面代码通过方括号结构和`Object.defineProperty`，将对象的属性名指定为一个 Symbol 值。

注意，Symbol 值作为对象属性名时，不能用点运算符。

```javascript
const mySymbol = Symbol();
const a = {};

a.mySymbol = 'Hello!';
a[mySymbol] // undefined
a['mySymbol'] // "Hello!"
```

上面代码中，因为点运算符后面总是字符串，所以不会读取`mySymbol`作为标识名所指代的那个值，导致`a`的属性名实际上是一个字符串，而不是一个 Symbol 值。

同理，在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。

```javascript
let s = Symbol();

let obj = {
  [s]: function (arg) { ... }
};

obj[s](123);
```

上面代码中，如果`s`不放在方括号中，该属性的键名就是字符串`s`，而不是`s`所代表的那个 Symbol 值。

采用增强的对象写法，上面代码的`obj`对象可以写得更简洁一些。

```javascript
let obj = {
  [s](arg) { ... }
};
```

Symbol 类型还可以用于定义一组常量，保证这组常量的值都是不相等的。

```javascript
const log = {};

log.levels = {
  DEBUG: Symbol('debug'),
  INFO: Symbol('info'),
  WARN: Symbol('warn')
};
console.log(log.levels.DEBUG, 'debug message');
console.log(log.levels.INFO, 'info message');
```

下面是另外一个例子。

```javascript
const COLOR_RED    = Symbol();
const COLOR_GREEN  = Symbol();

function getComplement(color) {
  switch (color) {
    case COLOR_RED:
      return COLOR_GREEN;
    case COLOR_GREEN:
      return COLOR_RED;
    default:
      throw new Error('Undefined color');
    }
}
```

常量使用 Symbol 值最大的好处，就是其他任何值都不可能有相同的值了，因此可以保证上面的`switch`语句会按设计的方式工作。

还有一点需要注意，Symbol 值作为属性名时，该属性还是公开属性，不是私有属性。

## 实例：消除魔术字符串

魔术字符串指的是，在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。风格良好的代码，应该尽量消除魔术字符串，改由含义清晰的变量代替。

```javascript
function getArea(shape, options) {
  let area = 0;

  switch (shape) {
    case 'Triangle': // 魔术字符串
      area = .5 * options.width * options.height;
      break;
    /* ... more code ... */
  }

  return area;
}

getArea('Triangle', { width: 100, height: 100 }); // 魔术字符串
```

上面代码中，字符串`Triangle`就是一个魔术字符串。它多次出现，与代码形成“强耦合”，不利于将来的修改和维护。

常用的消除魔术字符串的方法，就是把它写成一个变量。

```javascript
const shapeType = {
  triangle: 'Triangle'
};

function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });
```

上面代码中，我们把`Triangle`写成`shapeType`对象的`triangle`属性，这样就消除了强耦合。

如果仔细分析，可以发现`shapeType.triangle`等于哪个值并不重要，只要确保不会跟其他`shapeType`属性的值冲突即可。因此，这里就很适合改用 Symbol 值。

```javascript
const shapeType = {
  triangle: Symbol()
};
```

上面代码中，除了将`shapeType.triangle`的值设为一个 Symbol，其他地方都不用修改。

## 属性名的遍历

Symbol 作为属性名，遍历对象的时候，该属性不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。

但是，它也不是私有属性，有一个`Object.getOwnPropertySymbols()`方法，可以获取指定对象的所有 Symbol 属性名。该方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。

```javascript
const obj = {};
let a = Symbol('a');
let b = Symbol('b');

obj[a] = 'Hello';
obj[b] = 'World';

const objectSymbols = Object.getOwnPropertySymbols(obj);

objectSymbols
// [Symbol(a), Symbol(b)]
```

上面代码是`Object.getOwnPropertySymbols()`方法的示例，可以获取所有 Symbol 属性名。

下面是另一个例子，`Object.getOwnPropertySymbols()`方法与`for...in`循环、`Object.getOwnPropertyNames`方法进行对比的例子。

```javascript
const obj = {};
const foo = Symbol('foo');

obj[foo] = 'bar';

for (let i in obj) {
  console.log(i); // 无输出
}

Object.getOwnPropertyNames(obj) // []
Object.getOwnPropertySymbols(obj) // [Symbol(foo)]
```

上面代码中，使用`for...in`循环和`Object.getOwnPropertyNames()`方法都得不到 Symbol 键名，需要使用`Object.getOwnPropertySymbols()`方法。

另一个新的 API，`Reflect.ownKeys()`方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

```javascript
let obj = {
  [Symbol('my_key')]: 1,
  enum: 2,
  nonEnum: 3
};

Reflect.ownKeys(obj)
//  ["enum", "nonEnum", Symbol(my_key)]
```

由于以 Symbol 值作为键名，不会被常规方法遍历得到。我们可以利用这个特性，为对象定义一些非私有的、但又希望只用于内部的方法。

```javascript
let size = Symbol('size');

class Collection {
  constructor() {
    this[size] = 0;
  }

  add(item) {
    this[this[size]] = item;
    this[size]++;
  }

  static sizeOf(instance) {
    return instance[size];
  }
}

let x = new Collection();
Collection.sizeOf(x) // 0

x.add('foo');
Collection.sizeOf(x) // 1

Object.keys(x) // ['0']
Object.getOwnPropertyNames(x) // ['0']
Object.getOwnPropertySymbols(x) // [Symbol(size)]
```

上面代码中，对象`x`的`size`属性是一个 Symbol 值，所以`Object.keys(x)`、`Object.getOwnPropertyNames(x)`都无法获取它。这就造成了一种非私有的内部方法的效果。

## Symbol.for()，Symbol.keyFor()

有时，我们希望重新使用同一个 Symbol 值，`Symbol.for()`方法可以做到这一点。它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。

```javascript
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true
```

上面代码中，`s1`和`s2`都是 Symbol 值，但是它们都是由同样参数的`Symbol.for`方法生成的，所以实际上是同一个值。

`Symbol.for()`与`Symbol()`这两种写法，都会生成新的 Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。`Symbol.for()`不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的`key`是否已经存在，如果不存在才会新建一个值。比如，如果你调用`Symbol.for("cat")`30 次，每次都会返回同一个 Symbol 值，但是调用`Symbol("cat")`30 次，会返回 30 个不同的 Symbol 值。

```javascript
Symbol.for("bar") === Symbol.for("bar")
// true

Symbol("bar") === Symbol("bar")
// false
```

上面代码中，由于`Symbol()`写法没有登记机制，所以每次调用都会返回一个不同的值。

`Symbol.keyFor()`方法返回一个已登记的 Symbol 类型值的`key`。

```javascript
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

上面代码中，变量`s2`属于未登记的 Symbol 值，所以返回`undefined`。

注意，`Symbol.for()`为 Symbol 值登记的名字，是全局环境的，不管有没有在全局环境运行。

```javascript
function foo() {
  return Symbol.for('bar');
}

const x = foo();
const y = Symbol.for('bar');
console.log(x === y); // true
```

上面代码中，`Symbol.for('bar')`是函数内部运行的，但是生成的 Symbol 值是登记在全局环境的。所以，第二次运行`Symbol.for('bar')`可以取到这个 Symbol 值。

`Symbol.for()`的这个全局登记特性，可以用在不同的 iframe 或 service worker 中取到同一个值。

```javascript
iframe = document.createElement('iframe');
iframe.src = String(window.location);
document.body.appendChild(iframe);

iframe.contentWindow.Symbol.for('foo') === Symbol.for('foo')
// true
```

上面代码中，iframe 窗口生成的 Symbol 值，可以在主页面得到。

## 实例：模块的 Singleton 模式

Singleton 模式指的是调用一个类，任何时候返回的都是同一个实例。

对于 Node 来说，模块文件可以看成是一个类。怎么保证每次执行这个模块文件，返回的都是同一个实例呢？

很容易想到，可以把实例放到顶层对象`global`。

```javascript
// mod.js
function A() {
  this.foo = 'hello';
}

if (!global._foo) {
  global._foo = new A();
}

module.exports = global._foo;
```

然后，加载上面的`mod.js`。

```javascript
const a = require('./mod.js');
console.log(a.foo);
```

上面代码中，变量`a`任何时候加载的都是`A`的同一个实例。

但是，这里有一个问题，全局变量`global._foo`是可写的，任何文件都可以修改。

```javascript
global._foo = { foo: 'world' };

const a = require('./mod.js');
console.log(a.foo);
```

上面的代码，会使得加载`mod.js`的脚本都失真。

为了防止这种情况出现，我们就可以使用 Symbol。

```javascript
// mod.js
const FOO_KEY = Symbol.for('foo');

function A() {
  this.foo = 'hello';
}

if (!global[FOO_KEY]) {
  global[FOO_KEY] = new A();
}

module.exports = global[FOO_KEY];
```

上面代码中，可以保证`global[FOO_KEY]`不会被无意间覆盖，但还是可以被改写。

```javascript
global[Symbol.for('foo')] = { foo: 'world' };

const a = require('./mod.js');
```

如果键名使用`Symbol`方法生成，那么外部将无法引用这个值，当然也就无法改写。

```javascript
// mod.js
const FOO_KEY = Symbol('foo');

// 后面代码相同 ……
```

上面代码将导致其他脚本都无法引用`FOO_KEY`。但这样也有一个问题，就是如果多次执行这个脚本，每次得到的`FOO_KEY`都是不一样的。虽然 Node 会将脚本的执行结果缓存，一般情况下，不会多次执行同一个脚本，但是用户可以手动清除缓存，所以也不是绝对可靠。

## 内置的 Symbol 值

除了定义自己使用的 Symbol 值以外，ES6 还提供了 11 个内置的 Symbol 值，指向语言内部使用的方法。

### Symbol.hasInstance

对象的`Symbol.hasInstance`属性，指向一个内部方法。当其他对象使用`instanceof`运算符，判断是否为该对象的实例时，会调用这个方法。比如，`foo instanceof Foo`在语言内部，实际调用的是`Foo[Symbol.hasInstance](foo)`。

```javascript
class MyClass {
  [Symbol.hasInstance](foo) {
    return foo instanceof Array;
  }
}

[1, 2, 3] instanceof new MyClass() // true
```

上面代码中，`MyClass`是一个类，`new MyClass()`会返回一个实例。该实例的`Symbol.hasInstance`方法，会在进行`instanceof`运算时自动调用，判断左侧的运算子是否为`Array`的实例。

下面是另一个例子。

```javascript
class Even {
  static [Symbol.hasInstance](obj) {
    return Number(obj) % 2 === 0;
  }
}

// 等同于
const Even = {
  [Symbol.hasInstance](obj) {
    return Number(obj) % 2 === 0;
  }
};

1 instanceof Even // false
2 instanceof Even // true
12345 instanceof Even // false
```

### Symbol.isConcatSpreadable

对象的`Symbol.isConcatSpreadable`属性等于一个布尔值，表示该对象用于`Array.prototype.concat()`时，是否可以展开。

```javascript
let arr1 = ['c', 'd'];
['a', 'b'].concat(arr1, 'e') // ['a', 'b', 'c', 'd', 'e']
arr1[Symbol.isConcatSpreadable] // undefined

let arr2 = ['c', 'd'];
arr2[Symbol.isConcatSpreadable] = false;
['a', 'b'].concat(arr2, 'e') // ['a', 'b', ['c','d'], 'e']
```

上面代码说明，数组的默认行为是可以展开，`Symbol.isConcatSpreadable`默认等于`undefined`。该属性等于`true`时，也有展开的效果。

类似数组的对象正好相反，默认不展开。它的`Symbol.isConcatSpreadable`属性设为`true`，才可以展开。

```javascript
let obj = {length: 2, 0: 'c', 1: 'd'};
['a', 'b'].concat(obj, 'e') // ['a', 'b', obj, 'e']

obj[Symbol.isConcatSpreadable] = true;
['a', 'b'].concat(obj, 'e') // ['a', 'b', 'c', 'd', 'e']
```

`Symbol.isConcatSpreadable`属性也可以定义在类里面。

```javascript
class A1 extends Array {
  constructor(args) {
    super(args);
    this[Symbol.isConcatSpreadable] = true;
  }
}
class A2 extends Array {
  constructor(args) {
    super(args);
  }
  get [Symbol.isConcatSpreadable] () {
    return false;
  }
}
let a1 = new A1();
a1[0] = 3;
a1[1] = 4;
let a2 = new A2();
a2[0] = 5;
a2[1] = 6;
[1, 2].concat(a1).concat(a2)
// [1, 2, 3, 4, [5, 6]]
```

上面代码中，类`A1`是可展开的，类`A2`是不可展开的，所以使用`concat`时有不一样的结果。

注意，`Symbol.isConcatSpreadable`的位置差异，`A1`是定义在实例上，`A2`是定义在类本身，效果相同。

### Symbol.species

对象的`Symbol.species`属性，指向一个构造函数。创建衍生对象时，会使用该属性。

```javascript
class MyArray extends Array {
}

const a = new MyArray(1, 2, 3);
const b = a.map(x => x);
const c = a.filter(x => x > 1);

b instanceof MyArray // true
c instanceof MyArray // true
```

上面代码中，子类`MyArray`继承了父类`Array`，`a`是`MyArray`的实例，`b`和`c`是`a`的衍生对象。你可能会认为，`b`和`c`都是调用数组方法生成的，所以应该是数组（`Array`的实例），但实际上它们也是`MyArray`的实例。

`Symbol.species`属性就是为了解决这个问题而提供的。现在，我们可以为`MyArray`设置`Symbol.species`属性。

```javascript
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}
```

上面代码中，由于定义了`Symbol.species`属性，创建衍生对象时就会使用这个属性返回的函数，作为构造函数。这个例子也说明，定义`Symbol.species`属性要采用`get`取值器。默认的`Symbol.species`属性等同于下面的写法。

```javascript
static get [Symbol.species]() {
  return this;
}
```

现在，再来看前面的例子。

```javascript
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}

const a = new MyArray();
const b = a.map(x => x);

b instanceof MyArray // false
b instanceof Array // true
```

上面代码中，`a.map(x => x)`生成的衍生对象，就不是`MyArray`的实例，而直接就是`Array`的实例。

再看一个例子。

```javascript
class T1 extends Promise {
}

class T2 extends Promise {
  static get [Symbol.species]() {
    return Promise;
  }
}

new T1(r => r()).then(v => v) instanceof T1 // true
new T2(r => r()).then(v => v) instanceof T2 // false
```

上面代码中，`T2`定义了`Symbol.species`属性，`T1`没有。结果就导致了创建衍生对象时（`then`方法），`T1`调用的是自身的构造方法，而`T2`调用的是`Promise`的构造方法。

总之，`Symbol.species`的作用在于，实例对象在运行过程中，需要再次调用自身的构造函数时，会调用该属性指定的构造函数。它主要的用途是，有些类库是在基类的基础上修改的，那么子类使用继承的方法时，作者可能希望返回基类的实例，而不是子类的实例。

### Symbol.match

对象的`Symbol.match`属性，指向一个函数。当执行`str.match(myObject)`时，如果该属性存在，会调用它，返回该方法的返回值。

```javascript
String.prototype.match(regexp)
// 等同于
regexp[Symbol.match](this)

class MyMatcher {
  [Symbol.match](string) {
    return 'hello world'.indexOf(string);
  }
}

'e'.match(new MyMatcher()) // 1
```

### Symbol.replace

对象的`Symbol.replace`属性，指向一个方法，当该对象被`String.prototype.replace`方法调用时，会返回该方法的返回值。

```javascript
String.prototype.replace(searchValue, replaceValue)
// 等同于
searchValue[Symbol.replace](this, replaceValue)
```

下面是一个例子。

```javascript
const x = {};
x[Symbol.replace] = (...s) => console.log(s);

'Hello'.replace(x, 'World') // ["Hello", "World"]
```

`Symbol.replace`方法会收到两个参数，第一个参数是`replace`方法正在作用的对象，上面例子是`Hello`，第二个参数是替换后的值，上面例子是`World`。

### Symbol.search

对象的`Symbol.search`属性，指向一个方法，当该对象被`String.prototype.search`方法调用时，会返回该方法的返回值。

```javascript
String.prototype.search(regexp)
// 等同于
regexp[Symbol.search](this)

class MySearch {
  constructor(value) {
    this.value = value;
  }
  [Symbol.search](string) {
    return string.indexOf(this.value);
  }
}
'foobar'.search(new MySearch('foo')) // 0
```

### Symbol.split

对象的`Symbol.split`属性，指向一个方法，当该对象被`String.prototype.split`方法调用时，会返回该方法的返回值。

```javascript
String.prototype.split(separator, limit)
// 等同于
separator[Symbol.split](this, limit)
```

下面是一个例子。

```javascript
class MySplitter {
  constructor(value) {
    this.value = value;
  }
  [Symbol.split](string) {
    let index = string.indexOf(this.value);
    if (index === -1) {
      return string;
    }
    return [
      string.substr(0, index),
      string.substr(index + this.value.length)
    ];
  }
}

'foobar'.split(new MySplitter('foo'))
// ['', 'bar']

'foobar'.split(new MySplitter('bar'))
// ['foo', '']

'foobar'.split(new MySplitter('baz'))
// 'foobar'
```

上面方法使用`Symbol.split`方法，重新定义了字符串对象的`split`方法的行为，

### Symbol.iterator

对象的`Symbol.iterator`属性，指向该对象的默认遍历器方法。

```javascript
const myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] // [1, 2, 3]
```

对象进行`for...of`循环时，会调用`Symbol.iterator`方法，返回该对象的默认遍历器，详细介绍参见《Iterator 和 for...of 循环》一章。

```javascript
class Collection {
  *[Symbol.iterator]() {
    let i = 0;
    while(this[i] !== undefined) {
      yield this[i];
      ++i;
    }
  }
}

let myCollection = new Collection();
myCollection[0] = 1;
myCollection[1] = 2;

for(let value of myCollection) {
  console.log(value);
}
// 1
// 2
```

### Symbol.toPrimitive

对象的`Symbol.toPrimitive`属性，指向一个方法。该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值。

`Symbol.toPrimitive`被调用时，会接受一个字符串参数，表示当前运算的模式，一共有三种模式。

- Number：该场合需要转成数值
- String：该场合需要转成字符串
- Default：该场合可以转成数值，也可以转成字符串

```javascript
let obj = {
  [Symbol.toPrimitive](hint) {
    switch (hint) {
      case 'number':
        return 123;
      case 'string':
        return 'str';
      case 'default':
        return 'default';
      default:
        throw new Error();
     }
   }
};

2 * obj // 246
3 + obj // '3default'
obj == 'default' // true
String(obj) // 'str'
```

### Symbol.toStringTag

对象的`Symbol.toStringTag`属性，指向一个方法。在该对象上面调用`Object.prototype.toString`方法时，如果这个属性存在，它的返回值会出现在`toString`方法返回的字符串之中，表示对象的类型。也就是说，这个属性可以用来定制`[object Object]`或`[object Array]`中`object`后面的那个字符串。

```javascript
// 例一
({[Symbol.toStringTag]: 'Foo'}.toString())
// "[object Foo]"

// 例二
class Collection {
  get [Symbol.toStringTag]() {
    return 'xxx';
  }
}
let x = new Collection();
Object.prototype.toString.call(x) // "[object xxx]"
```

ES6 新增内置对象的`Symbol.toStringTag`属性值如下。

- `JSON[Symbol.toStringTag]`：'JSON'
- `Math[Symbol.toStringTag]`：'Math'
- Module 对象`M[Symbol.toStringTag]`：'Module'
- `ArrayBuffer.prototype[Symbol.toStringTag]`：'ArrayBuffer'
- `DataView.prototype[Symbol.toStringTag]`：'DataView'
- `Map.prototype[Symbol.toStringTag]`：'Map'
- `Promise.prototype[Symbol.toStringTag]`：'Promise'
- `Set.prototype[Symbol.toStringTag]`：'Set'
- `%TypedArray%.prototype[Symbol.toStringTag]`：'Uint8Array'等
- `WeakMap.prototype[Symbol.toStringTag]`：'WeakMap'
- `WeakSet.prototype[Symbol.toStringTag]`：'WeakSet'
- `%MapIteratorPrototype%[Symbol.toStringTag]`：'Map Iterator'
- `%SetIteratorPrototype%[Symbol.toStringTag]`：'Set Iterator'
- `%StringIteratorPrototype%[Symbol.toStringTag]`：'String Iterator'
- `Symbol.prototype[Symbol.toStringTag]`：'Symbol'
- `Generator.prototype[Symbol.toStringTag]`：'Generator'
- `GeneratorFunction.prototype[Symbol.toStringTag]`：'GeneratorFunction'

### Symbol.unscopables

对象的`Symbol.unscopables`属性，指向一个对象。该对象指定了使用`with`关键字时，哪些属性会被`with`环境排除。

```javascript
Array.prototype[Symbol.unscopables]
// {
//   copyWithin: true,
//   entries: true,
//   fill: true,
//   find: true,
//   findIndex: true,
//   includes: true,
//   keys: true
// }

Object.keys(Array.prototype[Symbol.unscopables])
// ['copyWithin', 'entries', 'fill', 'find', 'findIndex', 'includes', 'keys']
```

上面代码说明，数组有 7 个属性，会被`with`命令排除。

```javascript
// 没有 unscopables 时
class MyClass {
  foo() { return 1; }
}

var foo = function () { return 2; };

with (MyClass.prototype) {
  foo(); // 1
}

// 有 unscopables 时
class MyClass {
  foo() { return 1; }
  get [Symbol.unscopables]() {
    return { foo: true };
  }
}

var foo = function () { return 2; };

with (MyClass.prototype) {
  foo(); // 2
}
```

上面代码通过指定`Symbol.unscopables`属性，使得`with`语法块不会在当前作用域寻找`foo`属性，即`foo`将指向外层作用域的变量。