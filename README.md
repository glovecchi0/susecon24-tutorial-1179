# [TUTORIAL-1179] Automate NeuVector Federation Across Diverse Cloud Providers - Laboratory steps

[SUSECON24 Session](https://youtu.be/i0fqobee-tw)

### CHALLENGE

It’s widespread for an organization to have more than one Kubernetes cluster, which may be located on different cloud providers (or on-premises data centres).

In this case, it’s essential to ensure that you protect your clusters with a container security platform that can manage policies centrally, regardless of the specifics of the underlying infrastructure.

### SOLUTION

The solution consists of:

1. Ensure that Kubernetes clusters have SUSE NeuVector on board from day 1.
To do this, we must insert NeuVector in the same deployment phase as the K8s cluster.

2. Configure the NeuVector Federation feature.

# PURPOSE OF THIS REPOSITORY

The goal of this document is to describe the steps to:

1. Create a pair of Kubernetes clusters on different Cloud Providers (Google Cloud and AWS), secure from day-1 with [clustered NeuVector](https://open-docs.neuvector.com/navigation/multicluster).

2. Simulate some use cases of [Federated Policies](https://open-docs.neuvector.com/policy/federated) to create Federated rules to be pushed to each cluster.

### TERRAFORM FILES

#### Terraform Modules

[GitHub repository](https://github.com/glovecchi0/neuvector-tf/tree/main/tf-modules)

References:
- https://developer.hashicorp.com/terraform/language/modules
- https://developer.hashicorp.com/terraform/language/modules/develop/structure

#### [TUTORIAL-1179] Project

[GitHub repository](https://github.com/glovecchi0/neuvector-tf-federation)

<img src="images/0-repo%20url.png" width="150"/>

##### SCENARIO

| NV Role | Managed Kubernetes Cluster |
| ------- | -------------------------- |
| Primary/Controller NeuVector cluster | [GKE](https://github.com/glovecchi0/neuvector-tf/tree/main/tf-modules/google-cloud/gke) |
| Remote/Worker NeuVector cluster | [EKS](https://github.com/glovecchi0/neuvector-tf/tree/main/tf-modules/aws/eks) |

# DEMONSTRATION

#### Repository structure and example of terraform.tfvars file containing the minimum variables to proceed with deployment

![Post pull actions](images/1-post-pull%20actions.png)
-
![Terraform.tfvars example](images/2-terraformtfvars%20example.png)

#### An example of what happens when Terraform files are applied (initial and final piece) and what outputs are generated

![Copy & Paste "One-click creation" script](images/3-copy%26paste%20one-click%20creation.png)
-
!["One-click creation" script completed](images/4-creation%20completed.png)
-
![Terraform outputs](images/5-terraform%20output.png)

#### NeuVector Controller on GKE

![NV Controller on GKE](images/6-nv%20controller%20on%20gke.png)

#### NeuVector Controller on EKS

![NV Controller on EKS](images/7-nv%20controller%20on%20eks.png)

#### Management of the remote NeuVector Cluster from the Controller

![Management of various NV Clusters from the primary Cluster](images/8-how%20to%20manage%20the%20remote%20nv%20from%20the%20controller.png)
-
![Demonstration of shifting from the same console](images/9-remote%20nv%20nodes%20from%20the%20controller%20console.png)

#### To prepare for the first use case, enable Admission Control on both Clusters and create a federated Registry that scans the `elastic/logstash*` images from Docker Hub

![Enable Admission Control on the Controller Cluster](images/10-lab1-enabling%20admission%20control%20on%20the%20gke%20cluster.png)
-
![Enable Admission Control on the Worker Cluster](images/11-lab1-enabling%20admission%20control%20on%20the%20eks%20cluster.png)
-
![Federated registry for Docker Hub `elastic/logstash*` images](images/12-lab1-example1-scan%20logstash%20images%20on%20the%20gke%20cluster.png)
-
![End of scanning for `elastic/logstash*` images](images/13-lab1-example1-end%20of%20the%20scan.png)

#### USE CASE 1 - Admission Control Federated Policy - Deny the creation of pods using images affected by the Log4j vulnerability CVE-2021-44228

![Federated rule](images/14-lab1-example1-admission%20control%20x%20cve%20names%20criteria.png)
-
![Checking the rule via the CLI](images/15-lab1-example1-verification%20of%20the%20rule-CLI.png)
-
![Checking the rule via the UI](images/16-lab1-example1-verification%20of%20the%20rule-UI.png)

#### USE CASE 2 - Admission Control Federated Policy - Deny creation of pods in the demo2 namespace

![Federated rule](images/17-lab1-example2-admission%20control%20x%20namespace.png)
-
![Checking the rule via the CLI](images/18-lab1-example2-verification%20of%20the%20rule-CLI.png)
-
![Checking the rule via the UI](images/19-lab1-example2-verification%20of%20the%20rule-UI.png)

#### To prepare for the third (and final) use case, create a pod on the Worker Cluster (EKS) where the CURL package is installed and make HTTP/HTTPS calls to any URL

![Deploy the CURL Pod to the EKS Cluster](images/20-lab2-example1-deploy%20the%20curl%20pod%20to%20the%20eks%20cluster.png)
-
![Checking Network Rules after Pod creation](images/21-lab2-example1-network%20rules%20created%20post%20deploy%20of%20the%20curl%20pod.png)
-
![Checking Network Rules after CURL](images/22-lab2-example1-how%20network%20rules%20are%20created.png)

#### USE CASE 3 - Network Rules Federated Policy - Deny that the curl pod can reach external URLs via the HTTP protocol

![Federated rule](images/23-lab2-example1-network%20rule%20x%20federated%20policy-deny%20http.png)
-
![Application of the federated rule](images/24-lab2-example1-network%20rule%20x%20federated%20policy-deny%20http-pt2.png)
-
![Checking Network Rules on the EKS Cluster](images/25-lab2-example1-network%20rule%20x%20federated%20policy-deny%20http-pt3.png)
-
![Configure Network Security Policy Mode to Protect](images/26-lab2-example1-configure%20Network%20Security%20Policy%20Mode%20to%20Protect.png)
-
![Checking the rule via the UI](images/27-lab2-example1-verification%20of%20the%20rule-CLI%20%26%20UI.png)
