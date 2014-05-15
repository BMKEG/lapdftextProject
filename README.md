lapdftextProject
================

High-level build project for all LAPDF-Text submodules.

*Note* - we have currently deprecated the development of the LA-PDFText rule web application that is not tied to the digital library. This means that we will no longer be developing the [lapdftextServer](https://github.com/BMKEG/lapdftextServer) web application separately of the [sciKnowMine](https://github.com/BMKEG/sciKnowMine) system. Interested developers and users are referred to the [sciKnowMineProject](https://github.com/BMKEG/sciKnowMineProject) for more information. 

To run this project, you should be able to compile this system directly from the download. This project is now *only* concerned with the [lapdftext](https://github.com/BMKEG/lapdftext) library, which is the base level
library containing all functionality for directly interacting with PDF files through the 
use of the JPedal GPL open source library (Beta level of development). 

## Installation Instructions 

Follow this link to for one-click installers for LAPDF-Text:

http://bmkeg.github.io/lapdfInstallers.html

Once built, you can use the command line to issue text extraction commands on PDF files.
The current version of LA-PDFText requires Java 1.6 in order to run and supports the following operating systems: 

* MacOSX (Developed on Mountain Lion, but should work in most environments). 
* Windows XP
* Linux

Mac and windows users can run their installers by simply double-clicking the installer icon while Linux users are provided with a *.tar.gz archive. 

_Note: OS X 10.8 (Mountain Lion) introduced a new security feature that, by default, limits "acceptable" applications to only those downloaded from the Mac App store. Thankfully, you can alter this in the system preferences. Go to "Security & Privacy" and change the "Allow applications downloaded from:" to "Anywhere". LA-PDFText will launch successfully after this change._

## Command-line functionality 

Usage of the tool as a software application is currently based on a set of command-line functions. 

**$ blockStatistics** ```input-dir-or-file``` [```output-dir```]

* ```input-dir-or-file``` - the full path to the PDF file or directory to be extracted 
* ```output-dir``` (optional or '-') - the full path to the output directory 

Running this command on a PDF file or directory will generate 
statistics about each chunk in each pdf document .

**$ blockify** ```input-dir-or-file``` [```output-dir```]

* ```input-dir-or-file``` - the full path to the PDF file or directory to be extracted 
* ```output-dir``` (optional or '-') - the full path to the output directory 

Running this command on a PDF file or directory will attempt to generate 
one XML document per file with unnannotated text chunks .

**$ blockifyClassify** ```input-dir-or-file``` [```output-dir```] [```rule-file```]

* ```input-dir-or-file``` - the full path to the PDF file or directory to be extracted 
* ```output-dir``` (optional or '-') - the full path to the output directory 
* ```rule-file``` (optional or '-') - the full path to the rule file 

Running this command on a PDF file or directory will attempt to generate 
one XML document per file with text chunks annotated with section.

**$ debugChunkFeatures** ```input-dir-or-file``` [```output-dir```] [```rule-file```]

* ```input-dir-or-file``` - the full path to the PDF file or directory to be extracted 
* ```output-dir``` (optional or '-') - the full path to the output directory 
* ```rule-file``` (optional or '-') - the full path to the rule file 

Running this command on a PDF file or directory will generate a CSV file for the PDF that can act as a template for rule files. All available features are listed .

**$ extractFullText** ```input-dir-or-file``` [```output-dir```] [```rule-file```] [```sec1``` ... ```secN```]

* ```input-dir-or-file``` - the full path to the PDF file or directory to be extracted 
* ```output-dir``` (optional or '-') - the full path to the output directory 
* ```rule-file``` (optional or '-') - the full path to the rule file 
* ```sec1``` ... ```secN``` (optional) - a list of section names to be included in the dump 

Running this command on a PDF file or directory will attempt to extract uninterrupted
two-column- formatted text of the main narrative section of the paper with one 
font change (i.e. for papers that use a smaller font for methods sections).
Figure legends are moved to the end of the paper (but included), and 
tables are dropped.

**$ imagifyBlocks** ```input-dir-or-file``` [```output-dir```] 

* ```input-dir-or-file``` - the full path to the PDF file or directory to be extracted 
* ```output-dir``` (optional or '-') - the full path to the output directory 

Running this command on a PDF file or directory will attempt to generate 
one image per page with text chunks drawn out with labels describing 
the predominant Font + Style of each block. This is helpful in developing
rule files.

**$ imagifySections** ```input-dir-or-file``` [```output-dir```] [```rule-file```]

* ```input-dir-or-file``` - the full path to the PDF file or directory to be extracted 
* ```output-dir``` (optional or '-') - the full path to the output directory 
* ```rule-file``` (optional or '-') - the full path to the rule file 

Running this command on a PDF file or directory will attempt to generate 
one image per page with text chunks drawn out with section labels.
This is intended to provide a debugging tool.

**$ watchPdfFolder** ```COMMAND``` ```dir-to-be-watched``` ```output-dir``` [```rule-file```]

* ```COMMAND``` - the command to be executed: 
 - ImagifyBlock
 - ImagifySection
 - Blockify
 - BlockifyClassify
 - ReadText
* ```dir-to-be-watched``` - the full path to the directory to be watched 
* ```output-dir``` (optional or '-') - the full path to the output directory 
* ```rule-file``` (optional or '-') - the full path to the rule file 

This program maintains a watcher on this directory to execute the 
denoted command on any PDF files added to the directory. 
The system will then delete the appropriate files and folders
when the originating PDF file is removed.

Note that this function has not been fully tested and may fail. 

## Recommended use of the system.

### Step 1 - Organize your PDFs into subdirectories based on formatting

LAPDF-Text operates recursively over subdirectories to find all PDF files and extract text from them accordingly based on the formatting layout of documents. You should therefore organize your PDFS ahead of time based on whether papers are all two-column formats, or are from the same journal and so have similar layouts across many papers. This way, you can develop your own rule files to help extract text accurately.

### Step 2 - Run ```imagify``` functions to check that the system can read the PDFs correctly and eyeball the outputs. 

As an example, download this paper: [Makki et al. 2010, PLoS Biology 8:e1000441](http://www.plosbiology.org/article/fetchObject.action?uri=info%3Adoi%2F10.1371%2Fjournal.pbio.1000441&representation=PDF)

And then run ```imagifySections``` on it (i.e., using the PDF file as the only parameter). This should generate 12 image files that show each page with each block drawn out as a rectangle with a baseline classification added to the file. Running ```imagifySections``` is always a great sanity check to make sure that the system is working. 

### Step 3 - Run ```blockify``` or ```blockifyClassify``` to get XML

If you're happy with the results provided from the imagify runs, as shown above, you should then try the two blockify commands. These generate XML output that can be parsed and read as necessary. 

### Step 4 - Run ```extractFullText``` to get plain text
 
Note that this command will attempt to order the blocks to place text that does not form part of the main narrative at the end of the file. Making sure that the block classification is accurate is essential to make sure that the text is correctly ordered.

### Step 5 - Develop your own rule files with ```debugChunkFeatures```

If you run the ```debugChunkFeatures``` command on the PDF file, the system will generate a CSV file that you can use as a template for developing rules. If you open the CSV file in Excel, each row is a separate block and each column are the various features used to enact classification rules on each block. It is possible to code each rule column to improve the quality of the rules being used to classify each block. We will describe this process in more detail in another Wiki page on this site presently, but the file generated should provide a working model from which you can try to build your own rule files.  

Having started to develop your own rule files, iterate through steps 1-4 to try to improve performance for each set of documents with different formatting. This should allow you to extract text from PDF files accurately with a little time investment into developing your own rule files for your own documents.  
