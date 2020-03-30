# Building-Gene-Trees-workshop-1

This workshop aims for you to be able to run a gene tree analysis on your desktop. For very large trees, you will need to submit to the UW chtc

## Download programs

1. BLAST- basic local alignment (pairwise alignment). Good for finding distantly related genes and gene matches in a large sequence like a genome.

  windows:

           https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.10.0+-x64-win64.tar.gz
           
  mac:
  
           https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.10.0+-x64-macosx.tar.gz
           
  linux:
  
           https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.10.0+-x64-linux.tar.gz
           
2. Windows users only:
  
  Install Ubuntu:
  
  a.  turn on linux on windows (may require restart): 
      
          https://mafft.cbrc.jp/alignment/software/windowsfeatures.html
          
  b.	install ubuntu 
      
          https://mafft.cbrc.jp/alignment/software/jump.html?https://www.microsoft.com/store/p/ubuntu/9nblggh4msv6
          
 3. MAFFT- Multiple sequence alignment.. good for aligning multiple sequences that are already similar (ie. orthologs).
 
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
         
   c.  check mafft is installed:
    
         which mafft
        
        
  4. RAxML
  
   Navigate to:
   
         https://github.com/stamatak/standard-RAxML
         
   
        
