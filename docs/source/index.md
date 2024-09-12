```{include} ../README.md

```

# Welcome to hadge's documentation!

## **Introduction**

hadge is a one-stop pipeline for demultiplexing single cell mixtures. It consists of 14 methods across two workflows: hashing-based and genetics-based deconvolution methods, which can be run in 3 modes.

The genetics-based deconvolution workflow includes 5 methods:

- Demuxlet
- Freemuxlet
- scSplit
- Souporcell
- Vireo

The hashing-based deconvolution includes 7 methods:

- BFF
- Demuxem
- GMM_Demux
- hashedDrops
- HashSolo
- HTODemux
- Multiseq

## **Installation**

The hadge pipeline is implemented in Nextflow. To get started, you need to install Nextflow. Please refer to [Nextflow](https://www.nextflow.io/docs/latest/getstarted.html#installation) for more details. Alternatively, you can also install Nextflow via [conda](https://anaconda.org/bioconda/nextflow).

## **Quick start**

To execute the pipeline locally, start by cloning the repository into a directory, for example, named ${hadge_project_dir}.

```bash
cd ${hadge_project_dir} && git clone https://github.com/theislab/hadge.git
nextflow run ${hadge_project_dir}/hadge/main.nf -profile conda_singularity
```

It is also allowed to run the pipeline from a directory outside the hadge project folder.

Alternatively, you can also run the pipeline on the cloud:

```bash
nextflow run http://github.com/theislab/hadge -r main -profile conda_singularity
```

Please note:

- Choose the mode: `--mode=<genetic/hashing/rescue>`
- Specify the folder name `--outdir` to save the output files. This will create a folder automatically in the project directory.
- To run the pipeline with your own dataset, specify the input data and additional parrameters if needed.
- The pipeline can be run either locally or on a HPC with different resource specifications. As default, the pipeline will run locally. You can also set the SLURM executor by running the pipeline with `-profile cluster`.
- Please also check [](general) for more details.

To get familiar with hadge, we provide the test profile for a quick start. To access the test sample data, you can use the provided bash script to download the test data to the project directory of hadge and run the pipeline locally.

```bash
cd ${hadge_project_dir}/hadge && sh test_data/download_data.sh
nextflow run main.nf -profile test,conda_singularity
```

## Notebook

Check the [notebook](../../notebook.ipynb) to get familiar with the output of hadge.

## **Pipeline output**

By default, the pipeline is run on a single sample. In this case, all pipeline output will be saved in the folder `$projectDir/$params.outdir/$params.mode`.
When running the pipeline on multiple samples, the pipeline output will be found in the folder `"$projectDir/$params.outdir/$sampleId/$params.mode/`. To simplify this, we'll refer to this folder as `$pipeline_output_folder` from now on.

### **Intermediate output**

The pipeline saves the output of each process for two workflows separately, so you will find the results of hashing-based and genetics-based deconvolution methods in the folder `hash_demulti` and `gene_demulti` respectively.

If the pipeline is run on single sample, each demultiplexing process will generate some intermediate files in the folder in the format `$pipeline_output_folder/[method]/[method]_[task_ID]`, e.g. `htodemux/htodemux_1`.
If the pipeline is run on multiple samples, the `task_ID` will be replaced by `sampleId`. In the folder, you can find following files:

- `params.csv`: specified parameters in the task
- Output of the task, check [](genetic) and [](hashing) for more details.

### **Final output**

After each demultiplexing workflow is complete, the pipeline will generate TSV files to summarize the results in the folder `$pipeline_output_folder/[workflow]/[workflow]_summary`.

- `[method]_classification.csv`: classification of all trials for a given method
- `[method]_assignment.csv`: assignment of all trials for a given method
- `[method]_params.csv`: specified paramters of all trials for a given method
- `[mode]_classification_all.csv`: classification of all trials across different methods
- `[workflow]_assignment_all.csv`: save the assignment of all trials across different methods
- `adata` folder: stores Anndata object with filtered scRNA-seq read counts and assignment of each deconvolution method if `params.generate_anndata` is `True`.

### **Additional output for _rescue_ mode**

Before running the donor-matching preocess, the pipeline merges the results of hashing and genetic demultiplexing tools into `classification_all_genetic_and_hash.csv` and `assignment_all_genetic_and_hash.csv` in the `$pipeline_output_folder/summary` folder.

The output of the donor-matching process can be found in the folder `donor_match`, check [](rescue) for more details.

```{toctree}
:caption: 'Contents:'
:hidden: true
:maxdepth: 3
general
genetic
hashing
rescue
multisample
notebook
```

# Indices and tables

- {ref}`genindex`
- {ref}`modindex`
- {ref}`search`
