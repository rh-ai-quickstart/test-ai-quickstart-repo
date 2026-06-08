# Test AI Quickstart repo consistency checkers with intentionally inconsistent items and obviously broken stuff.

This is not an AI quickstart. Please do not fork it or clone it or use it. You will be disappointed if you do but also, you've been warned. REMEMBER: This is not an AI Quickstart. 

## Table of Contents

- [Overview](#overview)
- [Detailed description](#detailed-description)
  - [See it in action](#see-it-in-action)
  - [Architecture diagrams](#architecture-diagrams)
- [Requirements](#requirements)
  - [Minimum hardware requirements](#minimum-hardware-requirements)
  - [Minimum software requirements](#minimum-software-requirements)
  - [Required user permissions](#required-user-permissions)
- [Deploy](#deploy)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Validating the deployment](#validating-the-deployment)
  - [Delete](#delete)
- [Repository structure](#repository-structure)
- [References](#references)
- [Technical details](#technical-details)
- [Tags](#tags)

## Overview

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Red Hat AI Quickstart
provides ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in
reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui
officia deserunt mollit anim id est laborum.

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo
inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut
fugit, sed quia consequuntur magni dolores eos qui ratione Red Hat AI Enterprise voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem
ipsum quia dolor sit amet, consectetur, adipisci velit, sed quia non numquam eius modi tempora incidunt ut labore et dolore magnam aliquam quaerat
voluptatem.

At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas
molestias excepturi sint occaecati cupiditate non provident, similique sunt in culpa qui officia deserunt mollitia animi, id est laborum et dolorum
fuga. Red Hat Openshift AI et harum quidem rerum facilis est et expedita distinctio. Nam libero tempore, cum soluta nobis est eligendi optio cumque
nihil impedit quo minus id quod maxime placeat facere possimus, omnis voluptas assumenda est, omnis dolor repellendus.

## Detailed description

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Red Hat AI Quickstart
provides ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in
reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui
officia deserunt mollit anim id est laborum.

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo
inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut
fugit, sed quia consequuntur magni dolores eos qui ratione Red Hat AI Enterprise voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem
ipsum quia dolor sit amet, consectetur, adipisci velit, sed quia non numquam eius modi tempora incidunt ut labore et dolore magnam aliquam quaerat
voluptatem.

At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas
molestias excepturi sint occaecati cupiditate non provident, similique sunt in culpa qui officia deserunt mollitia animi, id est laborum et dolorum
fuga. Red Hat Openshift AI et harum quidem rerum facilis est et expedita distinctio. Nam libero tempore, cum soluta nobis est eligendi optio cumque
nihil impedit quo minus id quod maxime placeat facere possimus, omnis voluptas assumenda est, omnis dolor repellendus.

### Architecture diagrams


## Requirements

### Minimum hardware requirements

* GPU
* a lot of RAM
* some storage
* `oc`
* OpenShift AI or Red Hat AI Enterprise 
### Minimum software requirements

> **CONTRIBUTOR TODO: add minimum software requirements**
>
> This section is REQUIRED. Be SPECIFIC about versions.
>
> **Include:**
> - OpenShift version (e.g., "OpenShift 4.14 or later")
> - OpenShift AI version (e.g., "OpenShift AI 2.22 or later")
> - Other platform dependencies (operators, services)
> - Client tools (oc CLI, helm, etc.)
>
> **BAD:** "Requires OpenShift AI"  
> **GOOD:** "Tested with OpenShift AI 2.22-2.25 on OpenShift 4.14+"

### Required user permissions

* admin permisions requied

## Deploy


### Prerequisites

### Installation

1. Clone the repository:
```bash
git clone https://github.com/rh-ai-quickstart/YOUR_QUICKSTART_NAME.git
cd YOUR_QUICKSTART_NAME
```

2. Create a new OpenShift project:
```bash
PROJECT="my-quickstart"
oc new-project ${PROJECT}
```

3. Install using Helm:

```bash
helm install my-quickstart ./chart --namespace ${PROJECT}
```

#### If your quickstart requires a model

**Option A: Use your own model (MaaS - Model as a Service)**

If you have an existing model endpoint, provide the model name, endpoint, and API key:
```bash
helm install my-quickstart ./chart --namespace ${PROJECT} \
  --set model.name=YOUR_MODEL_NAME \
  --set model.endpoint=YOUR_MODEL_ENDPOINT \
  --set model.api_key=YOUR_API_KEY
```

> **Note**: The `model.endpoint` should be the full URL including protocol and port if needed (e.g., `https://my-model.example.com` or `http://my-model:8080`).

**Option B: Deploy with a model included in the chart**

If you don't provide any model configuration, the chart will deploy a default model on your cluster:
```bash
helm install my-quickstart ./chart --namespace ${PROJECT}
```

> **Note**: Option B requires a GPU available in your cluster for the LLM deployment. See [Minimum hardware requirements](#minimum-hardware-requirements) for details. You must add your own model InferenceService template under `chart/templates/` for this option to work.

#### Testing model access (before deploying)

If you are bringing your own model (Option A), you can verify the endpoint is reachable **before** installing the chart:

```bash
oc run test-model-access --rm -it --restart=Never \
  --image=registry.access.redhat.com/ubi9/ubi-minimal:latest \
  -- /bin/sh -c 'curl -sf --max-time 10 \
    -H "Authorization: Bearer YOUR_API_KEY" \
    -H "Content-Type: application/json" \
    -d "{\"model\": \"YOUR_MODEL_NAME\", \"messages\": [{\"role\": \"user\", \"content\": \"Say hello in one word.\"}], \"max_tokens\": 10}" \
    "YOUR_MODEL_ENDPOINT/v1/chat/completions" && echo "" && echo "SUCCESS" || echo "FAILED"'
```

Replace `YOUR_API_KEY`, `YOUR_MODEL_NAME`, and `YOUR_MODEL_ENDPOINT` with your actual values.

### Validating the deployment

> **CONTRIBUTOR TODO: add validation steps**
>
> Tell users how to verify the deployment was successful.
>
> **Include:**
> - Commands to check pod status
> - How to access the application (routes, URLs)
> - How to verify functionality (e.g., "run helm test")
> - Expected output/behavior
>
> **Example:**
>
> 1. Check all pods are running:
>    ```bash
>    oc get pods -n ${PROJECT}
>    ```
>    
> 2. Get the application URL:
>    ```bash
>    echo https://$(oc get route/my-app -n ${PROJECT} --template='{{.spec.host}}')
>    ```
>    
> 3. Test the endpoint is responding:
>    ```bash
>    curl -s https://$(oc get route/my-app -n ${PROJECT} --template='{{.spec.host}}')/health
>    ```
>
> If your quickstart uses a model, you can run the included Helm test:
> ```bash
> helm test my-quickstart --namespace ${PROJECT}
> ```
>
> If your quickstart does not use a model, remove the helm test step and add your own validation steps.

### Delete



## Repository structure



## References



## Tags

* **Industry:** Cross industry
* **Product:** Red Hat AI Enterprise 
