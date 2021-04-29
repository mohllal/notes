# Helm Documentation

[Docs](https://helm.sh/docs/)!

Date Started: Sunday, February 28, 2021

Date Finished: TBD

## Home

- Helm is the package manager for Kubernetes. As an operating system package manager makes it easy to install tools on an OS, Helm makes it easy to install applications and resources into Kubernetes clusters.

## The History of the Project

- Helm began as what is now known as Helm Classic, a Deis project begun and introduced at the inaugural KubeCon. Then, the project merged with a GCS tool called Kubernetes Deployment Manager, and the project was moved under [Kubernetes](https://kubernetes.io/).
- **When the Helm 3 development cycle began, Tiller was removed, bringing Helm closer to its original vision of being a client tool**. But Helm 3 continues to track releases inside of the Kubernetes cluster, making it possible for teams to work together on a common set of Helm releases.

## Using Helm

- Helm installs *charts* into Kubernetes, creating a new *release* for each installation. And to find new charts, you can search Helm chart *repositories*.
- **Finding Charts**:
  - `helm search hub` searches the Artifact Hub, which lists helm charts from dozens of different repositories.
  - `helm search repo` searches the repositories added to the local helm client (with `helm repo add`). This search is done over local data, and no public network connection is needed.
- **Installing a Package**:
  - `helm install` takes two arguments: release name, and chart name. Helm does not wait until all of the resources are running before it exits.
  - `helm install foo foo-0.1.1.tgz` installs a local chart archive.
  - `helm install foo path/to/foo` installs an unpacked chart directory.
  - `helm install foo https://example.com/charts/foo-1.2.3.tgz` installs from a full URL.
- **Track a Release**: `helm status` used to track of a release's state, or to re-read configuration information.
- **Customizing a Chart**: `helm show values` shows configurable options on a chart. If both of `--set` and `--values` are used, `--set` values are merged into --values with higher precedence. Overrides specified with `--set` are persisted in a ConfigMap. Values that have been `--set` can be viewed for a given release with `helm get values <release-name>`. Values that have been `--set` can be cleared by running helm upgrade with `--reset-values` specified.
- **Upgrading a Release**: `helm upgrade`
- **Recovering on Failure**: `helm rollback [RELEASE] [REVISION]`. A release version is an incremental revision. Every time an install, upgrade, or rollback happens, the revision number is incremented by 1. The first revision number is always 1. And we can use `helm history [RELEASE]` to see revision numbers for a certain release.
- **Helpful Options for Install/Upgrade/Rollback**:
  - `--timeout`: A Go duration value to wait for Kubernetes commands to complete. This defaults to *5m0s*.
  - `--wait`: Waits until all Pods are in a ready state, PVCs are bound, Deployments have minimum (Desired minus maxUnavailable) Pods in ready state and Services have an IP address (and Ingress if a LoadBalancer) before marking the release as successful. It will wait for as long as the `--timeout` value. If timeout is reached, the release will be marked as *FAILED*.
  - `--no-hooks`: Skips running hooks for the command.
- **Uninstalling a Release**: `helm uninstall [RELEASE]`. To keep a deletion release record, use `helm uninstall --keep-history`. Using `helm list --uninstalled` will only show releases that were uninstalled with the `--keep-history` flag.
- **Working with Repositories**:
  - `helm repo list`
  - `helm repo add [NAME] [URL]`
  - `helm repo update`
  - `helm repo remove [NAME]`
- **Working with Charts**
  - `helm create [CHART_NAME]`
  - `helm lint`
  - `helm package [CHART_NAME]`

## Chart Development Tips and Tricks

- It is always safer quoting the strings than leaving them as bare words: `name: {{ .Values.MyName | quote }}`.
- When working with integers do not quote the values: `port: {{ .Values.Port }}`.
- Go provides a way of including one template in another using a built-in `template` directive. However, the built-in function cannot be used in Go template pipelines `{{ include "toYaml" $value | indent 2 }}`.
- The `required` function gives developers the ability to declare a value entry as required for template rendering. If the entry is empty in values.yaml, the template will not render and will return an error message supplied by the developer `{{ required "A valid foo is required!" .Values.foo }}`.
- The `sha256sum` function can be used to ensure a deployment's annotation section is updated if another file changes: `checksum/config: {{ include (print $.Template.BasePath "/configmap.yaml") . | sha256sum }}` or `rollme: {{ randAlphaNum 5 | quote }}`.
- The annotation `"helm.sh/resource-policy": keep` instructs Helm to skip deleting this resource when a helm operation (such as helm uninstall, helm upgrade or helm rollback) would result in its deletion.

## Glossary

- **Chart**: *A Helm package that contains information sufficient for installing a set of Kubernetes resources into a Kubernetes cluster*. Charts contain a `Chart.yaml` file as well as templates, default values `values.yaml` file, and dependencies. Charts are developed in a well-defined directory structure, and then packaged into an archive format called a *chart archive*.
- **Chart Archive**: A chart archive is a *tarred and gzipped (and optionally signed)* chart.
- **Chart Dependency (Sub-charts)**:
  - **Soft dependency**: *A chart may simply not function without another chart being installed in a cluster*. Helm does not provide tooling for this case. In this case, dependencies may be managed separately.
  - **Hard dependency**: *A chart may contain (inside of its charts/ directory) another chart upon which it depends*. In this case, installing the chart will install all of its dependencies. In this case, a chart and its dependencies are managed as a collection. When a chart is packaged (via `helm package`) all of its hard dependencies are bundled with it.
- **Chart Version**: Charts are versioned according to the [SemVer 2](https://semver.org/) spec. A version number is required on every chart.
- **Chart.yaml**: Information about a chart is stored in a special file called `Chart.yaml`. Every chart must have this file.
- **Helm Configuration Files (XDG)**: Helm stores its configuration files in `XDG` directories. These directories are created the first time helm is run.
- **Chart Linting**: To lint a chart is to validate that it follows the conventions and requirements of the Helm chart standard. Helm provides tools to do this, notably the `helm lint` command.
- **Provenance (Provenance file)**: Helm charts may be accompanied by *a provenance file which provides information about where the chart came from and what it contains*. A provenance contains a cryptographic hash of the chart archive file, the Chart.yaml data, and a signature block (an OpenPGP "clearsign" block). *Provenance files have the `.prov` extension, and can be served from a chart repository server or any other HTTP server*.
- **Release**: When a chart is installed, the Helm library creates a release to track that installation.
- **Release Number (Release Version)**: A single release can be updated multiple times. *A sequential counter is used to track releases as they change. After a first helm install, a release will have release number 1*. Each time a release is upgraded or rolled back, the release number will be incremented.
- **Rollback**: A release can be upgraded to a newer chart or configuration. But since release history is stored, a release can also be rolled back to a previous release number. This is done with the `helm rollback` command.
- **Chart Repository**: *A chart repository server is a simple HTTP server that can serve an `index.yaml` file that describes a batch of charts, and provides information on where each chart can be downloaded from*. By default, Helm clients are not configured with any chart repositories. Chart repositories can be added at any time using the `helm repo add` command.
- **values.yaml**: Values provide a way to override template defaults with configurable information. *Helm Charts are parameterized, which means the chart developer may expose configuration that can be overridden at installation time*. Values can be set during `helm install` and `helm upgrade` operations, either by passing them in directly, or by using a `values.yaml` file.

## Key differences between Helm 2 and Helm 3

- **Removal of Tiller**: With Tiller gone, the security model for Helm is radically simplified. Helm 3 now supports all the modern security, identity, and authorization features of modern Kubernetes. Helm’s permissions are evaluated using the `kubeconfig` file.
- **Improved Upgrade Strategy: 3-way Strategic Merge Patches**: *Helm 2 used a two-way strategic merge patch*. During an upgrade, it compared the most recent chart's manifest against the proposed chart's manifest (the one supplied during helm upgrade). *Helm 3 considers the old manifest, its live state, and the new manifest when generating a patch*.
- **Release Names Are Now Scoped to The Namespace**: In Helm 2, each release information was stored in the same namespace as Tiller. *In practice, this meant that once a name was used by a release, no other release could use that same name, even if it was deployed in a different namespace. In Helm 3, information about a particular release is now stored in the same namespace as the release itself*.
- **Secrets as the Default Storage Driver**: *In Helm 3, Secrets are now used as the default storage driver. Helm 2 used ConfigMaps by default to store release information.* Changing to Secrets as the Helm 3 default allows for additional security in protecting charts in conjunction with the release of Secret encryption in Kubernetes.
- **Capabilities**: The `.Capabilities` built-in object available during the rendering stage has been simplified.
- **Validating Chart Values with JSONSchema**: *A JSON Schema can now be imposed upon chart values*. This ensures that values provided by the user follow the schema laid out by the chart maintainer, providing better error reporting when the user provides an incorrect set of values for a chart.
- **Consolidation of requirements.yaml into Chart.yaml**: *The Chart dependency management system moved from `requirements.yaml` and `requirements.lock` to `Chart.yaml` and `Chart.lock`.*
- **Name (or --generate-name) is now required on install**
- **Pushing Charts to OCI Registries**
- **Removal of helm serve**
- **Library chart support**: *Helm 3 supports a class of chart called a “library chart”. This is a chart that is shared by other charts, but does not create any release artifacts of its own*. A library chart’s templates can only declare `define` elements. Globally scoped non-`define` content is simply ignored.
- **Chart.yaml apiVersion bump**
- **XDG Base Directory Support**: *In Helm 2, Helm stored all this information in `~/.helm` (affectionately known as helm home)*, which could be changed by setting the `$HELM_HOME` environment variable, or by using the global flag `--home`. In Helm 3, Helm now respects the following environment variables as per the XDG Base Directory Specification:
  - `$XDG_CACHE_HOME`
  - `$XDG_CONFIG_HOME`
  - `$XDG_DATA_HOME`
- **CLI Command Renames**:
  - `helm delete` was renamed to `helm uninstall`
  - `helm inspect` was renamed to `helm show`
  - `helm fetch` was renamed to `helm pull`
- **Automatically creating namespaces**: *In Helm 2 when creating a release in a namespace that does not exist, Helm create the namespace. Helm 3 will create the namespace if the `--create-namespace` flag is explicitly specified*.
