## 基本代码规范

本篇规范制定了代码基本元素的相关标准，以确保协作的PHP代码的统一性。

## 关于「能愿动词」的使用

为了避免歧义，文档大量使用了「能愿动词」，对应的解释如下：

- `必须 (MUST)`：绝对，严格遵循，请照做，无条件遵守；
- `一定不可 (MUST NOT)`：禁令，严令禁止；
- `应该 (SHOULD)` ：强烈建议这样做，但是不强求；
- `不该 (SHOULD NOT)`：强烈不建议这样做，但是不强求；
- `可以 (MAY)` 和 `可选 (OPTIONAL)` ：选择性高一点，在这个文档内，此词语使用较少；

> 参见：[RFC 2119](http://www.ietf.org/rfc/rfc2119.txt)

## 1. 概览

- PHP代码文件 **必须** 以 `<?php` 标签开始，结尾必须不加`?>`；
- PHP代码文件 **必须** 以 `不带 BOM 的 UTF-8` 编码；
- 命名空间以及类 **必须** 符合 PSR 的自动加载规范：[PSR-4]() 中的一个；
- 类的命名 **必须** 遵循 `StudlyCaps` 大写开头的驼峰命名规范；
- 类中的常量所有字母都 **必须** 大写，单词间用下划线分隔；
- 方法名称 **必须** 符合 `camelCase` 式的小写开头驼峰命名规范。

## 2. 文件

### 2.1. PHP标签

PHP代码文件 **必须** 以 `<?php` 标签开始，结尾必须不加`?>`；
**一定不可** 使用其它自定义标签。

### 2.2. 字符编码

PHP代码 **必须** 且只可使用 `不带BOM的UTF-8` 编码。

### 2.3. 副作用

一份 PHP 文件中 **应该** 要不就只定义新的声明，如类、函数或常量等不产生 `副作用` 的操作，要不就只书写会产生 `副作用` 的逻辑操作，但 **不该** 同时具有两者。

「副作用」(side effects) 一词的意思是，仅仅通过包含文件，不直接声明类、函数和常量等，而执行的逻辑操作。

「副作用」包含却不仅限于：

- 生成输出
- 直接的 `require` 或 `include`
- 连接外部服务
- 修改 ini 配置
- 抛出错误或异常
- 修改全局或静态变量
- 读或写文件等

以下是一个 `反例`，一份包含「函数声明」以及产生「副作用」的代码：

```php
<?php
// 「副作用」：修改 ini 配置
ini_set('error_reporting', E_ALL);

// 「副作用」：引入文件
include "file.php";

// 「副作用」：生成输出
echo "<html>\n";

// 声明函数
function foo()
{
    // 函数主体部分
}
```

下面是一个范例，一份只包含声明不产生「副作用」的代码：

```php
<?php
// 声明函数
function foo()
{
    // 函数主体部分
}

// 条件声明 **不** 属于「副作用」
if (! function_exists('bar')) {
    function bar()
    {
        // 函数主体部分
    }
}
```

## 

## 3. 通则

### 3.1. 缩进

代码**必须**使用4个空格符的缩进，**一定不能**用 tab键 。

> 备注: 使用空格而不是tab键缩进的好处在于， 避免在比较代码差异、打补丁、重阅代码以及注释时产生混淆。 并且，使用空格缩进，让对齐变得更方便。

### 3.2. 关键字 以及 True/False/Null

PHP所有 [关键字](http://php.net/manual/en/reserved.keywords.php)**必须**全部小写。

常量 `true` 、`false` 和 `null` 也**必须**全部小写。

## 4. 命名空间

命名空间以及类的命名必须遵循 [PSR-4]()。

根据规范，每个类都独立为一个文件，且命名空间至少有一个层次：顶级的组织名称（vendor name）。

类的命名 **必须** 遵循 `StudlyCaps` 大写开头的驼峰命名规范。

PHP 5.3 及以后版本的代码 **必须** 使用正式的命名空间。

例如：

```php
<?php
// PHP 5.3及以后版本的写法
namespace Vendor\Model;

class Foo
{
}
```

5.2.x 及之前的版本 **应该** 使用伪命名空间的写法，约定俗成使用顶级的组织名称（vendor name）如 `Vendor_` 为类前缀。

```php
<?php
// 5.2.x及之前版本的写法
class Vendor_Model_Foo
{
}
```

## 5. 类的常量、属性和方法

此处的「类」指代所有的类、接口以及可复用代码块（traits）。

类和类方法的`{`花括号需要另起一行

### 5.1. 常量

类的常量中所有字母都 **必须** 大写，词间以下划线分隔。

参照以下代码：

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 5.2. 属性

类的属性命名 **可以** 遵循：

- 大写开头的驼峰式 (`$StudlyCaps`)
- 小写开头的驼峰式 (`$camelCase`) 
- 下划线分隔式 (`$under_score`)

本规范不做强制要求，但无论遵循哪种命名方式，都 **应该** 在一定的范围内保持一致。这个范围可以是整个团队、整个包、整个类或整个方法。

### 5.3. 方法

方法名称 **必须** 符合 `camelCase()` 式的小写开头驼峰命名规范。

## 6. 控制结构

控制结构的基本规范如下：

- 控制结构关键词后**必须**有一个空格。
- 左括号 `(` 后**一定不能**有空格。
- 右括号 `)` 前也**一定不**能有空格。
- 右括号 `)` 与开始花括号 `{` 间**一定**有一个空格。
- 结构体主体**一定**要有一次缩进。
- 结束花括号 `}`**一定**在结构体主体后单独成行。

每个结构体的主体都**必须**被包含在成对的花括号之中， 这能让结构体更加结构话，以及减少加入新行时，出错的可能性。

### 6.1. `if` 、 `elseif` 和 `else`

标准的 `if` 结构如下代码所示，留意 括号、空格以及花括号的位置， 注意 `else` 和 `elseif` 都与前面的结束花括号在同一行。

```
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

**应该**使用关键词 `elseif` 代替所有 `else if` ，以使得所有的控制关键字都像是单独的一个词。

### 6.2. `switch` 和 `case`

标准的 `switch` 结构如下代码所示，留意括号、空格以及花括号的位置。 `case` 语句**必须**相对 `switch` 进行一次缩进，而 `break` 语句以及 `case` 内的其它语句都 必须 相对 `case` 进行一次缩进。 如果存在非空的 `case` 直穿语句，主体里必须有类似 `// no break` 的注释。

```
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```

### 6.3. `while` 和 `do while`

一个规范的 `while` 语句应该如下所示，注意其 括号、空格以及花括号的位置。

```
<?php
while ($expr) {
    // structure body
}
```

标准的 `do while` 语句如下所示，同样的，注意其 括号、空格以及花括号的位置。

```
<?php
do {
    // structure body;
} while ($expr);
```

### 6.4. `for`

标准的 `for` 语句如下所示，注意其 括号、空格以及花括号的位置。

```
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 6.5. `foreach`

标准的 `foreach` 语句如下所示，注意其 括号、空格以及花括号的位置。

```
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 6.6. `try`, `catch`

标准的 `try catch` 语句如下所示，注意其 括号、空格以及花括号的位置。

```
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

## 7. 闭包

闭包声明时，关键词 `function` 后以及关键词 `use` 的前后都**必须**要有一个空格。

开始花括号**必须**写在声明的同一行，结束花括号**必须**紧跟主体结束的下一行。

参数列表和变量列表的左括号后以及右括号前，**必须不能**有空格。

参数和变量列表中，逗号前**必须不能**有空格，而逗号后**必须**要有空格。

闭包中有默认值的参数**必须**放到列表的后面。

标准的闭包声明语句如下所示，注意其 括号、逗号、空格以及花括号的位置。

```
<?php
$closureWithArgs = function($arg1, $arg2){
    // body
};

$closureWithArgsAndVars = function($arg1, $arg2)use($var1, $var2){
    // body
};
```

参数列表以及变量列表**可以**分成多行，这样，包括第一个在内的每个参数或变量都**必须**单独成行，而列表的右括号与闭包的开始花括号**必须**放在同一行。

以下几个例子，包含了参数和变量列表被分成多行的多情况。

```
<?php
$longArgs_noVars = function( $longArgument, $longerArgument, $muchLongerArgument ){
   // body
};

$noArgs_longVars = function()use( $longVar1, $longerVar2, $muchLongerVar3 ){
   // body
};

$longArgs_longVars = function( $longArgument, $longerArgument, $muchLongerArgument )use( $longVar1, $longerVar2, $muchLongerVar3 ){
   // body
};

$longArgs_shortVars = function( $longArgument, $longerArgument, $muchLongerArgument )use($var1){
   // body
};

$shortArgs_longVars = function($arg)use( $longVar1, $longerVar2, $muchLongerVar3 ){
   // body
};
```

注意，闭包被直接用作函数或方法调用的参数时，以上规则仍然适用。

```
<?php
$foo->bar(
    $arg1,
    function($arg2)use($var1){
        // body
    },
    $arg3
);
```
