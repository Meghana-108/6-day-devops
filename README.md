- apiVersion: apps/v1 used for Deployment  
- kind: Deployment specifies it's a deployment  
- metadata: name: blue-deploy  
- metadata: namespace: blue-ns  
- spec: replicas: 5  
- selector: matchLabels: app: ipl  
- template > metadata > labels: app: ipl  
- template > spec > containers > name: c-1  
- container image: daviddocker526/ipl-srh:latest  
- containerPort: 80 

- Login to Killercoda using Google  
- Run kubectl get nodes to verify access  

- On EC2: sudo su  
- kubectl create ns blue-ns  
- kubectl get ns  
- vi blue-deployment.yaml to write deployment  
- cat blue-deployment.yaml to verify  
- kubectl apply -f blue-deployment.yaml 
- kubectl get deployment -n blue-ns  
- kubectl get rs -n blue-ns
- kubectl get pods -n blue-ns
- kubectl describe pod <pod-name> -n blue-ns
- vi blue-service.yaml to create service  

- apiVersion: v1 used for service  
- kind: Service  
- metadata: name: blue-service 
- metadata: namespace: blue-ns  
- spec > selector: app: ipl
- spec > ports: port: 80, targetPort: 80, protocol: TCP  
- type: LoadBalancer for external access  

- Service types: ClusterIP, NodePort, LoadBalancer, ExternalName  
- ClusterIP = default, internal access only  
- NodePort = exposed using node IP and static port  
- LoadBalancer = for cloud load balancer  
- ExternalName = DNS-based redirection  

- To delete pod: kubectl delete pod <pod-name> -n blue-ns
- To change replicas: modify YAML, set replicas: 1  
- kubectl apply -f blue-deployment.yaml after change  
- Delete deployment: kubectl delete deployment blue-deploy -n blue-ns  
- Delete service: kubectl delete svc blue-service -n blue-ns 

- Launch EC2 (t3.medium, 20GB, port 22 open)  
- Use PuTTY to SSH using private key  
- sudo su  
- apt update -y

- Install Jenkins commands:  
  - wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  - echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | tee /etc/apt/sources.list.d/jenkins.list > /dev/null
  - apt-get update  
  - apt-get install jenkins

- Install Java if not installed  
- java --version to verify  
- sudo systemctl start jenkins 
- Access Jenkins: http://<EC2-PUBLIC-IP>:8080
- cat /var/lib/jenkins/secrets/initialAdminPassword to get password  
- Enter password in browser  
- Install suggested plugins  
- Create first admin user  
- Save and start using Jenkins  

- Install Jenkins plugins:  
  - AWS Credentials  
  - Kubernetes  
  - Kubernetes CLI  
  - Kubernetes Credentials  
  - Kubernetes Client API  
  - Kubernetes :: Pipeline :: DevOps Steps  
  - Kubernetes Credentials Provider  

- In AWS IAM → Create user → Programmatic access  
- Download Access Key ID and Secret  
- In Jenkins → Manage Jenkins → Credentials → System  
- Add AWS credentials → Global scope  
- Kind = AWS Credentials  
- Paste Access Key and Secret  
- Save  

- Install kubectl in Jenkins EC2  
- Install awscli: apt install awscli  
- Install unzip: apt install unzip  
- Run aws configure with AWS keys  
- Update kubeconfig:  
  - aws eks update-kubeconfig --region <region> --name <eks-cluster-name>
- Switch to Jenkins user: sudo su - jenkins 
- Run aws configure again under Jenkins user  
- cat ~/.kube/config → get contents for Jenkins config file  

- Jenkins → New Item → Pipeline  
- Enter Name → Choose Pipeline → OK  
- In Script section, write pipeline stages using pipeline {} 
- Use proper repo and deploy steps  
- Save → Apply → Run Build Now  
- View logs using "Console Output"  

