# repo


apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
- ../airflow-helm-chart/charts/airflow

namespace: airflow

patches:
- patch: |-
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: airflow-webserver
    spec:
      template:
        spec:
          containers:
          - name: airflow-webserver
            env:
            - name: LOAD_EXAMPLES
              value: "yes"
  target:
    kind: Deployment
    name: airflow-webserver
- patch: |-
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: airflow-scheduler
    spec:
      template:
        spec:
          containers:
          - name: airflow-scheduler
            env:
            - name: SCHEDULER_RUNS_PER_SECOND
              value: "2"
  target:
    kind: Deployment
    name: airflow-scheduler

