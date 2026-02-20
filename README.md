# LGTM Stack for Coolify

This repository contains the configuration for a complete LGTM (Loki, Grafana, Tempo, Mimir/Prometheus) observability stack, designed to be deployed on [Coolify](https://coolify.io).

It includes services for log aggregation, metrics, traces, and visualization, with a secure, public-facing endpoint for telemetry data ingestion.

## Services Included

### Core Stack
*   **Grafana:** The central dashboard for visualizing all your logs, metrics, and traces.
*   **Loki:** For collecting and querying logs.
*   **Tempo:** For collecting and querying traces.
*   **Prometheus:** For collecting and querying metrics.
*   **Alloy:** The collector that receives OpenTelemetry data (logs, metrics, traces) and forwards it to the appropriate backend service.
*   **PostgreSQL:** The database backend for Grafana.

### Monitoring Services
*   **cAdvisor:** Collects detailed container metrics from the host.
*   **Node Exporter:** Collects system-level metrics from the host.
*   **Nginx:** A reverse proxy used internally.

## Setup Instructions

1.  **Clone the Repository:**
    ```bash
    git clone <your-repository-url>
    cd automatearmy-lgtm-stack
    ```

2.  **Configure Environment Variables:**
    *   Rename the `.env.example` file to `.env`.
    *   Open the `.env` file and fill in the required values.
    *   **`SERVICE_FQDN_*`**: Set the public URLs for Grafana and Alloy. These will be the domains you point to your Coolify instance.
    *   **Passwords**: Set strong passwords for Grafana and PostgreSQL.
    *   **GCS Credentials**: Fill in your Google Cloud Storage bucket name and the full JSON for your service account key.
    *   **`ALLOY_BASIC_AUTH`**: Generate a credential string for your public Alloy endpoint using the `htpasswd` utility:
        ```bash
        # Example for user 'my-app' and password 'super-secret'
        htpasswd -nbB my-app super-secret
        ```
        Copy the entire output (e.g., `my-app:$2y$...`) and paste it as the value for `ALLOY_BASIC_AUTH`.

3.  **Deploy to Coolify:**
    *   Point your Coolify instance to this repository.
    *   Coolify will detect the `docker-compose.yml` file and automatically deploy the entire stack.
    *   Ensure your domain's DNS records are pointing to your Coolify server's IP address.

## Endpoints

*   **Grafana Dashboard:** `https://your-grafana-domain.com` (as defined in `.env`)
*   **Prometheus UI:** `http://<your-server-ip>:9090`
*   **cAdvisor UI:** `http://<your-server-ip>:8081`
*   **Alloy OTLP Ingestion Endpoint:** `https://your-alloy-domain.com` (as defined in `.env`)
    *   **Logs:** `/v1/logs`
    *   **Metrics:** `/v1/metrics`
    *   **Traces:** `/v1/traces`

    *Note: The Alloy endpoint is protected by basic authentication.*