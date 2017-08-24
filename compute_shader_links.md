# Compute shader

## Basic things.

This is the [introduction video](https://channel9.msdn.com/Events/PDC/PDC09/P09-16) by Microsoft.
It covers all features of ComputeShaders.
Also there is the [Microsoft Channel9 Blog](https://blogs.msdn.microsoft.com/chuckw/2010/07/14/directcompute/).


The [Microsoft RWTexture2D](https://msdn.microsoft.com/de-de/library/windows/desktop/ff471505(v=vs.85).aspx) documentation.

This is a [simple compute shader example](https://github.com/walbourn/directx-sdk-samples/tree/master/BasicCompute11) showing
how to use StructuredBuffer.

## Gaussian Blur filter

https://www.gamedev.net/topic/635255-texture-unorderedaccessview-problem-compute-shader/

## Basic example
http://stackoverflow.com/questions/32049639/directx-11-compute-shader-writing-to-an-output-resource

## Fluid simulation
https://developer.nvidia.com/gpugems/GPUGems/gpugems_ch38.html

## Particle Demo
http://stackoverflow.com/questions/20728757/unity-compute-shader-array-indexing-through-sv-dispatchthreadid

The [AMD holy smoke](https://de.slideshare.net/DevCentralAMD/holy-smoke-faster-particle-rendering-using-direct-compute-by-gareth-thomas)
presentation. It describes using compute shaders to build a particle system.

[Bitonic sort](https://code.msdn.microsoft.com/windowsdesktop/DirectCompute-Basic-Win32-7d5a7408) is a simple algorithm that works by sorting the data set into alternating ascending and 
descending sorted sequences. These sequences can then be combined and sorted to produce larger sequences. 
This is repeated until you produce one final ascending sequence for the sorted data.
