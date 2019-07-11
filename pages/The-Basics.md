---
layout: page
title:  The Basics
sitemap: true
---

# 基础

## 比较运算符

比较运算符在PHP中经常容易被忽略,但是它常常会导致一些意想不到的输出结果。下面是一个常见的例子，在使用比较运算符，比较整型变量和布尔值。

{% highlight php %}
<?php
$a = 5;   // 5 as an integer

var_dump($a == 5);       // compare value; return true
var_dump($a == '5');     // compare value (ignore type); return true
var_dump($a === 5);      // compare type/value (integer vs. integer); return true
var_dump($a === '5');    // compare type/value (integer vs. string); return false

//Equality comparisons
if (strpos('testing', 'test')) {    // 'test' is found at position 0, which is interpreted as the boolean 'false'
    // code...
}

// vs. strict comparisons
if (strpos('testing', 'test') !== false) {    // true, as strict comparison was made (0 !== false)
    // code...
}
{% endhighlight %}

* [比较运算符](http://php.net/language.operators.comparison)
* [PHP 类型比较表](http://php.net/types.comparisons)
* [比较运算符示例清单](http://phpcheatsheets.com/index.php?page=compare)

## 条件语句

### If 条件判断语句

当我们在函数或类方法中使用 'if/else' 条件判断语句时，存在一个常见的误解,'else'语句是必须使用的,以此保证其他的执行情况有明确的定义。然而，如果我们的输出结果是去定义返回值，那么'else'语句就不是必要的，我们可以直接通过return进行返回，使用多余的else语句将变得毫无意义。

{% highlight php %}
<?php
function test($a)
{
    if ($a) {
        return true;
    } else {
        return false;
    }
}

// vs.

function test($a)
{
    if ($a) {
        return true;
    }
    return false;    // else is not necessary
}

// or even shorter:

function test($a)
{
    return (bool) $a;
}

{% endhighlight %}

* [If 语句](http://php.net/control-structures.if)

### Switch 多重选择语句

Switch多重选择语句是一个非常好的方式去避免使用大量的'else/if','else',但是使用时也仍需注意以下几点：

- Switch 多重选择语句只能进行“值”的比较，不能进行“类型”的比较。 (相当于'==')
- 语句将遍历每一种case(情况)，直到找到匹配值.如果没有找到匹配值,将会执行默认的设置 (前提是已设置默认值)
- 进入匹配条件后，如果没有break;(中断退出语句), 将会继续执行匹配，直到找到第一个break;或return;退出方法。
- 在一个函数,使用'return'代替了使用'break'的必要性,因为它结束了当前函数。

{% highlight php %}
<?php
$answer = test(2);    // the code from both 'case 2' and 'case 3' will be implemented

function test($a)
{
    switch ($a) {
        case 1:
            // code...
            break;             // break is used to end the switch statement
        case 2:
            // code...         // with no break, comparison will continue to 'case 3'
        case 3:
            // code...
            return $result;    // within a function, 'return' will end the function
        default:
            // code...
            return $error;
    }
}
{% endhighlight %}

* [多重选择语句](http://php.net/control-structures.switch)
* [PHP switch](http://phpswitch.com/)

## 全局命名空间

当我们使用命名空间的时候，你可能想要找到你写过的被隐藏（没有明确空间）的方法，通过在调用方法前加反斜杠，你将解决这个问题。

{% highlight php %}
<?php
namespace phptherightway;

function fopen()
{
    $file = \fopen();    // Our function name is the same as an internal function.
                         // Execute the function from the global space by adding '\'.
}

function array()
{
    $iterator = new \ArrayIterator();    // ArrayIterator is an internal class. Using its name without a backslash
                                         // will attempt to resolve it within your namespace.
}
{% endhighlight %}

* [全局空间](http://php.net/language.namespaces.global)
* [全局规则](http://php.net/userlandnaming.rules)

## 字符串

### 字符串连接符

- 如果你想一行记录超过120个字符的，建议你使用字符串连接符。
- 为了更好地可读性，最好使用字符串连接符而不是赋值运算符。
- 当变量在原始范围内,使用字符串连接符新起一行时要对代码进行缩进。


{% highlight php %}
<?php
$a  = 'Multi-line example';    // concatenating assignment operator (.=)
$a .= "\n";
$a .= 'of what not to do';

// vs

$a = 'Multi-line example'      // concatenation operator (.)
    . "\n"                     // indenting new lines
    . 'of what to do';
{% endhighlight %}

* [字符串运算符](http://php.net/language.operators.string)

### 字符串类型

字符串是一系列字符，听起来很简单。当然，有一些不同类型的字符串，它们提供略有不同的语法，行为略有不同。

#### 单引号

单引号常常被用来表示“文字字符串”，而文字字符串不会解析变量和特殊符号。

如果你使用单引号，你可能像这样在一个字符串中输入一个变量名: `'some $thing'`, 你将会看到这样的输出`some $thing`. 如果你使用双引号, 他将会尝试解析 `$thing`这个变量名，如果变量没有找到将会报错。


{% highlight php %}
<?php
echo 'This is my string, look at how pretty it is.';    // no need to parse a simple string

/**
 * Output:
 *
 * This is my string, look at how pretty it is.
 */
{% endhighlight %}

* [单引号](http://php.net/language.types.string#language.types.string.syntax.single)

#### 双引号

双引号好比处理字符串的瑞士军刀，他不仅仅是像前文提到的能处理变量，还能处理分析各种特殊字符, 像 `\n` 换行, `\t` 缩进, etc.

{% highlight php %}
<?php
echo 'phptherightway is ' . $adjective . '.'     // a single quotes example that uses multiple concatenating for
    . "\n"                                       // variables and escaped string
    . 'I love learning' . $code . '!';

// vs

echo "phptherightway is $adjective.\n I love learning $code!"  // Instead of multiple concatenating, double quotes
                                                               // enables us to use a parsable string
{% endhighlight %}

使用双引号可以包含变量;这种操作称之为“插值”.

{% highlight php %}
<?php
$juice = 'plum';
echo "I like $juice juice";    // Output: I like plum juice
{% endhighlight %}

当我们使用插值时,经常会遇到一个变量包含另一个字符串。这样做的结果是将产生一些混乱，无法区分什么是变量名称，什么是文本字符串。

为了解决这种问题，我们使用大括号来包裹相对应的变量。

{% highlight php %}
<?php
$juice = 'plum';
echo "I drank some juice made of $juices";    // $juice cannot be parsed

// vs

$juice = 'plum';
echo "I drank some juice made of {$juice}s";    // $juice will be parsed

/**
 * 在大括号内的复杂变量也将被解析
 */

$juice = array('apple', 'orange', 'plum');
echo "I drank some juice made of {$juice[1]}s";   // $juice[1] will be parsed
{% endhighlight %}

* [Double quotes](http://php.net/language.types.string#language.types.string.syntax.double)

#### Nowdoc syntax （Nowdoc 语法）

Nowdoc 语法在PHP5.3中被介绍，他的使用方式与单引号相同，唯一区别是它可以使用多行字符串而无需进行连接。


{% highlight php %}
<?php
$str = <<<'EOD'             // initialized by <<<
Example of string
spanning multiple lines
using nowdoc syntax.
$a does not parse.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using nowdoc syntax.
 * $a does not parse.
 */
{% endhighlight %}

* [Nowdoc syntax](http://php.net/language.types.string#language.types.string.syntax.nowdoc)

#### Heredoc syntax （Heredoc 语法）

Heredoc 语法 插入行为与双引号相同，也适用于多行字符串，同时不需要进行字符串的连接。

{% highlight php %}
<?php
$a = 'Variables';

$str = <<<EOD               // initialized by <<<
Example of string
spanning multiple lines
using heredoc syntax.
$a are parsed.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using heredoc syntax.
 * Variables are parsed.
 */
{% endhighlight %}

* [Heredoc syntax](http://php.net/language.types.string#language.types.string.syntax.heredoc)

### 哪一种更快?

这里有一种谣言就是单引号会比双引号在使用上稍快一些，实际上这并不是真是的。

如果你定义了一个简单字符串，没有使用任何复杂变量和特殊字符串，使用单引号和双引号的效果是相同的，两者并不会谁更快。


如果要连接任意类型的多个字符串，或在双引号字符串中进行插值，则结果可能会有所不同。如果您使用的是少量的值，那么进行连接速度会稍微快一点。对于大量的值，进行插值操作速度要快得多。

无论你使用字符串做什么，这些类型都不会对你的应用产生任何明显的影响。尝试重写代码以使用其他方式是徒劳，所以请避免过度优化，除非您真正了解差异的含义和影响。

* [驳斥单引号的性能谣言](http://nikic.github.io/2012/01/09/Disproving-the-Single-Quotes-Performance-Myth.html)


## 三元运算符 

三元运算符是精简代码的好方法，但也往往存在着过度使用.当三元运算符可堆叠/嵌套时，建议保持每一行的可读性。

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? 'yay' : 'nay';
{% endhighlight %}

相比之下，这里有一个例子，为了缩减代码量而牺牲了所有形式的的代码可读性。

{% highlight php %}
<?php
echo ($a) ? ($a == 5) ? 'yay' : 'nay' : ($b == 10) ? 'excessive' : ':(';    // excess nesting, sacrificing readability
{% endhighlight %}

使用三元运算符的正确语法，来获得返回值。

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? return true : return false;    // this example will output an error

// vs

$a = 5;
return ($a == 5) ? 'yay' : 'nope';    // this example will return 'yay'

{% endhighlight %}

有一点需要被提醒，你不需要使用三元运算符来进行布尔值的判断和返回。例子如下：

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? true : false; // Will return true or false if $a == 3

// vs

$a = 3;
return $a == 3; // Will return true or false if $a == 3

{% endhighlight %}

同理适用于以下运算符(===, !==, !=, == etc).

#### 在三元运算符中对表达式和方法使用括号

当使用三元运算符时，括号可以帮助提高代码可读性，也可以帮助在块内声明。不需要使用括号的示例如下：

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? "yay" : "nope"; // return yay or nope if $a == 3

// vs

$a = 3;
return $a == 3 ? "yay" : "nope"; // return yay or nope if $a == 3
{% endhighlight %}

括号的包围还将我们要检查块的语句块视为一个整体。如下面这个例子，如果两个代码块（$ a == 3和$ b == 4）都为真且$ c == 5也成立，则返回true。

{% highlight php %}
<?php
return ($a == 3 && $b == 4) && $c == 5;
{% endhighlight %}

Another example is the snippet below which will return true if ($a != 3 AND $b != 4) OR $c == 5.

{% highlight php %}
<?php
return ($a != 3 && $b != 4) || $c == 5;
{% endhighlight %}

从PHP5.3开始，可以省略三元运算符的中间部分。如果expr1的计算结果为TRUE，则表达式“expr1？：expr3”将返回expr1，否则返回expr3。

* [三元运算符](http://php.net/language.operators.comparison)

## 变量声明

有时, 程序员们尝试让他们的代码看起来更整洁，通过不同的名称声明一个预定义变量。但是这也将消耗两倍的内存。对于下面这个例子,我们书写一个字符串将包含1MB的数据量。但是因为拷贝变量，当你在执行脚本的时候内存的消耗将增加到2MB。

{% highlight php %}
<?php
$about = 'A very long string of text';    // uses 2MB memory
echo $about;

// vs

echo 'A very long string of text';        // uses 1MB memory
{% endhighlight %}

* [性能提示](http://web.archive.org/web/20140625191431/https://developers.google.com/speed/articles/optimizing-php)
