# Release notes

This topic contains release notes for Tanzu Application Platform v1.0.

{{#unless vars.hide_content}}
## <a id='1-0-5'></a> v1.0.5

### <a id='1-0-5-known-issues'></a> Known issues

This release has the following known issues:

#### Grype scanner

**Scanning Java source code may not reveal vulnerabilities:** Source Code Scanning only scans
files present in the source code repository.
No network calls are made to fetch dependencies.
For languages that make use of dependency lock files, such as Golang and Node.js, Grype uses the
lock files to check the dependencies for vulnerabilities.

In the case of Java, dependency lock files are not guaranteed, so Grype instead uses the
dependencies present in the built binaries, such as `.jar` or `.war` files.

Because best practices do not include committing binaries to source code repositories, Grype
fails to find vulnerabilities during a Source Scan. The vulnerabilities are still found during the
Image Scan, after the binaries are built and packaged as images.

occurs during scanning, the Scan Phase field is not updated to `Error` and remains in the
`Scanning` phase. Read the scan pod logs to verify that there was an error.

#### <a id="1-0-5-known-issues-scst-scan"></a>Supply Chain Security Tools - Scan

**Blob Source Scan is reporting wrong source URL:**

- When running a Source Scan of a blob compressed file, Supply Chain Security Tools - Scan looks for
  a `.git` directory present in the files to extract information that is useful for the report sent
  to the Supply Chain Security Tools - Store deployment.

  The following workaround fixes this issue:

  1. This problem is resolved in Supply Chain Security Tools - Scan `v1.2.0`.
     Upgrade your Supply Chain Security Tools - Scan and Grype Scanner deployment to version `v1.2.0`
     or later.
  2. Configure your SourceScan or Workload to connect to the repository by using HTTPS instead of
     using SSH.
  3. Edit the FluxCD GitRepository resource to not include the `.git` directory.

### <a id='1-0-5-resolved-issues'></a> Resolved issues

This release has the following fixes:

#### <a id='1-0-5-resolved-issues-stk'></a> Services Toolkit

* Bump the services toolkit package to v0.5.2 in order to resolve CVE-2022-21698.

{{/unless}}

{{#unless vars.hide_content}}
## <a id='1-0-4'></a> v1.0.4

**Release Date**: September 13, 2022
{{/unless}}

## <a id='1-0-3'></a> v1.0.3

**Release Date**: April 1, 2022

### <a id='1-0-3-sec-issues'></a> Security issue

Tanzu Application Platform GUI is vulnerable to [CVE-2021-3918](https://nvd.nist.gov/vuln/detail/CVE-2021-3918) from the `json-schema` package.

### <a id='1-0-3-known-issues'></a> Known issues

This release has the following known issues:

#### Grype scanner

**Scanning Java source code may not reveal vulnerabilities:** Source Code Scanning only scans
files present in the source code repository.
No network calls are made to fetch dependencies.
For languages that make use of dependency lock files, such as Golang and Node.js, Grype uses the
lock files to check the dependencies for vulnerabilities.

In the case of Java, dependency lock files are not guaranteed, so Grype instead uses the
dependencies present in the built binaries, such as `.jar` or `.war` files.

Because best practices do not include committing binaries to source code repositories, Grype
fails to find vulnerabilities during a Source Scan. The vulnerabilities are still found during the
Image Scan, after the binaries are built and packaged as images.

occurs during scanning, the Scan Phase field is not updated to `Error` and remains in the
`Scanning` phase. Read the scan pod logs to verify that there was an error.

#### <a id="1-0-3-known-issues-scst-scan"></a>Supply Chain Security Tools - Scan

**Blob Source Scan is reporting wrong source URL:**
- When running a Source Scan of a blob compressed file, Supply Chain Security Tools - Scan looks for a `.git` directory present in the files to extract information that is useful for the report sent to the Supply Chain Security Tools - Store deployment.

- Workaround - The following workarounds fix this issue:

  1. This problem is resolved in Supply Chain Security Tools - Scan `v1.2.0`. Upgrade your Supply Chain Security Tools - Scan and Grype Scanner deployment to version `v1.2.0` or later.
  1. Configure your SourceScan or Workload to connect to the repository by using HTTPS instead of using SSH.
  1. Edit the FluxCD GitRepository resource to not include the `.git` directory.

### <a id='1-0-3-resolved-issues'></a> Resolved issues

This release has the following fix:

- [CVE-2022-22965](https://cve.mitre.org/cgi-bin/cvename.cgi?name=2022-22965): Spring Framework RCE via Data Binding on JDK 9+

## <a id='1-0-2'></a> v1.0.2

**Release Date**: March 8, 2022

### <a id='1-0-2-sec-issues'></a> Security issue

Tanzu Application Platform GUI is vulnerable to [CVE-2021-3918](https://nvd.nist.gov/vuln/detail/CVE-2021-3918) from the `json-schema` package.

### <a id='1-0-2-known-issues'></a> Known issues

This release has the following known issues:

#### Grype scanner

**Scanning Java source code may not reveal vulnerabilities:** Source Code Scanning only scans
files present in the source code repository.
No network calls are made to fetch dependencies.
For languages that make use of dependency lock files, such as Golang and Node.js, Grype uses the
lock files to check the dependencies for vulnerabilities.

In the case of Java, dependency lock files are not guaranteed, so Grype instead uses the
dependencies present in the built binaries, such as `.jar` or `.war` files.

Because best practices do not include committing binaries to source code repositories, Grype
fails to find vulnerabilities during a Source Scan. The vulnerabilities are still found during the
Image Scan, after the binaries are built and packaged as images.

#### Supply Chain Security Tools – Scan

- **Two scan jobs and two scan pods appear at the same time**: There is an edge case where two scan
jobs and two scan pods appear when a scan policy is updated.
This does not affect the result of the scan.
- **Scan Phase indicates `Scanning` incorrectly:** Scans have an edge case where, when an error
occurs during scanning, the Scan Phase field is not updated to `Error` and remains in the
`Scanning` phase. Read the scan Pod logs to verify if there was an error.


### <a id='1-0-2-resolved-issues'></a> Resolved issues

This release has the following fixes:

#### Services Toolkit

* Resolved an issue with the `tanzu services` CLI plugin which meant it was not compatible with Kubernetes clusters running on GKE.
* Fixed a potential race condition during reconciliation of ResourceClaims which could cause the Services Toolkit manager to crash.
* Updated configuration of the Services Toolkit carvel Package to prevent an unwanted build up of ConfigMap resources.

#### Supply Chain Security Tools – Scan

- Resolved the issue that events show `SaveScanResultsSuccess` when metadata store is not configured.
- CVE print columns are now properly populated.
- Fixed failing Blob source scans where `.git` directory is not provided.
- Prevent scan controller pod from failing when metadata store certificate is not available.
- Removed unnecessary reconciliation of resources upon deletion.
- Prevent scan controller failure upon Git clone fails.

## <a id='1-0-1'></a> v1.0.1

**Release Date**: February 8, 2022

### <a id='1-0-1-sec-issues'></a> Security issue

Tanzu Application Platform GUI is vulnerable to [CVE-2021-3918](https://nvd.nist.gov/vuln/detail/CVE-2021-3918) from the `json-schema` package.

### <a id='1-0-1-known-issues'></a> Known issues

This release has the following known issues:

#### Developer Conventions

**Debug Convention might not apply:** If you upgraded from Tanzu Application Platform v0.4
then the debug convention may not apply to the app run image. This is because of the
missing SBOM data in the image.
To prevent this issue, delete existing app images that were built using
Tanzu Application Platform v0.4.

#### Grype scanner

**Scanning Java source code may not reveal vulnerabilities:** Source Code Scanning only scans
files present in the source code repository.
No network calls are made to fetch dependencies.
For languages that make use of dependency lock files, such as Golang and Node.js, Grype uses
the lock files to check the dependencies for vulnerabilities.

In the case of Java, dependency lock files are not guaranteed, so Grype instead uses the
dependencies present in the built binaries (`.jar` or `.war` files).

Because best practices do not include committing binaries to source code repositories, Grype
fails to find vulnerabilities during a Source Scan. The vulnerabilities are still found
during the Image Scan, after the binaries are built and packaged as images.

#### Services Toolkit

- Resolved an issue with the `tanzu services` CLI plug-in which meant it was not compatible with Kubernetes clusters running on GKE.
- Fixed a potential race condition during reconciliation of ResourceClaims which might cause the Services Toolkit manager to stop responding.
- Updated configuration of the Services Toolkit carvel Package to prevent an unwanted build up of ConfigMap resources.

#### Application Accelerator

- Build scripts are provided as part of an accelerator now have the execute bit set when a
new project is generated from the accelerator.
- Accelerators that do not include an `accelerator.yaml` file or have an empty list of
options now render in the UI.
- The CLI plug-in no longer shows panic output for errors. It just adds the error message to
the output.
- The entity loader for Tanzu Application Platform GUI does not stop when encountering an
invalid accelerator.
- Deleted accelerators are no longer shown in Tanzu Application Platform GUI.
- The Tanzu Application Platform GUI Explore feature now shows engine errors.

#### Application Live View

- Updated pod security policies for Application Live View component
- Updated Spring Boot v2.5.7 to v2.5.8
- Application Live View connector now handles stream reset exceptions
- Increased requests and limits for Application Live View connector to fix pod restarts
- CVE vulnerability fix to update `protobuf-java` to `3.19.2` in the connector

#### Tanzu Application Platform GUI

- This release is vulnerable to [CVE-2021-3918](https://nvd.nist.gov/vuln/detail/CVE-2021-3918) from the json-schema package

### <a id='1-0-1-resolved-issues'></a> Resolved issues

This release has the following fixes:

#### Tanzu Dev Tools for VSCode

- v0.5.0 release does not install the extensions in the dependency "Extension Pack for Java". v0.5.2 installs "Debugger for Java" and "Language Support for Java(TM) by Red Hat" extensions directly instead of installing the extension pack.
- Users can run "Configure Tasks", "Configure the Default Build Task", or "Launch Extension Host" when using the Tanzu Dev Tools extension in a workspace without a `workload.yaml` file.
- Fixes [CVE 2022-0144](https://www.cvedetails.com/cve/CVE-2022-0144/)

## <a id='1-0'></a> v1.0

**Release Date**: January 11, 2022

### Known issues

This release has the following issues:

#### Installing

When you install Tanzu Application Platform on Google Kubernetes Engine (GKE), Kubernetes control
plane can be unavailable for several minutes during the installation.
Package installations can enter the `ReconcileFailed` state. When API server becomes available,
packages try to reconcile to completion.

This can happen on newly provisioned clusters that have not finished GKE API server autoscaling.
When GKE scales up an API server, the current Tanzu Application install continues, and any subsequent installs succeed without interruption.

#### Application Accelerator

- Build scripts provided as part of an accelerator do not have the execute bit set when a new
  project is generated from the accelerator.

    To resolve this issue, explicitly set the execute bit. For more information, see
    [Execute Bit Not Set for App Accelerator Build Scripts](troubleshooting.html#build-scripts-lack-execute-bit)
    in _Troubleshooting Tanzu Application Platform_.

- Upgraded log4j-api dependency to 2.16.0.

- Disabled the Exec Transform.

- Improved App Accelerator TAP GUI plug-in refresh cycle.

- Fixed Accelerator loading issues for TAP GUI plug-in.

#### Application Live View

The Live View section in Tanzu Application Platform GUI might show
"No live information for pod with ID" after deploying Tanzu Application Platform workloads.

Resolve this issue by recreating the Application Live View Connector pod. For more information, see
[No Live Information for Pod with ID Error](troubleshooting.html#no-live-information) in
_Troubleshooting Tanzu Application Platform_.

#### Convention Service

Convention Service does not currently support custom certificates for integrating with a private
registry. Support for custom certificates is planned for an upcoming release.

#### Developer Conventions

**Debug Convention might not apply:** If you upgraded from Tanzu Application Platform v0.4 then the
the debug convention might not apply to the app run image. This is because of the missing SBOM data
in the image.
To prevent this issue, delete existing app images that were built using Tanzu Application Platform
v0.4.

For more information, see [Debug Convention May Not Apply](troubleshooting.html#debug-convention) in
_Troubleshooting Tanzu Application Platform_.

#### Grype scanner

**Scanning Java source code may not reveal vulnerabilities:** Source Code Scanning only scans
files present in the source code repository.
No network calls are made to fetch dependencies.
For languages that make use of dependency lock files, such as Golang and Node.js, Grype uses the
lock files to check the dependencies for vulnerabilities.

In the case of Java, dependency lock files are not guaranteed, so Grype instead uses the
dependencies present in the built binaries (`.jar` or `.war` files).

Because best practices do not include committing binaries to source code repositories, Grype
fails to find vulnerabilities during a Source Scan. The vulnerabilities are still found during the
Image Scan, after the binaries are built and packaged as images.

#### Learning Center

- **Training Portal in pending state:** Under certain circumstances, the training portal is stuck in
a pending state. To resolve this issue, see
[Training portal stays in pending state](learning-center/troubleshoot-learning-center.md#training-portal-pending).

- **image-policy-webhook-service not found:** If you are installing a Tanzu Application Platform
profile, you might see the error:

    ```
    Internal error occurred: failed calling webhook "image-policy-webhook.signing.apps.tanzu.vmware.com": failed to call webhook: Post "https://image-policy-webhook-service.image-policy-system.svc:443/signing-policy-check?timeout=10s": service "image-policy-webhook-service" not found
    ```

    This is a rare condition error among some packages. To recover from this error, redeploy the
    `trainingPortal` resource.

- **Cannot Update Parameters:**
Normally you must update some parameters provided to the Learning Center Operator. These parameters
include ingressDomain, TLS secret, ingressClass, and others.

    After updating parameters, if the Training Portals do not work or you cannot see the updated values,
    redeploy `trainingportal` in a maintenance window where Learning Center is unavailable while the
    `systemprofile` is updated.

- **Increase your cluster's resources:** Node pressure may be caused by not enough nodes or not
enough resources on nodes for deploying the workloads you have. In this case, follow your cloud
provider instructions on how to scale out or scale up your cluster.

#### Supply Chain Choreographer

Deployment from a public Git repository might require a Git SSH secret.
Workaround is to configure SSH access for the public Git repository.

#### Supply Chain Security Tools – Scan

- **Failing Blob source scans:** Blob source scans have an edge case where, when a compressed file
without a `.git` directory is provided, sending results to the Supply Chain Security Tools - Store
fails and the scanned revision value is not set. The current workaround is to add the `.git`
directory to the compressed file.
- **Events show `SaveScanResultsSuccess` incorrectly:** `SaveScanResultsSuccess` appears in the
events when the Supply Chain Security Tools - Store is not configured. The `.status.conditions`
output, however, correctly reflects `SendingResults=False`.
- **Scan Phase indicates `Scanning` incorrectly:** Scans have an edge case where, when an error has
occurred during scanning, the Scan Phase field is not updated to `Error` and instead remains in the
`Scanning` phase. Read the scan Pod logs to verify there was an error.
- **CVE print columns are not getting populated:** After running a scan and using `kubectl get` on
the scan, the CVE print columns (CRITICAL, HIGH, MEDIUM, LOW, UNKNOWN, CVETOTAL) are not populated.
You can run `kubectl describe` on the scan and look for `Scan completed. Found x CVE(s): ...` under
`Status.Conditions` to find these severity counts and `CVETOTAL`.
- **Scan controller pod fails:** If there is a misconfiguration
(i.e. secretgen-controller not running, wrong CA secret name) after enabling the
metadata store integration, the controller pod fails. The current workaround is
to update the `tap-values.yaml` file with the proper configuration and update the application.
- **Deleted resources keep reconciling:** After creating a scan CR and deleting it,
the controllers keep trying to fetch the deleted resource, resulting in a `not found`
or `unable to fetch` log entry with every reconciliation cycle.
- **Scan controller crashes when Git clone fails:** If this occurs, confirm that
the Git URL and the SSH credentials are correct.

#### Supply Chain Security Tools - Sign

- **Blocked pod creation:** If all webhook nodes or pods are evicted by the cluster or scaled down,
the admission policy blocks any pods from being created in the cluster.
To resolve the issue, delete `MutatingWebhookConfiguration` and re-apply it when the cluster is
stable.

- **MutatingWebhookConfiguration prevents pods from being admitted:** Under certain circumstances,
  if the `image-policy-controller-manager` deployment pods do not start up before the
  `MutatingWebhookConfiguration` is applied to the cluster, it can prevent the admission of all
  pods.

    To resolve this issue, delete the `MutatingWebhookConfiguration` resource, then restore the
    `MutatingWebhookConfiguration` resource to re-enable image signing enforcement. For instructions,
    see [MutatingWebhookConfiguration Prevents Pod Admission](troubleshooting.html#pod-admission-prevented)
    in _Troubleshooting Tanzu Application Platform_.

- **Terminated kube-dns prevents new pods from being admitted:**
If `kube-dns` is terminated, it prevents the admission controller from being able to reach the image policy controller. This prevents new pods from being admitted, including core services like kube-dns.

    Modify the mutating webhook configuration to exclude the `kube-system` namespace from the admission check. This allows pods in the `kube-system` to appear, which should restore `kube-dns`

- **Priority class of webhook's pods might preempt less privileged pods:**
This component uses a privileged `PriorityClass` to start up its pods in order to prevent node
pressure from preempting its pods. However, this can cause other less privileged components to have
their pods preempted or evicted instead.

    To resolve this issue, see [Priority Class of Webhook's Pods Preempts Less Privileged Pods](troubleshooting.html#priority-class-preempts)
    in _Troubleshooting Tanzu Application Platform_.

#### Supply Chain Security Tools - Store

- **CrashLoopBackOff from password authentication failed:**
Supply Chain Security Tools - Store does not start up. You see the following error in the
`metadata-store-app` Pod logs:

    ```
    $ kubectl logs pod/metadata-store-app-* -n metadata-store -c metadata-store-app
    ...
    [error] failed to initialize database, got error failed to connect to `host=metadata-store-db user=metadata-store-user database=metadata-store`: server error (FATAL: password authentication failed for user "metadata-store-user" (SQLSTATE 28P01))
    ```

    This error results when the database password was changed between deployments. This is not
    supported. To resolve this issue, see [CrashLoopBackOff from Password Authentication Fails](troubleshooting.html#password-authentication-fails)
    in _Troubleshooting Tanzu Application Platform_.

    > **Warning:** Changing the database password deletes your Supply Chain Security Tools - Store data.

- **Persistent volume retains data**
  If Supply Chain Security Tools - Store is deployed, deleted, and then redeployed the
  `metadata-store-db` Pod fails to start if the database password changed during redeployment.
  This is caused by the persistent volume used by postgres retaining old data, even though the retention
  policy is set to `DELETE`.

    To resolve this issue, see [CrashLoopBackOff from Password Authentication Fails](troubleshooting.html#password-authentication-fails)
    in _Troubleshooting Tanzu Application Platform_.

    > **Warning:** Changing the database password deletes your Supply Chain Security Tools - Store data.

- **Missing persistent volume:**
After Supply Chain Security Tools - Store is deployed, `metadata-store-db` Pod might fail for missing
volume while `postgres-db-pv-claim` pvc is in the `PENDING` state.
This issue may occur if the cluster where Supply Chain Security Tools - Store is deployed does not have
`storageclass` defined.

    The provisioner of `storageclass` is responsible for creating the persistent volume after
    `metadata-store-db` attaches `postgres-db-pv-claim`. To resolve this issue, see
    [Missing Persistent Volume](troubleshooting.html#missing-persistent-volume)
    in _Troubleshooting Tanzu Application Platform_.

- **Querying local path source reports:**
If a source report has a local path as the name -- for example, `/path/to/code` -- the leading
`/` on the resulting repository name causes the querying packages and vulnerabilities to return
the following error from the client lib and the CLI: `{ "message": "Not found" }`.<br><br>
The URL of the resulting HTTP request is properly escaped. For example,
`/api/sources/%2Fpath%2Fto%2Fdir/vulnerabilities`.<br><br>
The rbac-proxy used for authentication handles this URL in a way that the response is a
redirect. For example, `HTTP 301\nLocation: /api/sources/path/to/dir/vulnerabilities`.
The Client Lib follows the redirect, making a request to the new URL which does not exist in the
Supply Chain Security Tools - Store API, resulting in this error message.

- **No support for installing in custom namespaces:**
All of our testing uses the `metadata-store` namespace. Using a different namespace breaks
authentication and certificate validation for the `metadata-store` API.


#### Tanzu CLI

- `tanzu apps workload get`: Passing in `--output json` with the `--export` flag returns YAML
rather than JSON. Support for honoring the `--output json` with `--export` is planned for the next
release.
- `tanzu apps workload create/update/apply`: `--image` is not supported by the default supply chain
in Tanzu Application Platform v0.3.
`--wait` functions as expected when a workload is created for the first time but might return
prematurely on subsequent updates when passed with `workload update/apply` for existing workloads.
When the `--wait` flag is included and you decline the "Do you want to create this workload?"
prompt, the command continues to wait and must be cancelled manually.

#### Tanzu Dev Tools for VSCode

**Unable to configure task:** Launching the `Extension Host`, and configuring `tasks` in a workspace
that does not contain workload YAML files might not work.
To solve this issue, uninstall the Tanzu Dev Tools extension.

**Extension Pack for Java:** In some cases, the Extension Pack for Java (`vscjava.vscode-java-pack`)
does not automatically install. In these cases debugging does not work after Tanzu Dev Tools
installs `live-update`.
To solve this issue, manually install `vscjava.vscode-java-pack` from the extension marketplace.

#### Services Toolkit

* It is not possible for more than one application workload to consume the same service instance.
Attempting to create two or more application workloads while specifying the same `--service-ref`
value causes only one of the workloads to bind to the service instance and reconcile successfully.
This limitation is planned to be relaxed in an upcoming release.
* The `tanzu services` CLI plug-in is not compatible with Kubernetes clusters running on GKE.

#### Tanzu Application Platform GUI

- This release is vulnerable to [CVE-2021-3918](https://nvd.nist.gov/vuln/detail/CVE-2021-3918) from the json-schema package

### <a id='1-0-security-issues'></a> Security issue

The installation specifies that the installer's Tanzu Network credentials be exported to all
namespaces. Customers can choose to mitigate this concern using one of the following methods:

- Create a Tanzu Network account with their own credentials and use this for the installation
exclusively.
- Using [Carvel tool's imgpkg](https://carvel.dev/imgpkg/) customers can create a dedicated OCI
registry on their own infrastructure that can comply with any required security policies that might
exist.


### <a id='1-0-breaking-changes'></a> Breaking changes

This release has the following breaking change:

**Supply Chain Security Tools - Store:** Changed package name to `metadata-store.apps.tanzu.vmware.com`.

### <a id='1-0-resolved-issues'></a> Resolved issues

This release has the following fixes:

#### Tanzu Dev Tools for VSCode

- Fixed issue where the Tanzu Dev Tools extension might not support projects with multi-document
YAML files
- Modified debug to remove any leftover port-forwards from past runs

#### Supply Chain Security Tools - Store

Upgrade golang version from `1.17.1` to `1.17.5`
