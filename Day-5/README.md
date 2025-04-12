
- drawbacks of docker:  
  self maintenance of container is not provided..  
  docker cannot handle load balancer.  
  it will not handle auto scaling.

- Kubernetes:(k8s)(8 features hence the name)  
  it is a containerization management tool  
  orchestration tool(to manage objects)  
  history:  
  google introduced Kubernetes but initially wasn't open source...  
  in 2014 google donated kubernetes to cncf(cloud native computing foundation)

- features:  
  1)autoscaling(able to scale up the server by Kubernetes itself also it will scale down the resurces when traffic is decreasing..)  
  2)auto-healing(deleted resources can be saved again)  
  3)load balancing(responsible to handle the application traffic between the servers equally....by load balancer)  
  4)platform independent(Kubernetes will work in any OS)  
  5)roll back(we can switch to previous versions)  
  6)health monitoring of containers  
  7)fault tolerance(Kubernetes is combination of nodes...if any nodes get delted..it will alert this to us)  
  8)orchestration(manage objects)

- kubernetes architecture:  
  cluster is a combination of nodes..  
  it has 2 types of nodes..  
  1)master node  
  2)worker node

- MASTER NODE  
  API SERVER IS ENTRYPOINT  
  ETCD-DATABASE FOR CLUSTER  
  CONTROLLER MANAGER - CONSISTS ALL INFORMATION AND SENDS IT TO API SERVER  
  SCHEDULER-MANAGES TIMINGS

- WORKER NODE  
  KUBELET -PRIMARY SOURCE ,CREATES POD FOR APPLICATION  
  POD-SMALLEST DEPLOYABLE UNIT(PROVIDED  BY KUBELET)  
  CONTAINER ENGINE-RUN CONTAINER IN AN ENVIRONMENT  
  KUBE-PROXY network configuration

- steps to create cluster:  
  ec2->launch instance  
  create a name  
  ubuntu os  
  t2.medium (Kubernetes requires more cpu)  
  allow ssh and http  
  memory upto 25  
  launch instance  
  connect to your ec2 instance  
  sudo  
  sudo su  
  to update the os:apt update -y

- kubectl has to be installed..  
  go to kubectl install website  
  1)Install kubectl binary with curl on Linux:  
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"  
  2)Validate the binary (optional):  
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"  
  or   echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check  
  3)Install kubectl  
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl  
  execute these 3 commands next:  
  chmod +x kubectl  
  mkdir -p ~/.local/bin  
  mv ./kubectl ~/.local/bin/kubectl  
  4)Test to ensure the version you installed is up-to-date:  
  kubectl version --client

- eksctl install on ubuntu os ..search  
  For Unix:  
  ARCH=amd64  
  PLATFORM=$(uname -s)_$ARCH  
  curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"  
  (Optional) Verify checksum  
  curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check  
  tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz  
  sudo mv /tmp/eksctl /usr/local/bin  
  go to server and paste it all together and enter....  
  go to your server and type:apt install unzip -y

- awscli install on ubuntu os:  
  Installing or updating to the latest version of the AWS CLI:  
  To install the AWS CLI, run the following commands:  
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"  
  unzip awscliv2.zip  
  sudo ./aws/install  
  aws --version  
  (it will show the version)

- access keys  
  using IAM user  
  users->  
  create user: give user name  
  attach policies  
  check in administrator access...next...create user.....  
  in the user:  
  security groups:  
  access keys:confirm and next..create access key.....  
  download .csv file

- command to provide the keys on the server:  
  aws configure  
  (to configure our access keys)  
  paste (copy access key id amd paste it)  
  default region name:provide the same name in the server..from ec2  
  next enter  
  enter

- eksctl create cluster --name blue-cluster(name) --region sa-east-1(your region) --zone sa-east-1a,sa-east-1b --nodegroup-name red-node --node-type t2.medium --node 2  
  (sa-east-1 its zones are :sa-east-1a,sa-east-1b,etc...)(this will be the master node)  
  we need to create worker node group....so: --nodegroup-name red-node...worker nodes instance type is t2.medium..to specify number of node :--node 2  
  it will take around 10 mins to create a cluster...

- ec2 instance....connect.. or we can use killercoda 3 labs....playgrounds...Kubernetes 1.32....sign-in with google....continue...2 check in ..and save...lab is available only for 1 hour...then session expires ...  
  using ec2 instance:(namespace is out object)  
  sudo su  
  kubectl get nodes  
  kubectl get namespace  
  kubectl create namespace white  
  kubectl get namespace

- YAML is a human-readable data serialization language that is often used for writing configuration files. Depending on whom you ask, YAML stands for yet another markup language or YAML ain’t markup language (a recursive acronym), which emphasizes that YAML is for data, not documents.  
  touch blue.yaml  
  ls  
  vi blue.yaml  
  ls  

yaml
apiVersion: v1  
kind: Namespace  
metadata:  
 name: white-2  


- esc :wq enter  
  cat blue.yaml  
  kubectl apply -f blue.yaml  
  kubectl get ns  
  (imperative approach we give command to create the namespace and declarative process we define the namespace in yaml file)  
  (so in 2 approaches we creates our objects i.e...our namespace..)

- vi blue-pod.yaml  
yaml
apiVersion: v1  
kind: Pod  
metadata:  
  name: blue-pod2  
  namespace: white-2  
spec:  
  containers:  
    - name: c-1  
      image: daviddocker526/ipl  
      ports:  
        - containerPort: 80  

- cat blue-pod.yaml  
  kubectl apply -f blue-pod.yaml  
  kubectl get pods -n white-2  
  kubectl logs blue-pod2 -n white-2(it is not sufficient info)  
  kubectl describe pod blue-pod2 -n white-2  
  kubectl get pods -n white-2  
  kubectl delete pod blue-pod2 -n white-2  
  kubectl get pods -n white-2

- its pods responsibility to run the applications...we need multiple pods to manage traffic...  
  design state number is the number of pods available/required...  
  replica set ..it will maintain the design state number....as per requirements it will maintain the fixed number of pods...  
  it will create a new pod..to reduce downtime...

- vi white-replica.yaml  

yaml
apiVersion: apps/v1  
kind: ReplicaSet  
metadata:  
  name: blue-replica1  
  namespace: white-2  
spec:  
  replicas: 5  
  selector:  
    matchLabels:  
      app: ipl  
  template:  
    metadata:  
      labels:  
        app: ipl  
    spec:  
      containers:  
        -  name: c-2  
          image: daviddocker526/ipl  
          ports:  
            - containerPort: 80  


- kubectl get rs -n white-replica  
  kubectl get pods -n white-2
