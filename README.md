# Решение домашнего задания к занятию «Сетевое взаимодействие в K8S. Часть 1»
https://github.com/netology-code/kuber-homeworks/blob/main/1.4/1.4.md
## Задание 1. Создать Deployment и обеспечить доступ к контейнерам приложения по разным портам из другого Pod внутри кластера
### 1. Манифест деплоймента
```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: delpoyment1
  labels:
    task: one
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-multitool
  template:
    metadata:
      labels:
        app: nginx-multitool
    spec:
      containers:
        - name: nginx
          image: nginx:1.24.0
          ports:
            - name: web
              containerPort: 80
        - name: multitool
          image: praqma/network-multitool:alpine-extra
          env:
            - name: HTTP_PORT
              value: "1180"
          ports:
            - name: http
              containerPort: 1180
              protocol: TCP
```
### 2. Манифест сервиса
```
---
apiVersion: v1
kind: Service
metadata:
  name: svc1
spec:
  selector:
    app: nginx-multitool
  type: ClusterIP
  ports:
    - name: nginx
      port: 9001
      targetPort: 80
    - name: multitool-http
      port: 9002
      targetPort: 1180
```
### 3. Манифест пода
```
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-multitool
  labels:
    app: pod-multitool
spec:
  containers:
    - name: multitool
      image: praqma/network-multitool:alpine-extra
```
### 4. Скриншот curl
![image](https://github.com/user-attachments/assets/05677ef4-43e9-4fdd-af92-458389e92a37)

