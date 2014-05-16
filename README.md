lapdftextProject
================

High-level build project for all LAPDF-Text submodules.

*Note* - we have currently deprecated the development of the LA-PDFText rule web application that is not tied to the digital library. This means that we will no longer be developing the [lapdftextServer](https://github.com/BMKEG/lapdftextServer) web application separately of the [sciKnowMine](https://github.com/BMKEG/sciKnowMine) system. Interested developers and users are referred to the [sciKnowMineProject](https://github.com/BMKEG/sciKnowMineProject) for more information. 

To run this project, you should be able to compile this system directly from the download. This project is now *only* concerned with the [lapdftext](https://github.com/BMKEG/lapdftext) library, which is the base level
library containing all functionality for directly interacting with PDF files through the 
use of the JPedal GPL open source library (Beta level of development). 

To run the LA-PDFText library from source:

```
   git clone https://github.com/BMKEG/lapdftextProject
   git submodule update --init --recursive
   mvn -DskipTests clean package 
   cd lapdftext
   mvn assembly:assembly
```

You may then run commands directly using java directly, based on instructions below:

Usage of the tool as a software application is currently based on a set of command-line functions. 
We recommend that you follow the instructions generated from the command itself since that is likely 
to be more reliable than statically-authored documentation pages. Details are likely to change as we 
add more features. 

For example, to see what the command line arguments are for the `BlockStatistics` command enter: 

```
   java -classpath target/lapdftext-1.7.4-SNAPSHOT-jar-with-dependencies.jar edu.isi.bmkeg.lapdf.bin.BlockStatistics 
```

Generates the following usage output:

```
   usage: <input-dir-or-file> [<output-dir>]

   <input-dir-or-file> - the full path to the PDF file or directory to be extracted 
   <output-dir> (optional or '-') - the full path to the output directory 

   Running this command on a PDF file or directory will generate 
   statistics about each chunk in each pdf document .
```

See our online technical documentation at [http://bmkeg.github.io/lapdftext](http://bmkeg.github.io/lapdftext) for full technical instructions.
