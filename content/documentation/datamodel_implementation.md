---
title: "Implementation of the Data Model"
date: 2022-07-29T13:57:17+02:00
draft: false
---

Although MEOS is written in C, it aims at using well-proven object-oriented concepts for its development. The 
 [data model]({{< ref "/documentation/datamodel" >}} "Data Model") for the temporal types is implemented using the following conceptual class hierarchy.

<img src="/images/meos_subtypes.png" alt="Temporal class hierarchy" style="width: 50%;height: auto;"/>

The superclass `Temporal` is a *template* class since it is used for all temporal types, such as temporal integers or temporal points.
The specific type of an instance is defined in the `temptype` attribute. `Temporal` is also an *abstract* class, since it cannot be instantiated, only one its subtypes can be instantiated. The specific subtype of an instance is specified in the `subtype` attribute. Finally, the `flags` attribute keeps internal information such as whether the interpolation is linear or not, whether there is a `Z` dimension for temporal types, etc.

The class `TInstant` is used for representing a *single observation* composed of a `value` and a timestamp `t`. Timestamps in MEOS keep associated time zone information. For temporal instant values, the `flags` attribute specifies whether the `value` is passed by value or by reference.

The class `TInstantSet` is used for representing a *set of observations* without any assumption about what happens between observations. The `count` attribute specifies the number of such observations and the `bbox` attribute specifies the bounding box that encompasses them. This bounding box can be of various types depending on the temporal type: `period` keeping only the time frame for temporal booleans and temporal texts, `TBOX` keeping the minimum and maximum values and timestamps for temporal numbers, and `STBOX` keeping the minimum and maximum values of 2D/3D coordinates and timestamps for temporal points. Finally, the `instants` attribute keeps the observations.

The class `TSequence` is used for representing a *set of observations* with the assumption that an interpolation function can be used for determining the value between two consecutive observations. MEOS provides two interpolation functions: *linear*, where the value evolves linearly between consecutive observations, and *stepwise* where the value states constant between consecutive observations. For temporal sequence values, the `flags` attribute specifies the interpolation function used. The `count` attribute specifies the number of  observations, the `bbox` attribute specifies the bounding box that encompasses them, the `period` atribute specifies their time frame, and the `instants` attribute keeps the observations.

The class `TSequenceSet` is used for representing a *set of sequences* having the same interpolation function that may have *temporal gaps* where the value is not known. This situation typically arrives in real-world applications due to signal loss. The `count` attribute specifies the number of such sequences, while the `totalcount` attribute specifies the number of observations across all sequences. Finally, the `bbox` attribute specifies the bounding box that encompasses all observations and the `sequences` attribute keeps the composing sequences.

Examples of values of the four temporal subtypes are given next.

<img src="/images/meos_subtypes_values.png" alt="Example of temporal values" style="width: 80%;height: auto;"/>




