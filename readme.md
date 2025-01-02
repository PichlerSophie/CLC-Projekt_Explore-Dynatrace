# Kubernetes Monitoring and Deployment Guide

## Research Summary

### Kubernetes Monitoring
- [Dynatrace Kubernetes Monitoring Overview](https://www.dynatrace.com/technologies/kubernetes-monitoring/)
- [Dynatrace Kubernetes Setup Documentation](https://docs.dynatrace.com/docs/ingest-from/setup-on-k8s)


### Log Monitoring
- [Dynatrace Log Monitoring Overview](https://www.dynatrace.com/de/platform/log-monitoring/)

### Application Security

(To be filled with specific application security instructions.)

---

## Setting Up Kubernetes Cluster and Deployment

### Steps:

1. **Create a New Project in Google Cloud**
   - Project Name: `clc3-project-g-h-p`

2. **Add All Group Members to the Team**
   - Assign the role of `Administrator` for Kubernetes Engine Cluster.

3. **Enable GKE API**
   ```bash
   gcloud services enable container.googleapis.com --project=clc3-project-g-h-p
   ```

4. **Create a New Kubernetes Cluster**
   ```bash
   gcloud container clusters create-auto online-boutique \
       --project=clc3-project-g-h-p \
       --region=us-central1
   ```

5. **Deploy the Demo Application**
   ```bash
   kubectl apply -f ./release/kubernetes-manifests.yaml
   ```
   - Wait for the pods to be ready. This may take a few minutes.
   ```bash
   kubectl get pods
   ```

6. **Open the Project in a Browser**
   - URL: [http://35.225.80.212](http://35.225.80.212)

7. **Connect to the Cluster**
   ```bash
   gcloud container clusters get-credentials online-boutique \
       --region us-central1 \
       --project clc3-project-g-h-p
   ```

---

## Setting Up Dynatrace

### Steps:

1. **Start a Dynatrace Free Trial**

2. **Install Helm**
   - Download Helm and add it to your path variable.
   - [Helm Releases on GitHub](https://github.com/helm/helm/releases)

3. **Search for Kubernetes Monitoring**

4. **Follow the Guide to Set Up Dynatrace Monitoring**
   - Refer to the [Quickstart Guide](https://docs.dynatrace.com/docs/ingest-from/setup-on-k8s/quickstart).

5. **Follow the Screenshots Provided** (Attach screenshots here if applicable).

---


# Proposal:

[Written Proposal](proposal.md)
