# Hadoop Chart

This chart is modified from [stable/hadoop](https://github.com/helm/charts/tree/master/stable/hadoop) and [mgit-at/helm-hadoop-3](https://github.com/mgit-at/helm-hadoop-3) and has been updated to:

- run use multi-architecture Docker image and
- use the currently latest version of Hadoop.

This chart is primarily intended to be used for YARN and MapReduce job execution where HDFS is just used as a means to transport small artifacts within the framework and not for a distributed filesystem. Data should be read from cloud based datastores such as Google Cloud Storage, S3 or Swift.

## Chart Details

## Installing the Chart

To install the chart with the release name `hadoop`:

```bash
helm helm repo add pfisterer-hadoop https://pfisterer.github.io/apache-hadoop-helm/
helm install --name hadoop pfisterer-hadoop/hadoop
```

## Configuration

The following table lists the configurable parameters of the Hadoop chart and their default values.

| Parameter                              | Description                                                    | Default                                                           |
| -------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------- |
| `image.repository`                     | Hadoop image                                                   | `farberg/apache-hadoop`                                           |
| `image.tag`                            | Hadoop image tag                                               | `3.3.2`                                                           |
| `imagee.pullPolicy`                    | Pull policy for the images                                     | `IfNotPresent`                                                    |
| `hadoopVersion`                        | Version of hadoop libraries being used                         | `3.3.2`                                                           |
| `antiAffinity`                         | Pod antiaffinity, `hard` or `soft`                             | `hard`                                                            |
| `hdfs.nameNode.pdbMinAvailable`        | PDB for HDFS NameNode                                          | `1`                                                               |
| `hdfs.nameNode.resources`              | resources for the HDFS NameNode                                | `requests:memory=256Mi,cpu=10m,limits:memory=2048Mi,cpu=1000m`    |
| `hdfs.dataNode.replicas`               | Number of HDFS DataNode replicas                               | `1`                                                               |
| `hdfs.dataNode.pdbMinAvailable`        | PDB for HDFS DataNode                                          | `1`                                                               |
| `hdfs.dataNode.resources`              | resources for the HDFS DataNode                                | `requests:memory=256Mi,cpu=10m,limits:memory=2048Mi,cpu=1000m`    |
| `hdfs.webhdfs.enabled`                 | Enable WebHDFS REST API                                        | `true`                                                            |
| `yarn.resourceManager.pdbMinAvailable` | PDB for the YARN ResourceManager                               | `1`                                                               |
| `yarn.resourceManager.resources`       | resources for the YARN ResourceManager                         | `requests:memory=256Mi,cpu=10m,limits:memory=2048Mi,cpu=1000m`    |
| `yarn.nodeManager.pdbMinAvailable`     | PDB for the YARN NodeManager                                   | `1`                                                               |
| `yarn.nodeManager.replicas`            | Number of YARN NodeManager replicas                            | `1`                                                               |
| `yarn.nodeManager.parallelCreate`      | Create all nodeManager statefulset pods in parallel (K8S 1.7+) | `false`                                                           |
| `yarn.nodeManager.resources`           | Resource limits and requests for YARN NodeManager pods         | `requests:memory=2048Mi,cpu=1000m,limits:memory=2048Mi,cpu=1000m` |
| `persistence.nameNode.enabled`         | Enable/disable persistent volume                               | `false`                                                           |
| `persistence.nameNode.storageClass`    | Name of the StorageClass to use per your volume provider       | `-`                                                               |
| `persistence.nameNode.accessMode`      | Access mode for the volume                                     | `ReadWriteOnce`                                                   |
| `persistence.nameNode.size`            | Size of the volume                                             | `50Gi`                                                            |
| `persistence.dataNode.enabled`         | Enable/disable persistent volume                               | `false`                                                           |
| `persistence.dataNode.storageClass`    | Name of the StorageClass to use per your volume provider       | `-`                                                               |
| `persistence.dataNode.accessMode`      | Access mode for the volume                                     | `ReadWriteOnce`                                                   |
| `persistence.dataNode.size`            | Size of the volume                                             | `200Gi`                                                           |


## Development

Help is always appreciated. Please create pull requests.

### Open Issues

- Include native libraries

### Upload a new version of the chart

```bash
helm lint
helm package .
mv apache-hadoop-helm-*.tgz docs/
helm repo index docs/ --url https://pfisterer.github.io/apache-hadoop-helm/
git add docs/
git commit -a -m "Updated helm repository"
git push origin master
```

## Changes

Version 1.2.0
- Initial release of this chart
- Use multi-architecture base image
- Apache Hadoop 3.3.2
