# Helm Documentation

[Docs](https://helm.sh/docs/)!

Date Started: Sunday, February 28, 2021

Date Finished: TBD

## Home

- Helm is the package manager for Kubernetes. As an operating system package manager makes it easy to install tools on an OS, Helm makes it easy to install applications and resources into Kubernetes clusters.

## Introduction

### Using Helm

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

## How-To

### Chart Development Tips and Tricks

- It is always safer quoting the strings than leaving them as bare words: `name: {{ .Values.MyName | quote }}`.
- When working with integers do not quote the values: `port: {{ .Values.Port }}`.
- Go provides a way of including one template in another using a built-in `template` directive. However, the built-in function cannot be used in Go template pipelines `{{ include "toYaml" $value | indent 2 }}`.
- The `required` function gives developers the ability to declare a value entry as required for template rendering. If the entry is empty in values.yaml, the template will not render and will return an error message supplied by the developer `{{ required "A valid foo is required!" .Values.foo }}`.
- The `sha256sum` function can be used to ensure a deployment's annotation section is updated if another file changes: `checksum/config: {{ include (print $.Template.BasePath "/configmap.yaml") . | sha256sum }}` or `rollme: {{ randAlphaNum 5 | quote }}`.
- The annotation `"helm.sh/resource-policy": keep` instructs Helm to skip deleting this resource when a helm operation (such as helm uninstall, helm upgrade or helm rollback) would result in its deletion.

### Syncing Your Chart Repository

- Guide to create local and remote chart repository:
  
  1. Create a local directory and place your packaged charts in that directory.
  2. Use Helm to generate an updated index.yaml file by passing in the directory path and the url of the remote repository to the `helm repo index`.
  3. Upload the contents of the directory to your GCS bucket by running `scripts/sync-repo.sh` and pass in the local directory name and the GCS bucket name.
  4. To keep a local copy of the contents of your chart repository or use `gsutil rsync` to copy the contents of your remote chart repository to a local directory.

## Best Practices

### General Conventions

- Chart names must be lower case letters and numbers. Words may be separated with dashes (-).
- The directory that contains a chart MUST have the same name as the chart.
- YAML files should be indented using two spaces (and never tabs).

### Values

- Variable names should begin with a lowercase letter, and words should be separated with camelcase.
- YAML is a flexible format, and values may be nested deeply or flattened. In most cases, flat should be favored over nested. The reason for this is that it is simpler for template developers and users.
- Avoid type conversion errors by being explicit about strings, and implicit about everything else. Or, in short, quote all strings. And to avoid the integer casting issues, it is advantageous to store your integers as strings as well, and use `{{ int $value }}` in the template to convert from a string back to an integer.
- Every defined property in `values.yaml` should be documented. The documentation string should begin with the name of the property that it describes, and then give at least a one-sentence description.

### Templates

- The `templates/` directory should be structured as follows:
  - Template files should have the extension `.yaml` if they produce YAML output. The extension `.tpl` may be used for template files that produce no formatted content.
  - Template file names should use dashed notation e.g.`my-example-configmap.yaml`, not camelcase.
  - Each resource definition should be in its own template file.
  - Template file names should reflect the resource kind in the name. e.g. `foo-pod.yaml`, `bar-svc.yaml`.
- Defined templates (templates created inside a `{{ define }}` directive) are globally accessible. That means that a chart and all of its sub-charts will have access to all of the templates created with `{{ define }}`. For that reason, all defined template names should be namespaced.
- Templates should be indented using two spaces (never tabs).
- It is preferable to keep the amount of whitespace in generated templates to a minimum. In particular, numerous blank lines should not appear adjacent to each other. But occasional empty lines (particularly between logical sections) is fine.

### Dependencies

- Where possible, use version ranges instead of pinning to an exact version. The suggested default is to use a patch-level version match.
- Where possible, use https:// repository URLs, followed by http:// URLs. If the repository has been added to the repository index file, the repository name can be used as an alias of URL. Use `alias:` or `@` followed by repository names.
- Conditions or tags should be added to any dependencies that are optional. When multiple sub-charts (dependencies) together provide an optional or swappable feature, those charts should share the same tags.

### Labels and Annotations

- An item of metadata should be a label under the following conditions:
  - It is used by Kubernetes to identify this resource
  - It is useful to expose to operators for the purpose of querying the system.
- Helm hooks are always annotations.

### Pods and PodTemplates

- A container image should use a fixed tag or the SHA of the image. It should not use the tags *latest*, *head*, *canary*, or other tags that are designed to be "floating".
- `helm create` sets the `imagePullPolicy` to `IfNotPresent` by default.
- All `PodTemplate` sections should specify a `selector`. This is a good practice because it makes the relationship between the set and the pod.

### Role-Based Access Control

- RBAC and ServiceAccount configuration should happen under separate keys. They are separate things. Splitting these two concepts out in the YAML disambiguates them and makes this clearer.
- `rbac.create` should be a boolean value controlling whether RBAC resources are created. The default should be `true`.
- `serviceAccount.name` should be set to the name of the ServiceAccount to be used by access-controlled resources created by the chart. If `serviceAccount.create` is true, then a ServiceAccount with this name should be created.

## Chart Template Guide

### Getting Started

- Helm charts file and folder structure:
  - The `templates/` directory is for template files. *When Helm evaluates a chart, it will send all of the files in the `templates/` directory through the template rendering engine*. It then collects the results of those templates and sends them on to Kubernetes.
  - The `values.yaml` file is also important to templates. *This file contains the default values for a chart*. These values may be overridden by users during `helm install` or `helm upgrade`.
  - The `Chart.yaml` file contains a description of the chart. You can access it from within a template.
  - The `charts/` directory may contain other charts (which we call sub-charts).
- If you take a look at the `templates/` directory, you'll notice a few files already there.
  - `NOTES.txt`: The "help text" for your chart. This will be displayed to your users when they run helm install.
  - `deployment.yaml`: A basic manifest for creating a Kubernetes deployment.
  - `service.yaml`: A basic manifest for creating a service endpoint for your deployment.
  - `_helpers.tpl`: A place to put template helpers that you can re-use throughout the chart.
- The `helm get manifest` command takes a release name and prints out all of the Kubernetes resources that were uploaded to the server. Each file begins with `---` to indicate the start of a YAML document, and then is followed by an automatically generated comment line that tells us what template file generated this YAML document.
- Hard-coding the `name:` into a resource is usually considered to be bad practice. Names should be unique to a release. So we might want to generate a name field by inserting the release name. This can be achieved by using the template directive `{{ .Release.Name }}`. The `Release` object is one of the built-in objects for Helm.
- `helm install --dry-run` tests the template rendering, but not actually install anything.

## Built-in Objects

- *Objects can be simple, and have just one value. Or they can contain other objects or functions*. For example. the Release object contains several objects (like Release.Name) and the Files object has a few functions.
- `Release`: This object describes the release itself. It has several objects inside of it:
  - `Release.Name`: The release name
  - `Release.Namespace`: The namespace to be released into (if the manifest doesn’t override)
  - `Release.IsUpgrade`: This is set to true if the current operation is an upgrade or rollback.
  - `Release.IsInstall`: This is set to true if the current operation is an install.
  - `Release.Revision`: The revision number for this release. On install, this is 1, and it is incremented with each upgrade and rollback.
  - `Release.Service`: The service that is rendering the present template. On Helm, this is always Helm.
- `Values`: *Values passed into the template from the `values.yaml` file and from user-supplied files*. By default, `Values` Object is empty.
- `Chart`: *The contents of the `Chart.yaml` file*. Any data in `Chart.yaml` will be accessible here. For example `{{ .Chart.Name }}-{{ .Chart.Version }}`.
- `Files`: *This provides access to all non-special files in a chart*. While you cannot use it to access templates, you can use it to access other files in the chart.
  - `Files.Get` is a function for getting a file by name for example `.Files.Get config.ini`.
  - `Files.GetBytes` is a function for getting the contents of a file as an array of bytes instead of as a string. This is useful for things like images.
  - `Files.Glob` is a function that returns a list of files whose names match the given shell glob pattern.
  - `Files.Lines` is a function that reads a file line-by-line. This is useful for iterating over each line in a file.
  - `Files.AsSecrets` is a function that returns the file bodies as Base 64 encoded strings.
  - `Files.AsConfig` is a function that returns file bodies as a YAML map.
- `Capabilities`: *This provides information about what capabilities the Kubernetes cluster supports*.
  - `Capabilities.APIVersions` is a set of versions.
  - `Capabilities.APIVersions.Has $version` indicates whether a version (e.g., batch/v1) or resource (e.g., apps/v1/Deployment) is available on the cluster.
  - `Capabilities.KubeVersion` and `Capabilities.KubeVersion.Version` is the Kubernetes version.
  - `Capabilities.KubeVersion.Major` is the Kubernetes major version.
  - `Capabilities.KubeVersion.Minor` is the Kubernetes minor version.
- `Template`: Contains information about the current template that is being executed
  - `Template.Name`: A namespaced file path to the current template (e.g. `mychart/templates/mytemplate.yaml`)
  - `Template.BasePath`: The namespaced path to the templates directory of the current chart (e.g. `mychart/templates`).
- *The built-in values always begin with a capital letter. This is in keeping with Go's naming convention*. It is always better to use only initial lower case letters in order to distinguish local names from those built-in.

## Values Files

- The `Values` object provides access to values passed into the chart. Its contents come from multiple sources:
  - The `values.yaml` file in the chart.
  - If this is a sub-chart, the `values.yaml` file of a parent chart.
  - A values file if passed into helm install or helm upgrade with the `-f` flag (`helm install -f myvals.yaml ./mychart`).
  - Individual parameters passed with `--set` flag (such as `helm install --set foo=bar ./mychart`). The `--set` flag has a higher precedence than the default `values.yaml`.
- If you need to delete a key from the default values, you may override the value of the key to be `null`, in which case Helm will remove the key from the overridden values merge.

## Template Functions and Pipelines

- Template functions follow the syntax `functionName arg1 arg2...`.
- We can quote strings in templates by calling the `quote` function in the template directive e.g. `{{ quote .Values.favorite.drink }}`.
- One of the powerful features of the template language is its concept of **pipelines**. *Drawing on a concept from UNIX, pipelines are a tool for chaining together a series of template commands to compactly express a series of transformations*. In other words, pipelines are an efficient way of getting several things done in sequence. e.g. `{{ .Values.favorite.food | upper | quote }}`.
- One function frequently used in templates is the `default` function: `default DEFAULT_VALUE GIVEN_VALUE`. This function allows you to specify a default value inside of the template, in case the value is omitted. e.g. `{{ .Values.favorite.drink | default "tea" | quote }}`.
- The `lookup` function can be used to look up resources in a running cluster. The synopsis of the lookup function is `lookup apiVersion, kind, namespace, name -> resource or resource list`. Here are some examples:
  - `kubectl get pod mypod -n mynamespace` -> `lookup "v1" "Pod" "mynamespace" "mypod"`
  - `kubectl get namespaces` -> `lookup "v1" "Namespace" "" ""`
- When `lookup` returns an object, it will return a dictionary. This dictionary can be further navigated to extract specific values. e.g. `(lookup "v1" "Namespace" "" "mynamespace").metadata.annotations`.
- When `lookup` returns a list of objects, it is possible to access the object list via the `items` field.
- For templates, the operators (`eq`, `ne`, `lt`, `gt`, `and`, or and so on) are all implemented as functions. In pipelines, operations can be grouped with parentheses `((, and ))`.

## Template Function List

- **Logic and control flow functions**: `and`, `coalesce`, `default`, `empty`, `eq`, `fail`, `ge`, `gt`, `le`, `lt`, `ne`, `not`, and `or`.
- **String functions**: `abbrev`, `abbrevboth`, `camelcase`, `cat`, `contains`, `hasPrefix`, `hasSuffix`, `indent`, `initials`, `kebabcase`, `lower`, `nindent`, `nospace`, `plural`, `print`, `printf`, `println`, `quote`, `randAlpha`, `randAlphaNum`, `randAscii`, `randNumeric`, `repeat`, `replace`, `shuffle`, `snakecase`, `squote`, `substr`, `swapcase`, `title`, `trim`, `trimAll`, `trimPrefix`, `trimSuffix`, `trunc`, `untitle`, `upper`, `wrap`, and `wrapWith`.
- **Type conversion functions**: `atoi`, `float64`, `int`, `int64`, `toDecimal`, `toString`, `toStrings`, `toJson`, `toPrettyJson`, and `toRawJson`.
- **Regular expression functions**: `regexFind (mustRegexFind)`, `regexFindAll (mustRegexFindAll)`, `regexMatch (mustRegexMatch)`, `regexReplaceAll (mustRegexReplaceAll)`, `regexReplaceAllLiteral (mustRegexReplaceAllLiteral)`, and `regexSplit (mustRegexSplit)`.
- **Cryptographic functions**: `adler32sum`, `buildCustomCert`, `decryptAES`, `derivePassword`, `encryptAES`, `genCA`, `genPrivateKey`, `genSelfSignedCert`, `genSignedCert`, `htpasswd`, `sha1sum`, and `sha256sum`.
- **Date functions**: `ago`, `date`, `dateInZone`, `dateModify (mustDateModify)`, `duration`, `durationRound`, `htmlDate`, `htmlDateInZone`, `now`, `toDate (mustToDate)`, and `unixEpoch`.
- **URL functions**: `urlParse`, `urlJoin`, and `urlquery`.

## Community

### The History of the Project

- Helm began as what is now known as Helm Classic, a Deis project begun and introduced at the inaugural KubeCon. Then, the project merged with a GCS tool called Kubernetes Deployment Manager, and the project was moved under [Kubernetes](https://kubernetes.io/).
- **When the Helm 3 development cycle began, Tiller was removed, bringing Helm closer to its original vision of being a client tool**. But Helm 3 continues to track releases inside of the Kubernetes cluster, making it possible for teams to work together on a common set of Helm releases.

## Frequently Asked Questions

### Key differences between Helm 2 and Helm 3

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