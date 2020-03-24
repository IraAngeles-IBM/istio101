# Exercise 5 - Expose the service mesh with the Istio Ingress Gateway

The components deployed on the service mesh by default are not exposed outside the cluster. External access to individual services so far has been provided by creating an external load balancer or node port on each service.

An Ingress Gateway resource can be created to allow external requests through the Istio Ingress Gateway to the backing services.

![Using istio gateway](../README_images/istio2.jpg)

## Expose the Guestbook app with Ingress Gateway

1. Configure the guestbook default route with the Istio Ingress Gateway. The `guestbook-gateway.yaml` file is in this repository (istio101) in the `workshop/plans` directory.

```shell
cd /userdata
git clone https://github.com/IBMAppModernization/istio101.git
cd istio101/workshop/plans
kubectl create -f guestbook-gateway.yaml
```

1. Get the **EXTERNAL-IP** of the Istio Ingress Gateway.

```shell
kubectl get service istio-ingressgateway -n istio-system
```

Output:

```shell
NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP      PORT                                                  AGE
istio-ingressgateway   LoadBalancer   172.21.88.116   169.60.156.219   15020:30582/TCP,80:31685/TCP,443:30035/TCP,...        3h10m
```

1. Make note of the external IP address that you retrieved in the previous step, as it will be used to access the Guestbook app in later parts of the course. Create an environment variable called $INGRESS_IP with your IP address.

Example:

```shell
export INGRESS_IP=169.60.156.219
```

## Connect Istio Ingress Gateway to the IBM Cloud Kubernetes Service NLB Host Name

NLB host names are the DNS host names you can generate for each IBM Cloud Kubernetes deployment exposed with the Network LoadBalancer(NLB) service. These host names come with SSL certificate, the DNS registration, and health checks so you can benefit from them for any deployments that you expose via the NLB on IBM Cloud Kubernetes Service.

You can run the IBM Cloud Kubernetes Service ALB, an API gateway of your choice, an Istio ingress gateway, and an MQTT server in parallel in your IBM Cloud Kubernetes Service cluster. Each one will have its own:

```text
1. Publicly available wildcard host name
2. Wildcard SSL certificate associated with the host name
3. Health checks that you can configure if you use multizone deployments.
```

Let's leverage this feature with Istio ingress gateway:

1. Let's first check if you have any NLB host names for your cluster:

    ```shell
    ibmcloud ks nlb-dnss --cluster $MYCLUSTER
    ```

   You may see a single entry that was created initially with the cluster. You will want a new NLB host specifically for the istio ingress gateway

1. Create the NLB host with the Istio ingress gateway's public IP address:

    ```shell
    ibmcloud ks nlb-dns create classic --cluster $MYCLUSTER --ip $INGRESS_IP
    ```

1. Verify the NLB host names for your cluster, this should now include the newly added host:

    ```shell
    ibmcloud ks nlb-dnss --cluster $MYCLUSTER
    ```

    Example output:

    ```shell
    ibmcloud ks nlb-dnss --cluster $MYCLUSTER
    OK
    Hostname                                                                                     IP(s)            Health Monitor   SSL Cert Status   SSL Cert Secret Name                                     Secret Namespace

   demo-cluster-5290c8c8e5797924dc1ad5d1b85b37c0-0000.us-south.containers.appdomain.cloud   169.60.156.218   None             created           demo-cluster-5290c8c8e5797924dc1ad5d1b85b37c0-0000   default
   demo-cluster-5290c8c8e5797924dc1ad5d1b85b37c0-0001.us-south.containers.appdomain.cloud   169.60.156.219   None             created           demo-cluster-5290c8c8e5797924dc1ad5d1b85b37c0-0001   default
    ```

1. Make note of the NLB host name (<nlb_host_name>), as it will be used to access your Guestbook app in later parts of the course. Create an environment variable for it and test using curl or visit in your browser.

    Example:

    ```shell
    export NLB_HOSTNAME=demo-cluster-5290c8c8e5797924dc1ad5d1b85b37c0-0001.us-south.containers.appdomain.cloud
    ```

    ```shell
    curl $NLB_HOSTNAME
    ```

1. Enable health check of the NLB host for Istio ingress gateway:

    ```shell
    ibmcloud ks nlb-dns monitor configure --cluster $MYCLUSTER --nlb-host $NLB_HOSTNAME --type HTTP --description "Istio ingress gateway health check" --path "/healthz/ready" --port 15020 --enable
    ```

1. Monitor the health check of the NLB host for Istio ingress gateway:

    ```shell
    ibmcloud ks nlb-dns monitor status --cluster $MYCLUSTER
    ```

    After waiting for a bit, you should start to see the health monitor's status changed to Enabled.

    Example output:

    ```shell
    Retrieving health check monitor statuses for NLB pods...
    OK
    Hostname                                                                                     IP               Health Monitor   H.Monitor Status
    demo-cluster-5290c8c8e5797924dc1ad5d1b85b37c0-0000.us-south.containers.appdomain.cloud   169.60.156.218   None             N/A
    demo-cluster-5290c8c8e5797924dc1ad5d1b85b37c0-0001.us-south.containers.appdomain.cloud   169.60.156.219   Enabled          Healthy  
    ```

Congratulations! You extended the base Ingress features by providing a DNS entry to the Istio service.

## References

* [Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)

* [Istio Ingress](https://istio.io/docs/tasks/traffic-management/ingress.html)

* [Bring your own ALB](https://www.ibm.com/blogs/bluemix/2019/04/bring-your-own-alb-dns-with-health-checks-and-ssl-certificates-beta/)

## [Continue to Exercise 6 - Traffic Management](../exercise-6/README.md)
