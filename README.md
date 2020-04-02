# Building-Gene-Trees-workshop-1

This workshop aims for you to be able to run a gene tree analysis on your desktop. For very large trees, you will need to submit to the UW chtc

## Part 1: Download programs

1. make a folder on your Desktop called workshop1. 

2. BLAST- basic local alignment (pairwise alignment). Good for finding distantly related genes and gene matches in a large sequence like a genome.

  windows:

           https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.10.0+-x64-win64.tar.gz
           
  mac:
  
           https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.10.0+-x64-macosx.tar.gz
           
  linux:
  
           https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.10.0+-x64-linux.tar.gz
           
  put in worskhop1 folder

3. Windows users only:
  
  Install Ubuntu:
  
  a.  turn on linux on windows (may require restart): 
      
          https://mafft.cbrc.jp/alignment/software/windowsfeatures.html
          
  b.	install ubuntu 
      
          https://mafft.cbrc.jp/alignment/software/jump.html?https://www.microsoft.com/store/p/ubuntu/9nblggh4msv6
          
 4. MAFFT- Multiple sequence alignment.. good for aligning multiple sequences that are already similar (ie. orthologs).
 
  a.  mac/linux:
    
          https://mafft.cbrc.jp/alignment/software/
          
   after installation, mafft is located here:
    
          /usr/local/bin/mafft
          
  b.  windows:
    
   open ubuntu, navigate to your working directory
    
          cd /mnt/c/Users/yourname/workshop1
          
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
   
  5. Dendroscope: for visualizing trees
  
         http://dendroscope.org/
         
  6. Download files from this Github repository to workshop1 folder
   
 ## Part 2: Basic Unix and Running BLAST from command line
 
  1. Open terminal and navigate to working directory
  
     a.	What directory are you in now?

          pwd
          
     b.	change directory to the workshop1 folder:
     
          cd /Users/Beth/Desktop/post_doc/workshop1/
          
  2. Untar and unzip BLAST files
  
     a. untar BLAST:
     
          tar -xzf ncbi-blast-2.10.0+-x64-linux.tar.gz
          
     b. list BLAST directory
     
          ls ncbi-blast-2.10.0+/bin/
          
     c. remove tar file
     
          rm ncbi-blast-2.10.0+-x64-linux.tar.gz
          
     d. what else is in this directory?
     
          ls
          
     e. unzip and check fasta files
     
          gunzip *.gz
          
          head *.fa*
          
   3. Make BLAST databases 
   
      a. for Streptochaeta:
   
          ncbi-blast-2.10.0+/bin/makeblastdb -in Streptochaeta_maker_max_proteins_V1.fasta -dbtype prot
          
      b. Now try the other fasta files

   4. BLAST ADH genes
   
      a.	for Athaliana:
      
          ncbi-blast-2.10.0+/bin/blastp -db Athaliana_TAIR10_pep_20101214.fa -query ADH_Athaliana.fa -out ADH_Athaliana_vs_Athaliana_BLresults.txt
          
      b.  look at results:
      
          less ADH_Athaliana_vs_Athaliana_BLresults.txt
          
      c.	try a different format:
      
          ncbi-blast-2.10.0+/bin/blastp -outfmt 6 -evalue 0.001 -db Athaliana_TAIR10_pep_20101214.fa -query ADH_Athaliana.fa -out ADH_Athaliana_vs_Athaliana_BLresults2.txt

      d.	get blasts for other genomes:
      
          ncbi-blast-2.10.0+/bin/blastp -outfmt 6 -evalue 0.001 -db Bdistachyon_556_v3.2.protein_primaryTranscriptOnly.fa -query ADH_Athaliana.fa -out ADH_Athaliana_vs_Bdistachyon_BLresults.txt
          
      e.	change file name, get rid of first Athaliana blast and replace with second:

          mv ADH_Athaliana_vs_Athaliana_BLresults2.txt ADH_Athaliana_vs_Athaliana_BLresults.txt
          
      f.	Notes: 
      
      output format 6 is ‘tabular’: 
      
      qseqid (Query Seq-id); sseqid (Subject Seq-id); pident (Percentage of identical matches); length (Alignment length); mismatch (Number of mismatches); gapopen (Number of gap openings); qstart (Start of alignment in query); qend (End of alignment in query); sstart (Start of alignment in subject); send (Start of alignment in subject); 
      evalue (Expect value- number of matches expected in database by chance- random background); bitscore (Bit score- natural log of the alignment score- estimates magnitude of search space needed to find this sequence by chance. A bit score of 30 means you need a sequence of length 2^30 to find the query sequence by chance)

   5.	Get lists of BLAST matches and then get the protein sequences for those genes
   
      a.	get gene list from BLAST output

          awk '{ a[$2]++ } END { for (b in a) { print b } }' ADH_Athaliana_vs_Acomosus_BLresults.txt

          awk '{ a[$2]++ } END { for (b in a) { print b } }' ADH_Athaliana_vs_Acomosus_BLresults.txt > Acomosus_ADH.txt
          
      b.	get protein sequence for gene list
      
          python ../scripts/FastaManager.py -f getseq2 -fasta Streptochaeta_maker_max_proteins_V1.fasta -name Streptochaeta_ADH.txt 
          
      c.	concatenate all of these sequences into one fasta file
      
          cat *_ADH.txt.fa > all_ADH.fa
          
          
## Part 3: MAFFT alignment

  MAFFT is a multiple sequence aligner
  
  MAFFT should be located here on Mac: /usr/local/bin/mafft 
  
  Windows user should follow part 1-4b to get MAFFT
  
  1.  Do MAFFT alignment.
  
      a.	simplify your concatenated fasta file:

          python ../scripts/FastaManager.py -f simplify -fasta all_ADH.fa

          less all_ADH.fa.mod.fa

      b.	run mafft:
      
          mafft --auto --anysymbol all_ADH.fa.mod.fa > all_ADH.fa.aln

          less all_ADH.fa.aln

## Part 4: RAxML

RAxML is a tree building software that uses maximum likelihood

1.	For Mac only: compile

        make -f Makefile.AVX2.PTHREADS.mac

    result:
    
        raxmlHPC-PTHREADS-AVX2

2.	run--Note: this part can take a long time, even for small trees-- up to 15 hours

    a.  list alignment files into one file:

        ls all_ADH.fa.aln > alignment_files.txt

    b.  use python script to make command file
    
        python 1a_WriteRAxMLCommands_bootstr_outgroup.py alignment_files.txt

    c.	run command for mac
    
        raxmlHPC-PTHREADS-AVX2 -T 4 -n all_ADH.fa.aln.RAxML.txt -f a -x 71994 -p 93537 -N 10 -m PROTGAMMAJTT -s all_ADH.fa.aln

    d.  run command for windows
    
        standard-RAxML-master/WindowsExecutables_v8.2.10/raxmlHPC-PTHREADS-AVX2.exe -T 4 -n all_ADH.fa.aln.RAxML -f a -x 71994 -p 93537 -N 100 -m PROTGAMMAJTT -# 100 -s all_ADH.fa.aln
    
    e.  What do all the options mean?

        -T= [number of computational cores (can look up in the about)], -n = [the output name], -f=[type of algorithm, a= [rapid Bootstrap analysis and search for best­scoring ML tree in one program run], -x= [rapidBootstrapRandomNumberSeed], -p= [parsimonyRandomSeed] ­#|­N = [numberOfRuns/ bootstraps], -m [model of nucleotide or amino acid substitution- PROTGAMMA: specified AA matrix + Optimization of substitution rates + GAMMA model of rate heterogeneity (alpha parameter will be estimated) JTT: specifies a ML estimate of base frequencies, JTT: gene1 = 1­500], -s= [the input (alignment) file]

        100 bootstraps can take ~15 hours. For testing purposes, just do 10 bootstraps (about ~6 minutes per bootstrap)!

    f.  if you want to specify outgroup: 
    
         -o AT5G3493, At1g1571
         
## Part 5: Dendroscope
