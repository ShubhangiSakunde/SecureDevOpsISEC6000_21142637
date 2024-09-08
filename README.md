### Project Overview



This project focuses on deploying and securing a modern e-commerce application using Saleor, a Python-based microservices platform. You will use Docker, Kubernetes, and Google Kubernetes Engine (GKE) to build, deploy, and manage the application. The goal is to gain practical experience in application deployment, security, and monitoring.



### Saleor Platform Overview

  

The **Saleor Platform** offers a streamlined approach for local development by providing all essential Saleor services in one environment:

  

-  [Core GraphQL API](https://github.com/saleor/saleor)

-  [Dashboard](https://github.com/saleor/saleor-dashboard)

-  **Mailpit** (for testing email functionality)

-  **Jaeger** (Application Performance Monitoring)

- Supporting databases, cache systems, and other necessary infrastructure components

  

## Prerequisites

  

Before starting, ensure you have the following tools installed:

  

1.  **kubectl**

2.  **docker**

3.  **gcloud**

  

## Repository Cloning

  

To begin, clone the repository using the following command:

  

```bash

git  clone  https://github.com/ShubhangiSakunde/SecureDevOpsISEC6000_21142637.git

```

  

## Running the Platform on a Kubernetes Cluster

  

Follow these steps to deploy the Saleor platform on your Kubernetes cluster:

  

### 1. Configure kubectl

  

Ensure that `kubectl` is correctly configured to interact with your Kubernetes cluster.

  

### 2. Navigate to the Cloned Directory

  

Move into the cloned project directory:

  

```bash

cd  SecureDevOpsISEC6000_21142637

```

  

### 3. Deploy the NGINX Ingress Controller

  

Deploy the **NGINX Ingress Controller**, which is required for handling external traffic to the Saleor services:

  

```bash

kubectl  apply  -f  https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.11.2/deploy/static/provider/cloud/deploy.yaml

```

  

### 4. Deploy Resources from the `k8s` Directory

  

Deploy all the necessary resources for the platform by running:

  

```bash

kubectl  apply  -f  k8s

```

  

### 5. Verify the Status of the Pods

  

Check the status of all deployed pods to ensure they are running properly:

  

```bash

kubectl  get  pods

```

  
![image](https://github.com/user-attachments/assets/670be217-2618-4b98-84cd-570c7ad0ec3a)




  

### 6. Access the API Pod

  

Once the pods are running, you can enter the `api` pod’s shell to manage the Saleor database:

  

```bash

kubectl  exec  -it  api-9bd8fd7c8-5cp5h  --  /bin/sh

```

  

### 7. Run Database Migrations

  

To set up the database schema, run the migrations:

  

```bash

python3  manage.py  migrate

```

  

### 8. Populate the Database and Create a Superuser

  

Create an initial superuser and populate the database with sample data by running:

  

```bash

python3  manage.py  populatedb  --createsuperuser

```

  

### 9. Exit the API Pod Shell

  

After completing the setup, exit the pod’s shell:

  

```bash

exit

```

  

### 10. Deploy the Ingress Resource

  

Deploy the Ingress resource, which handles external access to the Saleor application:

  

```bash

kubectl  apply  -f  k8s/ingress

```

  

### 11. Review the Ingress Configuration

  

Verify the details of the deployed Ingress by running:

  

```bash

kubectl  describe  ingress  saleor-ingress

```

  

![image](https://github.com/user-attachments/assets/2416a8c4-e156-49f5-b631-9f0b9e1c27fc)


  

### 12. Access the Application

  

Copy the address provided in the Ingress configuration and paste it into your browser to access the Saleor application. For example:

  

```bash

http://35.193.42.17

```

  

The Saleor platform will be up and running on your Kubernetes cluster, and you can begin interacting with the services through the provided URL.

## Docker Image Vulnerability Scanning using Trivy
### Installing Trivy
```shell
sudo snap install trivy
```

## For Docker images, we used trivy to scan the images for known vulnerabilities. Command used: 
```shell
trivy image <image name>
```
## Trivy example reports
### 1. shubh26/saleor-dashboard:latest
```
shubh26/saleor-dashboard:latest (alpine 3.20.2)
===============================================
Total: 7 (UNKNOWN: 0, LOW: 0, MEDIUM: 4, HIGH: 0, CRITICAL: 3)

┌────────────┬────────────────┬──────────┬────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────────────┐
│  Library   │ Vulnerability  │ Severity │ Status │ Installed Version │ Fixed Version │                            Title                            │
├────────────┼────────────────┼──────────┼────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ curl       │ CVE-2024-7264  │ MEDIUM   │ fixed  │ 8.9.0-r0          │ 8.9.1-r0      │ curl: libcurl: ASN.1 date parser overread                   │
│            │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2024-7264                   │
├────────────┼────────────────┤          │        ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libcrypto3 │ CVE-2024-6119  │          │        │ 3.3.1-r3          │ 3.3.2-r0      │ openssl: Possible denial of service in X.509 name checks    │
│            │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2024-6119                   │
├────────────┼────────────────┤          │        ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libcurl    │ CVE-2024-7264  │          │        │ 8.9.0-r0          │ 8.9.1-r0      │ curl: libcurl: ASN.1 date parser overread                   │
│            │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2024-7264                   │
├────────────┼────────────────┼──────────┤        ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libexpat   │ CVE-2024-45490 │ CRITICAL │        │ 2.6.2-r0          │ 2.6.3-r0      │ libexpat: Negative Length Parsing Vulnerability in libexpat │
│            │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2024-45490                  │
│            ├────────────────┤          │        │                   │               ├─────────────────────────────────────────────────────────────┤
│            │ CVE-2024-45491 │          │        │                   │               │ libexpat: Integer Overflow or Wraparound                    │
│            │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2024-45491                  │
│            ├────────────────┤          │        │                   │               ├─────────────────────────────────────────────────────────────┤
│            │ CVE-2024-45492 │          │        │                   │               │ libexpat: integer overflow                                  │
│            │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2024-45492                  │
├────────────┼────────────────┼──────────┤        ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libssl3    │ CVE-2024-6119  │ MEDIUM   │        │ 3.3.1-r3          │ 3.3.2-r0      │ openssl: Possible denial of service in X.509 name checks    │
│            │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2024-6119                   │
└────────────┴────────────────┴──────────┴────────┴───────────────────┴───────────────┴─────────────────────────────────────────────────────────────┘
```
### 2. shubh26/saleor-api:latest
```

shubh26/saleor-api:latest (debian 12.7)
=======================================
Total: 172 (UNKNOWN: 0, LOW: 114, MEDIUM: 36, HIGH: 18, CRITICAL: 4)

┌────────────────────┬─────────────────────┬──────────┬──────────────┬─────────────────────────┬───────────────┬──────────────────────────────────────────────────────────────┐
│      Library       │    Vulnerability    │ Severity │    Status    │    Installed Version    │ Fixed Version │                            Title                             │
├────────────────────┼─────────────────────┼──────────┼──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ apt                │ CVE-2011-3374       │ LOW      │ affected     │ 2.6.1                   │               │ It was found that apt-key in apt, all versions, do not       │
│                    │                     │          │              │                         │               │ correctly...                                                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2011-3374                    │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ bash               │ TEMP-0841856-B18BAF │          │              │ 5.2.15-2+b7             │               │ [Privilege escalation possible to other user than root]      │
│                    │                     │          │              │                         │               │ https://security-tracker.debian.org/tracker/TEMP-0841856-B1- │
│                    │                     │          │              │                         │               │ 8BAF                                                         │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ bsdutils           │ CVE-2022-0563       │          │              │ 1:2.38.1-5+deb12u1      │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                         │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┤          ├──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ coreutils          │ CVE-2016-2781       │          │ will_not_fix │ 9.1-1                   │               │ coreutils: Non-privileged session can escape to the parent   │
│                    │                     │          │              │                         │               │ session in chroot                                            │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-2781                    │
│                    ├─────────────────────┤          ├──────────────┤                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-18018      │          │ affected     │                         │               │ coreutils: race condition vulnerability in chown and chgrp   │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-18018                   │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ gcc-12-base        │ CVE-2023-4039       │ MEDIUM   │              │ 12.2.0-14               │               │ gcc: -fstack-protector fails to guard dynamic stack          │
│                    │                     │          │              │                         │               │ allocations on ARM64                                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-4039                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2022-27943      │ LOW      │              │                         │               │ binutils: libiberty/rust-demangle.c in GNU GCC 11.2 allows   │
│                    │                     │          │              │                         │               │ stack exhaustion in demangle_const                           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-27943                   │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ gpgv               │ CVE-2022-3219       │          │              │ 2.2.40-1.1              │               │ gnupg: denial of service issue (resource consumption) using  │
│                    │                     │          │              │                         │               │ compressed packets                                           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-3219                    │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libapt-pkg6.0      │ CVE-2011-3374       │          │              │ 2.6.1                   │               │ It was found that apt-key in apt, all versions, do not       │
│                    │                     │          │              │                         │               │ correctly...                                                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2011-3374                    │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libblkid1          │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u1        │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                         │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libc-bin           │ CVE-2010-4756       │          │              │ 2.36-9+deb12u8          │               │ glibc: glob implementation can cause excessive CPU and       │
│                    │                     │          │              │                         │               │ memory consumption due to...                                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2010-4756                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-20796      │          │              │                         │               │ glibc: uncontrolled recursion in function                    │
│                    │                     │          │              │                         │               │ check_dst_limits_calc_pos_1 in posix/regexec.c               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-20796                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010022    │          │              │                         │               │ glibc: stack guard protection bypass                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-1010022                 │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010023    │          │              │                         │               │ glibc: running ldd on malicious ELF leads to code execution  │
│                    │                     │          │              │                         │               │ because of...                                                │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-1010023                 │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010024    │          │              │                         │               │ glibc: ASLR bypass using cache of thread stack and heap      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-1010024                 │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010025    │          │              │                         │               │ glibc: information disclosure of heap addresses of           │
│                    │                     │          │              │                         │               │ pthread_created thread                                       │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-1010025                 │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-9192       │          │              │                         │               │ glibc: uncontrolled recursion in function                    │
│                    │                     │          │              │                         │               │ check_dst_limits_calc_pos_1 in posix/regexec.c               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-9192                    │
├────────────────────┼─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│ libc6              │ CVE-2010-4756       │          │              │                         │               │ glibc: glob implementation can cause excessive CPU and       │
│                    │                     │          │              │                         │               │ memory consumption due to...                                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2010-4756                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-20796      │          │              │                         │               │ glibc: uncontrolled recursion in function                    │
│                    │                     │          │              │                         │               │ check_dst_limits_calc_pos_1 in posix/regexec.c               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-20796                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010022    │          │              │                         │               │ glibc: stack guard protection bypass                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-1010022                 │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010023    │          │              │                         │               │ glibc: running ldd on malicious ELF leads to code execution  │
│                    │                     │          │              │                         │               │ because of...                                                │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-1010023                 │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010024    │          │              │                         │               │ glibc: ASLR bypass using cache of thread stack and heap      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-1010024                 │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010025    │          │              │                         │               │ glibc: information disclosure of heap addresses of           │
│                    │                     │          │              │                         │               │ pthread_created thread                                       │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-1010025                 │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-9192       │          │              │                         │               │ glibc: uncontrolled recursion in function                    │
│                    │                     │          │              │                         │               │ check_dst_limits_calc_pos_1 in posix/regexec.c               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-9192                    │
├────────────────────┼─────────────────────┤          ├──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libcairo2          │ CVE-2017-7475       │          │ will_not_fix │ 1.16.0-7                │               │ cairo: NULL pointer dereference with a crafted font file     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-7475                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-18064      │          │              │                         │               │ cairo: Stack-based buffer overflow via parsing of crafted    │
│                    │                     │          │              │                         │               │ WebKitGTK+ document                                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-18064                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-6461       │          │              │                         │               │ cairo: assertion problem in _cairo_arc_in_direction in       │
│                    │                     │          │              │                         │               │ cairo-arc.c                                                  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-6461                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-6462       │          │              │                         │               │ cairo: infinite loop in the function _arc_error_normalized   │
│                    │                     │          │              │                         │               │ in the file cairo-arc.c                                      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-6462                    │
├────────────────────┼─────────────────────┼──────────┼──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libexpat1          │ CVE-2024-45490      │ CRITICAL │ affected     │ 2.5.0-1                 │               │ libexpat: Negative Length Parsing Vulnerability in libexpat  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-45490                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-45491      │          │              │                         │               │ libexpat: Integer Overflow or Wraparound                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-45491                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-45492      │          │              │                         │               │ libexpat: integer overflow                                   │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-45492                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-52425      │ HIGH     │              │                         │               │ expat: parsing large tokens can trigger a denial of service  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-52425                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-52426      │ LOW      │              │                         │               │ expat: recursive XML entity expansion vulnerability          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-52426                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-28757      │          │              │                         │               │ expat: XML Entity Expansion                                  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-28757                   │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libgcc-s1          │ CVE-2023-4039       │ MEDIUM   │              │ 12.2.0-14               │               │ gcc: -fstack-protector fails to guard dynamic stack          │
│                    │                     │          │              │                         │               │ allocations on ARM64                                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-4039                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2022-27943      │ LOW      │              │                         │               │ binutils: libiberty/rust-demangle.c in GNU GCC 11.2 allows   │
│                    │                     │          │              │                         │               │ stack exhaustion in demangle_const                           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-27943                   │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libgcrypt20        │ CVE-2024-2236       │ MEDIUM   │              │ 1.10.1-3                │               │ libgcrypt: vulnerable to Marvin Attack                       │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-2236                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-6829       │ LOW      │              │                         │               │ libgcrypt: ElGamal implementation doesn't have semantic      │
│                    │                     │          │              │                         │               │ security due to incorrectly encoded plaintexts...            │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-6829                    │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libglib2.0-0       │ CVE-2012-0039       │          │              │ 2.74.6-2+deb12u3        │               │ glib2: hash table collisions CPU usage DoS                   │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2012-0039                    │
├────────────────────┤                     │          │              │                         ├───────────────┤                                                              │
│ libglib2.0-data    │                     │          │              │                         │               │                                                              │
│                    │                     │          │              │                         │               │                                                              │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libgnutls30        │ CVE-2011-3389       │          │              │ 3.7.9-2+deb12u3         │               │ HTTPS: block-wise chosen-plaintext attack against SSL/TLS    │
│                    │                     │          │              │                         │               │ (BEAST)                                                      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2011-3389                    │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libgssapi-krb5-2   │ CVE-2024-26462      │ HIGH     │              │ 1.20.1-2+deb12u2        │               │ krb5: Memory leak at /krb5/src/kdc/ndr.c                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26462                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-26458      │ MEDIUM   │              │                         │               │ krb5: Memory leak at /krb5/src/lib/rpc/pmap_rmt.c            │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26458                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-26461      │          │              │                         │               │ krb5: Memory leak at /krb5/src/lib/gssapi/krb5/k5sealv3.c    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26461                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-5709       │ LOW      │              │                         │               │ krb5: integer overflow in dbentry->n_key_data in             │
│                    │                     │          │              │                         │               │ kadmin/dbutil/dump.c                                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-5709                    │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libharfbuzz0b      │ CVE-2023-25193      │ HIGH     │              │ 6.0.0+dfsg-3            │               │ harfbuzz: allows attackers to trigger O(n^2) growth via      │
│                    │                     │          │              │                         │               │ consecutive marks                                            │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-25193                   │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libjbig0           │ CVE-2017-9937       │ LOW      │              │ 2.1-6.1                 │               │ libtiff: memory malloc failure in tif_jbig.c could cause     │
│                    │                     │          │              │                         │               │ DOS.                                                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-9937                    │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libk5crypto3       │ CVE-2024-26462      │ HIGH     │              │ 1.20.1-2+deb12u2        │               │ krb5: Memory leak at /krb5/src/kdc/ndr.c                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26462                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-26458      │ MEDIUM   │              │                         │               │ krb5: Memory leak at /krb5/src/lib/rpc/pmap_rmt.c            │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26458                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-26461      │          │              │                         │               │ krb5: Memory leak at /krb5/src/lib/gssapi/krb5/k5sealv3.c    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26461                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-5709       │ LOW      │              │                         │               │ krb5: integer overflow in dbentry->n_key_data in             │
│                    │                     │          │              │                         │               │ kadmin/dbutil/dump.c                                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-5709                    │
├────────────────────┼─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│ libkrb5-3          │ CVE-2024-26462      │ HIGH     │              │                         │               │ krb5: Memory leak at /krb5/src/kdc/ndr.c                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26462                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-26458      │ MEDIUM   │              │                         │               │ krb5: Memory leak at /krb5/src/lib/rpc/pmap_rmt.c            │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26458                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-26461      │          │              │                         │               │ krb5: Memory leak at /krb5/src/lib/gssapi/krb5/k5sealv3.c    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26461                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-5709       │ LOW      │              │                         │               │ krb5: integer overflow in dbentry->n_key_data in             │
│                    │                     │          │              │                         │               │ kadmin/dbutil/dump.c                                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-5709                    │
├────────────────────┼─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│ libkrb5support0    │ CVE-2024-26462      │ HIGH     │              │                         │               │ krb5: Memory leak at /krb5/src/kdc/ndr.c                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26462                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-26458      │ MEDIUM   │              │                         │               │ krb5: Memory leak at /krb5/src/lib/rpc/pmap_rmt.c            │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26458                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-26461      │          │              │                         │               │ krb5: Memory leak at /krb5/src/lib/gssapi/krb5/k5sealv3.c    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-26461                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-5709       │ LOW      │              │                         │               │ krb5: integer overflow in dbentry->n_key_data in             │
│                    │                     │          │              │                         │               │ kadmin/dbutil/dump.c                                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-5709                    │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libldap-2.5-0      │ CVE-2023-2953       │ HIGH     │              │ 2.5.13+dfsg-5           │               │ openldap: null pointer dereference in ber_memalloc_x         │
│                    │                     │          │              │                         │               │ function                                                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-2953                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2015-3276       │ LOW      │              │                         │               │ openldap: incorrect multi-keyword mode cipherstring parsing  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2015-3276                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-14159      │          │              │                         │               │ openldap: Privilege escalation via PID file manipulation     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-14159                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-17740      │          │              │                         │               │ openldap: contrib/slapd-modules/nops/nops.c attempts to free │
│                    │                     │          │              │                         │               │ stack buffer allowing remote attackers to cause...           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-17740                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2020-15719      │          │              │                         │               │ openldap: Certificate validation incorrectly matches name    │
│                    │                     │          │              │                         │               │ against CN-ID                                                │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2020-15719                   │
├────────────────────┼─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│ libldap-common     │ CVE-2023-2953       │ HIGH     │              │                         │               │ openldap: null pointer dereference in ber_memalloc_x         │
│                    │                     │          │              │                         │               │ function                                                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-2953                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2015-3276       │ LOW      │              │                         │               │ openldap: incorrect multi-keyword mode cipherstring parsing  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2015-3276                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-14159      │          │              │                         │               │ openldap: Privilege escalation via PID file manipulation     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-14159                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-17740      │          │              │                         │               │ openldap: contrib/slapd-modules/nops/nops.c attempts to free │
│                    │                     │          │              │                         │               │ stack buffer allowing remote attackers to cause...           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-17740                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2020-15719      │          │              │                         │               │ openldap: Certificate validation incorrectly matches name    │
│                    │                     │          │              │                         │               │ against CN-ID                                                │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2020-15719                   │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libmount1          │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u1        │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                         │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libncursesw6       │ CVE-2023-50495      │ MEDIUM   │              │ 6.4-4                   │               │ ncurses: segmentation fault via _nc_wrap_entry()             │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-50495                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-45918      │ LOW      │              │                         │               │ ncurses: NULL pointer dereference in tgetstr in              │
│                    │                     │          │              │                         │               │ tinfo/lib_termcap.c                                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-45918                   │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libopenjp2-7       │ CVE-2021-3575       │ HIGH     │              │ 2.5.0-2                 │               │ openjpeg: heap-buffer-overflow in color.c may lead to DoS or │
│                    │                     │          │              │                         │               │ arbitrary code execution...                                  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2021-3575                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-39327      │ MEDIUM   │              │                         │               │ openjpeg: Malicious files can cause the program to enter a   │
│                    │                     │          │              │                         │               │ large loop...                                                │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-39327                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-39328      │          │              │                         │               │ openjpeg: denail of service via crafted image file           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-39328                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-39329      │          │              │                         │               │ openjpeg: Resource exhaustion will occur in the              │
│                    │                     │          │              │                         │               │ opj_t1_decode_cblks function in the tcd.c...                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-39329                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2016-10505      │ LOW      │              │                         │               │ openjpeg: NULL pointer dereference in imagetopnm function in │
│                    │                     │          │              │                         │               │ convert.c                                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-10505                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2016-10506      │          │              │                         │               │ openjpeg: Division by zero in functions opj_pi_next_cprl,    │
│                    │                     │          │              │                         │               │ opj_pi_next_pcrl, and opj_pi_next_rpcl in pi.c...            │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-10506                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2016-9113       │          │              │                         │               │ openjpeg2: Multiple security issues                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-9113                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2016-9114       │          │              │                         │               │ openjpeg2: Multiple security issues                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-9114                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2016-9115       │          │              │                         │               │ openjpeg2: Multiple security issues                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-9115                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2016-9116       │          │              │                         │               │ openjpeg2: Multiple security issues                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-9116                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2016-9117       │          │              │                         │               │ openjpeg2: Multiple security issues                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-9117                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2016-9580       │          │              │                         │               │ openjpeg2: Integer overflow in tiftoimage causes heap buffer │
│                    │                     │          │              │                         │               │ overflow                                                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-9580                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2016-9581       │          │              │                         │               │ openjpeg2: Infinite loop in tiftoimage resulting into heap   │
│                    │                     │          │              │                         │               │ buffer overflow in convert_32s_C1P1...                       │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2016-9581                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-17479      │          │              │                         │               │ openjpeg: Stack-buffer overflow in the pgxtoimage function   │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-17479                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-16375      │          │              │                         │               │ openjpeg: Heap-based buffer overflow in pnmtoimage function  │
│                    │                     │          │              │                         │               │ in bin/jpwl/convert.c                                        │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-16375                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-16376      │          │              │                         │               │ openjpeg: Heap-based buffer overflow in function             │
│                    │                     │          │              │                         │               │ t2_encode_packet in src/lib/openmj2/t2.c                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-16376                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-20846      │          │              │                         │               │ openjpeg: out-of-bounds read in functions pi_next_lrcp,      │
│                    │                     │          │              │                         │               │ pi_next_rlcp, pi_next_rpcl, pi_next_pcrl, pi_next_rpcl, and  │
│                    │                     │          │              │                         │               │ pi_next_cprl...                                              │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-20846                   │
│                    ├─────────────────────┤          ├──────────────┤                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-6988       │          │ will_not_fix │                         │               │ openjpeg: DoS via memory exhaustion in opj_decompress        │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-6988                    │
├────────────────────┼─────────────────────┼──────────┼──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libpam-modules     │ CVE-2024-22365      │ MEDIUM   │ affected     │ 1.5.2-6+deb12u1         │               │ pam: allowing unprivileged user to block another user        │
│                    │                     │          │              │                         │               │ namespace                                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-22365                   │
├────────────────────┤                     │          │              │                         ├───────────────┤                                                              │
│ libpam-modules-bin │                     │          │              │                         │               │                                                              │
│                    │                     │          │              │                         │               │                                                              │
│                    │                     │          │              │                         │               │                                                              │
├────────────────────┤                     │          │              │                         ├───────────────┤                                                              │
│ libpam-runtime     │                     │          │              │                         │               │                                                              │
│                    │                     │          │              │                         │               │                                                              │
│                    │                     │          │              │                         │               │                                                              │
├────────────────────┤                     │          │              │                         ├───────────────┤                                                              │
│ libpam0g           │                     │          │              │                         │               │                                                              │
│                    │                     │          │              │                         │               │                                                              │
│                    │                     │          │              │                         │               │                                                              │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libperl5.36        │ CVE-2023-31484      │ HIGH     │              │ 5.36.0-7+deb12u1        │               │ perl: CPAN.pm does not verify TLS certificates when          │
│                    │                     │          │              │                         │               │ downloading distributions over HTTPS...                      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31484                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2011-4116       │ LOW      │              │                         │               │ perl: File:: Temp insecure temporary file handling           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2011-4116                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31486      │          │              │                         │               │ http-tiny: insecure TLS cert default                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31486                   │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libpixman-1-0      │ CVE-2023-37769      │          │              │ 0.42.2-1                │               │ stress-test master commit e4c878 was discovered to contain a │
│                    │                     │          │              │                         │               │ FPE vulne ......                                             │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-37769                   │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libpng16-16        │ CVE-2021-4214       │          │              │ 1.6.39-2                │               │ libpng: hardcoded value leads to heap-overflow               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2021-4214                    │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libpq5             │ CVE-2024-4317       │          │              │ 15.8-0+deb12u1          │               │ postgresql: PostgreSQL pg_stats_ext and pg_stats_ext_exprs   │
│                    │                     │          │              │                         │               │ lack authorization checks                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-4317                    │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libsmartcols1      │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u1        │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                         │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libsqlite3-0       │ CVE-2023-7104       │ HIGH     │              │ 3.40.1-2                │               │ sqlite: heap-buffer-overflow at sessionfuzz                  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-7104                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-0232       │ MEDIUM   │              │                         │               │ sqlite: use-after-free bug in jsonParseAddNodeArray          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-0232                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2021-45346      │ LOW      │              │                         │               │ sqlite: crafted SQL query allows a malicious user to obtain  │
│                    │                     │          │              │                         │               │ sensitive information...                                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2021-45346                   │
├────────────────────┼─────────────────────┼──────────┼──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libssl3            │ CVE-2024-5535       │ MEDIUM   │ fix_deferred │ 3.0.14-1~deb12u2        │               │ openssl: SSL_select_next_proto buffer overread               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-5535                    │
├────────────────────┼─────────────────────┤          ├──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libstdc++6         │ CVE-2023-4039       │          │ affected     │ 12.2.0-14               │               │ gcc: -fstack-protector fails to guard dynamic stack          │
│                    │                     │          │              │                         │               │ allocations on ARM64                                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-4039                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2022-27943      │ LOW      │              │                         │               │ binutils: libiberty/rust-demangle.c in GNU GCC 11.2 allows   │
│                    │                     │          │              │                         │               │ stack exhaustion in demangle_const                           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-27943                   │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libsystemd0        │ CVE-2013-4392       │          │              │ 252.30-1~deb12u2        │               │ systemd: TOCTOU race condition when updating file            │
│                    │                     │          │              │                         │               │ permissions and SELinux security contexts...                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2013-4392                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31437      │          │              │                         │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                         │               │ modify a...                                                  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31437                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31438      │          │              │                         │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                         │               │ truncate a...                                                │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31438                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31439      │          │              │                         │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                         │               │ modify the...                                                │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31439                   │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libtiff6           │ CVE-2023-52355      │ HIGH     │              │ 4.5.0-6+deb12u1         │               │ libtiff: TIFFRasterScanlineSize64 produce too-big size and   │
│                    │                     │          │              │                         │               │ could cause OOM                                              │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-52355                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-52356      │          │              │                         │               │ libtiff: Segment fault in libtiff in TIFFReadRGBATileExt()   │
│                    │                     │          │              │                         │               │ leading to denial of...                                      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-52356                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-7006       │          │              │                         │               │ libtiff: NULL pointer dereference in tif_dirinfo.c           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-7006                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-25433      │ MEDIUM   │              │                         │               │ libtiff: Buffer Overflow via /libtiff/tools/tiffcrop.c       │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-25433                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-26965      │          │              │                         │               │ libtiff: heap-based use after free via a crafted TIFF image  │
│                    │                     │          │              │                         │               │ in loadImage()...                                            │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-26965                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-26966      │          │              │                         │               │ libtiff: Buffer Overflow in uv_encode()                      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-26966                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-2908       │          │              │                         │               │ libtiff: null pointer dereference in tif_dir.c               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-2908                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-3618       │          │              │                         │               │ libtiff: segmentation fault in Fax3Encode in                 │
│                    │                     │          │              │                         │               │ libtiff/tif_fax3.c                                           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-3618                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-6277       │          │              │                         │               │ libtiff: Out-of-memory in TIFFOpen via a craft file          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-6277                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-16232      │ LOW      │              │                         │               │ libtiff: Memory leaks in tif_open.c, tif_lzw.c, and          │
│                    │                     │          │              │                         │               │ tif_aux.c                                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-16232                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-17973      │          │              │                         │               │ libtiff: heap-based use after free in                        │
│                    │                     │          │              │                         │               │ tiff2pdf.c:t2p_writeproc                                     │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-17973                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-5563       │          │              │                         │               │ libtiff: Heap-buffer overflow in LZWEncode tif_lzw.c         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-5563                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-9117       │          │              │                         │               │ libtiff: Heap-based buffer over-read in bmp2tiff             │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-9117                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-10126      │          │              │                         │               │ libtiff: NULL pointer dereference in the jpeg_fdct_16x16     │
│                    │                     │          │              │                         │               │ function in jfdctint.c                                       │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2018-10126                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2022-1210       │          │              │                         │               │ tiff: Malicious file leads to a denial of service in TIFF    │
│                    │                     │          │              │                         │               │ File...                                                      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-1210                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-1916       │          │              │                         │               │ libtiff: out-of-bounds read in extractImageSection() in      │
│                    │                     │          │              │                         │               │ tools/tiffcrop.c                                             │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-1916                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-3164       │          │              │                         │               │ libtiff: heap-buffer-overflow in extractImageSection()       │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-3164                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-6228       │          │              │                         │               │ libtiff: heap-based buffer overflow in cpStripToTile() in    │
│                    │                     │          │              │                         │               │ tools/tiffcp.c                                               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-6228                    │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libtinfo6          │ CVE-2023-50495      │ MEDIUM   │              │ 6.4-4                   │               │ ncurses: segmentation fault via _nc_wrap_entry()             │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-50495                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-45918      │ LOW      │              │                         │               │ ncurses: NULL pointer dereference in tgetstr in              │
│                    │                     │          │              │                         │               │ tinfo/lib_termcap.c                                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-45918                   │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libudev1           │ CVE-2013-4392       │          │              │ 252.30-1~deb12u2        │               │ systemd: TOCTOU race condition when updating file            │
│                    │                     │          │              │                         │               │ permissions and SELinux security contexts...                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2013-4392                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31437      │          │              │                         │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                         │               │ modify a...                                                  │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31437                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31438      │          │              │                         │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                         │               │ truncate a...                                                │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31438                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31439      │          │              │                         │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                         │               │ modify the...                                                │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31439                   │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libuuid1           │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u1        │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                         │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libxml2            │ CVE-2024-25062      │ HIGH     │              │ 2.9.14+dfsg-1.3~deb12u1 │               │ libxml2: use-after-free in XMLReader                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-25062                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-39615      │ MEDIUM   │              │                         │               │ libxml2: crafted xml can cause global buffer overflow        │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-39615                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-45322      │          │              │                         │               │ libxml2: use-after-free in xmlUnlinkNode() in tree.c         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-45322                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-34459      │ LOW      │              │                         │               │ libxml2: buffer over-read in xmlHTMLPrintFileContext in      │
│                    │                     │          │              │                         │               │ xmllint.c                                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-34459                   │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ login              │ CVE-2023-4641       │ MEDIUM   │              │ 1:4.13+dfsg1-1+b1       │               │ shadow-utils: possible password leak during passwd(1) change │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-4641                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2007-5686       │ LOW      │              │                         │               │ initscripts in rPath Linux 1 sets insecure permissions for   │
│                    │                     │          │              │                         │               │ the /var/lo ......                                           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2007-5686                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-19882      │          │              │                         │               │ shadow-utils: local users can obtain root access because     │
│                    │                     │          │              │                         │               │ setuid programs are misconfigured...                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-19882                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-29383      │          │              │                         │               │ shadow: Improper input validation in shadow-utils package    │
│                    │                     │          │              │                         │               │ utility chfn                                                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-29383                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ TEMP-0628843-DBAD28 │          │              │                         │               │ [more related to CVE-2005-4890]                              │
│                    │                     │          │              │                         │               │ https://security-tracker.debian.org/tracker/TEMP-0628843-DB- │
│                    │                     │          │              │                         │               │ AD28                                                         │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ mount              │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u1        │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                         │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ ncurses-base       │ CVE-2023-50495      │ MEDIUM   │              │ 6.4-4                   │               │ ncurses: segmentation fault via _nc_wrap_entry()             │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-50495                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-45918      │ LOW      │              │                         │               │ ncurses: NULL pointer dereference in tgetstr in              │
│                    │                     │          │              │                         │               │ tinfo/lib_termcap.c                                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-45918                   │
├────────────────────┼─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│ ncurses-bin        │ CVE-2023-50495      │ MEDIUM   │              │                         │               │ ncurses: segmentation fault via _nc_wrap_entry()             │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-50495                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-45918      │ LOW      │              │                         │               │ ncurses: NULL pointer dereference in tgetstr in              │
│                    │                     │          │              │                         │               │ tinfo/lib_termcap.c                                          │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-45918                   │
├────────────────────┼─────────────────────┼──────────┼──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ openssl            │ CVE-2024-5535       │ MEDIUM   │ fix_deferred │ 3.0.14-1~deb12u2        │               │ openssl: SSL_select_next_proto buffer overread               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2024-5535                    │
├────────────────────┼─────────────────────┤          ├──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ passwd             │ CVE-2023-4641       │          │ affected     │ 1:4.13+dfsg1-1+b1       │               │ shadow-utils: possible password leak during passwd(1) change │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-4641                    │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2007-5686       │ LOW      │              │                         │               │ initscripts in rPath Linux 1 sets insecure permissions for   │
│                    │                     │          │              │                         │               │ the /var/lo ......                                           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2007-5686                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-19882      │          │              │                         │               │ shadow-utils: local users can obtain root access because     │
│                    │                     │          │              │                         │               │ setuid programs are misconfigured...                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2019-19882                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-29383      │          │              │                         │               │ shadow: Improper input validation in shadow-utils package    │
│                    │                     │          │              │                         │               │ utility chfn                                                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-29383                   │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ TEMP-0628843-DBAD28 │          │              │                         │               │ [more related to CVE-2005-4890]                              │
│                    │                     │          │              │                         │               │ https://security-tracker.debian.org/tracker/TEMP-0628843-DB- │
│                    │                     │          │              │                         │               │ AD28                                                         │
├────────────────────┼─────────────────────┼──────────┤              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ perl               │ CVE-2023-31484      │ HIGH     │              │ 5.36.0-7+deb12u1        │               │ perl: CPAN.pm does not verify TLS certificates when          │
│                    │                     │          │              │                         │               │ downloading distributions over HTTPS...                      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31484                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2011-4116       │ LOW      │              │                         │               │ perl: File:: Temp insecure temporary file handling           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2011-4116                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31486      │          │              │                         │               │ http-tiny: insecure TLS cert default                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31486                   │
├────────────────────┼─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│ perl-base          │ CVE-2023-31484      │ HIGH     │              │                         │               │ perl: CPAN.pm does not verify TLS certificates when          │
│                    │                     │          │              │                         │               │ downloading distributions over HTTPS...                      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31484                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2011-4116       │ LOW      │              │                         │               │ perl: File:: Temp insecure temporary file handling           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2011-4116                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31486      │          │              │                         │               │ http-tiny: insecure TLS cert default                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31486                   │
├────────────────────┼─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│ perl-modules-5.36  │ CVE-2023-31484      │ HIGH     │              │                         │               │ perl: CPAN.pm does not verify TLS certificates when          │
│                    │                     │          │              │                         │               │ downloading distributions over HTTPS...                      │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31484                   │
│                    ├─────────────────────┼──────────┤              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2011-4116       │ LOW      │              │                         │               │ perl: File:: Temp insecure temporary file handling           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2011-4116                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31486      │          │              │                         │               │ http-tiny: insecure TLS cert default                         │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-31486                   │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ sysvinit-utils     │ TEMP-0517018-A83CE6 │          │              │ 3.06-4                  │               │ [sysvinit: no-root option in expert installer exposes        │
│                    │                     │          │              │                         │               │ locally exploitable security flaw]                           │
│                    │                     │          │              │                         │               │ https://security-tracker.debian.org/tracker/TEMP-0517018-A8- │
│                    │                     │          │              │                         │               │ 3CE6                                                         │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ tar                │ CVE-2005-2541       │          │              │ 1.34+dfsg-1.2+deb12u1   │               │ tar: does not properly warn the user when extracting setuid  │
│                    │                     │          │              │                         │               │ or setgid...                                                 │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2005-2541                    │
│                    ├─────────────────────┤          │              │                         ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ TEMP-0290435-0B57B5 │          │              │                         │               │ [tar's rmt command may have undesired side effects]          │
│                    │                     │          │              │                         │               │ https://security-tracker.debian.org/tracker/TEMP-0290435-0B- │
│                    │                     │          │              │                         │               │ 57B5                                                         │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ util-linux         │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u1        │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                         │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┤                     │          │              │                         ├───────────────┤                                                              │
│ util-linux-extra   │                     │          │              │                         │               │                                                              │
│                    │                     │          │              │                         │               │                                                              │
│                    │                     │          │              │                         │               │                                                              │
├────────────────────┼─────────────────────┤          │              ├─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ xdg-user-dirs      │ CVE-2017-15131      │          │              │ 0.18-1                  │               │ gnome-session: Xsession creation of XDG user directories     │
│                    │                     │          │              │                         │               │ does not honor system umask...                               │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2017-15131                   │
├────────────────────┼─────────────────────┼──────────┼──────────────┼─────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ zlib1g             │ CVE-2023-45853      │ CRITICAL │ will_not_fix │ 1:1.2.13.dfsg-1         │               │ zlib: integer overflow and resultant heap-based buffer       │
│                    │                     │          │              │                         │               │ overflow in zipOpenNewFileInZip4_6                           │
│                    │                     │          │              │                         │               │ https://avd.aquasec.com/nvd/cve-2023-45853                   │
└────────────────────┴─────────────────────┴──────────┴──────────────┴─────────────────────────┴───────────────┴──────────────────────────────────────────────────────────────┘

Python (python-pkg)
===================
Total: 2 (UNKNOWN: 0, LOW: 0, MEDIUM: 2, HIGH: 0, CRITICAL: 0)

┌─────────────────────────┬─────────────────────┬──────────┬────────┬───────────────────┬───────────────┬───────────────────────────────────────────────────────────┐
│         Library         │    Vulnerability    │ Severity │ Status │ Installed Version │ Fixed Version │                           Title                           │
├─────────────────────────┼─────────────────────┼──────────┼────────┼───────────────────┼───────────────┼───────────────────────────────────────────────────────────┤
│ Adyen (METADATA)        │ GHSA-f3q4-ggfp-jv34 │ MEDIUM   │ fixed  │ 4.0.0             │ 7.1.0         │ Adyen APIs Library for Python timing attack vulnerability │
│                         │                     │          │        │                   │               │ https://github.com/advisories/GHSA-f3q4-ggfp-jv34         │
├─────────────────────────┼─────────────────────┤          │        ├───────────────────┼───────────────┼───────────────────────────────────────────────────────────┤
│ cryptography (METADATA) │ GHSA-h4gh-qq45-vh27 │          │        │ 42.0.8            │ 43.0.1        │ pyca/cryptography has a vulnerable OpenSSL included in    │
│                         │                     │          │        │                   │               │ cryptography wheels                                       │
│                         │                     │          │        │                   │               │ https://github.com/advisories/GHSA-h4gh-qq45-vh27         │
└─────────────────────────┴─────────────────────┴──────────┴────────┴───────────────────┴───────────────┴───────────────────────────────────────────────────────────┘

/app/.jwt_key.pem (secrets)
===========================
Total: 1 (UNKNOWN: 0, LOW: 0, MEDIUM: 0, HIGH: 1, CRITICAL: 0)
```



### Conclusion



This project provides a hands-on experience in microservices-based e-commerce application deployment and security. By utilizing Docker, Kubernetes, and GKE, you will acquire important knowledge about configuring and overseeing scalable applications. Slick documentation and a flawless setup demonstration will show off your capacity to manage real-world DevOps issues and guarantee the effectiveness and security of contemporary e-commerce systems.



