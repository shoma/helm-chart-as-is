# as-is

helm chart renders the contents of `values.yaml` as is.

## Usage


### Basic

Create a file named `values.yaml` with the following content:

```yaml
# values.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key: value
```

To render this, run the following command:

```bash
helm template my-release . -f values.yaml
```

The output will be identical to the content of `values.yaml`:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key: value
```

## with helmfile

A usecase is with the `values.yaml.gotmpl` function of [helmfile](https://github.com/helmfile/helmfile).

This chart is particularly useful when you want to leverage `helmfile`'s Go templating (`values.yaml.gotmpl`) to generate the *entire* Kubernetes manifest dynamically, and you want that output to be rendered by Helm without any further templating interference from the chart itself.

see ./example
