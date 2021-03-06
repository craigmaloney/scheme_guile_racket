:title: Introduction to Scheme / Guile / Racket
:author: Craig Maloney
:css: presentation.css

.. title:: Introduction to Scheme / Guile / Racket
 
----

Introduction to Scheme, Guile, and Racket
=========================================

* Author: Craig Maloney
* Email: craig@decafbad.net
* Website: http://decafbad.net
* Artwork: David Revoy (CC-BY 4.0) http://peppercarrot.com

----

Three Types of Folks...
=======================

----

.. image:: images/scared.jpg
    :width: 800px

.. note::
    You've heard a little bit about LISP and have heard horror stories about it. You're bracing yourself for being overwhelmed with LISP and an arcane syntax of cars and cdrs (but you came to the talk anyway, so that's cool!).

----

.. image:: images/flying.gif

.. note::
    You've dabbled with LISP and want to dive in and learn more (and believe me, once you've dived in it never ends, much like this .gif)

----

.. image:: images/laser.gif

.. note::
    You just thought this was interesting but have no idea what those two last slides were about (but that's OK, because Scheme is designed as a teaching language, and Racket even more so).


----

Our story begins...
===================

----

Craig Decides to Learn JavaScript...
====================================

----

.. image:: images/pepper_studying.png
   :width: 800px

----

But somewhere I heard that JavaScript borrowed from Scheme...
=============================================================

----

So naturally...
===============

----

.. image:: images/pepper_exit.png
   :width: 800px

----

But it's not as orthogonal as you might imagine...
==================================================

----

:id: crockford

::

    JavaScript’s C-like syntax, including curly braces and the clunky
    for statement, makes it appear to be an ordinary procedural
    language. This is misleading because JavaScript has more in 
    common with functional languages like Lisp or Scheme
    than with C or Java.

**Douglas Crockford "Javascript, the Good Parts"**

::

    http://journal.stuffwithstuff.com/2013/07/18/javascript-isnt-scheme/

----

So why play with Scheme?
========================

----

.. image:: images/pepper_symbols.jpg
   :width: 500px

----

.. image:: images/pepper_summoning.png
   :width: 800px

----

But really...
=============

.. image:: images/pepper_tea.png
   :width: 800px

----

What we'll cover
================

* A brief history of Scheme
* Basic Scheme syntax
* Guile (GNU Scheme)
* Racket (DrRacket)

----

What's a Scheme?
================

* Created by Guy L. Steele and Gerald Jay Sussman

* Scheme is one of the main dialects of LISP

* Development is documented in "The Lambda Papers"

  * (1975-1980)
* Has had many standardization efforts

  * R5RS (1998)

  * R6RS (2007)
    
  * R7RS (2013)

----

:id: scheme

Alas...
=======

::

    Alas: Scheme has the unhappy distinction of being the world's most
    unportable programming language. It is almost misleading to call Scheme a
    "programming language;" it would be more accurate to characterise Scheme as
    a family of dialects, all loosely related by the common features of lexical
    scope, dynamic typing, list structure, higher-order functions, proper
    tail-recursion, garbage collection, macros, and (some form of) s-expression
    based lexical syntax.

    [...]

    This has led to a great deal of unnecessary fragmentation and duplication
    of effort: instead of standing on the shoulders of previous programmers,
    Schemers step on each others' toes.

    http://www.scheme-reports.org/2009/position-statement.html

----

Starting simple
===============

----

A Few Basic Datatypes
=====================

Atoms:

* ``2``
* ``4.2``
* ``"Hello World"``
* ``#f``
* ``'a``

----

Lists:

* ``(list 3 4 5)``
* ``'(3 4 5)``
* ``(cons 3 (cons 4 (cons 5 '())))``

----

Datatypes:

* ``(string? "quack")  => #t``
* ``(symbol? 'quack)  => #t``
* ``(string? 'quack)  => #f``

* ``(number? 42)  => #t``
* ``(number? 4.2)  => #t``
* ``(number? 1+1i)  => #t``
* ``(real? 1+1i)  => #f``

----

Datatypes (cont.):

* ``(real? 2/3)  => #t``
* ``(exact? 2/3)  => #t``
* ``(exact? 2.0)  => #f``
* ``(integer? 2.0)  => #t``
* ``(exact? (/ 4.0 2.0))  => #f``
* ``(integer? (/ 4.0 2.0))  => #t``

----

Quick note on symbols / strings
===============================

Symbols are "interned"; there is only one copy stored of a particular symbol:

* ``(define foo 'quack)``
* ``(define bar 'quack)``
* ``(eq? foo bar)  => #t``

----

Quick note on symbols / strings
===============================

Strings are not; each string is a separate object:

* ``(define foo "quack")``
* ``(define bar "quack")``
* ``(eq? foo bar)  => #f``

----

Arithmetic Operators
====================

* ``(+ 2 3)``  => 5
* ``(- 2 3)``  => -1
* ``(* 2 3)``  => 6
* ``(/ 2 3)``  => 2/3

----

More Arithmetic Operators
=========================

* ``(+ 4 5 6 7 8)``  => 30
* ``(- 4 (* 6 5))``  => -26
* ``(/ 2 3 5)``  => 2/15
* ``(* 8 8 8)`` => 512

----

Variables
=========

* ``(define two 2)``
* ``(+ two two)``  => 4
* ``(define two 3)``
* ``(+ two two)``  => 6

----

More Variables
==============

* ``(define two (+ 1 1))``
* ``(define four (+ two two))``
* ``four``  => 4
* ``(define eight (* four two))``
* ``eight``  => 8

----

Latent typing
=============
* ``(define duck 4)``
* ``(number? duck)  => #t``
* ``(set! duck 'quack)``
* ``(number? duck)  => #f``

----

Those are some nice parlour tricks...
=====================================

----

Built-in functions
==================

* ``(number? 4)``  => #t
* ``(number? 'moo)``  => #f
* ``(symbol? 'moo)``  => #t
* ``(zero? 4)``  => #f
* ``(string? "Hello, I am a string")``  => #t
* ``(null? '())``  => #t
* ``(list? '())``  => #t

----

Simple procedure (function)  for computing n^2
==============================================

* ``(define (square x) (* x x))``
* ``(define square (lambda(x) (* x x)))``

----

Pythagorean Theorem in Scheme
=============================

(Remember: a^2 + b^2 = c^2)

* ``(+ (square 5) (square 6))  => 61``

----

Procedures are variables
========================

::

    scheme@(guile-user)> (define (foo x) x)
    scheme@(guile-user)> ,tr (foo 4)
    trace: |  (#<procedure 1cf50e0> #(#<directory (guile-user) 1434c60> #f))
    trace: |  #(#<directory (guile-user) 1434c60> foo)
    trace: (#<procedure 1cff540 at <current input>:4:0 ()>)
    trace: (foo 4)
    trace: 4

    scheme@(guile-user)> (define bar foo)
    scheme@(guile-user)> ,tr (bar 5)
    trace: |  (#<procedure 1d33580> #(#<directory (guile-user) 1434c60> #f))
    trace: |  #(#<directory (guile-user) 1434c60> bar)
    trace: (#<procedure 1d41980 at <current input>:5:0 ()>)
    trace: (foo 5)
    trace: 5
    scheme@(guile-user)>

----

Procedures are variables
========================

::

    scheme@(guile-user)> (define (foo x) (display "I refuse"))
    scheme@(guile-user)> ,tr (bar 4)
    trace: |  (#<procedure 1d7b100> #(#<directory (guile-user) 1434c60> #f))
    trace: |  #(#<directory (guile-user) 1434c60> bar)
    trace: (#<procedure 1d85560 at <current input>:6:0 ()>)
    trace: (foo 4)
    trace: 4
    scheme@(guile-user)> ,tr (foo 4)
    trace: |  (#<procedure 1da7d60> #(#<directory (guile-user) 1434c60> #f))
    trace: |  #(#<directory (guile-user) 1434c60> foo)
    trace: (#<procedure 1dad240 at <current input>:6:0 ()>)
    trace: (foo 4)
    trace: (display "I refuse")
    I refusetrace: #<unspecified>
    scheme@(guile-user)>

----

Lambda
======

* ``(define (square x) (* x x))``
* ``(define square (lambda (x) (* x x)))``
* ``(square 4)  => 16`` 
* ``(lambda (x) (* x x))  => #<procedure 257c380 at <current input>:3:0 (x)>``
* ``((lambda (x) (* x x)) 4)  => 16``

----

Multiple Arguments
==================

Immediately-invoked Lambda:

* ``((lambda (x y) (+ x y)) 3 4) => 7``

Procedure:

* ``(define (add2 x y) (+ x y))``
* ``(add2 3 4)  => 7``

Lambda Procedure:

* ``(define ladd2 (lambda (x y) (+ x y))``
* ``(ladd2 3 4)  => 7``

----

Conditionals
============

::

    (if (zero? 0)
      (display "True\n")
      (display "False\n"))

    => True

----

Another Conditional
===================

::

    (cond 
      ((zero? 0) (display "True\n"))
      ((not (zero? 1)) (display "Also True\n")))

    => True

----

Equality (eq)
=============

* ``eq?`` => Return #t if x and y are the same object, except for numbers and characters.

  * ``(define foo 5)``
  * ``(define bar foo)``
  * ``(eq? foo bar)  => #t``
  * ``(eq? foo 5)  => #t``

----

Equality (eq)
=============

* ``eq?`` => Return #t if x and y are the same object, except for numbers and characters.

  * ``(define foo "5")``
  * ``(define bar "5")``
  * ``(eq? foo bar)  => #f``

----

Equality (equal)
================

* ``equal?`` => Return #t if x and y are the same type, and their contents or value are equal.

  * ``(define foo "5")``
  * ``(define bar "5")``
  * ``(equal? foo bar)  => #t``
  * ``(string=? foo bar)  => #t``

----

Equality (equal)
================

* ``equal?`` => Return #t if x and y are the same type, and their contents or value are equal.


  * ``(define foo '(3 4 5)``
  * ``(define bar '(3 4 5)``
  * ``(equal? foo bar)  =>  #t``
  * ``(eq? foo bar)  => #f``

----

Fibonacci Sequence
==================

::

  (define (fib n)
    (cond
      ((= n 0) 0)
      ((= n 1) 1)
      (else
        (+ (fib (- n 1))
           (fib (- n 2))))))

    scheme@(guile-user) [2]> (fib 30)
    $3 = 832040

----

Map
===

``(map (lambda (x) (* x x)) '(1 2 3 4 5)``

``=> (1 4 9 16 25)``

----

Filter
======

``(filter (lambda (x) (zero? (remainder x 2))) '(4 5 6 6 7 8))``

``=> (4 6 6 8)``

``(filter (lambda (x) (number? x)) '(3 4 5 "no"))``

``=> (3 4 5)``
           
----

The difference between map and filter
=====================================

* ``map`` applies the function / procedure against all items:

``(map (lambda (x) (number? x)) '(3 4 5 "no"))``

``=> (#t #t #t #f)``

----

The difference between map and filter
=====================================

* ``filter`` only permits items that return #t in the procedure / function:

``(filter (lambda (x) (number? x)) '(3 4 5 "no"))``

``=> (3 4 5)``

----

Local Variables / Environment
=============================

* Each procedure can have its own local environment
* We can create local variables with ``let``

----

::

    (define s 42)

    (define false-answer
      (lambda ()
        (let ((s #f))
          (display s))))

    (false-answer)  => #f

    (display s)  => 42

----

Closures
========

::

    (define counter
      (let ((count 0)) 
        (lambda (x) 
          (set! count (+ x count))
          count)))

    scheme@(guile-user)> (counter 4)
    $1 = 4
    scheme@(guile-user)> (counter 4)
    $2 = 8
    scheme@(guile-user)> (counter 4)
    $3 = 12
    scheme@(guile-user)> 

----

.. image:: images/taste.jpg
   :width: 500px

----

Implementations...
==================

----

Guile
=====

----

Guile
=====

* Part of the GNU Project
* GNU Ubiquitous Intelligent Language for Extensions
* Allows developers to use Scheme as an extension language in C code
* Used in Guix, GNU Cash, GDB, Lilypond, and more
* Not just for Scheme; also has partial ECMAScript 3 support

----

Demo of Guile
=============

----

Racket
======

----

Racket
======

* Started off life as PLT Scheme, renamed "Racket" at version 5.0
* Used as a teaching language
* Described in several books (How to Learn Programming, Realm of Racket)
* Adds many procedures to Scheme, and includes a graphical IDE (DrRacket)
 
----

Demo of DrRacket
================

----

Further down the rabbit hole
============================

.. image:: images/black_hole.jpg
   :width: 800px

----

Further reading
===============

* `Structures and Interpretation of Computer Programming <https://mitpress.mit.edu/sicp/full-text/book/book.html>`_
* `How to Design Programs <http://www.htdp.org/>`_
* `Realm of Racket <http://realmofracket.com/>`_
* `Guile Manual <https://www.gnu.org/software/guile/manual/>`_
* The Little Schemer / The Seasoned Schemer

----

Links
=====

* `Schemers.org <http://schemers.org/>`_
* `SICP Guile <https://github.com/zv/SICP-guile>`_
* `Learn Scheme in 15 minutes <https://github.com/zv/SICP-guile>`_

----

Thank you!
==========

----

Introduction to Scheme, Guile, and Racket
=========================================

* Author: Craig Maloney
* Email: craig@decafbad.net
* Website: http://decafbad.net
* Artwork: David Revoy (CC-BY 4.0) http://peppercarrot.com
