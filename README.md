### Kubernetes Operator
Operators are Kubernetes-specific applications (pods) that configure, manage and optimize other Kubernetes deployments automatically. 

A Kubernetes Operator might be able to:
  * Install and provide same initial configuration and sizing for your deployment, according to the specs of your Kubernetes cluster.
  * Perform live reloading of deployments and pods to accommodate for any user-requested parameter modification (hot config reloading).
  * Automatically scale up or down according to performance metrics.
  * Perform backups, integrity checks or any other maintenance task.
 
Kubernetes Operators make extensive use of Custom Resource Definitions (or CRDs) to create context-specific entities and objects that will be accessed like any other Kubernetes API resource. 

### Prometheus Operator
The *Prometheus Operator* for Kubernetes provides easy monitoring definitions for Kubernetes services, deployment and management of Prometheus instance

These include:
  * **Prometheus**, which defines the desired Prometheus deployment. The Operator ensures at all times that a deployment matching the resource definition is running.
  * **ServiceMonitor**, which declaratively specifies how groups of services should be monitored. The Operator automatically generates Prometheus scrape configuration based on the definition. The Prometheus resource connects to ServiceMonitors using a serviceMonitorSelector field. This way Prometheus sees what targets (apps) have to be scrapped.
  * **PrometheusRule**, which defines a desired Prometheus rule file, which can be loaded by a Prometheus instance containing Prometheus alerting and recording rules.
  * **Alertmanager**, which defines a desired Alertmanager deployment. The Operator ensures at all times that a deployment matching the resource definition is running.


### How it Works
The core idea of the Operator is to decouple deployment of Prometheus instances from the configuration of which entities they are monitoring. For that purpose two third party resources (TPRs) are defined: Prometheus and ServiceMonitor.

The Operator ensures at all times that for each Prometheus resource in the cluster a set of Prometheus servers with the desired configuration are running. This entails aspects like the data retention time, persistent volume claims, number of replicas, the Prometheus version, and Alertmanager instances to send alerts to. Each Prometheus instance is paired with a respective configuration that specifies which monitoring targets to scrape for metrics and with which parameters.

The user can either manually specify this configuration or let the Operator generate it based on the second TPR, the ServiceMonitor. The ServiceMonitor resource specifies how metrics can be retrieved from a set of services exposing them in a common way. A Prometheus resource object can dynamically include ServiceMonitor objects by their labels. The Operator configures the Prometheus instance to monitor all services covered by included ServiceMonitors and keeps this configuration synchronized with any changes happening in the cluster.


### Prometheus Pod 
3 containers are launched in the Prometheus Pod:
  1. Prometheus
  2. prometheus-config-reloader – an add-on to prometheus that monitors changes in prometheus.yaml and an HTTP request reloads the prometheus configuration
  3. rules-configmap-reloader – monitors changes in the alerts file and also reloads prometheus
  
      **Service Monitors** Processing Principle:
        * Prometheus Operator subscribes to Service Monitor resource events, monitoring their addition, removal or modification.
        * Based on ServiceMonitors, the prometheus generates part of the configuration file and stores the secret in kubernetes
        * From kubernetes secret config gets into under
        * Changes discovered by prometheus-config-reloader and reload prometheus
        * Prometheus reloads configuration after reboot  and then collects new metrics according to this logic
### Alertmanager Pod
As in the case with prometheus, 2 containers  run in one pod:
 1. alertmanager
 2. config-reloader – add-on to alertmanager which monitors changes and reloads alert manager via HTTP request

### Grafana Pod
  1. Grafana
  2. Grafana-sc-dashboard – an add-on to grafana which will subsribe to ConfigMaps resources and generate json dashboards for Grafana based on them
