For this assignment you will need to use OSC resources. It is expected that you fill the appropiate commands anywhere you see the legend: `Command(s) line`. I am providing you with specialized commands, and it is up to you to try to understand what they are doing, it will be usefult that you make notes of that. There are some questions in some lines, you may want to answer them.
* Your Working Directory on the scratch directory of the class: /fs/scratch/PAS3260
* Account number for the class: PAS3260

For this assignment you can use the command line environment directly on VSCodium through SSH, or alternatively using an Interactive session in OSC. Keep in mind that your repository has to be updated and customized! Then, you can proceed as follows:
0. Read the whole script first and create a mental image of what you have to get done.
1. Check when you can just work with the login node, and when you will need to use dedicated computing resources. 
2. Request an interactive session in Pitzer, be realistic in the number of hours considering how long are you going to be in front of the computer. The assignment can be completed in around one hour but you have to consider that you may need to try several times. Depending on the use of OSC cluster, it may take a while until you are able to start your session. Make sure that you use it because once the time expired you will need to request time again.
3. Feel free to use the Graphical User Interface (GUI) of the computer in OSC.
4. You are ready to start to follow the instructions to do your assignment. Follow the code page in this repo.

If you login using your own terminal, indicate how did you login (provide the commands you used, do not provide password). If you are accessing using either VSCode on OSC or just the terminal in the web browser. provide a screenshot of your active session.
```shell
# your ssh command:
I connect local VS Code to OSC using the Remote SSH extension.
I opened the command palette using Ctrl+Shift+P and then I typed "Remote-SSH: Connect to Host..." and selected the host corresponding to OSC (Pitzer cluster), which is configured in my SSH config file. After that, I entered my OSC username and password when prompted, and I am now connected to the OSC cluster through VS Code.
```
Move to your working directory and make a new directory called Assignment_2 and two new subdirectories, `Data` and `Analysis`, provide the commands you used:
```shell
# making your directories and subdirectories
pwd #/fs/scratch/PAS3260/kavya
mkdir Assignment_2/
mkdir Assignment_2/Data/ Assignment_2/Analysis/
```
Now, you are ready to get the data. Copy the tar and md5 files files in `/fs/scratch/PAS3260/Jonathan/Assignment_2` in you subdirectory **Data** (it should take around 5 min), provide the commands you used:
```shell
cp -r /fs/scratch/PAS3260/Jonathan/Assignment_2/*.tar Assignment_2/Data/
# copying md5 file
cp -r /fs/scratch/PAS3260/Jonathan/Assignment_2/*.md5 Assignment_2/Data/
```
Stay tuned because you are not going to receive any message that the copy has finished, you just will see the prompt blinking. What is a tar file?

A tar file- Tape Archive file is an archive of multiple files and directories within a single file. Unlike zip, tar does not compress the files. Instead it bundles them together.

Verify the integrity of data, you have to be patient to receive the corresponding OK, provide the commands you used and evidence of not issue with integrity of the files.
```shell
# Checking integrity
cd Assignment_2/Data/
md5sum -c md5sum_check.md5 > Data_integrity.txt
less Data_integrity.txt
ERX708228.tar: OK
ERX708229.tar: OK
ERX708230.tar: OK
ERX708231.tar: OK
grep "FAIL" Data_integrity.txt  #no output
```
Check the size of the files, provide the commands you used:
```shell
# Commands for checking size of files
ls -lh
total 37G
-rw-r--r-- 1 sskavya123 PAS1838   72 Feb 26 09:22 Data_integrity.txt
-rwxr-xr-x 1 sskavya123 PAS1838  15G Feb 25 20:47 ERX708228.tar
-rwxr-xr-x 1 sskavya123 PAS1838 5.9G Feb 25 20:47 ERX708229.tar
-rwxr-xr-x 1 sskavya123 PAS1838 4.5G Feb 25 20:47 ERX708230.tar
-rwxr-xr-x 1 sskavya123 PAS1838  12G Feb 25 20:48 ERX708231.tar
-rwxr-xr-x 1 sskavya123 PAS1838  192 Feb 25 20:54 md5sum_check.md5
```
Let's extract the reads (it takes around 2 minutes), I am going to help you with the commands here, but I need you to explain what is going on in this line, add a comment explaining that:
```shell
find -name "*tar" -exec tar xvf '{}' \;
tar --help
#The command works as follows:
#It finds file names ending with *tar through globbing and uses the output as an input to execute the command to extract all files from the archive.tar and the -v stands for verbose (Meaning the shell communicates what it is doing) the '{}' is a place holder variable which will be replaced with the input file and \; says the bash to extract individual files and command ends here. 
```
We will save all the fast5 in a unique folder called `FAST5` inside the `Data` subdirectpry, provide the commands to have such a directory, I am providing you with the command that will enable you to copy all the files, but again, I need you to explain in plain English what is going on with those instructions:
```shell
# Create directory
mkdir FAST5 
# Copying files, it can take more than 15 minutes, so stay pending of what is going on in your terminal.
find flowcell_* -name '*.fast5' -exec cp {} FAST5/ \;
```
#What this command basically does is - the find command can look for directories and files. So here the find command finds the directories with the name flowcell_* through globbing and looks for files ending in *.fast5. This output is used as an input to execute the command to copy the files to the FAST5 directory. The '{}' is a place holder variable which will be replaced with the input file and \; says the bash to extract individual files and command ends here.

Many times it does not show whether it is doing something or not, and it may take a while to get a terminal active again. Once the copying is done proceed in this way:
```shell
#ls FAST5/ Not a good idea to start with this you have plenty of files!
ls -Slh FAST5/ # make sure you are in Assignment_2/Data
ls --help # -S flag sorts files by size
ls -Slh FAST5/ | head -n 10 # this will show the 10 largest files in the FAST5 directory
-rwxr-xr-x 1 sskavya123 PAS1838   13M Feb 26 09:46 LomanLabz_PC_K12_0.4SPRI_1036_1_ch475_file100_strand.fast5
-rwxr-xr-x 1 sskavya123 PAS1838   12M Feb 26 09:46 LomanLabz_PC_K12_0.4SPRI_1036_1_ch251_file13_strand.fast5
-rwxr-xr-x 1 sskavya123 PAS1838   11M Feb 26 09:49 LomanLabz_PC_K12_0.4SPRI_Histag_2004_1_ch433_file81_strand.fast5
-rwxr-xr-x 1 sskavya123 PAS1838   10M Feb 26 09:42 LomanLabz_PC_Ecoli_K12_R7.3_2549_1_ch315_file83_strand.fast5
-rwxr-xr-x 1 sskavya123 PAS1838  9.4M Feb 26 09:46 LomanLabz_PC_K12_0.4SPRI_1736_1_ch120_file118_strand.fast5
-rwxr-xr-x 1 sskavya123 PAS1838  9.3M Feb 26 09:49 LomanLabz_PC_K12_0.4SPRI_Histag_5925_1_ch170_file54_strand.fast5
-rwxr-xr-x 1 sskavya123 PAS1838  8.6M Feb 26 09:47 LomanLabz_PC_K12_0.4SPRI_1736_1_ch212_file172_strand.fast5
-rwxr-xr-x 1 sskavya123 PAS1838  8.6M Feb 26 09:44 LomanLabz_K12_His_tag_1738_1_ch271_file78_strand.fast5
-rwxr-xr-x 1 sskavya123 PAS1838  8.4M Feb 26 09:45 LomanLabz_K12_His_tag_3617_1_ch305_file47_strand.fast5
cd FAST5/
ls -1 | wc -l
# ls -1 This command lists all files in the FAST5 directory one per line. Then the output is piped to wc -l, which counts the number of lines. Since each line corresponds to a file, this command gives us the total number of files in the FAST5 directory.
#22270
cd ..
```
Useful information, but what is the actual size of the directory? Look for a command that can give you that information (use Google, ChatGPT, Gemini, an actual friend, etc.), provide the command you used:
```shell
# Command to get the actual size of the FAST5
du -h FAST5 
#This command estimates the disk usage of the FAST5 directory and its contents. This indirectly gives us the actual size of the directory.
# 37G    FAST5
```
## **Important: check the integrity of all FAST5 files**
This takes a long time too.
```shell
cd FAST5/
# Check integrity against the file FAST5_md5sum.md5 in /fs/scratch/PAS3260/Jonathan/Assignment_2/FAST5_md5sum.md5, (do not copy or move the checksum file!) while creating a file that report the OK or FAIL of every file 
md5sum -c /fs/scratch/PAS3260/Jonathan/Assignment_2/FAST5/FAST5_md5sum.md5 > FAST5_integrity_check.txt
head FAST5_integrity_check.txt
LomanLabz_K12_His_tag_0612_1_ch104_file140_strand.fast5: OK
LomanLabz_K12_His_tag_0612_1_ch104_file43_strand.fast5: OK
LomanLabz_K12_His_tag_0612_1_ch104_file84_strand.fast5: OK
LomanLabz_K12_His_tag_0612_1_ch107_file110_strand.fast5: OK
LomanLabz_K12_His_tag_0612_1_ch107_file13_strand.fast5: OK
LomanLabz_K12_His_tag_0612_1_ch107_file42_strand.fast5: OK
LomanLabz_K12_His_tag_0612_1_ch107_file60_strand.fast5: OK
LomanLabz_K12_His_tag_0612_1_ch107_file64_strand.fast5: OK
LomanLabz_K12_His_tag_0612_1_ch109_file12_strand.fast5: OK
LomanLabz_K12_His_tag_0612_1_ch109_file30_strand.fast5: OK
# Use grep to check if there is any file with a failed integrity check. If you get a FAIL, delete everything and start fresh.
grep "FAIL" FAST5_integrity_check.txt
grep "FAIL" FAST5_integrity_check.txt #no output, so no file has a failed integrity check
```
We have plenty of .fast5 files! We need to convert them to unique fastq and fasta files for their processing. Let's make a new directory for the preprocessing. The structure should look like this: `Assignment_2/Analysis/Preprocessing/`. You want to keep your directory organized.
```shell
# Go to Assignment_2 (step by step)
cd ../
cd ../
# Make the directory Analysis/Preprocessing
mkdir -p Analysis/Preprocessing
```
In order to start the analysis, we will need to use a container with all the software that we are going to use, and which includes the programs: `assembly-stats`, `canu`, `mummer`, `poretools`, `prokka` and `R`. 
#assembly-stats caluculates statistics about the assembly, such as N50, total length, etc. particularly useful for denovo assemblies.
#canu is a denovo assembly tool for single molecule sequencing reads (Nanopore and PacBio) with high error rates. 
#mummer is a tool for comparing two genome sequences typically an assembly and a reference. It can be used to identify structural variations, rearrangements, and other differences between the two sequences.
#poretools is a tool for working with nanopore sequencing data. It is used for qc and downstream analysis of nanopore sequencing data. 
#prokka is for annotating prokaryote genomes. The output format is GFF3. 

Copy the container Assignment2.sif located `/fs/scratch/PAS3260/Jonathan/Assignment_2/` and verify that the container is copied in your working directory. Show evidence.
```shell
# copying the container
pwd
/fs/scratch/PAS3260/kavya/Assignment_2
cp /fs/scratch/PAS3260/Jonathan/Assignment_2/Assignment2.sif .
```
Then, access to the shell container using apptainer, provide the commands you used:
```shell
# accessing to shell of the container
apptainer --help
# shell       Run a shell within a container
apptainer shell Assignment2.sif
Singularity> 
I am inside the container now, the prompt has changed to "Singularity>" which indicates that I am in the shell of the container. I can run commands and access files within the container's environment.
```
In order to verify that you are actually running the container, we can check whether the operating system of the container (Ubuntu 18.04) is reported instead of RedHat run in OSC.
```
cat /etc/os-release
#This prints the contents of the /etc/os-release file, which contains information about the operating system. 
```
The output should look like this:
```shell
NAME="Ubuntu"
VERSION="18.04.3 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.3 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```
Provide evidence that you are actually using the shell of the container. A screenshot may be useful.
Thus, we start with producing unique fastq and fasta files using the fast5 files from the Nanopore sequencing (each step takes about 45min):
```shell
directory="Data/FAST5/"
#Setting the variable directory to the path of the FAST5 files, this will be used as an input for the poretools commands 
poretools fastq $directory > Analysis/Preprocessing/Reads.fastq
#poretools is a tool for working with nanopore sequencing data. The fastq command is used to extract the sequencing reads in fastq format. The output is redirected to Reads.fastq.
poretools fasta $directory > Analysis/Preprocessing/Reads.fasta
# This extracts the reads in fasta format and saves it to Reads.fasta. 
# I noticed in both the cases, the data integrity file is skipped because it has no reads. But the command does not give an error.
```
Check for the the size of the files, provide the commands you used:
```shell
ls -lh Analysis/Preprocessing/Reads.fastq
#The file size is 734 M 
ls -lh Analysis/Preprocessing/Reads.fasta
#The file size is 370 M
```
Quickly inspect the files, provide the commands you used:
```shell
# Inspecting files
#These are very large files so I am going to inspect using head.
head Analysis/Preprocessing/Reads.fastq
# I can see the @ read finder, the sequence, the + and the quality score. This is the typical format of a fastq file.
head Analysis/Preprocessing/Reads.fasta
# I can see the > read finder and the sequence. This is the typical format of a fasta file.
```
## **Which organism might this be? Explain how did you figured out, what is its genome size? It is important for subsequent steps.**
#I used BLASTn for identifying the organism. I randomly blasted one of the reads "channel_205_read_9 Data/FAST5//LomanLabz_PC_K12_0.4SPRI_Histag_5925_1_ch205_file10_strand.fast5" and the top hit was E-coli K12 strain with 99% query coverage and 78.05% identity. I will check with one other read. 

#I checked with "channel_68_read_108 Data/FAST5//LomanLabz_PC_Ecoli_K12_R7.3_2549_1_ch68_file116_strand.fast5" and the top hit is still E.coli but strain DH5a. I think the organism is E. coli. Differences in strain is expected during blast, but the identifier of read says its K12, so I will assume that the organism is E. coli K12. I found the accession ID  CP047127.1. The genome size is 4582546 bp (4.58 Mbp) according to NCBI.
Now, let's start the assessment of the reads in the fastq file:
```shell
assembly-stats Analysis/Preprocessing/Reads.fastq
# The output is as follows:
stats for Analysis/Preprocessing/Reads.fastq
sum = 381239419, n = 66810, ave = 5706.32, largest = 47133
N50 = 8873, n = 16178
N60 = 7996, n = 20699
N70 = 7089, n = 25752
N80 = 5983, n = 31571
N90 = 3882, n = 39194
N100 = 198, n = 66810
N_count = 0
Gaps = 0
```
Let's extract the length of all the reads in the fastq file and save them in a text file
```shell
cat Analysis/Preprocessing/Reads.fastq | paste - - - - | awk -F"\t" '{print length($2)}' > Analysis/Preprocessing/lengths.txt
# This command prints the Reads.fastq file using cat, then uses the output as input for paste which combines every four lines into one line (because in a fastq file, every four lines correspond to one read) and separates it by tab. The awk identifies tab-separated fields and prints the length of the second field, which is the sequence of the read. The output is redirected to lengths.txt.
head Analysis/Preprocessing/lengths.txt
#The output is a s follows:
9431
9109
8492
10250
9697
9362
10022
9400
9151
3196
```
Let's plot a quick histogram in R
```shell
#I am still inside the container.
R
```
In R type:
```R
getwd()
#"/fs/scratch/PAS3260/kavya/Assignment_2"
setwd("/fs/scratch/PAS3260/kavya/Assignment_2/Analysis/Preprocessing")
lengths <- read.table("lengths.txt")[,1]
# Reads the lengths.txt file and creates a data frame. The [ ,1] selects the first column of the data frame makes it a vector and assigns it to the variable lengths.
hist(lengths, xlim=c(0,30000), breaks=100, col="red")
# Draw histogram with lengths on the x-axis, limits from 0 to 30000, 100 breaks and red color.
hist(lengths, xlim=c(0,15000), breaks=50, col="red")
# Draw another histogram with limits from 0 to 15000 and 50 breaks. This is to get a better visualization of the distribution accounting for shorter reads.
pdf("HistLenghts.pdf", width=6, height=3, useDingbats=FALSE)
# This saves as HistLenghts.pdf. UseDingbats=FALSE is used to avoid issues with fonts.
par(mfrow=c(1,2))
# Print two histograms side by side.
hist(lengths, xlim=c(0,30000), breaks=100, col="red", xlab="Read lenght in bp")
hist(lengths, xlim=c(0,15000), breaks=50, col="red", xlab="Read lenght in bp")
dev.off()
# This closes the pdf device and saves the file.
quit("no")
```
Now, exit the container and request a full node of computing resources, remember that you can use only one hour (account PAS3260):
```shell
exit
# commands to get an interactive session
sinteractive -A PAS3260 -c 28 -t 59:59 -J Assignment2
```
# Checking correct working directory
/fs/scratch/PAS3260/kavya/Assignment_2
# and reenter to the container's shell
apptainer shell Assignment2.sif
```
# I think I found the actual publication https://doi.org/10.1038/nmeth.3444
Let's proceed with the assembly, the assumed genome size is "4.6 Mb"
```shell
canu useGrid=false -p Assembly -d Analysis/Assembly genomeSize="4.6m" -nanopore-raw Analysis/Preprocessing/Reads.fastq correctedErrorRate=0.16
```
The genomeSize is not correct, now represented as [X]y, so it will give an error when trying to run this commands. We have to provide a number (integer or decimal) for the genome size, in either megabase pairs (m) or gigabase pair (g), so the format is "X" is a integer number or decimal and "y" is the unit. What is the number that you will put there? What is your source? Provide a refrence or citation.

#Loman, Nicholas J., Joshua Quick, and Jared T. Simpson. "A complete bacterial genome assembled de novo using only nanopore sequencing data." Nature methods 12, no. 8 (2015): 733-735.

I think the time limit was not enough. The job did not finish, but I can see that the assembly is actually working because I can see the output files in the directory Analysis/Assembly/ and the log file shows that the program is running. I will try to run it again with more time. I will request 4 hours this time, which should be enough for the assembly to finish. I also inspected the Assembly.report and it doesnt have a proper finish.
```shell
44407428        TIMEOUT   01:00:17      0:0 
44407428.in+  CANCELLED   01:00:46      0:9 
44407428.ex+  COMPLETED   01:00:46      0:0 
````
Since the maximum time limit is 1 hour using sinteractive I will process to sbatch the job, which will allow me to request more time. I will request 4 hours this time, which should be enough for the assembly to finish.

```shell
sbatch canu.sh
```

```shell
Let's copy the fasta file resulted from the assembly and inspect it: (#I am guessing to pwd)
```shell
cp Analysis/Assembly/Assembly.contigs.fasta .
head Assembly.contigs.fasta 
```
How many contigs are reported in the fasta file?
just one contig.
```shell
grep ">" Assembly.contigs.fasta | wc -l
#1
```
Let's compare this assembly with the reference:
```shell
wget https://ftp.ncbi.nlm.nih.gov/genomes/refseq/bacteria/Escherichia_coli/reference/GCF_000005845.2_ASM584v2/GCF_000005845.2_ASM584v2_cds_from_genomic.fna.gz
gunzip GCF_000005845.2_ASM584v2_cds_from_genomic.fna.gz
```
#wget downloaded the reference genome in fasta format and gunzip uncompressed the file. The reference genome is for E. coli K12, which is the organism we are working with.

Making a comparison with mummer
```shell
nucmer --maxmatch -c 100 -p Assembly_v_Reference Assembly.contigs.fasta GCF_000005845.2_ASM584v2_cds_from_genomic.fna
#nucmer inside MUMmer is a tool for aligning two genome sequences DNA vs DNA.  --maxmatch option is given to find all maximal exact matches, -c 100 sets the minimum exact match length to 100 bp, -p sets the prefix for output files and the last two arguments are the query (assembly) and reference genome fasta files.
mummerplot -t png --fat --large --filter -p Assembly_v_Reference Assembly_v_Reference.delta
#The mummerplot visualizes the alignment results from nucmer. It takes .delta file as input. -t png for PNG image, --fat thicker alignment lines, --large large sequence (in our case whole bacterial genome), --filter reduces noisy alignments, -p sets the prefix.
```
Go to your OSC GUI browser and download Assembly_v_Reference.png and visualize it in the browser or in Adobe Acrobat or Preview.
Now, we can proceed to make the gene prediction and annotation of the genome using Prokka
```shell
export LC_ALL=C # This command is just to solve a very specific issue in a dependency for Prokka
prokka --outdir Annot_Assembly --prefix Assembly_prokka Assembly.contigs.fasta
#Prokka is prokaryote genome annotation tool. It takes canu assembly contigs fasta file as input and produces annotated files in the output directory Annot_Assembly with the prefix Assembly_prokka. The output is GFF3 file with annotatins.
```
Inspect output:
```shell
head Annot_Assembly/Assembly_prokka.gff
```
How many annotated proteins prokka find in the contigs? Does it make sense? Provide commands you used to figure that out:
#Since bacteria doesnt have introns, the number of annotated proteins should be equal to the number of CDS features in the GFF3 file. I will count the number of lines with "CDS".
```shell
# Command to estimate the number of proteins
grep ">" Annot_Assembly/Assembly_prokka.faa | wc -l
#8685
grep "CDS" Annot_Assembly/Assembly_prokka.gff | wc -l
#8685

```
Thus, you have run all the steps for the analysis. Now just close the container by typing:
```shell
exit
```
You will see that the bash prompt will be back to blinking indicating. Do not worry, all your files will stay in your directory Assignment2

### You have finished the hands on the assignment now review your files. See what you can improve and how you can customize this repo to make it more understandable for you.

#Git repo

git init
git add Readme.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:kavyasekarsusitha/Assignment_2_Genome_Analytics.git
git push -u origin main