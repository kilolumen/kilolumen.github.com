---
title: Clojure 学习笔记
date: 2016-03-23 17:57:19
categories:
tags:
---

<!--more-->


；注释以分号开始
；Clojure 代码由一个个form组成，即写在小括号里有空格分开的一组语句
；Clojure解释器会把第一个元素当做一个函数或者一个宏来调用，其余被认为是参数

ns 指定命名空间 (ns noteclojure)

---------------基本
str 创建字符串
+ - * \ =
not 逻辑非

---------------类型
class 查看类型
‘ quote
eval 运算求值

---------------集合
[] 数组
‘()链表 (list)
coll? 集合
seq? 序列
rang 序列 (range 4) ; => (0 1 2 3) , (range) ;=>(0 1 2 3 …), (take 4 (range));=>(0 1 2 3)
cons 列表或向量起始位置添加元素 (cons 4 [1 2 3]);=>[4 1 2 3]
conj 更高效的添加元素 (conj [1 2 3] 4);=>[1 2 3 4] ,(conj ‘(1 2 3) 4);=> (4 1 2 3)
concat 合并列表或向量 (concat [1 2] ‘(3 4)); => (1 2 3 4)
filter 过滤集合元素 (filter even? [1 2 3]); => (2)
map 根据指定的函数映射到一个新的集合 (map inc [1 2 3]); => (2 3 4)
reduce 使用函数来规约集合 (reduce + [1 2 3 4]); = (+ (+ (+ 1 2) 3) 4); => 10
            指定一个初始参数 (reduce conj [] ‘(3 2 1)); = (conj (conj (conj [] 3) 2) 1); ==> [3 2 1]

--------------函数
fn 创建函数，返回值是最后一个表达式的值 (fn [] “Hello World”); => fn
    嵌套小括号来调用它 ((fn [] “Hello World”)); => “Hello World”
def 创建一个变量var  (def x 1)   x; => 1
      将函数定义为一个变量 (def hello-world (fn [] “Hello World”)) (hello -world); => “Hello World”
defn 简化函数定义  (defn hello-world [] “Hello World”)
        中括号的内容是函数的参数  (defn hello [name] (str “Hello” name))  (hello “Steve”)  ;=>”Hello Steve”
	简写  (def hello2 #(str “Hello” %1))  (hello2 “Fanny”)
	多个参数列表 (defn hello3 ([] “Hello wolrd”) ([name] (str “Hello “ name)))  (hello3 “Jake”);=>”Hello Jake”
        (hello3);=> “Hello World”
	可以定义变参函数，即把&后面的参数全部放入一个序列
	(defn count-args [& args] (str “You passed “ (count args) “ args: "))  (count-args 1 2 3) ;=> “You passed 3 args: (1 2 3)”
	可以混用定参和变参（用&来界定）(defn hello-count [name & args] (str “Hello ” name “, you passed “ (count args) “ extra args”))   (hello-count “Finn” 1 2 3) ;=> “Hello Finn, you passed 3 extra args”

--------------哈希表
；基于hash的map和基于数组的map（即array map），实现了相同的接口，hash map查询比较快，但不保证元素顺序
(class {:a 1 :b 2 :c 3})
(class (hash-map :a 1 :b 2 :c 3))
arraymap在足够大的时候，大多数操作会将其自动转换成hashmap
(def stringmap {“a” 1, “b” 2, “c” 3})   stringmap   ;=> {“a” 1, “b” 2, “c” 3}   逗号可有可无
(def keymap {:a 1, :b 2, :c 3})   keymap   ;=> {:a 1, :c 3, :b 2}
(stringmap “a”)    (keymap :a)   (:b keymap)  (stringmap “d”);=> nil  从map查找元素

assoc 函数来向hashmap添加元素  (def newkeymap (assoc keymap :d 4))   newkemap
          newkeymap ;=> {:a 1, :b 2, :c 3, :d 4}   keymap ;=> {:a 1, :b 2, :c 3}  数据类型不可变
dissoc 移除元素    (dissoc keymap :a :b) ;=> {:c 3}


--------------集合set  #
(class #{1 2 3}) ;=> clojure.lang.PersistentHashSet
(set [1 2 3 1 2 3 3 2 1 3 2 1]) ;=> #{1 2 3}
conj 新增元素  (conj #{1 2 3} 4)  ;=> #{1 2 3 4}
disj  移除元素  (disj #{1 2 3} 1)   ; => #{2 3}
(#{1 2 3} 1)   ;=> 1   (#{1 2 3} 4)  ;=> nil 把集合当做函数调用来检查元素是否存在


---------------常用的form
macro  clojure里的逻辑控制结构都是用宏实现的
            (if false “a” “b”)  ;=> “b”
            (if false “a”) ;=> nil
let        来创建临时的绑定变量  (let [a 1 b 2] (> a b))   ; => false
do        将多个语句组合在一起依次执行  (do (print “Hello”) “World”)  ;=> “World” (spring “Hello”)
	    函数定义里有一个隐式的do
(defn print-and-say-hello [name]
  (print “Saying hello to “ name)
  (str “Hello “ name) )
(pring-and-say-hello “Jeff”)   ;=> “Hello Jeff” (prints “Saying hello to Jeff”);
	    let也是如此
(let [name “burgle”]
  (pring “Saying hello to ” name)
  (str “Hello ” name) )  ; => “Hello Urkel” (prints “Saying hello to Urkel”)

---------------模块
use  用来导入模块里的所有函数
        (use ‘clojure.set)
        然后可以使用set相关的函数了
        (intersection #{1 2 3} #{2 3 4})  ; => #{2 3}
        (difference #{1 2 3} #{2 3 4}) ; => #{1}

        也可以从一个模块里导入一部分函数
        (use ‘[clojure.set :only [intersection]])

require  用来导入一个模块    (require ‘clojure.string)
/           用来调用模块里的函数   (clojure.string/blank? “”)  ;=> true
            在’import’里可以给模块名指定一个较短的别名
            (require ‘[clojure.string :as str])  (str/replace “This is a test.” #”[a-o]” str /upper-case) ;=> “THIs Is A tEst.”

#""       用来表示一个正则表达式

:require 可以在一个namespace定义里用:require的方式来require模块（或use，但最好不要用）
(ns test
  (:require
    [clojure.string :as str]
    [clojure.set :as set] ))

----------------java
import    导入java类   (import java.util.Date)
(ns test
  (:import java.util.Date
               java.util.Calendar ))
(Date.)  用类名末尾加’.’的方式来new一个java对象
    用’.’操作符来调用方法，或用’.method’的简化方式
(. (Date.) getTime)           (.getTime (Date.))
    用’/‘调用静态方法
(System/currentTimeMillis)
    用’doto’来更方便的使用（可变）类
(import java.util.Calendar)
(doto (Calendar/getInstance)
  (.set 2000 1 1 0 0 0)
  .getTime )   ;=> A Date. set to 2000-01-01 00:00:00


—————————STM
软件内存事务（Software Transactional Memory） 被clojure用来处理持久化的状态

atom   atom是最简单的 (def my-atom (atom {}))
swap!  更新atom   (swap! my-atom assoc :a 1) ;=> (assoc {} :a 1)
                             (swap! my-atom assoc :b 2) ;=> (assoc {:a 1} :b 2)
@        读取atom的值  @my-atom   ;=> {:a 1 :b 2}

下例是一个使用atom实现的简单计数器
(def counter (atom 0))
(def inc-counter []
  (swap! counter inc))

(inc-counter)
(inc-counter)
(inc-counter)

@counter   ;=>5


其他STM相关的结构是ref和agent




clojure macros

defmacro   定义宏。宏应该输出一个可以作为clojure代码演算的列表
(defmacro my-first-macro []
  (list reverse “Hello World”))   ; => (reverse “Hello World”)

使用macroexpand或macroexpand-1查看宏的结果，注意调用需要引用
(macroexpand ‘(my-first-macro))

可以直接 eval macroexpand 的结果
(eval (macroexpand ‘(my-first-macro)))   ; => (\d \l \o \r \W \space \o \l \l \e \H)
(my-first-macro)  ; => 同上

创建宏的时候可以使用更简短的引用形式来创建列表
(defamer my-first-quoted-macro []
  ‘(reverse “Hello World") )

(macroexpand ‘(my-first-quoted-macro))  ; => (reverse “Hello World”)  注意reverse不再是一个函数对象，而是一个符号

宏可以传入参数
(defmacro inc2 [arg]
  (list + 2 arg))

(inc2 2)  ; => 4
