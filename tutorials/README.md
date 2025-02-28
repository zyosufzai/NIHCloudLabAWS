# AWS Tutorial Resources

---------------------------------
## Overview of Page Contents

+ [Biomedical Workflows on AWS](#bio)
+ [Download SRA Data](#sra)
+ [GWAS](#gwas)
+ [Medical Imaging](#im)
+ [RNAseq](#rna)
+ [scRNAseq](#sc)
+ [BLAST](#bl)
+ [Long Read Sequencing Analysis](#long)
+ [AI/ML Pipeline](#ai)
+ [Open Data](#open)

---------------------------------
## **Biomedical Workflows on AWS** <a name="bio"></a>

There are a lot of ways to run workflows on AWS. Here we list a few possibilities each of which may work for different research aims. As you walk through the various tutorials below, think about how you could possibly run that workflow more efficiently using a one of the other methods listed here. If you are unfamiliar with any of the terms or concepts here, please review the [AWS 101](https://github.com/STRIDES/NIHCloudLabAWS) page. 

- The most simple is probably to spin up an EC2 instance, and run your command interactively, or using `screen` or, as a [startup script](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) attached as metadata. See the [GWAS tutorial](https://training.nih-cfde.org/en/latest/Bioinformatic-Analyses/GWAS-in-the-cloud) below for more info on how to run a pipeline using EC2. 
- You could also run your pipeline via a SageMaker notebook, either by splitting out each command as a different block, or by running a workflow manager (Nextflow etc.). See [here](https://aws.amazon.com/blogs/machine-learning/scheduling-jupyter-notebooks-on-sagemaker-ephemeral-instances/) about scheduling a notebook to let it run longer. You can find some example notebooks in the [tutorials below](/tutorials/notebooks/).
- If you are running bioinformatic workflows, you can leverage the serverless functionality of AWS using [Amazon Omics](https://aws.amazon.com/omics/). Read [this blog](https://aws.amazon.com/blogs/industries/automated-end-to-end-genomics-data-storage-and-analysis-using-amazon-omics/) for more detailed information and also see if any new blogs have come out. Being a new product, AWS is writing a lot of new content on the Omics service. They also have new managed workflows called Ready2Run workflows, which you can read more about [here](https://docs.aws.amazon.com/omics/latest/dev/service-workflows.html). Note that you can use the Omics service from [the Console as well as with the APIs](https://docs.aws.amazon.com/omics/?icmpid=docs_homepage_ml). 
-  If you are using a workflow manager other than WDL or Nextflow (e. g. Snakemake), use [AWS Genomics CLI](https://aws.amazon.com/genomics-cli/), which is a wrapper for genomics workflow managers and AWS Batch (serverless computing cluster). See our [docs](/docs/agc.md) on how to set up the AGC CLI for Cloud Lab. 
- Finally, one benefit of the cloud is access to GPUs for workflow acceleration. While a lot of focus on GPU implementation will focus on AI/ML workflows, NVIDIA has software called Parabricks that will accelerate genomic workflows for pretty low costs. See the full list of command line options [here](https://docs.nvidia.com/clara/parabricks/3.7.0/index.html)) to see if your specific workflow is accelerated. For specific details on how to use Parabricks within Cloud Lab see our [guide](/docs/parabricks.md).

**For many of these tutorials, you will need Short Term Access Keys to create and use resources, particularly whenever a tutorial calls for "access key ID" and "secret key." Use [this guide](/docs/Intramural_STAKs.md) for an explanation of how to obtain and use Short Term Access Keys.**

 **Please also note, GPU machines cost more than most CPU machines, so be sure to shut these machines down after use, or apply an EC2 [lifecycle configuration](/docs/auto-shutdown-instance.md). You may also encounter service quotas to protect you from the accidental use of expensive machine types. If that happens, and you still want to use a certain instance type, follow these [instructions](/docs/service_quotas.md).**

## **Download Data From the Sequence Read Archive (SRA)** <a name="sra"></a>
Next Generation genetic sequence data is housed in the NCBI Sequence Read Archive (SRA). You can access these data using the SRA Toolkit. We walk you through this using [this notebook](/tutorials/notebooks/SRADownload), which also walks you through how to set up and search Athena tables to generate an accession list. You can also read [this guide](https://www.ncbi.nlm.nih.gov/sra/docs/sra-aws-download/) for more information on available dataset tables. Additional example notebooks can be found at this [NCBI repo](https://github.com/ncbi/ASHG-Workshop-2021). In particular, we recommend this notebook(https://github.com/ncbi/ASHG-Workshop-2021/blob/main/3_Biology_Example_AWS_Demo.ipynb), which goes into more detail on using Athena to access the results of the SRA Taxonomic Analysis Tool, which often differ from the user input species name due to contamination, error, or due to samples being metagenomic in nature.

## **Genome Wide Association Studies** <a name="gwas"></a>
Genome-wide association studies (GWAS) are large-scale investigations that analyze the genomes of many individuals to identify common genetic variants associated with traits, diseases, or other phenotypes.
- This [NIH CFDE written tutorial](https://training.nih-cfde.org/en/latest/Bioinformatic-Analyses/GWAS-in-the-cloud
) walks you through running a simple GWAS using EC2. The tutorials asks you to select the Ohio region, make sure you change your region to N. Virginia otherwise you will have network issues. Note that the CFDE page has a few other bioinformatics related tutorials like BLAST and Illumina read simulation. We also converted the GWAS tutorial to a simplified [notebook version](/tutorials/notebooks/GWAS) if you prefer that format. See our [notebook guide](/docs/Jupyter_notebook.md) for help with setting up a Jupyter environment.

## **Medical Imaging Analysis** <a name="im"></a>
Medical imaging analysis requires the analysis of large image files and often requires elastic storage and accelerated computing.
- Most medical imaging analyses are done using notebooks, so we would recommend accessing this [Jupyter Notebook](/tutorials/notebooks/SpleenLiverSegmentation) and cloning it into SageMaker. The tutorial walks through image segmentation.
- AWS has a nice intro to Machine Learning in a SageMaker notebook that predicts breast cancer from features extracted from image data, which walks you through both image analysis and some of the ML functionality of SageMaker, the notebook is found [here](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_applying_machine_learning/breast_cancer_prediction/Breast%20Cancer%20Prediction.ipynb).
- You can also view this [AWS blog](https://aws.amazon.com/blogs/machine-learning/annotate-dicom-images-and-build-an-ml-model-using-the-monai-framework-on-amazon-sagemaker/) on how to annotate DICOM images and build a custom AI model with the data.
- You can learn to deidentify medical images following this AWS [tutorial](https://aws.amazon.com/blogs/machine-learning/de-identify-medical-images-with-the-help-of-amazon-comprehend-medical-and-amazon-rekognition/).

## **RNAseq** <a name="rna"></a>
RNA-seq analysis is a high-throughput sequencing method that allows the measurement and characterization of gene expression levels and transcriptome dynamics. Workflows are typically run using workflow managers, and final results can often be visualized in notebooks.
- You can run this [Nextflow tutorial](https://nf-co.re/rnaseq/3.7) for RNAseq a variety of ways on AWS. Following the instructions outlined above, you could use EC2, SageMaker, or AWS Batch(/docs/Genomics_Workflows.md).
- For a notebook version of a complete RNAseq pipeline from Fastq to Salmon quantification from the King Lab of the University of Maine INBRE use this [notebook](/tutorials/notebooks/rnaseq-myco-tutorial-main), which we re-wrote to work on AWS. You can also use any of Ben King's excellent [notebooks](https://github.com/King-Laboratory/rnaseq-myco-notebook) as well, but they are originally written for GCP.

## **Single Cell RNAseq** <a name="sc"></a>
Single-cell RNA sequencing (scRNA-seq) is a technique that enables the analysis of gene expression at the individual cell level, providing insights into cellular heterogeneity, identifying rare cell types, and revealing cellular dynamics and functional states within complex biological systems.
- This [AWS blog](https://aws.amazon.com/blogs/publicsector/driving-innovation-single-cell-analysis-aws/) lays out a potential method that integrates a lot of the AWS native tools for running an scRNAseq pipeline. It is less of a tutorial, and more of a demo of what is possible.
-  This [NVIDIA blog](https://developer.nvidia.com/blog/accelerating-single-cell-genomic-analysis-using-rapids/) details how to run an accelerated scRNAseq pipeline using RAPIDS. You can find a link to the GitHub that has lots of example notebooks [here](https://github.com/clara-parabricks/rapids-single-cell-examples). For each example use case they show some nice benchmarking data with time and cost for each machine type. You will see that most runs cost less than $1.00 with GPU machines. If you want a CPU version that users Scanpy you can use this [notebook](https://github.com/clara-parabricks/rapids-single-cell-examples/blob/master/notebooks/hlca_lung_cpu_analysis.ipynb). Pay careful attention to the environment setup as there are a lot of dependencies for these notebooks. Create a conda environment in the terminal, then run the notebook. Consider using [mamba](https://github.com/mamba-org/mamba) to speed up environment creation. We created a [guide](/docs/create_conda_env.md) for conda environment set up as well.

## **ElasticBLAST** <a name="bl"></a>
NCBI BLAST (Basic Local Alignment Search Tool) is a widely used bioinformatics program provided by the National Center for Biotechnology Information (NCBI) that compares nucleotide or protein sequences against a large database to identify similar sequences and infer evolutionary relationships, functional annotations, and structural information. The NCBI team has written a version of BLAST for the cloud called ElasticBLAST, and you can read all about it [here](https://blast.ncbi.nlm.nih.gov/doc/elastic-blast/index.html). Essentially, ElasticBLAST helps you submit BLAST jobs to AWS Batch and write the results back to S3. Feel free to experiment with the example tutorial in Cloud Shell, or try our [notebook version](/tutorials/notebooks/ElasticBLAST/run_elastic_blast.ipynb).

## **Long Read Sequence Analysis** <a name="long"></a>
Long read DNA sequence analysis involves analyzing sequencing reads typically longer than 10 thousand base pairs (bp) in length, compared with short read sequencing where reads are about 150 bp in length.
Oxford Nanopore has a pretty complete offering of notebook tutorials for handling long read data to do a variety of things including variant calling, RNAseq, Sars-Cov-2 analysis and much more. Access the notebooks [here](https://labs.epi2me.io/nbindex/).  These notebooks expect you are running locally and accessing the epi2me notebook server. To run them in Cloud Lab, skip the first cell that connects to the server and then the rest of the notebook should run correctly, with a few tweaks. If you are just looking to try out notebooks, don't start with these. If you are interested in long read sequence analysis, then some troubleshooting may be needed to adapt these to the Cloud Lab environment. You may even need to rewrite them in a fresh notebook by adapting the commands. Feel free to reach out to our support team for help.

## **AI/ML Pipelines** <a name="ai"></a>
Machine learning is a subfield of artificial intelligence that focuses on the development of algorithms and models that enable computers to learn from and make predictions or decisions based on data, without being explicitly programmed. Artificial intelligence and machine learning algorithms are being applied to a variety of biomedical research questions, ranging from image classification to genomic variant calling. AWS is moving all AI/ML workflows to SageMaker, the Juptyer notebook platform we have used a few times already. AWS has a very general tutorial [here](https://aws.amazon.com/getting-started/hands-on/build-train-deploy-machine-learning-model-sagemaker/) on how to build out an AI pipeline on SageMaker. You can also look at the [breast cancer tutorial](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_applying_machine_learning/breast_cancer_prediction/Breast%20Cancer%20Prediction.ipynb) from the imaging section above for a more applied example. 
You can also submit a training job to SageMaker, and have your final model uploaded to S3 using [PyTorch](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html#train-a-model-with-pytorch), [Tensorflow](https://docs.aws.amazon.com/sagemaker/latest/dg/tf.html) or [Apache MXNet](https://docs.aws.amazon.com/sagemaker/latest/dg/mxnet.html).

## **Open Data** <a name="open"></a>
AWS has a lot of public data that you can integrate into your testing or use in your own research. You can access these datasets at the [Registry of Open Data on AWS](https://registry.opendata.aws/). There you can click on any of the datasets to view the S3 path to the data, as well as publications that have used those data and tutorials if available. To demonstrate, we can click the [gnomad dataset](https://registry.opendata.aws/broad-gnomad/), then get the S3 path and view the files at the command line by pasting `https://registry.opendata.aws/broad-gnomad/`. 
