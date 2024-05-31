# custom-github-runners

# Table of Contents
1. [Overview](#Overview)
2. [Runners](#Runners)
3. [Instructions](#Instructions)



## Overview
GitHub runners allow GitHub workflows to execute CI actions using a Container that is customized to suit the needs of the executing action.  GitHub runners can be hosted in GitHub or they can be [self-hosted](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners#usage-limits).

For purposes of this demo, self-hosted runners have been implemented.  The runners are instantiated on an OpenShift cluster and authenticate with GitHub.  A workflow can attach to a runner using the [runs-on](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/using-self-hosted-runners-in-a-workflow) directive.

###### example of `runs-on`
```yaml
runs-on: [self-hosted, java-build]
```

This project has a number of custom GitHub Runner images.  Not all have been utilized in the demo.  

## Runners

This is a list of runner images built for this demo.  Not all were utilized.  A [separate project](#) is used for installation of the runners on an OpenShift cluster.

<br/>

<table>
  <tr style="background-color:silver;">
    <th>Runner</th>
    <th>Description</th>
    <th>Status</th>
  </tr>
  <tr style="background-color:green;color:white;">
    <td>base</td>
    <td>Used as parent image by all runner images</td>
    <td>Implemented</td>
  </tr>
  <tr style="background-color:white">
    <td>java-build-11</td>
    <td>Provides a JDK for Java 11 builds using maven / gradle</td>
    <td>Implemented</td>
  </tr>
  <tr style="background-color:green;color:white">
    <td>java</td>
    <td>Provides a JRE for Java 11 runtime</td>
    <td>Not utilized</td>
  </tr>
  </tr>
  <tr style="background-color:white">
    <td>buildah</td>
    <td>Uses buildah for building Dockerfile / Containerfile based images</td>
    <td>Implemented</td>
  </tr>
  <tr style="background-color:green;color:white">
    <td>node</td>
    <td>NodeJS runtime image</td>
    <td>Not utilized</td>
  </tr>
  <tr style="background-color:white">
    <td>image-scan</td>
    <td>RHEL based image with ACS CLI (roxctl)</td>
    <td>Implemented</td>
  </tr> 
  <tr style="background-color:green;color:white">
    <td>k8s-tools</td>
    <td>RHEL based image with kubernetes tools (jq/yq/oc)</td>
    <td>Implemented</td>
  </tr>  
</table>

<br/>

> :exclamation: **Be aware of your platform architecture**: Most container runtimes want to run on linux/amd64 architecture.  If you're using **podman** on an *arm64* or *aarch64* architecture, you'll need to use the `podman build --platform linux/amb64` approach.

<br/>

---

### Base runner image
> **base**
├── Containerfile
├── entrypoint.sh
├── get_github_app_token.sh
├── get-runner-release.sh
├── README.md
├── register.sh
└── uid.sh

This image is the parent image for all other images.  

---

### Java runner image
> **java**
├── Containerfile
└── README.md

This is a Java 11 JRE enabled image.  You can use it to host java applications.

---

### Java build runner image
> **java-build-11**
├── Containerfile
└── README.md

This is a **Java 11 JDK** enabled image.  It also includes **maven** and **gradle** for executing java builds.

---

### Buildah runner image
> **buildah**
├── Containerfile
└── README.md

This is a RHEL based image with **buildah** for creating OCI images.  For this demo, it is used for building **Dockerfile / Containerfile** based images.

---

### dotnet 6.0 runtime image
> **dotnet-6.0**
├── Containerfile
└── README.md

This is a dotnet 6.0 runtime image.  It is not utilized for this demo.

---


## Instructions

The following is a list of steps to be followed when building runner images.


### Pre-requisites
- Using a bash enabled environment.
- `podman` or `docker` should be installed and configured
- access to an image registry you'll use for hosting / installation of runners
  * For this demo, I used quay.io

### Steps to build an image
1. Make changes to the Containerfile (and supporting scripts if necessary) of the relevant runner image.
2. Build the image using `podman` or `docker`

  ```bash
  podman build -t buildah-runner-image:v1.0.1 -f Containerfile .
  ```   
3. Login to the remote registry

  ```bash
  podman login -u ****** -p ******* <remote registry url>
  ```
4. Push image from local registry to the remote registry
  ```bash
  podman push localhost/buildah-runner-image:v1.0.1 <remote registry url>/repository/smbc-demo/buildah-runner-image:v1.0.1
  ```

The image is now ready for use by the [runner installation project](#).  

> :information_source: You will need to make sure that project points to your \<remote registry url\>.




