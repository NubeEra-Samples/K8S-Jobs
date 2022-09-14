## Create Single Non-Parallel Job

cat > non-parallel-job.yml

apiVersion: batch/v1
kind: Job
metadata:
  name: wait
spec:
  template:
    metadata:
      name: wait
    spec:
      containers:
      - name: wait
        image: ubuntu
        command: ["sleep",  "20"]
      restartPolicy: Never

kubectl create -f non-parallel-job.yml

kubectl get jobs

kubectl describe job wait

kubectl get -w pods

kubect delete -f non-parallel-job.yml

## Create Multiple Non-Parallel Job
cat > multiple-non-parallel.yml

apiVersion: batch/v1
kind: Job
metadata:
  name: wait
spec:
  completions: 5
  template:
    metadata:
      name: wait
    spec:
      containers:
      - name: wait
        image: ubuntu
        command: ["sleep",  "20"]
      restartPolicy: Never

## Get Pod detail
pods=$(kubectl get pods --selector=job-name=pi --output=jsonpath='{.items[*].metadata.name}')
echo $pods


kubectl logs $pods



## Create Parallel Job
cat > parallel-job.yml

apiVersion: batch/v1
kind: Job
metadata:
  name: wait
spec:
  completions: 6
  parallelism: 2
  template:
    metadata:
      name: wait
    spec:
      containers:
      - name: wait
        image: ubuntu
        command: ["sleep",  "20"]
      restartPolicy: Never


kubectl create -f parallel-job.yml

kubectl get jobs

kubectl describe job wait

kubectl get -w pods

kubect delete -f parallel-job.yml
 