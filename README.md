<p align="center" >
    <img src="https://github.com/SAMtoBAM/fusemblr/blob/main/logo/fusemblr.png" width=100%>
</p>


[![Zenodo DOI](https://zenodo.org/badge/963417762.svg)](https://doi.org/10.5281/zenodo.15190276)
[![Anaconda_version](https://anaconda.org/samtobam/fusemblr/badges/version.svg)](https://anaconda.org/samtobam/fusemblr)
[![Anaconda_platforms](https://anaconda.org/samtobam/fusemblr/badges/platforms.svg)](https://anaconda.org/samtobam/fusemblr)
[![Anaconda_downloads](https://anaconda.org/samtobam/fusemblr/badges/downloads.svg)](https://anaconda.org/samtobam/fusemblr)
[![Anaconda-Server Badge](https://anaconda.org/samtobam/fusemblr/badges/latest_release_date.svg)](https://anaconda.org/samtobam/fusemblr)

<i>fusemblr</i> is a pipeline wrapper designed for the assembly of complex genomes using nanopore reads and paired-end illumina

<i>fusemblr</i> was designed for the <i>Fusarium oxysporum</i> assembly project (hence the name) <br/>
The pipeline uses Nanopore (the longer and higher coverage the better) and paired-end illumina reads (PacBio is optional but recommended) <br/>

Pipeline in 5 steps: <br/>
1. Downsampling of reads to a designated coverage using <i>Filtlong</i> (Default: 100X; appears to help using this coverage)
2. Polishing of downsampled reads with the paired-end illumina reads using <i>Ratatosk</i>
3. Assembly with <i>Flye</i> <br/>
   &nbsp; &nbsp; <i>removed the hard coded maximium value for the minimum overlap threshold (previously 10kb) <br/>
   &nbsp; &nbsp; by default the minimum overlap value is automatically provided as the read N95 after polishing</i>
4. Optional: Polishing of assembly with PacBio Hifi and paired-end illumina reads using <i>NextPolish2</i>
5. Filtering (minimum length 10kb), reordering and renaming using <i>Seqkit</i> and <i>awk</i>


## Easy installation

	conda install samtobam::fusemblr



## How to run

 
	fusemblr.sh -n nanopore.fq.gz -1 illumina.R1.fq.gz -2 illumina.R2.fq.gz -g 70000000
	
	Required inputs:
	-n | --nanopore		Nanopore long reads used for assembly in fastq or fasta format (*.fastq / *.fq) and can be gzipped (*.gz)
	-1 | --pair1		Paired end illumina reads in fastq format; first pair. Used for Rataosk polishing and PAQman evaluation. Can be gzipped (*.gz)
	-2 | --pair2		Paired end illumina reads in fastq format; second pair. Used for Rataosk polishing and PAQman evaluation. Can be gzipped (*.gz)	
	-g | --genomesize	Estimation of genome size, required for downsampling and assembly

	Recommended inputs:
	-h | --hifi		Pacbio HiFi reads required for assembly polishing with NextPolish2 (Recommended if available)
	-t | --threads		Number of threads for tools that accept this option (default: 1)
	
	Optional parameters:
	-m | --minsize		Minimum size of reads to keep during downsampling (Default: 5000)
	-x | --coverage		The amount of coverage for downsampling (X), based on genome size, i.e. coverage*genomesize (Default: 100)
	-v | --minovl		Minimum overlap for Flye assembly,  (Default: Calculated during run as N95 of reads used for assembly)
 	-w | --weight		The weighting used by Filtlong for selecting reads; balancing the length vs the quality (Default: 5)
	-p | --prefix		Prefix for output (default: name of assembly file (-a) before the fasta suffix)
	-o | --output		Name of output folder for all results (default: fusemblr_output)
	-c | --cleanup		Remove a large number of files produced by each of the tools that can take up a lot of space. Choose between 'yes' or 'no' (default: 'yes')
	-h | --help		Print this help message




<p align="center" >
    <img src="https://github.com/SAMtoBAM/fusemblr/blob/main/figures/fusemblr_schematic.png" width=70%>
</p>



Following assembly it is recommended that you run [PAQman](https://github.com/SAMtoBAM/PAQman) on your resulting assembly to comprehensively check the quality <br/>
This can also help you compare any assemblies you have to check for the best.


