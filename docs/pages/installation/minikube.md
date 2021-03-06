---
layout: default
title: Minikube
permalink: installation/platforms/minikube
type: installation
display-title: "false"
language: en
list: include
image: /assets/img/platforms/minikube.png
---

{% include installation_prerequisites.html %}

## Setup and run Meshery on Minikube

1. [Start Minikube](#1-start-minikube)
1. [Configure Meshery to use minkube](#2-configure-meshery-to-use-minikube)
1. [Run Meshery](#3-set-up-meshery)

#### Compatibility
The following minimum component versions are required:

<table id="compatibility-table">
  <tr>
    <th id="model">Name</th>
    <th id="model">Version</th> 
  </tr>
  <tr>
    <td><a href="https://kubernetes.io/docs/tasks/tools/install-minikube/">Minikube</a></td>
    <td>1.0.0 </td>
  </tr>
  <tr>
    <td><a href="https://istio.io/docs/setup/kubernetes/prepare/platform-setup/minikube/">Kubernetes</a></td>
    <td>1.14.1</td>
  </tr>
  <tr>
    <td><a href="https://kubernetes.io/docs/tasks/tools/install-kubectl/">kubectl</a></td>
    <td>1.14.1</td>
  </tr>
</table>

### Steps
Perform the following steps in order:

#### 1. Start minikube

 <pre class="codeblock-pre"><div class="codeblock">
 <div class="clipboardjs">
 minikube start --cpus 4 --memory 8192 --kubernetes-version=v1.14.1
 </div></div>
 </pre>

*Note: minimum memory required is --memory=4096 (for Istio deployments only)*

*Note: If you are using docker driver, after completing meshery installation steps execute below command to establish connectivity between Meshery and Kubernetes server.*

 <pre class="codeblock-pre"><div class="codeblock">
 <div class="clipboardjs">
 docker network connect bridge meshery_meshery_1
 </div></div>
 </pre>

#### 2. Configure Meshery to use minikube

1. Login to Meshery. Under your user profile, click *Get Token*.
2. Use [mesheryctl](/docs/installation#using-mesheryctl) to configure Meshery to use minikube. Execute:

 <pre class="codeblock-pre"><div class="codeblock">
 <div class="clipboardjs">
 mesheryctl system config minikube -t ~/Downloads/auth.json
 </div></div>
 </pre>

You may also manually generate and load kubeconfig file for Meshery to use:

This configuration file will be used by Meshery.

 <pre class="codeblock-pre"><div class="codeblock">
 <div class="clipboardjs">
 kubectl config view --minify --flatten > config_minikube.yaml
 </div></div>
 </pre>

 <pre class="codeblock-pre">
 <div class="codeblock"><div class="clipboardjs">
 apiVersion: v1
 clusters:
 - cluster:
     certificate-authority-data: < cert shortcutted >
     server: https://192.168.99.100:8443
   name: minikube
 contexts:
 - context:
     cluster: minikube
     user: minikube
   name: minikube
 current-context: minikube
 kind: Config
 preferences: {}
 users:
 - name: minikube
   user:
     client-certificate-data: < cert shortcutted >
     client-key-data: < key shortcutted >
 </div></div>
 </pre>

Note: Make sure *current-context* is set to *minikube*.

### 3. Set up Meshery

Follow the [installation steps](/docs/installation) to install the mesheryctl CLI. 

Meshery should now be connected with your managed Kubernetes instance. Take a look at the [Meshery guides](/docs/guides) for advanced usage tips.

