# Convention Service

## <a id="overview"></a>Overview

Convention Service provides a means for people in operational roles to express
their hard-won knowledge and opinions about how applications should run on Kubernetes as a convention.
Convention Service applies these opinions to fleets of developer workloads as they are deployed to the platform,
saving operator and developer time.

The service is composed of two components:

* **The convention controller:**
  The convention controller provides the metadata to the convention server and executes the updates to Pod Template Spec as per the convention server's requests.

* **The convention server:**
  The convention server receives and evaluates metadata associated with a workload and
  requests updates to the Pod Template Spec associated with that workload.
  You can have one or more convention servers for a single convention controller instance.
  Convention Service currently supports defining and applying conventions for Pods.

## <a id="about-apply-conventions"></a>About applying conventions

The convention server uses criteria defined in the convention to determine
whether the configuration of a workload should be changed.
The server receives the OCI metadata from the convention controller.
If the metadata meets the criteria defined by the convention server,
the conventions are applied.
It is also possible for a convention to apply to all workloads regardless of metadata.

### <a id="apply-by-image-metadata"></a>Applying conventions by using image metadata

You can define conventions to target workloads by using properties of their OCI metadata.

Conventions can use this information to only apply changes to the configuration of workloads
when they match specific criteria (for example, Spring Boot or .Net apps, or Spring Boot v2.3+).
Targeted conventions can ensure uniformity across specific workload types deployed on the cluster.

You can use all the metadata details of an image when evaluating workloads. To see the metadata details, use the docker CLI command `docker image inspect IMAGE`.

> **Note:** Depending on how the image was built, metadata might not be available to reliably identify
the image type and match the criteria for a given convention server.
Images built with Cloud Native Buildpacks reliably include rich descriptive metadata.
Images built by some other process may not include the same metadata.

### <a id="apply-wo-image-metadata"></a>Applying conventions without using image metadata

Conventions can also be defined to apply to workloads without targeting build service metadata.
Examples of possible uses of this type of convention include appending a logging/metrics sidecar,
adding environment variables, or adding cached volumes.
Such conventions are a great way for you to ensure infrastructure uniformity
across workloads deployed on the cluster while reducing developer toil.

> **Note:** Adding a sidecar alone does not magically make the log/metrics collection work.
  This requires collector agents to be already deployed and accessible from the Kubernetes cluster,
and also configuring required access through role-based access control (RBAC) policy.
