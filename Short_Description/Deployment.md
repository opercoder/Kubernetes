Example of yaml file:  
``` bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: zabbix-db
  namespace: dev
spec:
  replicas: 1
  minReadySeconds: 10
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
    type: RollingUpdate
  selector:
    matchLabels:
      app: zabbix-db
  template:
    metadata:
      name: zabbix-db
      labels:
        type: monitoring
        app: zabbix-db
        env: dev
      annotations:
        application: "PostgreSQL-14 with TimescaleDB for Zabbix"
    spec:
      containers:
      - name: zabbix-db
        image: registry.sirius.mix/timescaledb_for_zabbix
        env:
        - name: POSTGRES_USER
          value: "user"
        - name: POSTGRES_PASSWORD
          value: "password"
        readinessProbe:
          tcpSocket:
            port: 5432
          initialDelaySeconds: 30
          periodSeconds: 30
```
1. **Create a rollout with a record in a history.**
``` bash
kubectl apply -f zabbix-db.yaml --record
```
2. **Show a status of the rollout.**
``` bash
kubectl rollout status deployment zabbix-db -n dev
```
3. **Roll back the rollout.**
``` bash
kubectl rollout undo deployment zabbix-db -n dev
```
4. **Show a history of rollouts.**
``` bash
kubectl rollout history deployment zabbix-db -n dev
```
5. **Roll back to the defined rollout.**
``` bash
kubectl rollout undo deployment zabbix-db --to-revision=5 -n dev
```
6. **Pause/resume the rollout.**
``` bash
kubectl rollout/resume pause deployment zabbix-db -n dev
```
