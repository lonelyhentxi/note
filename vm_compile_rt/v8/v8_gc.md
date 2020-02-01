# V8 GC

[Reference](https://www.cnblogs.com/xiaoyuxy/p/12258874.html)

## V8 Memory Structure

![v8_memory_structure](./v8_memory_structure.png)

## New Space

Divide to two semi space, managed by Scavenger (Minor GC).

Scan the current semi space, sweep the dead ones, and move the lives to another semi space.

## Old Space

After two scan at the new space, if a object still survive, it will be moved to the old space.

The old pointer space contains all objects which have pointers referring to others. And the old data space contains all objects which only have data in them.

And the space use Major GC (mark-sweep & mark-compact).

![major_gc](major_gc.gif)

## Big Object Space

It contains the objects which are large than the other space. It will not be clear by gc.

## Code Space

It contains the code compiled by JIT.

## Map and Cell Space

Storing the map and cell data, they all have simple structure.