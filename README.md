LIBF 0.1
========

Libf is a functional programming extension of C.

The basic idea is to take C++11, and strip out a functional programming subset
that is also a superset of C.  This part is relatively easy, however, the
problem is that the C++ standard library is completely unsuitable for
functional programming.  For example:

    vector<size_t> xs;
    for (size_t i = 0; i < 10; i++)
        xs.push_back(i);

This is typical OOP-style where `xs' is a *mutable* object.  In LibF the
equivalent code is the following:

    Vector<size_t> xs;
    for (size_t i = 0; i < 10; i++)
        xs = push_back(xs, i);

Unlike stdlib++, where each xs is an *immutable* object, making the following
code possible:

    Vector<size_t> xs;
    for (size_t i = 0; i < 5; i++)
        xs = push_back(xs, i);
    Vector<size_t> ys = xs;
    for (size_t i = 0; i < 5; i++)
        xs = push_back(xs, i);
    printf("%s\n%s\n", cstr(string(ys)), cstr(string(xs)));

This code will print:

    <0,1,2,3,4>
    <0,1,2,3,4,0,1,2,3,4>

The object ys was unaffected by the changes made to xs.  And just like
stdlib++, the vector push_back operation is still O(1) -- although the
constant factor may differ!

LibF currently provides the following basic data types:

* Lists.
* Maps.
* Strings.
* Tuples.
* Vectors.

Each of these is immutable, and has complexity similar to that of the stdlib++
equivalent.

Other features:
---------------

* *Garbage Collected* libf uses automatic memory management provided by the
  Boehm garbage collector.
* *Strict* C and C++ are strict (not lazy) languages, so libf is also strict.
* *Discriminated Union Types* in the form of the Multi pointer type.
* *Small binaries* The implementation handles polymorphism by casting to void.

C++ subset:
-----------

The idea is to restrict ourself to a *minimal* functional C++ subset.  The
following are the main features we use.  Everything else should be discarded.

The features we use are:

* Lambda syntax.
* Templates (for polymorphism).

Everything else should be discarded to avoid feature creep.

Building:
---------

The only non-standard dependency for libf is the Boehm garbage collector.  It
can be installed as follows on Linux:

    $ sudo apt-get install libgc-dev

Afterwards, just run make:

    $ make
