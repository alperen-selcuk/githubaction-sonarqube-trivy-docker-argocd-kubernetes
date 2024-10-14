# githubaction-trivy-sonarqube-docker-argocd-kubernetes


![image](https://github.com/user-attachments/assets/e07ce44f-7406-4c4e-9392-a475ae6c7940)



### GITHUB RUNNER


first you need install github-runner on-premise server. you can find runner page how to install. you can find runner settings on

**github repository -> settings -> actions -> runners -> self-hosted runners -> add runner**

```
mkdir actions-runner && cd actions-runner
curl -o actions-runner-linux-x64-2.308.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.308.0/actions-runner-linux-x64-2.308.0.tar.gz
tar xzf ./actions-runner-linux-x64-2.308.0.tar.gz
./config.sh --url https://github.com/your-username/your-repo --token YOUR_REGISTRATION_TOKEN
sudo ./svc.sh install
sudo ./svc.sh start
```

### SONARQUBE

you can install sonarqube on dockerhost and it work really good.

you can use sonarqube.yaml on the repo and install with docker compose

```
docker compose -f sonarqube.yaml up -d
```

you can access sonarqube on 9000 port. user: admin pass: admin

after installation first you create project on sonarqube

![image](https://github.com/user-attachments/assets/72a6278f-f741-4c83-a286-c52534aa3a2f)

than generate token on sonarqube. save this token.

![image](https://github.com/user-attachments/assets/810fc6aa-0752-4fc6-ae2a-ba992d40370b)


### GITHUB SECRET 

on your repo you need add secret variable to use pipelines.

which registry you use need username password to push docker images. we use also github push for argocd for this stage we need github PAT. 

<img width="923" alt="image" src="https://github.com/alperen-selcuk/githubaction-docker-argocd-kubernetes/assets/78741582/31d910c8-6b1a-425c-a299-b57034dbc017">

add sonar URL and token for github yaml via secrets too.

<img width="574" alt="image" src="https://github.com/user-attachments/assets/5c0d1eca-d07b-4488-9251-4babdaf53792">



### KUBERNETES 

after that you can create kubernetes cluster, mine will be KinD on same machine with github-runner and sonarqube. by this way runner can reach kubernetes cluster locally. you can use public kubernetes cluster such as GKE, AKS, EKS etc

below this script is enough to creation of KinD cluster. it should install docker and create kind kubernetes.

```
curl https://raw.githubusercontent.com/alperen-selcuk/kind-install/main/kind.sh | bash -
```

after installation you can see KinD cluster IP local and port different. because of docker network


<img width="1277" alt="image" src="https://github.com/alperen-selcuk/githubaction-docker-argocd-kubernetes/assets/78741582/90419fc5-e935-46fd-8130-7d82955d9491">


### ARGOCD 

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
