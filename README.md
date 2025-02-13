# MLOps: Predicting House price based on ML using python, flask
```sh
git clone https://github.com/MohammadPooyaMalek/MLOps.git ~/ml_models
cd ~/ml_models
```
## Virtual Environment
```sh
python -m venv ml_models
pip install -r requirements.txt
## Training the model(RFR)
jupyter notebook --ip=0.0.0.0 --port=2501 --no-browser
python model_training.py # to create pkl model
```
## building docker image to make it CloudNative app/Portable!
```sh
docker build -t ml_model_app .
docker run -p 2501:2501 ml_model_app
mlflow ui --host 0.0.0.0 --port 2500 # Visualize the results using MLFLOW
### curl -X POST http://127.0.0.1:2501/predict -H "Content-Type: application/json" -d '{"features": [8.3252, 41, 6.984, 1.023, 322, 2.555, 37.88, -122.23]}'
```

## deploy on Kubernetes(K8s)
```sh
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
### curl -X POST http://127.0.0.1:32400/predict -H "Content-Type: application/json" -d '{"features": [8.3252, 41, 6.984, 1.023, 322, 2.555, 37.88, -122.23]}'
```


## Installing Kubeflow
```sh
export PIPELINE_VERSION=2.3.0
kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/cluster-scoped-resources?ref=$PIPELINE_VERSION"
kubectl wait --for condition=established --timeout=60s crd/applications.app.k8s.io
kubectl apply -k "github.com/kubeflow/pipelines/manifests/kustomize/env/dev?ref=$PIPELINE_VERSION"
kubectl port-forward -n kubeflow svc/ml-pipeline-ui 8080:80
```
