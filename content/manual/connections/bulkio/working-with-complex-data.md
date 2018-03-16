---
title: "Working with Complex Data"
weight: 40
---

If the StreamSRI mode field of the incoming data is set to 1, the associated input data is complex (i.e., it is comprised of real and imaginary parts). Complex data is sent as alternating real and imaginary values. A developer can work with this data in any fashion; however, this section provides common methods for converting the data into a more workable form.

### Converting Complex Data in C++

In C++, the incoming BulkIO data block provides a `complex()` method to check whether the data is complex, and `cxdata()` and `cxsize()` methods to provide access to the sample data as an array of `std::complex` values. For example:

```c++
bulkio::ShortDataBlock = stream.read();
if (block.complex()) {
    const std::complex<short>* data = block.cxdata();
    const size_t size = block.cxsize();
}
```

### Converting Complex Data in Python

The helper functions `bulkioComplexToPythonComplexList` and `pythonComplexListToBulkioComplex`, defined in the module `ossie.utils.bulkio.bulkio_helpers`, provide an efficient translation to and from lists of Python complex numbers.

### Converting Complex Data in Java

Unlike with C++ and Python, Java does not have a ubiquitous means for representing complex numbers; therefore, when using Java, users are free to map the incoming BulkIO data to the complex data representation of their choosing.
