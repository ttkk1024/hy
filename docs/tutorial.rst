========
手册
========

.. TODO
..
..  - How do I index into arrays or dictionaries?
..  - How do I do array ranges?  e.g. x[5:] or y[2:10]
..  - Blow your mind with macros!
..  - Where's my banana???


欢迎来到Hy手册

简而言之，Hy是Lisp方言，但是它将它结构转换为Python...自由的转换为
Python抽象语法树！（或者用更直白的术语来描述，Hy就是建立在Python上的lisp手杖！）
这将非常的酷因为Hy有几个特点：
 - Pythonic感觉的Lisp
 - 对Lisp开发者来说，这不仅可以使用Lisp疯狂的能力，而且可以使用大量的Python库（为什么不这样呢，你可以用lisp写Django程序）
 - 对Python开发者来说，可以在熟悉的Python环境中，探索Lisp！
 - 对每个人来说：优雅的语言包含大量简洁的理念！


本手册假设你在Python3上使用Hy。如果你仍在使用Python2，会稍微有点差异


针对Python开发者简要介绍Lisp
===================================

好吧，你可能以前从来没有用过Lisp，但是你一定用过Python！


在Hy中“hello world”程序实际上超级简单。让我们小试一把：

.. code-block:: clj

   (print "hello world")

怎么样？非常的简单！正如你想象的，程序跟Python版本是一样的

::

  print("hello world")


做一些超级简单的数学运行，我们可以这样做:

.. code-block:: clj

   (+ 1 3)


计算结果返回4，跟下面的计算是等效的：

.. code-block:: clj

   1 + 3



正如你看到的那样，列表中的第一个元素是调用的函数，其他元素是传入的参数。
事实上，在hy（跟绝大多数lisp一样）我们可以向加号操作符传入多个参数:

.. code-block:: clj

   (+ 1 3 55)

结果返回59.


你以前可能听说过lisp，但是对它了解不多。lisp可能没有你想象的那么难，而且
Hy继承于Python，所以Hy是学习lisp很好的方式。对lisp最大的印象可能是它有
大量的括号。首先让你看着非常的迷惑，但是它没有那么难。让我们看看一些包含
在括号中的一些数学运算，输入到Hy解释器看看运行结果：

.. code-block:: clj

   (setv result (- (/ (+ 1 3 88) 2) 8))

结果将返回38.0.为什么会是这样呢？好吧，让我们看看
Python的等价表达式::

  result = ((1 + 3 + 88) / 2) - 8

如果你尝试用python计算上面的表达式，
你可以通过计算每个内部的括号来实现，这个是hy的基本思路。
让我们首先在python中来练习::

  result = ((1 + 3 + 88) / 2) - 8
  # 简化为...
  result = (92 / 2) - 8
  # 简化为...
  result = 46.0 - 8
  # 简化为...
  result = 38.0

Now let's try the same thing in Hy:

.. code-block:: clj

   (setv result (- (/ (+ 1 3 88) 2) 8))
   ; simplified to...
   (setv result (- (/ 92 2) 8))
   ; simplified to...
   (setv result (- 46.0 8))
   ; simplified to...
   (setv result 38.0)

As you probably guessed, this last expression with ``setv`` means to
assign the variable "result" to 38.0.

See?  Not too hard!

This is the basic premise of Lisp. Lisp stands for "list
processing"; this means that the structure of the program is
actually lists of lists.  (If you're familiar with Python lists,
imagine the entire same structure as above but with square brackets
instead, any you'll be able to see the structure above as both a
program and a data structure.)  This is easier to understand with more
examples, so let's write a simple Python program, test it, and then
show the equivalent Hy program::

  def simple_conversation():
      print("Hello!  I'd like to get to know you.  Tell me about yourself!")
      name = input("What is your name? ")
      age = input("What is your age? ")
      print("Hello " + name + "!  I see you are " + age + " years old.")

  simple_conversation()

If we ran this program, it might go like::

  Hello!  I'd like to get to know you.  Tell me about yourself!
  What is your name? Gary
  What is your age? 38
  Hello Gary!  I see you are 38 years old.

Now let's look at the equivalent Hy program:

.. code-block:: clj

   (defn simple-conversation []
      (print "Hello!  I'd like to get to know you.  Tell me about yourself!")
      (setv name (input "What is your name? "))
      (setv age (input "What is your age? "))
      (print (+ "Hello " name "!  I see you are "
                 age " years old.")))

   (simple-conversation)

If you look at the above program, as long as you remember that the
first element in each list of the program is the function (or
macro... we'll get to those later) being called and that the rest are
the arguments, it's pretty easy to figure out what this all means.
(As you probably also guessed, ``defn`` is the Hy method of defining
methods.)

Still, lots of people find this confusing at first because there's so
many parentheses, but there are plenty of things that can help make
this easier: keep indentation nice and use an editor with parenthesis
matching (this will help you figure out what each parenthesis pairs up
with) and things will start to feel comfortable.

There are some advantages to having a code structure that's actually a
very simple data structure as the core of Lisp is based on.  For one
thing, it means that your programs are easy to parse and that the
entire actual structure of the program is very clearly exposed to you.
(There's an extra step in Hy where the structure you see is converted
to Python's own representations ... in "purer" Lisps such as Common
Lisp or Emacs Lisp, the data structure you see in the code and the
data structure that is executed is much more literally close.)

Another implication of this is macros: if a program's structure is a
simple data structure, that means you can write code that can write
code very easily, meaning that implementing entirely new language
features can be very fast.  Previous to Hy, this wasn't very possible
for Python programmers ... now you too can make use of macros'
incredible power (just be careful to not aim them footward)!


Hy is a Lisp-flavored Python
============================

Hy converts to Python's own abstract syntax tree, so you'll soon start
to find that all the familiar power of python is at your fingertips.

You have full access to Python's data types and standard library in
Hy.  Let's experiment with this in the hy interpreter::

  => [1 2 3]
  [1, 2, 3]
  => {"dog" "bark"
  ... "cat" "meow"}
  {'dog': 'bark', 'cat': 'meow'}
  => (, 1 2 3)
  (1, 2, 3)
  => #{3 1 2}
  {1, 2, 3}
  => 1/2
  Fraction(1, 2)

Notice the last two lines: Hy has a fraction literal like Clojure.

If you start Hy like this (a shell alias might be helpful)::

  $ hy --repl-output-fn=hy.contrib.hy-repr.hy-repr

the interactive mode will use :ref:`hy-repr-fn` instead of Python's
native ``repr`` function to print out values, so you'll see values in
Hy syntax rather than Python syntax::

  => [1 2 3]
  [1 2 3]
  => {"dog" "bark"
  ... "cat" "meow"}
  {"dog" "bark" "cat" "meow"}

If you are familiar with other Lisps, you may be interested that Hy
supports the Common Lisp method of quoting:

.. code-block:: clj

   => '(1 2 3)
   (1 2 3)

You also have access to all the built-in types' nice methods::

  => (.strip " fooooo   ")
  "fooooo"

What's this?  Yes indeed, this is precisely the same as::

  " fooooo   ".strip()

That's right---Lisp with dot notation!  If we have this string
assigned as a variable, we can also do the following:

.. code-block:: clj

   (setv this-string " fooooo   ")
   (this-string.strip)

What about conditionals?:

.. code-block:: clj

   (if (try-some-thing)
     (print "this is if true")
     (print "this is if false"))

As you can tell above, the first argument to ``if`` is a truth test, the
second argument is the body if true, and the third argument (optional!)
is if false (ie. ``else``).

If you need to do more complex conditionals, you'll find that you
don't have ``elif`` available in Hy.  Instead, you should use something
called ``cond``.  In Python, you might do something like::

  somevar = 33
  if somevar > 50:
      print("That variable is too big!")
  elif somevar < 10:
      print("That variable is too small!")
  else:
      print("That variable is jussssst right!")

In Hy, you would do:

.. code-block:: clj

   (setv somevar 33)
   (cond
    [(> somevar 50)
     (print "That variable is too big!")]
    [(< somevar 10)
     (print "That variable is too small!")]
    [True
     (print "That variable is jussssst right!")])

What you'll notice is that ``cond`` switches off between a statement
that is executed and checked conditionally for true or falseness, and
then a bit of code to execute if it turns out to be true.  You'll also
notice that the ``else`` is implemented at the end simply by checking
for ``True`` -- that's because ``True`` will always be true, so if we get
this far, we'll always run that one!

You might notice above that if you have code like:

.. code-block:: clj

   (if some-condition
     (body-if-true)
     (body-if-false))

But wait!  What if you want to execute more than one statement in the
body of one of these?

You can do the following:

.. code-block:: clj

   (if (try-some-thing)
     (do
       (print "this is if true")
       (print "and why not, let's keep talking about how true it is!"))
     (print "this one's still simply just false"))

You can see that we used ``do`` to wrap multiple statements.  If you're
familiar with other Lisps, this is the equivalent of ``progn``
elsewhere.

Comments start with semicolons:

.. code-block:: clj

  (print "this will run")
  ; (print "but this will not")
  (+ 1 2 3)  ; we'll execute the addition, but not this comment!

Hashbang (``#!``) syntax is supported:

.. code-block:: clj

   #! /usr/bin/env hy
   (print "Make me executable, and run me!")

Looping is not hard but has a kind of special structure.  In Python,
we might do::

  for i in range(10):
      print("'i' is now at " + str(i))

The equivalent in Hy would be:

.. code-block:: clj

  (for [i (range 10)]
    (print (+ "'i' is now at " (str i))))


You can also import and make use of various Python libraries.  For
example:

.. code-block:: clj

   (import os)

   (if (os.path.isdir "/tmp/somedir")
     (os.mkdir "/tmp/somedir/anotherdir")
     (print "Hey, that path isn't there!"))

Python's context managers (``with`` statements) are used like this:

.. code-block:: clj

     (with [f (open "/tmp/data.in")]
       (print (.read f)))

which is equivalent to::

  with open("/tmp/data.in") as f:
      print(f.read())

And yes, we do have List comprehensions!  In Python you might do::

  odds_squared = [
    pow(num, 2)
    for num in range(100)
    if num % 2 == 1]

In Hy, you could do these like:

.. code-block:: clj

  (setv odds-squared
    (list-comp
      (pow num 2)
      (num (range 100))
      (= (% num 2) 1)))


.. code-block:: clj

  ; And, an example stolen shamelessly from a Clojure page:
  ; Let's list all the blocks of a Chessboard:

  (list-comp
    (, x y)
    (x (range 8)
     y "ABCDEFGH"))

  ; [(0, 'A'), (0, 'B'), (0, 'C'), (0, 'D'), (0, 'E'), (0, 'F'), (0, 'G'), (0, 'H'),
  ;  (1, 'A'), (1, 'B'), (1, 'C'), (1, 'D'), (1, 'E'), (1, 'F'), (1, 'G'), (1, 'H'),
  ;  (2, 'A'), (2, 'B'), (2, 'C'), (2, 'D'), (2, 'E'), (2, 'F'), (2, 'G'), (2, 'H'),
  ;  (3, 'A'), (3, 'B'), (3, 'C'), (3, 'D'), (3, 'E'), (3, 'F'), (3, 'G'), (3, 'H'),
  ;  (4, 'A'), (4, 'B'), (4, 'C'), (4, 'D'), (4, 'E'), (4, 'F'), (4, 'G'), (4, 'H'),
  ;  (5, 'A'), (5, 'B'), (5, 'C'), (5, 'D'), (5, 'E'), (5, 'F'), (5, 'G'), (5, 'H'),
  ;  (6, 'A'), (6, 'B'), (6, 'C'), (6, 'D'), (6, 'E'), (6, 'F'), (6, 'G'), (6, 'H'),
  ;  (7, 'A'), (7, 'B'), (7, 'C'), (7, 'D'), (7, 'E'), (7, 'F'), (7, 'G'), (7, 'H')]


Python has support for various fancy argument and keyword arguments.
In Python we might see::

  >>> def optional_arg(pos1, pos2, keyword1=None, keyword2=42):
  ...   return [pos1, pos2, keyword1, keyword2]
  ...
  >>> optional_arg(1, 2)
  [1, 2, None, 42]
  >>> optional_arg(1, 2, 3, 4)
  [1, 2, 3, 4]
  >>> optional_arg(keyword1=1, pos2=2, pos1=3, keyword2=4)
  [3, 2, 1, 4]

The same thing in Hy::

  => (defn optional-arg [pos1 pos2 &optional keyword1 [keyword2 42]]
  ...  [pos1 pos2 keyword1 keyword2])
  => (optional-arg 1 2)
  [1 2 None 42]
  => (optional-arg 1 2 3 4)
  [1 2 3 4]

You can call keyword arguments like this::

  => (optional-arg :keyword1 1
  ...              :pos2 2
  ...              :pos1 3
  ...              :keyword2 4)
  [3, 2, 1, 4]

You can unpack arguments with the syntax ``#* args`` and ``#** kwargs``,
similar to `*args` and `**kwargs` in Python::

  => (setv args [1 2])
  => (setv kwargs {"keyword2" 3
  ...              "keyword1" 4})
  => (optional-arg #* args #** kwargs)
  [1, 2, 4, 3]

There's also a dictionary-style keyword arguments construction that
looks like:

.. code-block:: clj

  (defn another-style [&key {"key1" "val1" "key2" "val2"}]
    [key1 key2])

The difference here is that since it's a dictionary, you can't rely on
any specific ordering to the arguments.

Hy also supports ``*args`` and ``**kwargs`` in parameter lists.  In Python::

  def some_func(foo, bar, *args, **kwargs):
    import pprint
    pprint.pprint((foo, bar, args, kwargs))

The Hy equivalent:

.. code-block:: clj

  (defn some-func [foo bar &rest args &kwargs kwargs]
    (import pprint)
    (pprint.pprint (, foo bar args kwargs)))

Finally, of course we need classes!  In Python, we might have a class
like::

  class FooBar(object):
      """
      Yet Another Example Class
      """
      def __init__(self, x):
          self.x = x

      def get_x(self):
          """
          Return our copy of x
          """
          return self.x
          
And we might use it like::

  bar = FooBar(1)
  print(bar.get_x())


In Hy:

.. code-block:: clj

  (defclass FooBar [object]
    "Yet Another Example Class"

    (defn --init-- [self x]
      (setv self.x x))

    (defn get-x [self]
      "Return our copy of x"
      self.x))
      
And we can use it like:

.. code-block:: clj

  (setv bar (FooBar 1))
  (print (bar.get-x))
  
Or using the leading dot syntax!

.. code-block:: clj

  (print (.get-x (FooBar 1)))
      

You can also do class-level attributes.  In Python::

  class Customer(models.Model):
      name = models.CharField(max_length=255)
      address = models.TextField()
      notes = models.TextField()

In Hy:

.. code-block:: clj

  (defclass Customer [models.Model]
    [name (models.CharField :max-length 255})
     address (models.TextField)
     notes (models.TextField)])

Macros
======

One really powerful feature of Hy are macros. They are small functions that are
used to generate code (or data). When program written in Hy is started, the
macros are executed and their output is placed in the program source. After this,
the program starts executing normally. Very simple example:

.. code-block:: clj

  => (defmacro hello [person]
  ...  `(print "Hello there," ~person))
  => (hello "Tuukka")
  Hello there, Tuukka

The thing to notice here is that hello macro doesn't output anything on
screen. Instead it creates piece of code that is then executed and prints on
screen. This macro writes a piece of program that looks like this (provided that
we used "Tuukka" as parameter):

.. code-block:: clj

  (print "Hello there," "Tuukka")

We can also manipulate code with macros:

.. code-block:: clj

  => (defmacro rev [code]
  ...  (setv op (last code) params (list (butlast code)))
  ...  `(~op ~@params))
  => (rev (1 2 3 +))
  6

The code that was generated with this macro just switched around some of the
elements, so by the time program started executing, it actually reads:

.. code-block:: clj

  (+ 1 2 3)

Sometimes it's nice to be able to call a one-parameter macro without
parentheses. Tag macros allow this. The name of a tag macro is typically
one character long, but since Hy operates well with Unicode, we aren't running
out of characters that soon:

.. code-block:: clj

  => (deftag ↻ [code]
  ...  (setv op (last code) params (list (butlast code)))
  ...  `(~op ~@params))
  => #↻(1 2 3 +)
  6

Macros are useful when one wishes to extend Hy or write their own
language on top of that. Many features of Hy are macros, like ``when``,
``cond`` and ``->``.

What if you want to use a macro that's defined in a different
module? The special form ``import`` won't help, because it merely
translates to a Python ``import`` statement that's executed at
run-time, and macros are expanded at compile-time, that is,
during the translate from Hy to Python. Instead, use ``require``,
which imports the module and makes macros available at
compile-time. ``require`` uses the same syntax as ``import``.

.. code-block:: clj

   => (require tutorial.macros)
   => (tutorial.macros.rev (1 2 3 +))
   6

Hy <-> Python interop
=====================

Using Hy from Python
--------------------

You can use Hy modules in Python!

If you save the following in ``greetings.hy``:

.. code-block:: clj

    (defn greet [name] (print "hello from hy," name))

Then you can use it directly from Python, by importing Hy before importing
the module. In Python::

    import hy
    import greetings

    greetings.greet("Foo")

Using Python from Hy
--------------------

You can also use any Python module in Hy!

If you save the following in ``greetings.py`` in Python::

    def greet(name):
        print("hello, %s" % (name))

You can use it in Hy (see :ref:`import`):

.. code-block:: clj

    (import greetings)
    (.greet greetings "foo")

More information on :doc:`../language/interop`.


Protips!
========

Hy also features something known as the "threading macro", a really neat
feature of Clojure's. The "threading macro" (written as ``->``) is used
to avoid deep nesting of expressions.

The threading macro inserts each expression into the next expression's first
argument place.

Let's take the classic:

.. code-block:: clj

    (require [hy.contrib.loop [loop]])

    (loop (print (eval (read))))

Rather than write it like that, we can write it as follows:

.. code-block:: clj

    (require [hy.contrib.loop [loop]])

    (-> (read) (eval) (print) (loop))

Now, using `python-sh <http://amoffat.github.com/sh/>`_, we can show
how the threading macro (because of python-sh's setup) can be used like
a pipe:

.. code-block:: clj

    => (import [sh [cat grep wc]])
    => (-> (cat "/usr/share/dict/words") (grep "-E" "^hy") (wc "-l"))
    210

Which, of course, expands out to:

.. code-block:: clj

    (wc (grep (cat "/usr/share/dict/words") "-E" "^hy") "-l")

Much more readable, no? Use the threading macro!
