# Prerequisites for Trivy Scanner (Alpha)

This topic describes prerequisites for installing SCST - Scan (Trivy) from the VMware package repository.

>**Important** This integration is in Alpha, which means that it is still in active
>development by the Tanzu Practice Global Tech Team and might be subject to
>change at any point. Users might encounter unexpected behavior.

## Verify the latest alpha package version

Run the following command to output a list of available tags.

Use the latest version returned in place of the sample version in this topic, such as `0.1.0-alpha.19` in the following output. 

```console
imgpkg tag list -i projects.registry.vmware.com/tanzu_practice/tap-scanners-package/trivy-repo-scanning-bundle | grep -v sha | sort -V

0.1.0-alpha.19
```

## Relocate images to a registry

VMware recommends relocating the images from VMware Tanzu Network registry to
your own container image registry before installing. The Trivy Scanner is currently in Alpha development phase, and as such, is not packaged as part of the Tanzu Application Platform package and is hosted on the VMware Project Repository instead of TanzuNet. Therefore, if you relocated the Tanzu Application Platform images, you may also wany to relocate the Trivy Scanner package. If you don’t relocate the
images, the Trivy Scanner installation depends on VMware Tanzu Network for
continued operation, and VMware Tanzu Network offers no uptime guarantees. The
option to skip relocation is documented for evaluation and proof-of-concept
only.

For information about supported registries, please see the documentation.

To relocate images from the VMware Project Registry to your registry:

1. Install Docker if it is not already installed.

2. Log in to your container image registry by running:

    ```console
    docker login MY-REGISTRY
    ```

    Where `MY-REGISTRY` is your own registry.

3. Set up environment variables for installation by running:

    ```console
    export INSTALL_REGISTRY_USERNAME=MY-REGISTRY-USER
    export INSTALL_REGISTRY_PASSWORD=MY-REGISTRY-PASSWORD
    export INSTALL_REGISTRY_HOSTNAME=MY-REGISTRY
    export VERSION=VERSION-NUMBER
    export INSTALL_REPO=TARGET-REPOSITORY
    ```

    Where:

    - `MY-REGISTRY-USER` is the user with write access to MY-REGISTRY.
    - `MY-REGISTRY-PASSWORD` is the password for `MY-REGISTRY-USER`.
    - `MY-REGISTRY` is your own registry.
    - `VERSION` is your Trivy Scanner version. For example, `0.1.0-alpha.19`.
    - `TARGET-REPOSITORY` is your target repository, a directory or repository on `MY-REGISTRY` that serves as the location for the installation files for Trivy Scanner.

4. [Install the Carvel tool imgpkg CLI](https://docs.vmware.com/en/Cluster-Essentials-for-VMware-Tanzu/1.4/cluster-essentials/deploy.html#optionally-install-clis-onto-your-path-6).

5. Relocate the images with the imgpkg CLI by running:

    ```console
    imgpkg copy -b projects.registry.vmware.com/tanzu_practice/tap-scanners-package/trivy-repo-scanning-bundle:${VERSION} --to-repo ${INSTALL_REGISTRY_HOSTNAME}/${INSTALL_REPO}/trivy-repo-scanning-bundle
    ```

> **Note**
> The VMware project repository does not require authentication, so there is no need to perform a docker login for it.

## Add the Trivy Scanner package repository

Tanzu CLI packages are available on repositories. Adding the Trivy Scanning
package repository makes the Trivy Scanning bundle and its packages available
for installation.

> **Note**
> VMware recommends, but does not require, relocating images to a registry for installation. This section assumes that you relocated images to a registry. See the earlier section to fill in the variables.

VMware recommends installing the Trivy Scanner objects in the existing `tap-install` namespace to keep the Trivy Scanner grouped logically with the other Tanzu Application Platform components.

1. Add the Trivy Scanner package repository to the cluster by running:

    ```console
    tanzu package repository add trivy-scanner-repository \
      --url ${INSTALL_REGISTRY_HOSTNAME}/${INSTALL_REPO}/trivy-repo-scanning-bundle:$VERSION \
      --namespace tap-install
    ```

2. Get the status of the Trivy Scanner package repository, and ensure that the status updates to `Reconcile succeeded` by running:

    ```console
    tanzu package repository get trivy-scanner-repository --namespace tap-install
    ```

    For example:

    ```console
   tanzu package repository get trivy-scanner-repository --namespace tap-install
   
   NAME:          trivy-scanner-repository
   VERSION:       7750726
   REPOSITORY:    projects.registry.vmware.com/tanzu_practice/tap-scanners-package/trivy-repo-scanning-bundle
   TAG:           0.1.0-alpha.19
   STATUS:        Reconcile succeeded
   REASON:
    ```

3. List the available packages by running:

    ```console
    tanzu package available list --namespace tap-install
    ```

    For example:

    ```console
    $ tanzu package available list --namespace tap-install
    / Retrieving available packages...
      NAME                                                 DISPLAY-NAME                       SHORT-DESCRIPTION
      trivy.scanning.apps.tanzu.vmware.com                   trivy                              Default scan templates using Trivy
    ```

## Prepare the Trivy Scanner configuration

Before installing the Trivy Scanner, you'll need to create the configuration necessary to install  

Define the `--values-file` flag to customize the default configuration. You must define the following fields in the `values.yaml` file for the Trivy
Scanner configuration. You can add fields as needed to activate or deactivate behaviors. You can append the values to this file as shown later in this
topic. Create a `values.yaml` file by using the following configuration:

```yaml
    ---
    namespace: DEV-NAMESPACE
    targetImagePullSecret: TARGET-REGISTRY-CREDENTIALS-SECRET
    targetSourceSshSecret: TARGET-SOURCE-SSH-SECRET
```

   Where:

   - `DEV-NAMESPACE` is your developer namespace.
   > **Note** To use a namespace other than the default namespace, ensure that
   the namespace exists before you install. If the namespace does not exist,
   the scanner installation fails.
   - `TARGET-REGISTRY-CREDENTIALS-SECRET` is the name of the secret that contains the
     credentials to pull an image from a private registry for scanning.
   - `TARGET-SOURCE-SSH-SECRET` is the name of the secret containing SSH credentials for cloning private repositories 


If you want to see all available values options, you can see those by running the following command using your desired version.
```shell
tanzu package available get trivy.scanning.apps.tanzu.vmware.com/$VERSION --values-schema -n tap-install
```

Example output
```shell
  KEY                                           DEFAULT                                                           TYPE    DESCRIPTION                                                                       
  targetImagePullSecret                                                                                           string  Reference to the secret used for pulling images from private registry             
  targetSourceSshSecret                                                                                           string  Reference to the secret containing SSH credentials for cloning private            
                                                                                                                          repositories                                                                      
  trivy.cli.source.additionalArguments                                                                             string  additional arguments to be appended to the fs scan command                        
  trivy.cli.image.additionalArguments                                                                              string  additional arguments to be appended to the image scan command                     
  trivy.cli.repositoryUrl                                                                                          string  location of the CLI tar in an OCI registry to be used in place of the embedded    
                                                                                                                          version                                                                           
  trivy.db.repositoryUrl                                                                                           string  location of the vulnerability database in an OCI registry to be used as the       
                                                                                                                          download location prior to running a scan                                         
  metadataStore.caSecret.name                   app-tls-cert                                                      string  Name of deployed Secret with key ca.crt holding the CA Cert of the Insight        
                                                                                                                          Metadata Store                                                                    
  metadataStore.caSecret.importFromNamespace    metadata-store                                                    string  Namespace from which to import the Insight Metadata Store CA Cert                 
  metadataStore.clusterRole                     metadata-store-read-write                                         string  Name of the deployed ClusterRole for read/write access to the Insight Metadata    
                                                                                                                          Store deployed in the same cluster                                                
  metadataStore.url                             https://metadata-store-app.metadata-store.svc.cluster.local:8443  string  Url of the Insight Metadata Store                                                 
  metadataStore.authSecret.importFromNamespace                                                                    string  Namespace from which to import the Insight Metadata Store auth_token              
  metadataStore.authSecret.name                                                                                   string  Name of deployed Secret with key auth_token                                       
  namespace                                     default                                                           string  Deployment namespace for the Scan Templates                                       
  resources.limits.cpu                          1000m                                                             string  Limits describes the maximum amount of cpu resources allowed.                     
  resources.requests.memory                     128Mi                                                             string  Requests describes the minimum amount of memory resources                         
  resources.requests.cpu                        250m                                                              string  Requests describes the minimum amount of cpu resources required.                  
  scanner.docker.password                                                                                         string  <nil>                                                                             
  scanner.docker.server                                                                                           string  <nil>                                                                             
  scanner.docker.username                                                                                         string  <nil>                                                                             
  scanner.pullSecret                                                                                              string  <nil>                                                                             
  scanner.serviceAccount                        trivy-scanner                                                      string  Name of scan pod's service ServiceAccount                                         
  scanner.serviceAccountAnnotations             <nil>                                                             <nil>   Annotations added to ServiceAccount   
```

The Trivy integration can work with or without the SCST - Store integration.
The values.yaml file is slightly different for each configuration.

## Supply Chain Security Tools - Store integration

When using SCST - Store integration, to persist the results
found by the Trivy Scanner, you can enable the SCST -
Store integration by appending the fields to the `values.yaml` file.

The Grype, Snyk, Prisma, Carbon Black, and Trivy Scanner Integrations enable the Metadata Store. To
prevent conflicts, the configuration values are slightly different based on
whether another scanner integration is installed or not. If Tanzu Application
Platform is installed using the Full Profile, the Grype Scanner Integration is
installed unless it is explicitly excluded.

If any other scanner integration is installed in the same dev-namespace where the Trivy Scanner is installed:

```yaml
#! ...
metadataStore:
  #! The url where the Store deployment is accessible.
  #! Default value is: "https://metadata-store-app.metadata-store.svc.cluster.local:8443"
  url: "STORE-URL"
  caSecret:
    #! The name of the secret that contains the ca.crt to connect to the Store Deployment.
    #! Default value is: "app-tls-cert"
    name: "CA-SECRET-NAME"
    importFromNamespace: "" #! since both Trivy and Grype/Snyk both enable store, one must leave importFromNamespace blank
  #! authSecret is for multicluster configurations.
  authSecret:
    #! The name of the secret that contains the auth token to authenticate to the Store Deployment.
    name: "AUTH-SECRET-NAME"
    importFromNamespace: "" #! since both Trivy and Grype/Snyk both enable store, one must leave importFromNamespace blank
```

Where:

- `STORE-URL` is the URL where the Store deployment is accessible.
- `CA-SECRET-NAME` is the name of the secret that contains the ca.crt to connect
  to the Store Deployment. Default is `app-tls-cert`.
- `AUTH-SECRET-NAME` is the name of the secret that contains the authentication token to
  authenticate to the Store Deployment.

If the other scanner integration is not installed in the same dev-namespace Trivy Scanner is installed:

```yaml
#! ...
metadataStore:
  #! The url where the Store deployment is accessible.
  #! Default value is: "https://metadata-store-app.metadata-store.svc.cluster.local:8443"
  url: "STORE-URL"
  caSecret:
    #! The name of the secret that contains the ca.crt to connect to the Store Deployment.
    #! Default value is: "app-tls-cert"
    name: "CA-SECRET-NAME"
    #! The namespace where the secrets for the Store Deployment live.
    #! Default value is: "metadata-store"
    importFromNamespace: "STORE-SECRETS-NAMESPACE"
  #! authSecret is for multicluster configurations.
  authSecret:
    #! The name of the secret that contains the auth token to authenticate to the Store Deployment.
    name: "AUTH-SECRET-NAME"
    #! The namespace where the secrets for the Store Deployment live.
    importFromNamespace: "STORE-SECRETS-NAMESPACE"
```

Where:

- `STORE-URL` is the URL where the Store deployment is accessible.
- `CA-SECRET-NAME` is the name of the secret that contains the `ca.crt` to connect to the Store Deployment. Default is `app-tls-cert`.
- `STORE-SECRETS-NAMESPACE` is the namespace where the secrets for the Store Deployment live. Default is `metadata-store`.
- `AUTH-SECRET-NAME` is the name of the secret that contains the authentication token to authenticate to the Store Deployment.

**Without Supply Chain Security Tools - Store Integration:** If you don’t want
to enable the SCST - Store integration, explicitly deactivate the integration by
appending the following fields to the values.yaml file that is enabled by
default:

```yaml
# ...
metadataStore:
  url: "" # Configuration is moved, so set this string to empty
```

## Prepare the ScanPolicy

The following sample ScanPolicy allows you to control whether the SupplyChain passes or fails based on the CycloneDX vulnerability results returned from the Trivy Scanner.

```yaml
---
apiVersion: scanning.apps.tanzu.vmware.com/v1beta1
kind: ScanPolicy
metadata:
   name: trivy-scan-policy
   labels:
      app.kubernetes.io/part-of: enable-in-gui
spec:
   regoFile: |
      package main

      import future.keywords.in
      import future.keywords.every

      # Accepted Values: "critical", "high", "medium", "low", unknown"
      notAllowedSeverities := ["critical", "high", "unknown"]
      notAllowedSet := {x | x := notAllowedSeverities[_]}
      ignoreCves := []

      isSafe(match) {
        severities := { e | e := match.ratings.rating.severity } | { e | e := match.ratings.rating[_].severity }
        every severity in severities {
            not severity in notAllowedSet
        }
      }

      isIgnored(match) {
        match.id in ignoreCves
      }

      deny[msg] {
        notAllowedVulnerabilities := { vulnerability |
          vulnerabilities := {e | e := input.bom.vulnerabilities.vulnerability} | {e | e := input.bom.vulnerabilities.vulnerability[_]}
          some vulnerability in vulnerabilities
          not isIgnored(vulnerability)
          not isSafe(vulnerability)
        }
        formattedVulnerabilityMessages := { message |
          some vulnerability in notAllowedVulnerabilities
          ratings := {e | e := vulnerability.ratings.rating.severity} | {e | e := vulnerability.ratings.rating[_].severity}
          formattedRatings := concat(", ", ratings)
          affectedComponents := {e | e := vulnerability.affects.target.ref} | {e | e := vulnerability.affects.target[_].ref}
          formattedComponents := concat("\\\n", affectedComponents)
          message = sprintf("CVE: %s \\\nRatings: %s\\\nAffected Components: \\\n%s", [vulnerability.id, formattedRatings, formattedComponents])
        }
        some formattedVulnerabilityMessage in formattedVulnerabilityMessages
        msg := formattedVulnerabilityMessage
      }
```

Apply the YAML:

```console
kubectl apply -n $DEV-NAMESPACE -f SCAN-POLICY-YAML
```

Where:

- `DEV-NAMESPACE` is the name of the developer namespace you want to use.
- `SCAN-POLICY-YAML` is the name of your SCST - Scan YAML.

## Install Trivy Scanner

After all prerequisites are completed, install the Trivy Scanner. See [Install another scanner for Supply Chain Security Tools - Scan](install-scanners.hbs.md).

## Air-Gap Configuration

### Prerequisites

Install the `oras` cli utilizing their [instructions](https://oras.land/cli/)

### Relocate Trivy DB to your registry

>Note: Using a relocated database means you are taking responsibility for keeping it up to date in a timely manner to ensure security scans are relevant. Stale databases weaken your security posture.

If you have a host with access, you can use oras cli to perform a copy.
```shell
oras copy -r ghcr.io/aquasecurity/trivy-db:2 registry.company.com/project_name/trivy-db:2 # the tag of 2 is required

Copying 4a39b38cf2fd db.tar.gz
Copied  4a39b38cf2fd db.tar.gz
Copied ghcr.io/aquasecurity/trivy-db:2 => registry.company.com/project_name/trivy-db:2
Digest: sha256:ed57874a80499e858caac27fc92e4952346eb75a2774809ee989bcd2ce48897a
```

If not, you can use oras cli to download the database and manifest and then push to your registry.

1. Download the trivy-db
   ```shell
   oras pull ghcr.io/aquasecurity/trivy-db:2
   Downloading 1612cc15d377 db.tar.gz
   Downloaded  1612cc15d377 db.tar.gz
   Pulled ghcr.io/aquasecurity/trivy-db:2
   Digest: sha256:af903c7ddbe7516f18b06254b6297cf53c0ece918def07322925c71d2f694860
   ```

2. Download the manifest for trivy-db
   ```shell
   oras manifest fetch ghcr.io/aquasecurity/trivy-db:2 > trivy-db-manifest.json
   ```

3. Push the prior downloaded trivy-db to your registry
   ```shell
   oras push registry.company.com/project_name/trivy-db:2 \
     --export-manifest trivy-db-manifest.json \
     ./db.tar.gz
   
   Uploading 1612cc15d377 db.tar.gz
   Uploaded  1612cc15d377 db.tar.gz
   Pushed registry.company.com/project_name/trivy-db:2
   Digest: sha256:41a7eeab8837e90d8a5afd56cfce73936e15d3db04c5294f992ecff9492971dc
   ```

### Update data values with db repository url

Edit your values.yaml to add the following
```yaml
trivy:
  db:
    repositoryUrl: "registry.company.com/project_name/trivy-db"
```
Note: the url should leave off the tag of `2`

## Use another Trivy Version

### Prerequisites

Install the `oras` cli utilizing their [instructions](https://oras.land/cli/)

### Download the CLI

Download the version of the CLI you are interested in from their [GitHub releases page](https://github.com/aquasecurity/trivy/releases). 
For example: https://github.com/aquasecurity/trivy/releases/download/v0.36.0/trivy_0.36.0_Linux-64bit.tar.gz
```shell
wget -c https://github.com/aquasecurity/trivy/releases/download/v0.36.0/trivy_0.36.0_Linux-64bit.tar.gz -O trivy.tar.gz
Length: 48363295 (46M) [application/octet-stream]
Saving to: ‘trivy.tar.gz’

trivy.tar.gz 100%[==>]  46.12M  50.7MB/s    in 0.9s

2023-01-25 10:47:55 (50.7 MB/s) - ‘trivy.tar.gz’ saved [48363295/48363295]
```

### Relocated the CLI to your registry
```shell
oras push registry.company.com/project_name/trivy-cli:0.36.0 \
--artifact-type trivy/cli \
./trivy.tar.gz:application/gzip

Uploading 121f4d8282aa trivy.tar.gz
Uploaded  121f4d8282aa trivy.tar.gz
Pushed registry.company.com/project_name/trivy-cli:0.36.0
Digest: sha256:5bdb18378e8f66a72f4bef4964edeccfcc2f21883e7a6caca6dbf7a3d7233696
```

### Update data values with CLI repository url

Edit your values.yaml to add the location of your cli.

```yaml
trivy:
  cli:
    repositoryUrl: "registry.company.com/project_name/trivy-cli:0.36.0"
```

## Known Limitations

- None
