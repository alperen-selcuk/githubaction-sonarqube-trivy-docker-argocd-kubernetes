# githubaction-docker-argocd-kubernetes

![image](https://github.com/alperen-selcuk/githubaction-docker-argocd-kubernetes/assets/78741582/94067901-e44c-4451-8b05-5005892a803b)

first you need install github-runner on-premise server. you can find runner page how to install

```
mkdir actions-runner && cd actions-runner
curl -o actions-runner-linux-x64-2.308.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.308.0/actions-runner-linux-x64-2.308.0.tar.gz
tar xzf ./actions-runner-linux-x64-2.308.0.tar.gz
./config.sh --url https://github.com/your-username/your-repo --token YOUR_REGISTRATION_TOKEN
sudo ./svc.sh install
sudo ./svc.sh start
```

after that you can create kubernetes cluster, mine will be KinD same machine with github-runner so runner can reach kubernetes cluster locally. you can use public kubernetes cluster such as GKE, AKS, EKS etc

you can use this script for create KinD cluster.

```
curl https://raw.githubusercontent.com/alperen-selcuk/kind-install/main/kind.sh | bash -
```

after installation you can see KinD cluster IP local and port different. because of docker network


<img width="1277" alt="image" src="https://github.com/alperen-selcuk/githubaction-docker-argocd-kubernetes/assets/78741582/90419fc5-e935-46fd-8130-7d82955d9491">

