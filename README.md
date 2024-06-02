GitOps Config Repository
This repository contains the configuration files and Helm charts for managing Kubernetes applications using GitOps with Argo CD.

Overview
This project demonstrates a GitOps approach for managing Kubernetes applications. It includes the following components:

Argo CD: For continuous deployment.
Helm: For managing Kubernetes applications.
Bitnami Sealed Secrets: Tool for managing sensitive information securely.
EFK Stack (Elasticsearch, Fluent Bit, Kibana): For logging and monitoring.
Prometheus stack: For application monitoring.
Pre-requisites
A Kubernetes Cluster
GitLab account
Argo CD installed on the Kubernetes cluster
Helm installed on your local machine
Sealed Secrets controller installed on the Kubernetes cluster
Setup
Clone the repository:

sh
Copy code
git clone https://gitlab.com/orondevops1/gitops-config.git
cd gitops-config
Set up Argo CD:

Apply the app-of-apps.yaml to set up Argo CD with the App-of-Apps pattern.

sh
Copy code
kubectl apply -f app-of-apps.yaml
Configure Helm charts:

Ensure the values.yaml files for each Helm chart are configured according to your environment.

The Helm charts are located in the infra and carapp directories.

Deploying Applications
Deploying the Car App:
Ensure the car-app.yaml and related Helm charts are configured.
Push the configuration to GitLab.
Argo CD will automatically synchronize and deploy the application.
Managing Secrets
Encrypting Secrets:
Use kubeseal to encrypt your secrets before committing them to the repository.

sh
Copy code
kubeseal --format=yaml --cert=public-key-cert.pem <./infra/carapp/templates/secret.yaml >./infra/carapp/templates/sealed-secret.yaml
Example Secret:
yaml
Copy code
apiVersion: bitnami.com/v1alpha1
kind: SealedSecret
metadata:
  name: car-app-mongodb-secret
  namespace: car-app
spec:
  encryptedData:
    ROOT_PASSWORD: AgBkxVa/dGNHhZXzB7CV/b9rBxvuokZZ2xXQiNjHnCL6HU3ixf1XxFa7GdxZGn20C8mRElLL+sGUl2GaMe/Pm9W5qeWi+YdRkiKRkWMksgj2NnsePl9qWnhbPdc0eSkiqjRpamAJqJyeMPBow+83Zm09jU9qcABRaCIWy/fdsPzWEW43aEOaWDCpZVwmOKn3MjudhPWVCbaeep98A7V+XfJwsR3wrenWittDaYwKqIFq9mobwMlYX8JCXfaYa9T9WMf93STac2VsgpZKNojTvxaJvjRTm5NDyJbsKaV0QPjkwSWc7LW04xrI9iGgHhs0TMZ9+gjieXuiTn0h3MyHa35RxAlX63FMhi2e1capk28PBvrzkNellIv8StlGETmBOnali6alpy7rH9eYJKoLyc3WoL3bIEZoCmZkRNnpwDucNKnjCVvmAhadfKyHzRGEy7BC+Usg0OiwPo5gDX42VpAlDgpPKIN6MFpuBgDuK03Ke6J6z8Lw651fAzmpLtetjHlz2kb2dygV7ILHzFkZ4bwV8X+5rDE2jAtJGs+8UzJHmgDhxY/pqbtrR+VttHFfiXUIcPfu11IE4j4oeZFlKSaJqJXsFbhBvz8A5sEcIhnsLnXcAjneP7u1kORl6RpJiMpdN6u6BEWejMBmHFD4wKMdnuzK63nmfICVT6rE4v030zSWiuCv0rpxbeutABQPUMQ1Ss4R
    ROOT_USER: AgAyUZZHMGA+llNhLoFrGQP45xBOIVpEU+mRGXVxgGEXkOKC28FEuYXYuRsQBhHHna70qUJx1GlCHR1YQSZE6qw6SBuuxFPVZ1YlgOtlL8fnwFLmxyCGlK8ZWgjbUwhq6clhHIhEKkteDnMH2Y5vcMGV63bmxWJ6+H+cPQtO08yhe3Sylrg4Zhd+k6o/nV4YjQio4P5XusJALCG0IFwK2wZWJ1bXQ9PBwXjmYjytD2EbQa9cvcdesKGOjrncU5a7kYSGOFL9CmObtkeVwmvQCdy+PBwWcocGijtcOgH00OjP2T6mA9LpQA3+eD0AzwPw/Wvmwm4SH+xXFOjlxhd/0RB3MfttxniPZgQzmaB6RyOah66F1XmPOd0XUEtaW1awEJQhT9WUybHGtmi7UQD02bHje6wLDFgQIA1yhPm15wlIbeZZi42iQx58mLcHpecUS271E2SNRtB5UovZKrNChghcolfreexe/oUHoP5LLiUDTbdjv1ab4wyzLYftqM/vjWCE1JMuJQ3QP4uqnqwC867wLHp15d+ihFir4GgmypROXcRSYQOEqrsRgWVs9oXP87rZbNg8GfLQ8rSd+KnMX+gdZiZXi5m2ub5ObCfq+UYnrxhk4WCZqXFYgFEvALRP7fDUvKjqjy6qlRZh0X48JvSYatz04Y9yXHupNS4NdANPCEs5FD5M2ewKM/G4kVTzfRWRlOED
  template:
    metadata:
      name: car-app-mongodb-secret
      namespace: car-app
Monitoring and Logging
EFK Stack:
Ensure Fluent Bit, Elasticsearch, and Kibana are deployed and configured.
Fluent Bit configuration (fluentbit-values.yaml) should point to the correct log paths.
Prometheus:
Ensure Prometheus is deployed and configured for application metrics.
Use the prometheus-values.yaml file to configure Prometheus.
Troubleshooting
Check Argo CD for application sync and health status.
Check logs for any errors in application deployment and execution.
Use kubectl commands to inspect resources and debug issues.
sh
Copy code
kubectl logs -n <namespace> <pod-name>
kubectl describe pod -n <namespace> <pod-name>
Contributing
Fork the repository
Create a new branch (git checkout -b feature-branch)
Commit your changes (git commit -am 'Add new feature')
Push to the branch (git push origin feature-branch)
Create a new Pull Request
License
This project is licensed under the MIT License.

