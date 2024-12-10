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

Still in the temporary folder, download genome annotations in the GFF format. 

Here is the genome annotations files' link that we will be download from NCBI:
1. Vaccinia:  https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/860/085/GCF_000860085.1_ViralProj15241/GCF_000860085.1_ViralProj15241_genomic.gff.gz
2. Monkeypox: https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/006/465/845/GCA_006465845.1_ASM646584v1/GCA_006465845.1_ASM646584v1_genomic.gff.gz
3. Cowpox:    https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/839/185/GCF_000839185.1_ViralProj14174/GCF_000839185.1_ViralProj14174_genomic.gff.gz
4. Smallpox:  https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/859/885/GCF_000859885.1_ViralProj15197/GCF_000859885.1_ViralProj15197_genomic.gff.gz
5. Fowlpox:   https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/838/605/GCF_000838605.1_ViralProj14052/GCF_000838605.1_ViralProj14052_genomic.gff.gz

Please use the following command line to get the data into your tmp folder, and do 1 virus per time (Do the second virus after you add track to the jbrowse).
Let use vaccinia as the example:

```
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/860/085/GCF_000860085.1_ViralProj15241/GCF_000860085.1_ViralProj15241_genomic.gff.gz
gunzip GCF_000860085.1_ViralProj15241_genomic.gff.gz
```

```
jbrowse sort-gff GCF_000860085.1_ViralProj15241_genomic.gff.gz > vaccinia_ann.gff
bgzip vaccinia_ann.gff
tabix vaccinia_ann.gff.gz
```

Then we would like to load the annotation track into jbrowse by the following command line:
```
jbrowse add-track genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy
```
Please do the same command with different links of virus and different names to upload the virus.


## 3.0 Embedded the Plugin for the feature
Now on your terminal, go to the jbrowse2 folder with this kind of path: /var/www/html/jbrowse2
Open the file called "config.json" by using the text editor. In this documentation, it will be perform by nano.
On the path: /var/www/html/jbrowse2
```
nano config.json
```
After this command line, it will open a text editor, please add the following to the file:
Copy paste the following after the default session json object.

```
"plugins": [
    {
      "name": "Protein3d",
      "url": "https://unpkg.com/jbrowse-plugin-protein3d/dist/jbrowse-plugin-protein3d.umd.production.min.js"
    },
{
      "name": "MsaView",
      "url": "https://unpkg.com/jbrowse-plugin-msaview/dist/jbrowse-plugin-msaview.umd.production.min.js"
    }
  ]
```
shows like the following: {picture link}

This will make the plugin embedded into the jbrowse all the time. Don't need to installed it everytime when open a new session.

## 4.0 Download minimap2 to generate the paf file for linear synteny view feature
First make sure that you have the minimap2, please use the link to install the minimap. (https://github.com/lh3/minimap2?tab=readme-ov-file#install)
After you install the minimap2,
we will using the minimap2 to generate the paf file for the track.

Example: I want to create a paf file to see the common between monkeypox and other viruses.
From the previous step we already have the viruses FASTA file in our tmp directory.
Use the below command:

```
minimap2 monkeypox.fna cowpox.fna smallpox.fna fowlpox.fna vaccinia.fna > other_vs_monkeypox.paf
```
after you create the paf file
we need to add track to our jbrowse
**Important** The order is really matters! from the above command, it will creating the paf file that the common between cowpox and monkeypox, smallpox and monkeypox etc. So when adding the track, we should let the monkeypox assembly name at the last, and follow the order when we create in the for pad file.

```
jbrowse add-track other_vs_monkeypox.paf --assemblyNames cowpox,smallpox,fowlpox,vaccinia,monkeypox --load copy --out /var/www/html/jbrowse2
```


## 5.0 Use MAFFT to create multiple sequence aligment
Installing the MAFFT by using the following linke: https://mafft.cbrc.jp/alignment/software/


## 2. Upload data from the NCBI to the Jbrowse by using the command line tool.
### 2.1 
```

```
