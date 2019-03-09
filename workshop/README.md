# Beyond the Basics: Istio and IBM Cloud Kubernetes Service
[Istio](https://www.ibm.com/cloud/info/istio) is an open platform to connect, secure, control and observe microservices, also known as a service mesh, on cloud platforms such as Kubernetes in IBM Cloud Kubernetes Service and VMs. With Istio, You can manage network traffic, load balance across microservices, enforce access policies, verify service identity, secure service communication and observe what exactly is going on with your services.

In this course, you can see how to install Istio alongside microservices for a simple mock app called [Guestbook](https://github.com/IBM/guestbook). When you deploy Guestbook's microservices into an IBM Cloud Kubernetes Service cluster where Istio is installed, you can choose to inject the Istio Envoy sidecar proxies in the pods of certain microservices.

## Objectives
After you complete this course, you'll be able to:
- Download and install Istio in your cluster
- Deploy the Guestbook sample app
- Use metrics, logging and tracing to observe services
- Set up the Istio Ingress Gateway
- Perform simple traffic management, such as A/B tests and canary deployments
- Secure your service mesh
- Enforce policies for your microservices

## Setup

1) If following this lab as part of an IBM instructor led workshop, please follow these instructions to setup your workshop environment: https://gist.github.com/jzaccone/0cdc321e5dc8adb0dca98ca861284c01

2) Clone this repository. 

```sh
git clone https://github.com/jzaccone/istio101.git 
```

The Kubernetes cluster we have provided for the workshop has been installed with Istio in advanced.

## Workshop setup
- [Exercise 1 - Deploying Guestbook with Istio Proxy](exercise-3/README.md)
- Skip Exercise 2
- Skip Exercise 3

## Do this workshop first
- [Exercise 5 - Expose the service mesh with the Istio Ingress Gateway](exercise-5/README.md)

## Learn about the features of Istio

- [Exercise 6 - Perform traffic management](exercise-6/README.md)
- [Exercise 4 - Observe service telemetry: metrics and tracing](exercise-4/README.md)
- [Exercise 8 - Enforce policies for microservices](exercise-8/README.md)
- [Exercise 7 - Secure your service mesh](exercise-7/README.md)

## Cleaning up the Workshop

We have given you a [script](cleanup/clean_your_k8s_cluster.sh) as a conveant way to remove Istio and the guestbook
application from your instance.

**NOTE**: This puts your kubernetes cluster in a empty state, so do not run this on anything other then
a place you are willing to loose everything.
