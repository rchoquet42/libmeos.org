---
title: "Data model"
date: 2022-07-29T13:57:17+02:00
draft: false
---

MEOS provides **temporal types**, which represent the evolution on time of values of some element type, called the **base type** of the temporal type. For instance, temporal integers may be used to represent the evolution on time of the gear used by a moving car. In this case, the data type is temporal integer and the base type is integer. Similarly, a temporal float may be used to represent the evolution on time of the temperature of the interior of a car. As another example, a temporal point may be used to represent the evolution on time of the location of a car, as reported by GPS devices. Temporal types are useful because representing values that evolve in time is essential in many applications, especially in mobility applications. Furthermore, the operators on the base types (such as arithmetic operators for integers and floats, spatial relationships and distance for geometries) can be intuitively generalized when the values evolve in time.

MEOS has six built-in temporal types, namely `tbool`, `tint`, `tfloat`, `ttext`, `tgeompoint`, and `tgeogpoint`, which are, respectively, based on the alphanumeric base types bool (a boolean value), int (a 4-byte integer number), float (an 8-byte floating point number), and text (a string of characters of variable size), as well as the geospatial base types geometry, and geography borrowed from [PostGIS](https://postgis.net/) and restricted to 2D or 3D points with Z dimension.

The **interpolation** of a temporal value states how the value evolves between successive instants. The interpolation is **stepwise** when the value remains constant between two successive instants. For example, the gear used by a moving car may be represented with a temporal integer, which indicates that its value is constant between two time instants. On the other hand, the interpolation is **linear** when the value evolves linearly between two successive instants. For example, the temperature inside a car may be represented with a temporal float, which indicates that the values are known at the two time instants but continuously evolve between them. Similarly, the location of a vehicule may be represented by a temporal point where the location between two consecutive GPS readings is obtained by linear interpolation. Temporal types based on **discrete** base types, that is the tbool, tint, or ttext evolve necesssarily in a stepwise manner. On the other hand, temporal types based on **continuous** base types, that is tfloat, tgeompoint, or tgeogpoint may evolve in a stepwise or linear manner.

The **subtype** of a temporal value states the temporal extent at which the evolution of values is recorded. Temporal values come in four subtypes, namely, **instant**, **instant set**, **sequence**, and **sequence set**.

A temporal value of **instant** subtype (briefly, an instant value) represents the value at a time instant, for example

`'17@2018-01-01 08:00:00'`

A temporal value of **instant set** subtype (briefly, an instant set value) represents the evolution of the value at a set of time instants, where the values between these instants are unknown. An example is as follows:

`'{17@2018-01-01 08:00:00, 17.5@2018-01-01 08:05:00, 18@2018-01-01 08:10:00}'`

A temporal value of **sequence** subtype (briefly, a sequence value) represents the evolution of the value during a sequence of time instants, where the values between these instants are interpolated using either a stepwise or a linear function (see below). An example is as follows:

`'(10@2018-01-01 08:00:00, 20@2018-01-01 08:05:00, 15@2018-01-01 08:10:00]'`

As can be seen, a sequence value has a lower and an upper bound that can be inclusive (represented by ‘[’ and ‘]’) or exclusive (represented by ‘(’ and ‘)’). A sequence value with a single instant such as `'[10@2018-01-01 08:00:00]'` is called an **instantaneous** sequence. In that case, both bounds must be inclusive.

The value of a temporal sequence is interpreted by assuming that the period of time defined by every pair of consecutive values v1@t1 and v2@t2 is lower inclusive and upper exclusive, unless they are the first or the last instants of the sequence and in that case the bounds of the whole sequence apply. Furthermore, the value taken by the temporal sequence between two consecutive instants depends on whether the **interpolation** is **stepwise** or **linear**. For example, the temporal sequence above

`'(10@2018-01-01 08:00:00, 20@2018-01-01 08:05:00, 15@2018-01-01 08:10:00]'`

represents that the value evolves from `10` to `20` during `(2018-01-01 08:00:00, 2018-01-01 08:05:00)` and evolves from `20` to `15` during `[2018-01-01 08:05:00,2018-01-01 08:10:00]`. On the other hand, the temporal sequence

`'Interp=Stepwise;(10@2018-01-01 08:00:00, 20@2018-01-01 08:05:00, 15@2018-01-01 08:10:00]'`

represents that the value is `10` during `(2018-01-01 08:00:00, 2018-01-01 08:05:00)`, `20` during `[2018-01-01 08:05:00,2018-01-01 08:10:00)`, and `15` at the end instant `2018-01-01 08:10:00`.

Finally, a temporal value of **sequence set** subtype (briefly, a sequence set value) represents the evolution of the value at a set of sequences, where the values between these sequences are unknown. An example is as follows:

`{[17@2018-01-01 08:00:00, 17.5@2018-01-01 08:05:00], [18@2018-01-01 08:10:00, 18@2018-01-01 08:15:00]}`

Each temporal type is associated to another type, referred to as its **bounding box**, which represent its extent in the value and/or the time dimension. The bounding box of the various temporal types are as follows: 

- The `period` type for the `tbool` and `ttext` types, where only the temporal extent is considered.
- A `tbox` (temporal box) type for the `tint` and `tfloat` types, where the value extent is defined in the X dimension and the temporal extent in the T dimension.
- A `stbox` (spatiotemporal box) type for the `tgeompoint` and `tgeogpoint` types, where the spatial extent is defined in the X, Y, and Z dimensions (where the Z dimension is optional), and the temporal extent in the T dimension.

A rich set of functions and operators is available to perform various operations on temporal types.




