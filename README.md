# Final Project Documentation
The documentation will walk you through the Jbrowse2 Genome Browser and upload the data from poxvirus specially for monkeypox, cowpox, smallpox, fowlpox, and vaccinia.
At the end, you can be interact with the linear synteny view, 3D protein visualization, multiple sequence alignment view and the pylogenetic tree.

## 1.Pre-setting for the Jbrowse
Please follow the link to setup the jbrowse with the preferred platform. (https://github.com/bioe131/lab-8-KarinaJin) or the documentation from jbrowse website.(https://jbrowse.org/jb2/docs/quickstart_web/)

Before the next step, please make sure that you can see the following:

In your browser, now type in http://yourhost/jbrowse2/, where yourhost is either localhost or the IP address from earlier. 
Now you should see the words "It worked!" with a green box underneath saying "JBrowse 2 is installed." with some additional details.

## 2. Load and process poxvirus data

### 2.1. Download and process reference genome
Make sure you are in the temporary folder you created from the above instruction, then download the poxivirus in fasta format.

Here is the genome sequence link that we will be download from NCBI:
1. Vaccinia:  https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/860/085/GCF_000860085.1_ViralProj15241/GCF_000860085.1_ViralProj15241_genomic.fna.gz
2. Monkeypox: https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/006/465/845/GCA_006465845.1_ASM646584v1/GCA_006465845.1_ASM646584v1_genomic.fna.gz
3. Cowpox:    https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/839/185/GCF_000839185.1_ViralProj14174/GCF_000839185.1_ViralProj14174_genomic.fna.gz
4. Smallpox:  https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/859/885/GCF_000859885.1_ViralProj15197/GCF_000859885.1_ViralProj15197_genomic.fna.gz
5. Fowlpox:   https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/838/605/GCF_000838605.1_ViralProj14052/GCF_000838605.1_ViralProj14052_genomic.fna.gz

Please use the following command line to get the data into your tmp folder, and do 1 virus per time (Do the second virus after you add assembly to the jbrowse).
Let use vaccinia as the example:

```
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/860/085/GCF_000860085.1_ViralProj15241/GCF_000860085.1_ViralProj15241_genomic.fna.gz
```

Unzip the gzipped reference genome, rename it, and index it. This will allow jbrowse to rapidly access any part of the reference just by coordinate.

```
gunzip GCF_000860085.1_ViralProj15241_genomic.fna.gz
mv GCF_000860085.1_ViralProj15241_genomic.fna.gz vaccinia.fa
samtools faidx vaccinia.fa
```

Then we would like to upload it to the jbrowse2
-n means to specify the which assembly this file belongs to.

```
jbrowse add-assembly vaccinia.fa --out /var/www/html/jbrowse2 --load copy -n vaccinia
```
Please do the same command with different links of virus and different names to upload the virus.


### 2.2. Download and process genome annotations

Still in the temporary folder, download ENSEMBLE genome annotations in the GFF3 format. 

```
export GFF_ROOT=https://ftp.ensembl.org/pub/release-110/gff3/homo_sapiens
wget $GFF_ROOT/Homo_sapiens.GRCh38.110.chr.gff3.gz
gunzip Homo_sapiens.GRCh38.110.chr.gff3.gz
```

Use jbrowse to sort the annotations. jbrowse sort-gff sorts the GFF3 by refName (first column) and start position (fourth column), while making sure to preserve the header lines at the top of the file (which start with “#”). We then compress the GFF with bgzip (block gzip, which zips files into little blocks for rapid access), and index with tabix. The tabix command outputs a file named genes.gff.gz.tbi in the same directory, and we then refer to “genes.gff.gz” as a “tabix indexed GFF3 file”.

```
jbrowse sort-gff Homo_sapiens.GRCh38.110.chr.gff3 > genes.gff
bgzip genes.gff
tabix genes.gff.gz
```

### 4.5. Load annotation track into jbrowse

```
jbrowse add-track genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy
```

### 4.6. Index for search-by-gene

Run the “jbrowse text-index” command to allow users to search by gene name within JBrowse 2.

In the temporary work directory, run the following command.

```
jbrowse text-index --out $APACHE_ROOT/jbrowse2
```

## 5.0 Use your genome browser to explore a gene of interest

## 2. Upload data from the NCBI to the Jbrowse by using the command line tool.
### 2.1 
```

```
