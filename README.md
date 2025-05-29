# as-is

## Basic Usage

This chart takes the content of a user-provided values file (e.g., `my-values.yaml`) and renders it directly as Kubernetes manifest(s).

### Example with a non-empty values file

Create a file named `my-values.yaml` with the following content:

```yaml
# my-values.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key: value
```

To render this, run the following command:

```bash
helm template my-release . -f my-values.yaml
```

The expected output will be identical to the content of `my-values.yaml`:

```yaml
# my-values.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key: value
```

### Example with an empty values file

Create an empty file, for example `empty-values.yaml`:

```bash
touch empty-values.yaml
```

Then, run the render command:

```bash
helm template my-release . -f empty-values.yaml
```

The expected output will be a null YAML document (e.g. `null`) or an empty document marker (e.g. `---`), because `{{- toYaml .Values }}` will be operating on an empty `.Values` map.

## Helmfile Use Case

A usecase is with the `values.yaml.gotmpl` function of [helmfile](https://github.com/helmfile/helmfile).

This chart is particularly useful when you want to leverage `helmfile`'s Go templating (`values.yaml.gotmpl`) to generate the *entire* Kubernetes manifest dynamically, and you want that output to be rendered by Helm without any further templating interference from the chart itself.

For example, in your `helmfile.yaml`:

```yaml
# helmfile.yaml
releases:
  - name: my-dynamic-manifest
    chart: ./path/to/this/as-is-chart # Or its published chart name
    values:
      - values.yaml.gotmpl # This file generates the full K8s manifest
```

In this scenario, the `as-is` chart's `templates/resource.yaml` (which contains `{{- toYaml .Values }}`) will simply take the fully formed YAML output by `values.yaml.gotmpl` and present it as the final Kubernetes resource.
