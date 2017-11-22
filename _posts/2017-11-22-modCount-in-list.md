---
layout:     post
title:      "modCount in list"
subtitle:   ""
date:       2017-11-22
author:     "Kanon"
header-img: "/img/post-bg-2015.jpg"
catalog: true
tags:
    - java
---

The number of times this list has been <i>structurally modified</i>.Structural modifications are those that change the size of the list, or otherwise perturb it in such a fashion that iterations in
progress may yield incorrect results.

<p>This field is used by the iterator and list iterator implementation
returned by the {@code iterator} and {@code listIterator} methods.
If the value of this field changes unexpectedly, the iterator (or list
iterator) will throw a {@code ConcurrentModificationException} in
response to the {@code next}, {@code remove}, {@code previous},
     * {@code set} or {@code add} operations.  This provides
     * <i>fail-fast</i> behavior, rather than non-deterministic behavior in
     * the face of concurrent modification during iteration.
     *
     * <p><b>Use of this field by subclasses is optional.</b> If a subclass
     * wishes to provide fail-fast iterators (and list iterators), then it
     * merely has to increment this field in its {@code add(int, E)} and
     * {@code remove(int)} methods (and any other methods that it overrides
     * that result in structural modifications to the list).  A single call to
     * {@code add(int, E)} or {@code remove(int)} must add no more than
     * one to this field, or the iterators (and list iterators) will throw
     * bogus {@code ConcurrentModificationExceptions}.  If an implementation
     * does not wish to provide fail-fast iterators, this field may be
     * ignored.
