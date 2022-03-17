+++
title = "TmpToml"
description = "An overview of tmptoml and what we use it for"
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 420
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

Tmptoml is a CLI tool/Library that renders [Tera][] templates using a toml config file as the variable source. It was born out of the need for a simple templating solution to handle Kubernetes manifests. Tmptoml is designed to be fast, simple and easy to use.

# Overview

Tmptoml is built in Rust and is available on [crates.io][] as well as on our [Github Repository][]. It uses It is designed to render one template at a time with a toml config file as the variable source. The toml config file can contain multiple groups and secondary set of groups.

Tmptoml is able to use variables from the specified group as well as the variables from the secondary group. This allows you to share variables between multiple templates and then specify unique variables for a specific template file.

# Usage Example

```
./tmptoml config.toml template.yaml qa system1
```

The breakdown of the above command:

- `./tmptoml` is the TmpToml binary.
- `config.toml` is the path to the configuration file.
- `template.yaml` is the path to the template file.
- `qa` is the primary section/environment.
- `system1` is the name of the secondary section.

TmpToml then renders the template file to STDOUT, which allows you to do whatever you want with it. You can also pipe it to a file or redirect it to a variable.

## Example Config File

```toml
[qa]
nodeEnv="qa"
[qa.system1]
replicas=2
image="123.dkr.ecr.us-west-2.amazonaws.com/system1:latest"
[qa.system2]
replicas=2
image="123.dkr.ecr.us-west-2.amazonaws.com/system2:latest"
[production]
nodeEnv="production"
[production.system1]
replicas=2
image="456.dkr.ecr.us-west-2.amazonaws.com/system1:latest"
[production.system2]
replicas=2
image="456.dkr.ecr.us-west-2.amazonaws.com/system2:latest"
```

## Example Template

```yaml
kind: Service
apiVersion: v1
metadata:
  name: faktory-worker
spec:
  clusterIP: None
  selector:
    app: faktory-worker

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: faktory-worker
  labels:
    app: faktory-worker
spec:
  replicas: { { replicas } }
  selector:
    matchLabels:
      app: faktory-worker
  template:
    metadata:
      labels:
        app: faktory-worker
    spec:
      containers:
        - name: faktory-worker
          image: { { image } }
          env:
            - name: NODE_ENV
              value: { { nodeEnv } }
```

[tera]: https://crates.io/crates/tera
[crates.io]: https://crates.io/crates/tmptoml
[github repository]: https://github.com/uptech/tmptoml
