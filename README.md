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

Available Commands
---

**edu.isi.bmkeg.lapdf.bin.BlockStatistics** 

Running this command on a PDF file or directory will generate 
statistics about each chunk in each pdf document .

**edu.isi.bmkeg.lapdf.bin.Blockify** 

Running this command on a PDF file or directory will attempt to generate 
one XML document per file with unnannotated text chunks .

**edu.isi.bmkeg.lapdf.bin.BlockifyClassify** 

Running this command on a PDF file or directory will attempt to generate 
one XML document per file with text chunks annotated with section.

**edu.isi.bmkeg.lapdf.bin.DebugLapdfFeatures**

Running this command on a PDF file or directory will generate a CSV file for the PDF that can act as a template for rule files. All available features are listed .

**edu.isi.bmkeg.lapdf.bin.ReadSectionText**

Running this command on a PDF file or directory will attempt to extract uninterrupted
two-column- formatted text of the main narrative section of the paper with one 
font change (i.e. for papers that use a smaller font for methods sections).
Figure legends are moved to the end of the paper (but included), and 
tables are dropped.

**edu.isi.bmkeg.lapdf.bin.ImagifyBlocks**

Running this command on a PDF file or directory will attempt to generate 
one image per page with text chunks drawn out with labels describing 
the predominant Font + Style of each block. This is helpful in developing
rule files.

**edu.isi.bmkeg.lapdf.bin.ImagifySections**

Running this command on a PDF file or directory will attempt to generate 
one image per page with text chunks drawn out with section labels.
This is intended to provide a debugging tool.

**edu.isi.bmkeg.lapdf.bin.WatchDirectory** 

This program maintains a watcher on this directory to execute the 
denoted command on any PDF files added to the directory. 
The system will then delete the appropriate files and folders
when the originating PDF file is removed.

Note that this function has not been fully tested and may fail. 

## Recommended use of the system.

### Step 1 - Organize your PDFs into subdirectories based on formatting

LAPDF-Text operates recursively over subdirectories to find all PDF files and extract text from them accordingly based on the formatting layout of documents. You should therefore organize your PDFS ahead of time based on whether papers are all two-column formats, or are from the same journal and so have similar layouts across many papers. This way, you can develop your own rule files to help extract text accurately.

### Step 2 - Run `Imagify*` functions to check that the system can read the PDFs correctly and eyeball the outputs. 

As an example, download this paper: [Makki et al. 2010, PLoS Biology 8:e1000441](http://www.plosbiology.org/article/fetchObject.action?uri=info%3Adoi%2F10.1371%2Fjournal.pbio.1000441&representation=PDF)

And then run `edu.isi.bmkeg.lapdf.bin.ImagifySections` on it (i.e., using the PDF file as the only parameter). This should generate 12 image files that show each page with each block drawn out as a rectangle with a baseline classification added to the file. Running ```imagifySections``` is always a great sanity check to make sure that the system is working. 

### Step 3 - Run `edu.isi.bmkeg.lapdf.bin.Blockify` or `edu.isi.bmkeg.lapdf.bin.BlockifyClassify` to get XML

If you're happy with the results provided from the imagify runs, as shown above, you should then try the two blockify commands. These generate XML output that can be parsed and read as necessary. 

### Step 4 - Run `edu.isi.bmkeg.lapdf.bin.ReadSectionText` to get plain text
 
Note that this command will attempt to order the blocks to place text that does not form part of the main narrative at the end of the file. Making sure that the block classification is accurate is essential to make sure that the text is correctly ordered.

### Step 5 - Develop your own rule files with `DebugLapdfFeatures`

If you run the `DebugLapdfFeatures` rules. If you open the CSV file in Excel, each row is a separate block and each column are the various features used to enact classification rules on each block. It is possible to code each rule column to improve the quality of the rules being used to classify each block. We will describe this process in more detail in another Wiki page on this site presently, but the file generated should provide a working model from which you can try to build your own rule files.  

Having started to develop your own rule files, iterate through steps 1-4 to try to improve performance for each set of documents with different formatting. This should allow you to extract text from PDF files accurately with a little time investment into developing your own rule files for your own documents.  



   
