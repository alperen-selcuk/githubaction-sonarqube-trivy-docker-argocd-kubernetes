# githubaction-trivy-sonarqube-docker-argocd-kubernetes


![image](https://github.com/user-attachments/assets/80377cfc-5a71-4926-a22a-95abde9efa58)



first you need install github-runner on-premise server. you can find runner page how to install

```
mkdir actions-runner && cd actions-runner
curl -o actions-runner-linux-x64-2.308.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.308.0/actions-runner-linux-x64-2.308.0.tar.gz
tar xzf ./actions-runner-linux-x64-2.308.0.tar.gz
./config.sh --url https://github.com/your-username/your-repo --token YOUR_REGISTRATION_TOKEN
sudo ./svc.sh install
sudo ./svc.sh start
```

on your repo you need add secret variable to use pipelines.

<img width="923" alt="image" src="https://github.com/alperen-selcuk/githubaction-docker-argocd-kubernetes/assets/78741582/31d910c8-6b1a-425c-a299-b57034dbc017">


after that you can create kubernetes cluster, mine will be KinD same machine with github-runner so runner can reach kubernetes cluster locally. you can use public kubernetes cluster such as GKE, AKS, EKS etc

you can use this script for create KinD cluster.

```
curl https://raw.githubusercontent.com/alperen-selcuk/kind-install/main/kind.sh | bash -
```

after installation you can see KinD cluster IP local and port different. because of docker network


<img width="1277" alt="image" src="https://github.com/alperen-selcuk/githubaction-docker-argocd-kubernetes/assets/78741582/90419fc5-e935-46fd-8130-7d82955d9491">

### sonarqube

you can install sonarqube on dockerhost and it work reaaly good.

you can use sonarqube.yaml on the repo and install with docker compose

```
docker compose -f sonarqube.yaml up -d
```

![image](https://github.com/user-attachments/assets/f273aebc-ea59-47c1-89c0-e841b6f07a5d)


### argocd 
you can install argocd on kubernetes simple yaml file. crd and argocd

```
kubectl apply -k https://github.com/argoproj/argo-cd/manifests/crds\?ref\=stable
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
```
you can find password on secret

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```

<img width="1448" alt="image" src="https://github.com/alperen-selcuk/githubaction-docker-argocd-kubernetes/assets/78741582/d8ce15d8-fe99-4925-a6f9-338d941dd240">


then add argocd application on github and see how to work

<img width="753" alt="image" src="https://github.com/alperen-selcuk/githubaction-docker-argocd-kubernetes/assets/78741582/7bf8582a-601c-40e4-9f00-e6e979834bef">


.
