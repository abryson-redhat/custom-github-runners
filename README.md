# custom-github-runners
custom github runners demo


## Overview
GitHub runners allow GitHub workflows to execute CI actions using a Container that is customized to suit the needs of the executing action.  GitHub runners can be hosted in GitHub or they can be [self-hosted](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners#usage-limits).

For purposes of this demo, self-hosted runners have been implemented.  The runners are instantiated on an OpenShift cluster and authenticate with GitHub.  A workflow can attach to a runner using the [runs-on](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/using-self-hosted-runners-in-a-workflow) directive.

###### example of `runs-on`
```yaml
runs-on: [self-hosted, java-build]
```

This project has a number of custom GitHub Runner images.  Not all have been utilized in the demo.  

## Runners

<style>
    .heatMap {
        width: 70%;
        text-align: center;
    }
    .heatMap th {
        background: silver;
        color: black;
        word-wrap: break-word;
        text-align: center;
    }
    .heatMap tr:nth-child(1) { background: green; color: white; text-align: center;}
    .heatMap tr:nth-child(2) { background: white; text-align: center;}
    .heatMap tr:nth-child(3) { background: green; color: white; text-align: center;}
    .heatMap tr:nth-child(4) { background: white; text-align: center;}
    .heatMap tr:nth-child(5) { background: green; color: white; text-align: center;}
    .heatMap tr:nth-child(6) { background: white; text-align: center;}
    .heatMap tr:nth-child(7) { background: green; color: white; text-align: center;}
    .heatMap tr:nth-child(8) { background: white; text-align: center;}
</style>


<div class="heatMap">

| Runner | Description | Status |
| ----------- | ----------- | ------  |
| base | Used as parent image by all runner images | Implemented |
| java-build-11 | Provides a JDK for Java 11 builds using maven / gradle | Implemented |
| java | Provides a JRE for Java 11 runtime |  Not utilized |
| buildah | Uses buildah for building Dockerfile / Containerfile based images | Implemented |
| node | NodeJS runtime image | Not utilized |
| dotnet-6.0 | dotnet-6.0 builder image | Not utilized |
| image-scan | RHEL based image with ACS CLI (roxctl) | Implemented |
| k8s-tools | RHEL based kubernetes tools (jq/yq/oc) | Implemented |

</div>



