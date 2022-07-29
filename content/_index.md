---
title: "LibMEOS"
date: 2022-07-28T12:25:21Z
draft: false
---

## Disclaimer: The MEOS library and this website are in development!

MEOS (Mobility Engine, Open Source) is a C library which makes it easy to work with temporal and spatio-temporal data. It is based on [MobilityDB's](https://mobilitydb.com) data types and functions.

MobilityDB extends [the ISO 19141:2008](https://www.iso.org/standard/41445.html) standard (Geographic information — Schema for moving features) for representing the change of non-spatial attributes of features, which is not covered by the standard. It also takes into account the fact that when collecting mobility data it is necessary to represent “temporal gaps”, that is, for some period of time no observations were collected, for instance, due to GPS signal loss.

MEOS is heavily inspired by a similar library called [GEOS](https://libgeos.org/) (Geometry Engine, Open Source, — hence the name. A [first version](https://github.com/adonmo/meos)) of the MEOS library written in C++ has been proposed by Krishna Chaitanya Bommakanti. However, due to the fact that MEOS codebase is actually a subset of MobilityDB codebase, which is almost entire written in C, this version of the library allows us to evolve both programming environments simultaneously.
