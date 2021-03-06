使用 Go 的逻辑运算符判断是否闰年
========================================

.. note::

    本文摘录自即将出版的《Go语言趣学指南》，
    请访问 `gpwgcn.com <http://gpwgcn.com>`_  以获取更多相关信息。

    .. image:: image/gpwgcn.jpg
       :scale: 80%

在 Go 中，逻辑运算符 ``||`` 代表“*逻辑或*”，而逻辑运算符 ``&&``
则代表“*逻辑与*”。 这些逻辑运算符可以一次检查多个条件，图 3-1 和 3-2
展示了它们的求值方式。

--------------

图 3-1 逻辑或：当 ``a`` 、 ``b`` 两值中至少有一个为真时， ``a || b`` 为真

.. image:: image/3-1.jpg

|

图 3-2 逻辑与：当且仅当 ``a`` 、 ``b`` 两值都为真时， ``a && b`` 为真

.. image:: image/3-2.jpg

--------------

代码清单 3-4 展示的是一段判断 2100
年是否为闰年的程序，其中用到的判断指定年份是否为闰年的规则如下：

-  能够被 4 整除但是不能被 100 整除的年份为闰年

-  可以被 400 整除的年份也是闰年

..

   {注意}

   正如之前所说， 取模运算符 ``%`` 可以计算出两个整数相除时所得的余数，
   而余数为零则表示一个数被另一个数整除了。

.. image:: image/gopher.jpg
   :align: right

--------------

代码清单 3-4 闰年识别器： ``leap.go``

::

    fmt.Println("The year is 2100, should you leap?")

    var year = 2100
    var leap = year%400 == 0 || (year%4 == 0 && year%100 != 0)

    if leap {
        fmt.Println("Look before you leap!")
    } else {
        fmt.Println("Keep your feet on the ground.")
    }

--------------

执行代码清单 3-4 中的程序将得到以下输出：

.. code-block:: text

   The year is 2100, should you leap?
   Keep your feet on the ground.

跟大多数编程语言一样，Go 也采用了\ *短路逻辑*\ ： 如果位于 ``||``
运算符之前的第一个条件为真，那么位于运算符之后的条件就可以被忽略，没有必要再对其进行求值。
具体到代码清单 3-4 中的例子，当给定年份可以被 400
整除时，程序就不必再进行后续的判断了。

``&&`` 运算符的行为跟 ``||`` 运算符正好相反：
只有在两个条件都为真的情况下，运算结果才会为真。 对于代码清单 3-4
中的例子，如果给定年份无法被 4 整除，那么程序就不会求值后续条件。

*逻辑非*\ 运算符 ``!`` 可以将一个布尔值从 ``false`` 转变为 ``true``
，又或者将 ``true`` 转变为 ``false`` 。 作为例子，代码清单 3-5
将在玩家没有火把或者未点燃火把时打印出一条信息。

--------------

代码清单 3-5 逻辑非运算符： ``torch.go``

::

    var haveTorch = true
    var litTorch = false

    if !haveTorch || !litTorch {
        fmt.Println("Nothing to see here.") // 打印出“Nothing to see here.”
    }

--------------

   {速查 3-4}

   1. 首先请使用纸和笔， 将代码清单 3-4 中闰年表达式的年份替换为
      ``2000`` ； 接着求值所有取模运算，
      计算出它们的余数（如果有需要可以使用计算器）； 在此之后， 求值
      ``==`` 和 ``!=`` 条件以得出 ``true`` 或者 ``false`` ； 最后，
      求值逻辑运算符 ``&&`` 和 ``||`` ， 并最终判断出 2000
      年是否为闰年。
   2. 如果我们在求值 ``2000%400 == 0`` 为 ``true`` 时使用短路逻辑，
      是不是就可以节省一些时间了？ —

..

   {速查 3-4 答案}

   1. 是的， 2000 年的确是闰年：

      .. code:: go

          2000%400 == 0 || (2000%4 == 0 && 2000%100 != 0) 0 == 0 || (0 == 0 && 0 != 0)
          true || (true && false)
          true || (false)
          true

   2. 是的， 计算并写下等式的后半部分需要花费额外的时间。
      虽然计算机执行相同计算的速度要快得多，
      但短路逻辑仍然能够起到节约时间的作用。


