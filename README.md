# NeboTask-ConfigureSecretManagement

My notes about task:
https://www.youtube.com/watch?v=Rmgo6vCytsg    - AWS Secret Manager
https://www.youtube.com/watch?v=ZdrhnGRE2Gs    - Voult

1. Create IAM User with Full Access
Create admin user and place it in Admin IAM group
Configure aws cli aws configure

- Create admin user and place it in Admin IAM group
- Configure aws cli aws configure

2. Create secretmanager record using awscli:
```js
aws --region us-east-1 secretsmanager create-secret --name db_creds34 --secret-string '{"username":"root_db", "password":"gensomepasswd", "endpoint":"mydb2.123456789012.us-east-1.rds.amazonaws.com"}' --profile private
```

3. Create aws EKS cluser:

```js
eksctl create cluster -f eks.yaml --profile private
```
- Check connection to EKS cluster
```js
kubectl get svc or kubectl get nodes
```


4)  Install the Secrets Store CSI Driver -  Deployment using Helm <br>
Link: https://secrets-store-csi-driver.sigs.k8s.io/getting-started/installation.html <br>
https://github.com/aws/secrets-store-csi-driver-provider-aws
- first - Secret Store CSI Driver
```js
helm repo add secrets-store-csi-driver https://kubernetes-sigs.github.io/secrets-store-csi-driver/charts
helm install csi-secrets-store secrets-store-csi-driver/secrets-store-csi-driver --namespace kube-system
```

- second - Installing the AWS Provider
To install the Secrets Manager and Config Provider use the YAML file in the deployment directory:
```js
kubectl apply -f https://raw.githubusercontent.com/aws/secrets-store-csi-driver-provider-aws/main/deployment/aws-provider-installer.yaml
```