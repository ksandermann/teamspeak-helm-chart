# Helm Chart for Teamspeak 3 Server

[Teamspeak](https://www.teamspeak.com) is a voice and chat tool for teams providing voice channel features.

## TL;DR;

```bash
helm upgrade teamspeak \
    ./chart \
    --install \
    --namespace default \
    -f ./chart/values.yaml
```

## Installing the chart
The [configuration](#configuration) section lists
the parameters that can be configured during installation.

You can specify each parameter using the `--set key=value[,key=value]`
argument to `helm install`. This is not recommended - settings should be
set inside a `values.yaml` file that can be placed into an scm like git.

Recommended installation:

```bash
kubectl config set-context $(kubectl config current-context) --namespace=default

helm upgrade ts3 \
    ./chart \
    --install \
    --namespace default \
    -f values.yaml \
    --dry-run

helm upgrade ts3 \
    ./chart \
    --install \
    --namespace default \
    -f values.yaml

kubectl get pods
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm del --purge my-release
```

The command removes all the Kubernetes components associated with the chart
and deletes the release.

## Configuration

The following table lists the configurable parameters of the chart and their default
values.

| Parameter                   | Description                                               | Default         |
|-----------------------------|-----------------------------------------------------------|-----------------|
| `image.repository`          | Image repository                                          | `teamspeak`     |
| `image.tag`                 | Image tag                                                 | `3.12.0`        |
| `image.pullPolicy`          | Image pull policy                                         | `Always`        |
| `image.pullSecret`          | Specify image pull secrets                                | `nil`           |
| `pod.annotations`           | Specify annotations for the pod                           | `nil`           |
| `service.type`              | Specify the service type - only LB and NodePort supported | `LoadBalancer`  |
| `service.nodePort`          | Specify the service nodePort                              | `30987`         |
| `resources`                 | Kubernetes resources                                      | `nil`           |
| `nodeSelector`              | Kubernetes nodeSelectors for the deployment               | `nil`           |
| `tolerations`               | Kubernetes tolerations for the deployment                 | `nil`           |
| `affinity`                  | Kubernetes affinities for the deployment                  | `nil`           |
| `persistence.enabled`       | Enable/Disable persistence                                | `disabled`      |
| `persistence.accessMode`    | Accessmode for the PVC                                    | `nil`           |
| `persistence.existingClaim` | Name of an existing PVC to use                            | `nil`           |
| `persistence.annotations`   | Annotations for the PVC                                   | `nil`           |
| `persistence.storageClass`  | StorageClass for the PVC                                  | `nil`           |
| `persistence.storageSize`   | Size of the PVC                                           | `nil`           |

## Accessing the server
After deploying the helm chart, you will see instructions to access your server in the commandline's stdout.

## Known Restrictions
1. This chart currently only supports the UDP ports used for TS3. This means you can access and manage only the single
default virtual server inside the Teamspeak server. If you want to manage multiple server, you can simply deploy this
chart multiple times.
