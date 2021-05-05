# Helpy Helm charts (beta)

The Helpy Helm chart can be used to launch Helpy Community Edition or Helpy Pro on a Kubernetes cluster.  It is designed
to work out of the box, but for production use, some customization of the configuration values is recommended.

NOTE: This is currently a beta release and is missing certain features and could have other issues.  Please report issues and submit PRs.

## Usage

You will need a Kubernetes cluster and a system with Helm installed.  Then add the Helpy helm repository, 
using this command:

```
helm repo add helpyio https://scott.github.io/helpy-helm/
helm repo update
```

To run a Helpy Pro instance, use the following:

```helm install release-name helpyio/helpy ```

To run a Helpy CE instance, use the following:

```helm install release-name helpyio/helpy --set image.repository=helpy/helpy --set image.tag=kubernetes --set image.name=helpyce```

## Configuration
This Helm chart is intended to be highly configurable for different needs:

Configuring SSL: https://github.com/scott/helpy-helm/wiki/Enable-SSL
Variables:

| Variable                      | Description                                                   | Default Value
|-------------------------------|---------------------------------------------------------------|----------|
| image.name                    | The name of the app you are running                           | helpypro |
| image.repository              | The container to pull from for the app server.                | helpyio/helpypro |
| image.tag                     | The tag to pull                                               | latest |
| image.pullPolicy              | The container pull policy                                     | Always |
| appReplicaCount               | The number of app servers to run                              | 2 |
| workerReplicaCount            | The number of workers to run                                  | 2 |
| service.type                  | The service type                                              | NodePort |
| service.externalPort          | The port the service should accept traffic to                 | 80 |
| service.internalPort          | The port the service forwards traffic to                      | 3000   |
| ingress.enabled               | Whether or not to use the ingress.                            | true |
| ingress.annotations           | Ingress Annotations                                           | {} |
| ingress.hosts.host            | Hostname to reach the service                                 | Helpy.local |
| ingress.hosts.paths           | Path to reach the service                                     | ["/"]
| ingress.tls
| loadBalancer.enabled          | Whether or not to use a LB                                    |  true   |
| loadBalancer.annotations      | Annotations to pass to the Load Balancer                      | {} |
| Helpy Pod/Rails configurations| Use these settings to adjust how the app runs and resources expected in the pod | |
| rails.railsEnv                | The environment to run the app in                             | production |
| rails.logToStandardOut        |                                                               | true |
| rails.serveStaticFiles        | Whether or not to serve static files.                         | false |
| rails.pumaConcurency          |                                                               | '1' |
| rails.podRam                  | How much RAM to give each app POD in the cluster              | '2048' |
| rails.secretKey               | The secret key Rails uses                                     | 'your_secret_here' |
| Redis Configuration           |
| redis.url                     | The URL to your redis service                                 | 'redis://redis.default.svc.cluster.local:6379/0/cache' |
| redis.port                    | The Port to your redis service                                | '6379' |
| redis.repository              | The redis repository                                          | 'redis' |
| redis.tag                     | The tag of redis to use                                       | 4.0 |
| Postgresql configuration      |
| postgres.enabled              | Whether or not to use the Postgres container in the cluster   | true |
| postgres.repository           | The repository to get Postgres from                           | postgres |
| postgres.tag                  | The container version of Postgres to use                      | 10.7-alpine |
| postgres.capacity             | The size of the DB                                            | 24Gi |
| postgres.host                 | The host url of the Postgres service                          | postgres |
| postgres.port                 | The port to connect to the Postgres service                   | '5432' |
| postgres.db                   | The name of the Database to connect to                        | 'helpy_production' |
| postgres.user                 | The username to log in to the database                        | 'postgres' |
| postgres.password             | The password used to log in to the database                   | 'helpy' |
| Storage Configuration- Helpy looks for a S3 compatible store for attachments, images, etc. |
| storage.remote                | Whether file storage is enabled or not                        | true |
| storage.key                   | The S3 key of the file store                                  | yourkey |
| storage.secret                | The S3 secret of the file store                               | yoursecret |
| storage.region                | The region of the file store                                  | sfo2 |
| storage.endPoint              | The endPoint of the storage service                           | endpoint |
| storage.bucketName            | The bucketname where files should be stored                   | helpy-k8s |
| IMAP service- whether or not to connect to an IMAP server to fetch tickets |
| imap.enabled                  | Whether or not to fetch tickets from an IMAP box              | false |
| imap.frequencyInMinutes       | How often to check for new tickets in minutes                 | 1 |
| Helpy Pro License- You can add your license here |
| license.keyName               | This is the URL of the site you have licensed                 | yoursite.com |
| license.key                   | The license key given to you by Helpy                         | your_key |

To provide your own values, you can either make a copy of the `values.yaml` file, or pass variables in the command line
using the `-set` option. 

To customize `values.yml`, first copy `values.yaml` to `values.override.yaml` and add your customizations.  Then run to use your customizations:

```helm install -f values.override.yaml release-name helpyio/helpy```

To pass values in the command line, install using

```helm install release-name helpyio/helpy -s variable=value -s variable2=value```

## License  
Copyright 2021, Helpy.io, Inc, Scott Miller and Contributors. This Helm chart is released under
the MIT License.
