# Continuous Adaptation with VertexAI's AutoML and Pipeline

This repository contains two notebooks to demonstrate how to automate to produce a new AutoML model when the new dataset comes in. This project uses `Vertex AI` in general, `Vertex Managed Dataset`, `Vertex Pipeline`, Vertex AutoML`, `Cloud Storage`, and `Cloud Function` in Google Cloud Platform.

![](https://i.ibb.co/FsVN6My/Screen-Shot-2022-01-14-at-12-34-03-PM.png)

# About the notebooks

There are two notebooks for this project. Everything can be setup by running each cell in the notebooks. The only thing you need to do manually is to setup IAMs. 

## IAMs Setup

1. For `Vertex Pipeline`, we need `Vertex Admin`, `Cloud Storage Viewer`, `Cloud Storage Editor` permissions(Some ML components need to access the managed dataset, and the Pipeline itself is stored in GCS(Google Cloud Storage) bukcet, so we need the listed permissions.). This can be setup under `compute` service account since `Vertex Pipeline` uses compute engines to run each component of the ML Pipeline. Also don't forget to enable `compute service account` and Vertex AI API.

2. For `Cloud Function`, we need `Vertex Admin` and `Cloud Build` enabled. Since the docker image that the `Cloud Function` bases should be built by `Cloud Build`, we need `Cloud Build API` enabled. Also, `Cloud Funcion` will trigger the `Vertex Pipeline`, so we need `Vertex Admin`

## Notebooks

1. cifar-10-vertex-autoML-pipeline
- This notebook should be run before the second notebook. It will prepare the `Kubeflow Pipeline` with two additional custom components which determines if there is existing dataset or not. The entire notebook produces the pipeline spec `json` file and put it in the GCS bucket.

<img src="https://i.ibb.co/kXcVrcm/Screen-Shot-2022-01-14-at-10-55-29-AM.png" height="1000"/>

2. prepare-cifar10-subset
- This notebook creates two subsets of CIFAR10 dataset for simulating purpose. It also provides the codebase for `Cloud Function`, and you can directly deploy it within the notebook. Lastly, it tries to simulate the continuous adaptation scenario by putting each subset of data sequentially. 
