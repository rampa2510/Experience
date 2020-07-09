# Architecture
1) Operations can be perfromed on same data types
2) While sending string through api calls we can convert string to bytes(utf8) implementation or rune (utf32) implementation to reduce data size.
3) When appedning to a slice how go completes the process is 
   1) If the length of new slice is less than double its capacity of previous slice then the capacity of new slice is doubled from the previous one and elements are appended
   2) If the length of new slice is more than double the capacity of previous slice it equates the capacity to the length of new element.
   3) If the data type increase like int32 it equates the length to be 25% more than the length of new array
4) Make func returns a pointer
5) Slices are actually refrences to actual memory blocks so they are called by refrences
6) Array are call by value 
7) If the first letter of var is CAPITAL the varaible will be exposed at global level
8) If the slice you want to append to is very big then what we can do is use the make func to create a slice with the cap we want and then append to it