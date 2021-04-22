The Schema Field
==================================

##################################
Game-specific implementations
##################################

You might be confused as to how one single format can possibly anticipate everything a rhythm game could throw at it.

Simple: It doesn't try to.

RGC works as a skeleton schema. It requires certain properties to be defined, then uses a parameter to define a ``schema``.

This ``schema`` defines an extension to the spec, for what that specific game may want.

With this, we can enforce sensible properties for games, while giving the schema free reign to extend where necessary.

##################################
Avoiding Fragmentation
##################################

This is cool and all, but surely this suffers the standard fragmentation mentioned before? What if two people define different schemas for one game?
Do I have to make my own schema?

This has also been thought through. The ``schema`` string property in RGC **MUST** follow a ``namespace/name`` format.

As an example, if I wanted to define my own schema for CHUNITHM, I would use the schema name ``zkldi/chunithm``, or similar.

The point of the namespacing is to vastly reduce chances of collision between users.

And as for avoiding initial fragmentation, the RGC spec also defines a standard library of schemas for popular games. These are prefixed with the ``std/`` namespace.

The RGC spec reserves the ability to define anything under the ``std/`` namespace, and as such, you **MUST** not implement custom schemas inside of it.

As an example, if I wanted to parse Single Player IIDX, I would use the ``std/iidx-SP`` schema.

.. note::

    There are other restrictions on what characters can go inside the ``schema`` field. These restrictions can be found at :ref:`rgc_meta`

##################################
Why custom schemas?
##################################

There are a lot of rhythm games, and it is a lot of work to define a schema for every single one. With dedicated custom schema support, people are free to define that part themselves.

Those games may become a part of the standard in the future, and because of the namespacing, that would not break existing custom schemas for that game.

##################################
What can a schema extend?
##################################

A schema is free to add any field inside ``chartMeta``, and is free to define new Event Types inside ``events``. More information about this can be found at :ref:`rgc_events`.