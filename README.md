# pyphlawd
Python version of PHLAWD

## Setup
PyPHLAWD should be easy to setup. 

### Requirements
You will need to have 
- a database created by `PHLAWD` or the `phlawd_db_maker` (_COMING SOON:_ or your own sequences)
- python2 
	- you will also need python libraries for sqlite3 and clint (for text coloring)
- mafft : You will need a recent version (>=v7.3 works well) that has threading and merging. 
- FastTree : (if you have `treemake` on)
- blast+ : Currently, this runs `blastn` and `makeblastdb` from the blast+ package. Soon, it will use `blastp` as well.
- mcl : Markov clustering for the clustering runs (you won't need this if you only bait)

### Database
I would recommend that you use `phlawd_db_maker` to make the necessary sequence database. 

### Using your own sequences
Coming soon! The idea here is to be able to either supplement the NCBI sequences or to conduct your own analysis entirely of your own sequences. To do this, you will still want to create your own database of the sequences using the table structure already in use with the `phlawd_db_maker`. 

To add your own sequences to an analysis, regardless of whether they are supplemental or not, you will want to add your sequences to the database. You can have this data in two forms: one file per taxon with sequence data in a fasta format in that file or one file with format `>taxon_name@seq_id`. 

## Running 
There are two basic ways to run PyPHLAWD:
- `python setup_clade.py Dipsacales PHLAWD_DBS/pln.db OUTDIR/`
- `python setup_clade_bait.py Dipsacales bait_dir/ PHLAWD_DBS/pln.db OUTDIR/`

### Clustering runs
Clustering runs use both `blast` and `mcl` to cluster sequences. They can also break clusters by trees as in Yang and Smith 2014 and Yang et al. 2015 but this hasn't been necessary yet with testing and so it is turned off by default. To run a clustering run you run `python setup_clade.py Dipsacales PHLAWD_DBS/pln.db OUTDIR/` where `Dipsacales` is the name in NCBI, `PHLAWD_DBS/pln.db` is where ever the relevant database is that was made with `phlawd_db_maker`, and `OUTDIR/` is where ever you want the resulting directory to be.

### Bait runs
In addition to the clustering runs (that will look at all the available data, you can also look at only the gene regions you're interest in (as the old PHLAWD).

#### Bait files
Bait files need to be in a directory and need to have the file ending `.fa` and need to be fasta files. This requirement is necessary in order to recognize which files are intended to be the bait in the directory (so you can have other files in the directory).

### Subsetting taxa
With either of the above runs types, you can limit what taxa are considered. For this, you will need a file that has an ncbi id on each line. These will be the only ones considered. Then you run like:
- `python setup_clade.py Dipsacales PHLAWD_DBS/pln.db OUTDIR/ taxalist` 
- `python setup_clade_bait.py Dipsacales bait_dir/ PHLAWD_DBS/pln.db OUTDIR/ taxalist` 

There is a helper script that will help make your taxa list file. If you have a file that has `Genus species` on each line, you can use the script 
- `python get_ncbi_ids_for_names.py names_file PHLAWD_DBS/pln.db > taxalist`

### Updating analyses
Soon you will be able to add sequences to these runs without completely redoing all the analyses.
Coming soon!

### conf.py
There is a configuration file that has some basic configuration settings. 

## Output
PyPHLAWD will make a bunch of directories. In each directory, there will be a `clusters` folder that has either the constructed bait files or the clusters. There is also an `info.html` file in each directory. If you open the root one (the one inside the main directory made) you will see the stats for each cluster/bait that has more than 3 taxa. There will be an alignment file (with `.aln` file ending for each file) and a `.tre` if you have treemake on.

## Runtime considerations 
There are a number of things that can make the runtime shorter. The first is to do baited analyses. 
