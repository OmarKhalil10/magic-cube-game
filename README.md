# Getting Started with ACK using kubectl

## Overview

Welcome to the guide on using Alibaba Cloud Container Service for Kubernetes (ACK) with kubectl. This guide will walk you through the process of deploying, exposing, and monitoring a containerized application within an ACK cluster. ACK provides high-performance management services for containerized applications, allowing you to efficiently manage applications running on the cloud.

![ack-cube application](/Images/ack-cube-application.png)

**Updated at:** 03-10-2024

## Background Information

This guide demonstrates the deployment of an `ack-cube` application within a professional Kubernetes cluster using a container image. The `ack-cube` application is an online magic cube game. By following the steps outlined in this guide, you'll create and deploy a professional Kubernetes cluster along with an application providing a magic cube game.

The container image used for deploying the sample application is built based on an open-source project, and its address is `registry.cn-hangzhou.aliyuncs.com/acr-toolkit/ack-cube:1.0`.

## Prerequisites

Before you begin, make sure you have the following:

- [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) - a command-line tool provided by Kubernetes for connecting to and managing Kubernetes clusters.

- You are familiar with the basic concepts of Kubernetes.

## Procedure

![Procedure](/Images/Procedure.png)

## Step 1: Activate and Grant Permissions to ACK

If this is your first time using Alibaba Cloud Container Service for Kubernetes (ACK), follow the steps below to activate ACK and grant the necessary permissions:

1. Go to the [Container Service for Kubernetes page](https://cs.console.aliyun.com/).

2. Read and select the Container Service for Kubernetes Terms of Service.

3. Click **Activate Now.**

4. Log in to the [ACK console](https://cs.console.aliyun.com/?spm=a2c63.p38356.0.0.13801cb3tEIE8B).

5. On the **Container service needs to create default roles** page, click **Go to RAM console**. On the [Cloud Resource Access Authorization](https://ram.console.aliyun.com/?spm=a2c63.p38356.0.0.13801cb3tEIE8B#/role/authorize?request=%7B%22ReturnUrl%22:%22https:%2F%2Fcs.console.aliyun.com%2F%22,%22Service%22:%22CS%22,%22Requests%22:%7B%22request1%22:%7B%22RoleName%22:%22AliyunCSManagedLogRole%22,%22TemplateId%22:%22AliyunCSManagedLogRole%22%7D,%22request2%22:%7B%22RoleName%22:%22AliyunCSManagedCmsRole%22,%22TemplateId%22:%22AliyunCSManagedCmsRole%22%7D,%22request3%22:%7B%22RoleName%22:%22AliyunCSManagedCsiRole%22,%22TemplateId%22:%22AliyunCSManagedCsiRole%22%7D,%22request4%22:%7B%22RoleName%22:%22AliyunCSManagedVKRole%22,%22TemplateId%22:%22AliyunCSManagedVKRole%22%7D,%22request5%22:%7B%22RoleName%22:%22AliyunCSClusterRole%22,%22TemplateId%22:%22Cluster%22%7D,%22request6%22:%7B%22RoleName%22:%22AliyunCSServerlessKubernetesRole%22,%22TemplateId%22:%22ServerlessKubernetes%22%7D,%22request7%22:%7B%22RoleName%22:%22AliyunCSKubernetesAuditRole%22,%22TemplateId%22:%22KubernetesAudit%22%7D,%22request8%22:%7B%22RoleName%22:%22AliyunCSManagedNetworkRole%22,%22TemplateId%22:%22AliyunCSManagedNetworkRole%22%7D,%22request9%22:%7B%22RoleName%22:%22AliyunCSDefaultRole%22,%22TemplateId%22:%22Default%22%7D,%22request10%22:%7B%22RoleName%22:%22AliyunCSManagedKubernetesRole%22,%22TemplateId%22:%22ManagedKubernetes%22%7D,%22request11%22:%7B%22RoleName%22:%22AliyunCSManagedArmsRole%22,%22TemplateId%22:%22AliyunCSManagedArmsRole%22%7D%7D%7D) page, click **Confirm Authorization Policy**.

6. After assigning the Resource Access Management (RAM) roles to ACK, log in to the ACK console again to get started with ACK.

## Step 2: Create an ACK Pro Cluster

This step shows how to create an ACK Pro cluster. Default settings are used for most cluster parameters. For more information about cluster parameters, see Create an [ACK Pro cluster](https://www.alibabacloud.com/help/en/doc-detail/176833.html#task-skz-qwk-qfb).

1. Log in to the [ACK console](https://www.alibabacloud.com/help/en/doc-detail/176833.html#task-skz-qwk-qfb).

2. In the left-side navigation pane of the ACK console, click **Clusters**.

3. In the upper-right corner of the **Clusters** page, click **Create Kubernetes Cluster**.

4. On the **Managed Kubernetes** tab, set cluster parameters as described in the following table. Use default settings for the parameters that are not included in the table.
   
## Step 1: Cluster Information

| Parameter | Description |
|-----------|-------------|
| Cluster Name | Enter a name for the cluster. In this example, the name is set to "ACK-Demo." |
| Cluster Specification | Choose the type of the cluster. In this example, "Professional" is selected. Refer to the [Overview of ACK Pro clusters](https://www.alibabacloud.com/help/doc-detail/216614.htm) for more information. |
| Region | Select a region to deploy the cluster. In this example, the "China (Beijing)" region is selected. |
| VPC | ACK clusters must be deployed in virtual private clouds (VPCs). Specify a VPC in the same region as the cluster. In this example, a VPC named "vpc-ack-demo" is created in the "China (Beijing)" region. To create a VPC, click "Create VPC." For more information, see [Create and manage a VPC](https://www.alibabacloud.com/help/doc-detail/27739.htm). |

## Step 2: vSwitch Configuration

| Parameter | Description |
|-----------|-------------|
| vSwitch | Select vSwitches for nodes in the cluster to communicate with each other. In this example, a vSwitch named "vswitch-ack-demo" is created in the "vpc-ack-demo" VPC. Select "vswitch-ack-demo" in the vSwitch list. To create a vSwitch, click "Create vSwitch." For more information, see [Create and manage a vSwitch](https://www.alibabacloud.com/help/doc-detail/27741.htm). |
| Access to API Server | Specify whether to expose the Kubernetes API server to the Internet. In this example, "Expose API Server with EIP" is selected. |

## Step 3: Node Pool Configurations

| Parameter | Description |
|-----------|-------------|
| Instance Type | Select instance types used to deploy nodes. Choose instance types with at least 4 vCPUs and 8 GiB of memory for stability. Refer to [Overview of instance families](https://www.alibabacloud.com/help/doc-detail/25378.htm) for more information. |
| Quantity | Specify the number of worker nodes. In this example, it's set to 2 to avoid single points of failure. |
| System Disk | Set the system disk for nodes. In this example, "Enhanced SSD" is selected, and the disk size is set to 40 GiB. |
| Logon Type | Select the logon type for nodes. In this example, "Password Logon" is selected, and a password is specified. |

## Step 4: Confirm Order

- Click **Next: Component Configurations** and use default settings for all component parameters.
- Click **Next: Confirm Order**, read and select **Terms of Service**, and then click **Create Cluster**.

Now, your ACK cluster will be deployed based on the specified configurations. You can proceed with further management using kubectl or the ACK console.

# Connect to the ACK Cluster

This step guides you on how to connect to your Alibaba Cloud Container Service for Kubernetes (ACK) cluster using a kubectl client. Follow the methods below for seamless connectivity.

## Connect to the Cluster using kubectl Client

1. Log in to the [ACK console](https://partners-intl.console.aliyun.com/#/cs).

2. In the left-side navigation pane, click on "Clusters."

3. On the Clusters page, select the name of your cluster.

4. Navigate to the "Cluster Information" page and click on the "Connection Information" tab.

5. Click "Copy" on the "Public Access" tab to copy the credentials for accessing the cluster over the Internet.

6. Open the config file in the `$HOME/.kube` directory using a text editor. If the `.kube` folder and the config file do not exist in the `$HOME/` directory, manually create the folder and file.

7. Paste the copied credentials into the config file, save it, and then exit the text editor.

   > **Note**: Ensure that the `.kube` folder and the config file exist in the `$HOME/` directory.

8. Run the following kubectl command to connect to the cluster:

   ```bash
   kubectl get namespace
   ```

   Expected output:
   ```
   NAME              STATUS   AGE
   arms-prom         Active   4h39m
   default           Active   4h39m
   kube-node-lease   Active   4h39m
   kube-public       Active   4h39m
   kube-system       Active   4h39m
   ```

Now you are connected to your ACK cluster, and you can proceed with managing namespaces and other configurations using kubectl commands.

# Deploy and Expose an Application

In this step, you will deploy a stateless application using kubectl, creating a Deployment, and exposing it with a LoadBalancer Service. Follow the instructions below:

1. Create a file named `ack-cube.yaml` and use the following YAML template:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: ack-cube
     labels:
       app: ack-cube
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: ack-cube
     template:
       metadata:
         labels:
           app: ack-cube
       spec:
         containers:
         - name: ack-cube
           image: registry.cn-hangzhou.aliyuncs.com/acr-toolkit/ack-cube:1.0
           ports:
           - containerPort: 80
           resources:
             limits:
               cpu: '1'
               memory: 1Gi
             requests:
               cpu: 500m
               memory: 512Mi
   ```

2. Deploy the `ack-cube` application using the following command:

   ```bash
   kubectl apply -f ack-cube.yaml
   ```

3. Query the deployment status with the following command:

   ```bash
   kubectl get deployment ack-cube
   ```

   Expected output:

   ```plaintext
   NAME       READY   UP-TO-DATE   AVAILABLE   AGE
   ack-cube   2/2     2            2           96s
   ```

4. Create a file named `ack-cube-svc.yaml` and use the following YAML template:

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     labels:
       app: ack-cube
     name: ack-cube-svc
     namespace: default
   spec:
     ports:
     - port: 80
       protocol: TCP
       targetPort: 80
     selector:
       app: ack-cube
     type: LoadBalancer
   ```

5. Create a LoadBalancer Service to expose the application:

   ```bash
   kubectl apply -f ack-cube-svc.yaml
   ```

6. Verify the creation of the LoadBalancer Service:

   ```bash
   kubectl get svc ack-cube-svc
   ```

   Expected output:

   ```plaintext
   NAME           TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)        AGE
   ack-cube-svc   LoadBalancer   172.16.72.161   47.94.xx.xx   80:31547/TCP   32s
   ```

7. **Step 5: Test the application**
   - Enter the IP address (**EXTERNAL-IP**) in your browser's address bar to start the magic cube game.

8. **Step 6: Monitor the application**
   - For monitoring instructions, refer to the [Observability overview](https://www.alibabacloud.com/help/en/ack/ack-managed-and-ack-dedicated/user-guide/observability-overview#task-2042182).

## References

- To enable auto-scaling, configure Horizontal Pod Autoscaler (HPA), Cron Horizontal Pod Autoscaler (CronHPA), and Vertical Pod Autoscaler (VPA). Refer to [Auto scaling overview](https://www.alibabacloud.com/help/en/ack/ack-managed-and-ack-dedicated/user-guide/auto-scaling-overview#concept-1918532).
- Use Ingresses for Layer 7 application traffic routing. See [Create an NGINX Ingress](https://www.alibabacloud.com/help/en/ack/ack-managed-and-ack-dedicated/user-guide/create-an-nginx-ingress-1#task-1796525).
- Explore observability features for monitoring container and cluster performance, and business operations. Learn more in the [Observability overview](https://www.alibabacloud.com/help/en/ack/ack-managed-and-ack-dedicated/user-guide/observability-overview#task-2042182).
- For cost optimization, consider deleting unused clusters. Refer to [Delete a cluster](https://www.alibabacloud.com/help/en/ack/ack-managed-and-ack-dedicated/user-guide/delete-a-cluster-1#task-2372991).