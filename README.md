# Ansible Roles for Monitoring and Logging Setup

This repository contains four Ansible roles to automate the deployment of a monitoring and logging stack consisting of Prometheus, Node Exporter, Elasticsearch, Kibana, and FluentBit on a server.

## Roles Overview

1. **Prometheus Role**
   - **Purpose**: Install and configure Prometheus for monitoring external hosts.
   - **Tasks**:
     - Install Prometheus from the official repository.
     - Create required users, directories with proper permissions.
     - Configure Prometheus to monitor itself.
     - Manage Prometheus as a systemd service.
   - **Handlers**:
     - Reload Prometheus configuration when the configuration file is updated.
   - **Variables**:
     - `prometheus_version`: Version of Prometheus to install.
     - `prometheus_config_dir`: Directory for Prometheus configuration files.
     - `prometheus_service_name`: Name of the Prometheus systemd service.
   - **Templates**:
     - `prometheus.yml.j2`: Main configuration file for Prometheus.
   - **Files**:
     - `prometheus.service`: systemd service file for Prometheus.

2. **Node Exporter Role**
   - **Purpose**: Install and configure Node Exporter for gathering metrics from the server and adding it to Prometheus configuration.
   - **Tasks**:
     - Install Node Exporter from the official repository.
     - Create required users and directories with proper permissions.
     - Add the server as a target in the Prometheus configuration (remote or local).
     - Manage Node Exporter as a systemd service.
     - Ensure Prometheus reloads its configuration when a new target is added.
   - **Variables**:
     - `node_exporter_version`: Version of Node Exporter to install.
     - `node_exporter_service_name`: Name of the Node Exporter systemd service.
   - **Templates**:
     - `node_exporter.yml.j2`: Prometheus target configuration for Node Exporter.
   - **Files**:
     - `node_exporter.service`: systemd service file for Node Exporter.

3. **Elasticsearch & Kibana Role**
   - **Purpose**: Install and configure Elasticsearch and Kibana for log storage and visualization.
   - **Tasks**:
     - Install Elasticsearch and Kibana from the official repositories.
     - Create required users and directories with proper permissions.
     - Manage Elasticsearch and Kibana as systemd services.
   - **Variables**:
     - `elasticsearch_version`: Version of Elasticsearch to install.
     - `kibana_version`: Version of Kibana to install.
     - `elasticsearch_service_name`: Name of the Elasticsearch systemd service.
     - `kibana_service_name`: Name of the Kibana systemd service.
   - **Files**:
     - `elasticsearch.service`: systemd service file for Elasticsearch.
     - `kibana.service`: systemd service file for Kibana.

4. **FluentBit Role**
   - **Purpose**: Install and configure FluentBit to collect system logs from `journald` and send them to Elasticsearch.
   - **Tasks**:
     - Install FluentBit from the official repository.
     - Create required users and directories with proper permissions.
     - Configure FluentBit to collect logs from `journald` and forward them to Elasticsearch.
     - Manage FluentBit as a systemd service.
   - **Variables**:
     - `fluentbit_version`: Version of FluentBit to install.
     - `fluentbit_service_name`: Name of the FluentBit systemd service.
     - `elasticsearch_host`: The hostname or IP address of the Elasticsearch server to send logs to.
     - `fluentbit_conf_dir`: Directory for FluentBit configuration.
   - **Templates**:
     - `fluentbit.conf.j2`: Configuration file for FluentBit.
   - **Files**:
     - `fluentbit.service`: systemd service file for FluentBit.

## Prerequisites

- Ansible 2.9+
- Access to a server running Ubuntu or other supported Linux distribution.
- Ensure Python is installed on the target hosts.

## Best Practices

- All roles are designed to be idempotent and use Ansible handlers to restart or reload services only when necessary.
- Variables are placed in defaults/main.yml to allow easy customization without modifying the roles.
- Configuration templates for Prometheus, FluentBit, and other services are written using Jinja2 to allow flexibility and customization.