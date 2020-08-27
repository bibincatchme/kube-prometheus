### Kubernetes Operator
Operators are Kubernetes-specific applications (pods) that configure, manage and optimize other Kubernetes deployments automatically. 

A Kubernetes Operator might be able to:
  * Install and provide same initial configuration and sizing for your deployment, according to the specs of your Kubernetes cluster.
  * Perform live reloading of deployments and pods to accommodate for any user-requested parameter modification (hot config reloading).
  * Automatically scale up or down according to performance metrics.
  * Perform backups, integrity checks or any other maintenance task.
 
Kubernetes Operators make extensive use of Custom Resource Definitions (or CRDs) to create context-specific entities and objects that will be accessed like any other Kubernetes API resource.
The Prometheus Operator for Kubernetes provides easy monitoring definitions for Kubernetes services, deployment and management of Prometheus instance
These include:
Prometheus, which defines the desired Prometheus deployment. The Operator ensures at all times that a deployment matching the resource definition is running.
ServiceMonitor, which declaratively specifies how groups of services should be monitored. The Operator automatically generates Prometheus scrape configuration based on the definition. The Prometheus resource connects to ServiceMonitors using a serviceMonitorSelector field. This way Prometheus sees what targets (apps) have to be scrapped.
PrometheusRule, which defines a desired Prometheus rule file, which can be loaded by a Prometheus instance containing Prometheus alerting and recording rules.
Alertmanager, which defines a desired Alertmanager deployment. The Operator ensures at all times that a deployment matching the resource definition is running.
