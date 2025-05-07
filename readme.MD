#Ingress Controller

REGION_CODE=us-east-1
CLUSTER_NAME=expense
1. OIDC provider


---
eksctl utils associate-iam-oidc-provider \
    --region $REGION_CODE \
    --cluster $CLUSTER_NAME \
    --approve
---

* IAM Policy

---
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.12.0/docs/install/iam_policy.json

---

* Create IAM Role

---
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json

---

## Install Drivers

* Add the EKS chart repo to Helm
---
* helm repo add eks https://aws.github.io/eks-charts
---

* 
Install AWS Load Balancer Controller

---
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=$CLUSTER_NAME --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller

---
