Efficient Neighbohood Ops
=========================

A topic from the seminar "BioImage Analysis Algorithms - Theory and Applications" mentored by Christian Dietz at the University of Konstanz.

Goal
----

 1. Benchmark existing neighborhood operations and algorithms on images in the imglib2 and imagej2-ops libraries.
 2. Find a more efficient way to do neighborhood operations
 3. Implement one or two algorithms using the new method.

The Projects
------------

 - **neighborhood-bench:** Maven project benchmarking imagej2-ops and imglib2 using the jmh benchmarking library.

*Future:*

 - **imglib2:** git submodule linked to a fork of imglib2 containing the branch in which the new neighborhood operations is going to be implemented.
 - **imagej2-ops:** git submodule linked to a fork of imagej2-ops containing the branch in which the algorithms are going to be implemented.

Attempts for Optimization
-------------------------

 - **dim0-opt**: Very small optimization providing a final shortcut for access to `dimensions[ 0 ]` in RectangleNeighborhoodCursor. See [this commit](https://github.com/Squareys/imglib2-algorithm/commit/94513c19ef65cc7968b9c5b30554d83b03c21161). Speedup was ~2-4%, should be actually verified by some new benchmark results, though.
 - **co-opt**: Have the structuring element stored as a set of concecutive offests. An approach wich was used in some imglib before. Problem here was that the random accessible was always moved with `ra.move()` which obviously is mildly inefficient for RA's with optimized `fwd()` methods. The next optimization attemp tried to get rid of this:
 - **cmo-opt**: Have the structuring element stored as a set of conecutive move operations, therefore, Operations which move the random accessible correctly. This removes need for `nextLine()`, but was not efficient at all. Speedup was ~(-100)%.
 - **racarr-opt**: Use an array of RandomAccess as a structuring element. This way, the structuring element could be moved with the cursor once. Since one usually only iterates over it once, this is not a use case we should optimize for. Speedup ~(-300)% for MinimumFinder benchmark. See [this commit](https://github.com/Squareys/imglib2-algorithm/commit/5bce45bf3c1dbdbf77ebee14cda45117652075f8)
