
# Machine Learning with Microservices

## Project Overview

This project operationalizes a Machine Learning Microservice API. It uses a pre-trained `sklearn` model to predict housing prices in Boston based on various features, such as the average number of rooms in a house, highway access, teacher-to-pupil ratios, and more. The data was initially sourced from Kaggle and can be found on [the data source page](https://www.kaggle.com/c/boston-housing).

The microservice serves housing price predictions via API calls. This framework can be extended to other pre-trained machine learning models, including those for image recognition or data labeling. The entire application has been containerized using Docker and deployed with Kubernetes.

## File Structure

### Application:
- `app.py`: The main application script
- `model_data/`: Folder containing the trained model and prediction data
- `requirements.txt`: Lists the dependencies for the app

### Outputs:
- `output_txt_files/`: Folder containing output files for Docker and Kubernetes

### Docker:
- `Dockerfile`: Defines the image for deploying the app within a container

### Utils/Tools:
- `Makefile`: Contains useful commands for setup, installation, testing, linting, running Docker, running Kubernetes, uploading to DockerHub, and more
- `run_docker.sh`: Script to build and start the container
- `run_kubernetes.sh`: Script to deploy the app on Kubernetes
- `upload_docker.sh`: Script to upload the Docker image to DockerHub
- `make_prediction.sh`: Script to test the application and generate predictions

## Setting Up the Environment

1. Create a virtual environment and activate it.
2. Run `make install` to install the necessary dependencies.

## Running `app.py`

You can run the app in three different ways: Standalone, Docker, or Kubernetes.

### 1. Standalone:

Run the app with the following command:

```bash
python app.py
```

> The app will be accessible at [http://localhost:80](http://localhost:80).

### 2. Run in Docker:

Use the `run_docker.sh` script to build and start the container:

```bash
./utils/run_docker.sh
```

The script will:
- Build a Docker image
- List images to verify that the app is dockerized
- Run the container with port mapping (host port 5000 to container port 80)

> The app will be accessible at [http://localhost:5000](http://localhost:5000).

### 3. Run in Kubernetes:

Use the `run_kubernetes.sh` script to deploy the app on a Kubernetes cluster:

```bash
./utils/run_kubernetes.sh
```

The script will:
- Start a container in the Kubernetes cluster (ensure you have a cluster ready, e.g., via `minikube`)
- Wait for the pod to start
- List the pod to confirm it is running
- Forward port 5000 (host) to port 80 (container)

> The app will be accessible at [http://localhost:5000](http://localhost:5000).

## Testing the App

To test the application, run the `make_prediction.sh` script. The model will return a predicted price based on the input provided. A sample input JSON is as follows:

```json
{  
   "CHAS":{  
      "0":0
   },
   "RM":{  
      "0":9.575
   },
   "TAX":{  
      "0":296.0
   },
   "PTRATIO":{  
      "0":15.3
   },
   "B":{  
      "0":396.9
   },
   "LSTAT":{  
      "0":4.98
   }
}
```
