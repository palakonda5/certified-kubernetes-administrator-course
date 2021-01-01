# Creating a new user on a pure Kubernetes 1.19.2 cluster

Using documentation: https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#normal-user

## Step 1 Create private key

```
openssl genrsa -out ken.key 2048
```

## Step 2 Create CSR

```
openssl req -new -key john.key -out john.csr -subj "/CN=ken"
```

## Step 3 Create Kubernetes certificate

Get the csr request in base64 and replace it in `/vagrant/ubuntu/user/csr.yaml.orig`
with a new file `/vagrant/ubuntu/user/csr.yaml.generated`

Then run `kubectl apply -f /vagrant/ubuntu/user/csr.yaml.generated`

## Step 4 Approve CSR

`kubectl certificate approve ken`

## Step 5 Create Namespace

`kubectl create ns ken`

## Step 5 Create Role

`kubectl create -f /vagrant/ubuntu/user/role.yaml`

## Step 6 Create Role Binding

`kubectl create -f /vagrant/ubuntu/user/rolebinding.yaml`

## Step 7 Download Certificate

`kubectl get csr/ken -o jsonpath="{.status.certificate}"`

to `/vagrant/ubuntu/user/ken.crt`

## Step 8 Create User

```
kubectl config set-credentials ken --client-key=/vagrant/ubuntu/user/ken.key --client-certificate=/vagrant/ubuntu/user/ken.crt --embed-certs=true
```

## Step 8 Create Context

```
kubectl config set-context ken --cluster=kubernetes --user=ken
```

## Step 9 Text Context

```
kubectl config use-context ken
```
