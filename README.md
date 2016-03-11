sortmerna
=========

[![Build Status](https://travis-ci.org/biocore/sortmerna.png?branch=master)](https://travis-ci.org/biocore/sortmerna)

This is ongoing development of SortMeRNA. See 'release' on this page for
releases or visit http://bioinfo.lifl.fr/RNA/sortmerna/ for more information.

NOTE: the Clang compiler on Mac (distributed through Xcode) does not support multithreading.
A preliminary implementation of OpenMP for Clang has been made at "http://clang-omp.github.io"
though has not been yet incorporated into the Clang mainline. The user may follow the
steps outlined in the above link to install the version of Clang with multithreading support, 
though this version has not yet been tested with SortMeRNA. Otherwise, the user is 
recommended to install the original GCC compiler via MacPorts (contains full multithreading support).
  
Documentation:
--------------

If you have [Doxygen](http://www.stack.nl/~dimitri/doxygen/) installed, you can generate the documentation
by modifying the following lines in ```doxygen_configure.txt```:

```
INPUT = /path/to/sortmerna/include /path/to/sortmerna/src
IMAGE_PATH = /path/to/sortmerna/algorithm
```

and running the following command:

```
doxygen doxygen_configure.txt
```

This command will generate a folder ```html``` in the directory from which the
command was run.


To compile on Linux OS:
-----------------------

NOTE: You will require ```autoconf``` to build from the cloned
repository or from source code in the `Source code` tar
balls under release Downloads. ```autoconf``` is installable
via ```conda```, see [here](https://anaconda.org/biobuilds/autoconf-update) for details.

(0) Prepare your build system for compilation:

```bash
bash autogen.sh
```

(1) Check your GCC compiler is version 4.0 or higher:

```bash
gcc --version
```

(2) Run configure and make scripts:

```bash
./configure
make
```

(3) To install:

```bash
make install
```

You can define an alternative installation directory by
specifying ```--prefix=/path/to/installation/dir``` to ```configure```.


To compile on Mac OS:
---------------------

NOTE: You will require ```autoconf``` to build from the cloned
repository or from source code in the `Source code` tar
balls under release Downloads. ```autoconf``` is installable
via ```conda```, see [here](https://anaconda.org/biobuilds/autoconf-update) for details.

(0) Prepare your build system for compilation:

```bash
bash autogen.sh
```

(1) Check the version of your C/C++ compiler:

```bash
gcc --version
```

If the compiler is LLVM-GCC (deprecated, see [Deprecation and Removal Notice](https://developer.apple.com/library/ios/documentation/DeveloperTools/Conceptual/WhatsNewXcode/Articles/xcode_5_0.html)), 
then you must set it to Clang or the original GCC compiler (installable via MacPorts).

(2a) To set your compiler to Clang (check you have it "clang --version", if not then 
see "Install Clang for Mac OS" below):

```bash
export CC=clang
export CXX=clang++
```

Run configure and make scripts with default parameters:

```bash
./configure
make
```

(2b) To set your compiler to the original GCC (check you have it "gcc-mp-4.8 --version", 
if not then see "Install GCC through MacPorts" below):

```bash
export CC=gcc-mp-4.8
export CXX=g++-mp-4.8
```

If using the original GCC compiler, Zlib should also be installed via MacPorts in order
for compression to work (read .zip and .gz files). See section "Install GCC and Zlib
though MacPorts" below for installation instructions.

Assuming you have Zlib installed:

Run configure and make scripts (if compression feature wanted):

```bash
./configure --with-zlib="/opt/local"
make
```

Otherwise (if compression feature not wanted):

```bash
./configure --without-zlib"
make
```

(3) To install:

```bash
make install
```

You can define an alternative installation directory by
specifying ```--prefix=/path/to/installation/dir``` to ```configure```.


Install Clang for Mac OS 
------------------------

Installing Xcode (free through the App Store) and Xcode command line tools will automatically 
install the latest version of Clang supported with Xcode. 

After installing Xcode, the Xcode command line tools may be installed via:

Xcode -> Preferences -> Downloads

Under "Components", click to install "Command Line Tools"


Install GCC and Zlib though MacPorts
------------------------------------

Assuming you have MacPorts installed, type:

```bash
sudo port selfupdate
sudo port install gcc48
sudo port install zlib
```

After the installation, you should find the compiler installed in /opt/local/bin/gcc-mp-4.8 and /opt/local/bin/g++-mp-4.8
as well as Zlib in /opt/local/lib/libz.dylib and /opt/local/include/zlib.h .


Tests
-----

Usage tests can be run with the following command:
```
python ./tests/test_sortmerna.py
```
Make sure the ```data``` folder is in the same directory as ```test_sortmerna.py```


Third-party libraries
---------------------
Various features in SortMeRNA are dependent on third-party libraries, including:
	1. [ALP](http://www.ncbi.nlm.nih.gov/CBBresearch/Spouge/html_ncbi/html/software/program.html?uid=6): computes statistical parameters for Gumbel distribution (K and Lambda)
	2. [CMPH](http://cmph.sourceforge.net): C Minimal Perfect Hashing Library
	3. [KSEQ](http://lh3lh3.users.sourceforge.net/parsefastq.shtml): FASTA/FASTQ parser (including compressed files)
	4. [SSW](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0082138): SIMD Smith-Waterman C/C++ Library

Galaxy Wrapper
--------------

Thanks to Björn Grüning and Nicola Soranzo, an up-to-date Galaxy wrapper exists for SortMeRNA.
Please visit Björn's [github page](https://github.com/bgruening/galaxytools/tree/master/tools/rna_tools/sortmerna) for installation.

Debian package
--------------

Thanks to the [Debian Med](https://www.debian.org/devel/debian-med/) team, SortMeRNA 2.0 is now a package in Debian.
Thanks to Andreas Tille for the sortmerna and indexdb_rna man pages (version 2.0).
These have been updated for 2.1 in the master repository.

GNU Guix package
----------------

Thanks to Ben Woodcroft for adding SortMeRNA 2.1 to GNU Guix, find the package [here](https://www.gnu.org/software/guix/packages/).

Running in QIIME
----------------

SortMeRNA 2.0 can be used in [QIIME](http://qiime.org)'s [pick_closed_reference_otus.py](http://qiime.org/scripts/pick_closed_reference_otus.html),
[pick_open_reference_otus.py](http://qiime.org/scripts/pick_open_reference_otus.html) and [assign_taxonomy.py](http://qiime.org/scripts/assign_taxonomy.html) scripts.

Note: At the moment, only 2.0 is compatible with QIIME.

Taxonomies
----------

The folder `rRNA_databases/silva_ids_acc_tax.tar.gz` contains SILVA taxonomy strings (extracted from XML file generated by ARB)
for each of the reference sequences in the representative databases. The format of the files is three tab-separated columns,
the first being the reference sequence ID, the second being the accession number and the final column is the taxonomy.

Citation
--------

If you use SortMeRNA, please cite:
Kopylova E., Noé L. and Touzet H., "SortMeRNA: Fast and accurate filtering of ribosomal RNAs in metatranscriptomic data", Bioinformatics (2012), doi: 10.1093/bioinformatics/bts611.



