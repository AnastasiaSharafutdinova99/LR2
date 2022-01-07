Технологии разработки программного обеспечения

Лабораторная работа №2: создание кластера Kubernetes и деплой приложения

Шарафутдинова Анастасия Сергеевна МБД2131

Целью лабораторной работы является знакомство с кластерной архитектурой на примере Kubernetes, а также деплоем приложения в кластер

deployment.yaml

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-deployment
    spec:
      replicas: 10
      selector:
        matchLabels:
          app: my-app
      strategy:
        rollingUpdate:
          maxSurge: 1
          maxUnavailable: 1
        type: RollingUpdate
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
            - image: simpleapi:latest
              # https://medium.com/bb-tutorials-and-thoughts/how-to-use-own-local-doker-images-with-minikube-2c1ed0b0968
              # указыаает на то, что образы нужно брать только из локального registry. В продакшене никогда не использовать
              imagePullPolicy: Never 
              name: simpleapi
              ports:
                - containerPort: 8080
          hostAliases:
          - ip: "192.168.49.1" # The IP of localhost from MiniKube
            hostnames:
            - postgres.local
            
service.yaml

        apiVersion: v1
        kind: Service
        metadata:
          name: my-service
        spec:
          type: NodePort
          ports:
            - nodePort: 31317
              port: 8080
              protocol: TCP
              targetPort: 8080
          selector:
            app: my-app

Обзор созданного кластера
https://drive.google.com/file/d/1FkDkBK7qe0qM_mLuS1O66-_RtyuSYXe2/view?usp=sharing
