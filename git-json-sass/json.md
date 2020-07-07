# json

## json简介

json：全称“JavaScript Object Notation（JavaScript对象表示法）”，起源于JavaScript的对象和数组。JSON，说白了就是JavaScript用来处理数据的一种格式，这种格式非常简单易用，不过没有XML通用。

## json的2种结构

1.对象结构

对象结构：对象结构是使用大括号“{}”括起来的，大括号内是由0个或多个用英文逗号分隔的“关键字:值”对（key:value）构成的。注意，这里的键名是字符串，但是值可以是数值、字符串、对象、数组或逻辑true和false。

2.数组结构

JSON数组结构是用中括号“[]”括起来，中括号内部由0个或多个以英文逗号“,”分隔的值列表组成。这里的键名是字符串，但是值可以是数值、字符串、对象、数组或逻辑true和false。

## json对象和字符串

1.普通字符串

普通字符串就是使用单引号或双引号括起来的一串字符

2.json对象

指符合json格式要求的javascript对象

3.json字符串

json字符串就是在json对象外面加一对单引号

4.json对象转换为json字符串

var jsonstr = JSON.stringify(json对象名）

5.将json字符串转换为json对象

var jsonboj = JSON.parse(字符串名)