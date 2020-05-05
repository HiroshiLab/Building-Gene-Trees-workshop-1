# Building-Gene-Trees-workshop-1

This workshop aims for you to be able to run a gene tree analysis on your desktop. For very large trees, you will need to submit to the UW chtc

## Part 1: Download programs

1. Make a folder on your Desktop called workshop1 and download the following programs into it. 

2. BLAST- basic local alignment (pairwise alignment). Good for finding distantly related genes and gene matches in a large sequence like a genome.

  windows:

           https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.10.0+-x64-win64.tar.gz
           
  mac:
  
           https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.10.0+-x64-macosx.tar.gz
           
  linux:
  
           https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.10.0+-x64-linux.tar.gz
           

3. Windows users only:
  
  Install Ubuntu:
  
  a.  turn on linux on windows (may require restart): 
      
          https://mafft.cbrc.jp/alignment/software/windowsfeatures.html
          
  b.	install ubuntu 
      
          https://mafft.cbrc.jp/alignment/software/jump.html?https://www.microsoft.com/store/p/ubuntu/9nblggh4msv6
          
  c. Follow instructions "solving makeblastdb windows issue" in workshop folder
 
4. MAFFT- Multiple sequence alignment.. good for aligning multiple sequences that are already similar (ie. orthologs).
 
  a.  mac/linux:
    
          https://mafft.cbrc.jp/alignment/software/
          
   after installation, mafft is located here:
    
          /usr/local/bin/mafft
          
  b.  windows:
    
   open ubuntu, navigate to your working directory (the workshop1 folder)
    
          cd /mnt/c/Users/yourname/Desktop/workshop1
          
   download mafft:
    
         wget https://mafft.cbrc.jp/alignment/software/mafft_7.450-1_amd64.deb
    
   unpack (if you don't have sudo, you should get an error message telling you how to install it.. follow instructions then run):
    
         sudo dpkg -i mafft_7.450-1_amd64.deb
         
   c.  check mafft is installed (in terminal or ubuntu):
    
         which mafft
        
        
  4. RAxML
  
   Navigate to:
   
         https://github.com/stamatak/standard-RAxML
         
   click clone or download, then download zip to workshop1 folder
   
  5. Dendroscope: for visualizing trees.
  
  Download and install on your computer.
  
         http://dendroscope.org/
         
  6. Download files from this Github repository to workshop1 folder
  
  Zipped fasta files (.fa.gz) can also be downloaded from NCBI or Phytozome.
   
 ## Part 2: Basic Unix and Running BLAST from command line
 
  1. Open terminal and navigate to working directory
  
     a.	What directory are you in now?

          pwd
          
     b.	change directory to the workshop1 folder:
     
          cd /Users/Beth/Desktop/workshop1/
          
       if you need to go back to the previous directory do:
       
          cd ../
          
       list contents of the folder you are in:
       
          ls
          
       Workshop1 should contain ~10 files now.
          
  2. Untar and unzip BLAST files
  
     a. Untar BLAST for mac:
     
          tar -xzf ncbi-blast-2.10.0+-x64-macosx.tar.gz
          
       for windows:
       
          tar -xzf ncbi-blast-2.10.0+-x64-win64.tar.gz
     
     b. List BLAST directory
     
          ls ncbi-blast-2.10.0+/bin/
          
     c. Remove tar file for mac
     
          rm ncbi-blast-2.10.0+-x64-macosx.tar.gz
          
        for windows:
        
          rm ncbi-blast-2.10.0+-x64-win64.tar.gz
     
     d. What else is in this directory?
     
          ls
          
     e. Change to the github directory which should be within workshop1:
     
          cd Building-Gene-Trees-workshop-1-master/
     
     f. Unzip and check species fasta files
     
        unzip:
     
          gunzip *.gz
          
        Now print the first 5 lines of each fasta file. If you get an error you likely have not correctly unzipped the files.
          
          head *.fa*
          
        NOTE: some fasta files are labeled .fa, others are .fasta. If you are a Windows user then rename fasta files so that they all have the same ending (either all .fa or all .fasta)
          
   3. Make BLAST databases (you should be in the directory where your fasta files are- Building-Gene-Trees-workshop-1)  Note: If you are using Windows you may have to change the fasta file names so they end in .fasta instead of .fa. Additionally, windows programs end with .exe, so this must be added on to the program name. See below.
   
      a. for Brachypodium mac:
   
          ncbi-blast-2.10.0+/bin/makeblastdb -in Bdistachyon_556_v3.2.protein_primaryTranscriptOnly.fa -dbtype prot
          
      b. for Brachypodium windows:
   
           ncbi-blast-2.10.0+/bin/makeblastdb.exe -in Bdistachyon_556_v3.2.protein_primaryTranscriptOnly.fa -dbtype prot
   
      c. Now try the other fasta files- be sure to change the input fasta file (after -in)

   4. BLAST ADH genes (or your gene of interest)
   
      -query is the fasta file with your genes of interest
   
      a.	for Athaliana:
      
      for mac:
          
          ncbi-blast-2.10.0+/bin/blastp -db Athaliana_TAIR10_pep_20101214.fa -query Athaliana_ADH.txt.fa -out ADH_Athaliana_vs_Athaliana_BLresults.txt
          
      for windows:
      
          ncbi-blast-2.10.0+/bin/blastp.exe -db Athaliana_TAIR10_pep_20101214.fa -query Athaliana_ADH.txt.fa -out ADH_Athaliana_vs_Athaliana_BLresults.txt
      
      b.  look at results:
      
          less ADH_Athaliana_vs_Athaliana_BLresults.txt
          
      hit the key "q" to get out of less
      
      c.	try a different format:
      
          ncbi-blast-2.10.0+/bin/blastp -outfmt 6 -evalue 0.001 -db Athaliana_TAIR10_pep_20101214.fa -query Athaliana_ADH.txt.fa -out ADH_Athaliana_vs_Athaliana_BLresults2.txt

      d.	get blasts for other genomes: 
      
      change -db to the different species fasta files and the output name (-out)
      
          ncbi-blast-2.10.0+/bin/blastp -outfmt 6 -evalue 0.001 -db Acomosus_321_v3.protein_primaryTranscriptOnly.fa -query ADH_Athaliana.fa -out ADH_Athaliana_vs_Acomosus_BLresults.txt
          
      e.	change file name, get rid of first Athaliana blast and replace with second (mv overwites the first results and replaces it with the second results):

          mv ADH_Athaliana_vs_Athaliana_BLresults2.txt ADH_Athaliana_vs_Athaliana_BLresults.txt
          
      f.	Notes on BLAST output: 
      output format 6 is ‘tabular’: 
      
          qseqid (Query Seq-id); sseqid (Subject Seq-id); pident (Percentage of identical matches); length (Alignment length); mismatch (Number of mismatches); gapopen (Number of gap openings); qstart (Start of alignment in query); qend (End of alignment in query); sstart (Start of alignment in subject); send (Start of alignment in subject); 
          
          evalue (Expect value- number of matches expected in database by chance- random background); bitscore (Bit score- natural log of the alignment score- estimates magnitude of search space needed to find this sequence by chance. A bit score of 30 means you need a sequence of length 2^30 to find the query sequence by chance)

  5. Get lists of BLAST matches and then get the protein sequences for those genes using awk
   
     a.	get gene list from BLAST output
      
     NOTE: ">" character indicates you are writing to a new file

          awk '{ a[$2]++ } END { for (b in a) { print b } }' ADH_Athaliana_vs_Acomosus_BLresults.txt > Acomosus_ADH.txt
          
     b.	get protein sequence for gene list
      
          python Building-Gene-Trees-workshop-1-master/FastaManager.py -f getseq2 -fasta Acomosus_321_v3.protein_primaryTranscriptOnly.fa -name Acomosus_ADH.txt 
          
     NOTE: if your computer says "no python, use python3" then run the command starting with python3 instead of python
          
     c.  do the same for the rest of the BLAST outputs. Don't forget to change the inputs and outputs
      
           awk '{ a[$2]++ } END { for (b in a) { print b } }' blastresult_file > output1
           
           python Building-Gene-Trees-workshop-1-master/FastaManager.py -f getseq2 -fasta species_fasta_file -name output1
      
     d.	concatenate all of these sequences into one fasta file
      
     NOTE: * indicates "wild card", so anything ending with "_ADH.txt.fa"_ is concatenated into the new file
      
          cat *_ADH.txt.fa > all_ADH.fa
          
          
## Part 3: MAFFT alignment

  MAFFT is a multiple sequence aligner
  
  MAFFT should be located here on Mac: /usr/local/bin/mafft 
  
  Windows user should follow part 1-4b to get MAFFT
  
  1.  Check if mafft is properly installed:
  
          which mafft
  
  2.  Do MAFFT alignment.
  
      a.	simplify your concatenated fasta file:

          python Building-Gene-Trees-workshop-1-master/FastaManager.py -f simplify -fasta all_ADH.fa

          less all_ADH.fa.mod.fa

      b.	run mafft:
      
          mafft --auto --anysymbol all_ADH.fa.mod.fa > all_ADH.fa.aln

          less all_ADH.fa.aln
          
  3. Move the alignment file to the workshop1 folder and return to this folder
  
          mv all_ADH.fa.aln ../
          
          cd ../

## Part 4: RAxML

RAxML is a tree building software that uses maximum likelihood

1. Go to the RAxML folder

        cd standard-RAxML-master

2.	For Mac only: compile

        make -f Makefile.AVX2.PTHREADS.mac

    result:
    
        raxmlHPC-PTHREADS-AVX2

3.	For both Mac and Windows: run--Note: this part can take a long time, even for small trees-- can be multiple hours

    a.  Go back to the workshop-1 directory:
    
        cd ../

    b.  list alignment files into one file:

        ls *.fa.aln > alignment_files.txt

    c.  use python script to make command file
    
        python Building-Gene-Trees-workshop-1-master/1_WriteRAxMLCommands_bootstr.py alignment_files.txt

    d.  open the command file that was generated and copy line to run. Remember to press "q" to quit:
    
        less RAxMLCommands-Select.cc
    
    e.	run command for mac: 
    
    Note we have to add the folder name standard-RAxML-master/ to tell the command where the program is.
    
        standard-RAxML-master/raxmlHPC-PTHREADS-AVX2 -T 4 -n all_ADH.fa.aln.RAxML.txt -f a -x 71994 -p 93537 -N 10 -m PROTGAMMAJTT -s all_ADH.fa.aln

    f.  run command for windows
    
    Note for Windows we have to add an additional folder WindowsExecutables_v8.2.10/ because the windows executables are in this folder
    
        standard-RAxML-master/WindowsExecutables_v8.2.10/raxmlHPC-PTHREADS-AVX2.exe -T 4 -n all_ADH.fa.aln.RAxML -f a -x 71994 -p 93537 -N 100 -m PROTGAMMAJTT -# 100 -s all_ADH.fa.aln
    
    g.  What do all the options mean?

        -T= [number of computational cores (can look up in your computer's "about" or just remember that for most laptops 4 is good)], -n = [the output name], -f=[type of algorithm, a= [rapid Bootstrap analysis and search for best­scoring ML tree in one program run], -x= [rapidBootstrapRandomNumberSeed], -p= [parsimonyRandomSeed] ­#|­N = [numberOfRuns/ bootstraps (this is normally 100 but for a test run or for the workshop use 10)], -m [model of nucleotide or amino acid substitution- PROTGAMMA: specified AA matrix + Optimization of substitution rates + GAMMA model of rate heterogeneity (alpha parameter will be estimated) JTT: specifies a ML estimate of base frequencies, JTT: gene1 = 1­500], -s= [the input (alignment) file]

        100 bootstraps can take ~15 hours. For testing purposes, just do 10 bootstraps (about ~6 minutes per bootstrap)!

    f.  if you want to specify outgroup add: 
    
         -o AT5G3493, At1g1571
         
## Part 5: Dendroscope

1.  Go to applications or start on your computer to locate and open Dendroscope.

2.  Open "RAxML_bestTree.." file to see overall topology.

3.  Open "RAxML_bipartitions.." file to see best tree with bootstrap values.

4.  Open "RAxML_bootstrap.." file to see all of the bootstrap trees that were constructed.

5.  Try changing to different tree types

6.  Select a specific branch (by placing the pointing finger on that branch) to root the tree with that leaf. The root button has a sideways red arrow at the base of a phylogeny.
