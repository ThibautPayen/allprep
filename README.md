# allprep
Tool for preprocessing and demultiplexing of sequencing data.
Barcode Preparation Toolbox README

DISCLAIMER:
Meric Lieberman, 2013
This work is the property of UC Davis Genome Center - Comai Lab

Use at your own risk. 
We cannot provide support.
All information obtained/inferred with these scripts is without any 
implied warranty of fitness for any purpose or use whatsoever. 


SUMMARY:
The scripts included are for use in preparation of raw illumina reads for further analysis. 
Traditionally, we do all prep work with a single script called "Allprep", that does barcode 
check and match, 'N' filtering, primary and secondary adapter contamination, quality 
conversion, quality trimming, length trimming, and library separation.

However, if parts of the preparation process needs to be performed independently, we also 
provide smaller scripts to do the process modularly.

Please note that:
- all of the scripts have a description of what they do, input parameters,
   and running directions at the beginning of each program. 
- all scripts use command line parameters as input and all scripts can be run with 
   ./"programname" provided Python 2.4+ is installed on your system. 
- these scripts can be used with paired end or single ended data, and some can be 
   run on data without barcodes.


CONTENTS:

This .tgz includes:

allPrep-8.py - Single script to do all processing as detailed above.

no-demultiplex-allprep-8.py - Single script to all processing above, but does not demultiplex. For use on a directory of pre-demultiplexed files.

interleaveSwitcher.py - This script will interleave two files or uninterleave a file. 
	This is only applicable to paired-end reads. "Interleaved" means that the two 
	ends of a read are placed together in the final file. View parameters in
	script for running directions.

	
--------------------------------------------------------------------------
--------------------------------------------------------------------------	
	
SCRIPT DETAILS:
The following information can be found at the top of every script, 
after disclaimer and library import.

--------------------------------------------------------------------------

allprep-8.py

Usage: 
For all modes the run command looks like this, with [...] indicating files needed by specific read type, and {....} indication optional parameters
allprep-8.py -b barcode-file.txt -f forward-read-file.fq [-r reverse-read-file.fq] [-i index1-file.fq] [-I index2-file.fq] {-m} {-E} {-n} {-q} {-d}

For a quick view of parameters form the command line, simply use the -h for help.
allprep-8.py -h

This program that takes a barcode file and splits the lane sequence.txt files into specified
library.txt (lib#.txt) files, does barcode check and match, 'N' filtering, primary and
secondary adapter contamination, quality conversion, quality trimming (mean quality of 20 over 5 base window),
length trimming (default 35), and library separation.

Input:
This script takes a barcode file as specified in the sample sheet in the README, as well as 
the forward read file, and optionally the reverse, index and secondary index
These files are loaded with -b, -f, -r, -i, and -I respectively.
There are also five additional operating options:
-m for mismatch mode, this allows a 1 bp mismatch between the index / barcode and the best matching barcode. 
-E for error reads mode, outputs the rejected reads to a file
-n for N allowed mode, this turns off the check that rejects a read if there are any 'N' nucleotides in the sequence
-q to convert a file using Illumina 1.5 qualities to Sanger/Illumina1.8 (standard)
-D for only demultiplexing mode, this only splits the reads by barcode and does no additional trimming or checks

This program can take a file with most combinations of barcode/indexing. The possibilities are:
1. single ended, barcoded
2. pair ended, barcoded
3. single ended, one index
4. single ended, two index
5. pair ended, one index
6. pair ended, two index

Please see the README-barcode-file.txt and sample-barcode-file.txt for help in creating the barcode file for your dataset.

--------------------------------------------------------------------------

no-demultiplex-allprep-8.py <optional parameters>


For a quick view of parameters from the command line, simply use the -h for help.
no-demultiplex-allprep-8.py -h

This program that looks at all .fastq or .fq files in the current working directory (lib#.fq)
 files, does optional combinations of: 'N' filtering, primary and
secondary adapter contamination, quality conversion, quality trimming (mean quality of 20 over 5 base window),
length trimming (default 35)

Input:
This script works on a directory of fastq files, the files must end with a common fastq file extension
For each file, depending on the parameters will process a predemultiplexed read file

Default Behavior
Rejected reads not output
'N' nuecleotide reads removed
Adapter contamination trimmed out
Qualtites are Phred+33 (samger or illumina 1.8+)
minimum trimmed sequence length is 35
single ended reads
prefix for copy of file that have been changed will be "prepped"

The options to change behavior
-E, output rejected reads per file
-n, allow reads with 'N' nuecleotides
-a, do not trim out adapter conamination
-q, convert form phred+64 (illumina 1.5-1.7) to phred+33(samger, 1.8+)
-M <n>, change minimum sequence length to integer <n>
-p, ALL input libs are pair ended. PE filss must be pre-interleaved
-o, a prefix to apply to the new processed file of reads, this work will be put on the front of all of the result files


--------------------------------------------------------------------------

interleaveSwitcher.py

Usage: programName.py i forwardFile.fq reverseFile.fq outFileName.fq
       ______OR_______
Usage: programName.py u interleavedFile

This program has two operating modes, defined by the first command line parameter.

Interleave mode, first parameter an 'i' or an 'I'.
   This will take two read files of equal length and interleave them.
Input Parameters:
forwardFile.fq = original file input, Sanger or illumina file forward side
reverseFile = original file input, Sanger or illumina reverse side
outFileName.fq = interleaved output file name/location

Uninterleave mode, first parameter an 'u' or an 'U'.
   This will take one interleaved paired read file and split them into two paired read files.
   The -1.fq and -2.fq result files will be forward and backward respectively.
Input Parameters:
interleavedFile.fq = original file input, Sanger or illumina interleaved paired reads

Be sure to put the i/u as the first parameter!

--------------------------------------------------------------------------



