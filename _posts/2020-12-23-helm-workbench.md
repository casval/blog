---
layout: post
title: workbench helm command
categories:
  - back-end
tags:
  - kubernetes
  - helm chart
---
## Helm

주로 사용하는 명령어는 아래와 같다. 

- helm ls : 현재 배포된 char의 release 정보를 알 수 있다. 
- helm ls --all : 현재 배포된 chart 외에 실패하거나 지워진 chart의 정보를 모두 볼 수 있다. 
- helm install <chart> --dry-run --debug 
  - install 실행을 시뮬레이션 해주고 chart를 rendering하여 출력해 준다.
- helm lint <chart>
  - chart의 문법 체크를 해준다.

Delete 명령어는 해당 chart의 sub chart까지 모두 delete 하므로 (관련 pod, service, ingress, ...) 주의해서 사용해야 한다.

- helm delete <release name>
  - 배포된 chart를 삭제 한다. chart는 이전 revision으로 변경되거나 delete 상태로 바뀐다.
  - 실제로 chart가 삭제가 된 상태가 아니기 때문에 release name을 재사용 불가하다.
- helm delete --purge <release name>
  - 배포된 chart를 완전히 삭제한다. revision을 모두 지워버리고 해당 이름도 삭제한다.
  - 실제 chart가 삭제된 경우로 release name을 재사용 가능.

배포시 jenkins에서 사용되는 명령어

- helm install <chart> --name=<release name> --namespace=<namespace name>

  - chart dir에 있는 Chart.yalm을 시작으로 helm chart를 rendering 한 후 rendering 된 yaml 파일을 가지고 배포를 실행한다.
  - helm ls시에 release name 으로 확인 가능
  - relelase name은 중복이 불가하다.
  - namespace를 주지 않을 경우 default namespace에 배포 된다.

-  helm upgrade <release name> <chart> --namespace=<namespace name>

  - strategy

    에 따라 배포를 실행해 준다.

    - **Recreate** deployment: mysql과 같은 경우 앱을 다시 시작한다.
    - **RollingUpdate** Deployment: rolling update를 실행한다.



## Kubectl

### pod 확인

- kubectl get pod --namespace=<namespace name>

- ```bash
  [root@dvpdbp01 ~]kubectl get pod --namespace=workbench-dev
  NAME                                          READY     STATUS    RESTARTS   AGE
  workbench-db-db-db6cb74d-l4rbj                1/1       Running   0          5h
  workbench-server-workbench-574d7957b7-xc5xw   1/1       Running   0          3h
  ```

### service 확인

- kubectl get service --namespace=<namespace name>

- ```bash
  [root@dvpdbp01 ~]kubectl get service --namespace=workbench-dev
  NAME                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
  workbench-db-db              ClusterIP   10.108.29.208   <none>        3306/TCP   5h
  workbench-server-workbench   ClusterIP   10.103.165.47   <none>        80/TCP     5h
  ```

### ingress 확인

- kubectl get ingerss --namespace=<namespace name>

- ```bash
  [root@dvpdbp01 ~]kubectl get ingress --namespace=workbench-dev
  NAME                                            HOSTS                          ADDRESS   PORTS     AGE
  workbench-dev.dep.deppaas.io-workbench-server   workbench-dev.dep.deppaas.io             80        5h
  ```

### secret 확인

- kubectl get secrets --namespace=<namespace name>

- ```bash
  [root@dvpdbp01 ~]kubectl get secret --namespace=workbench-dev
  NAME                         TYPE                                  DATA      AGE
  default-token-nv525          kubernetes.io/service-account-token   3         5h
  redii-workbench              kubernetes.io/dockercfg               1         5h
  workbench-db-db              Opaque                                1         5h
  workbench-server-workbench   Opaque                                2         5h
  ```

### pod에 접속

- kubectl exec -it <pod name> --namespace=<namespace name> – /bin/bash

- ```bash
  [root@dvpdbp01 ~]kubectl exec -it workbench-server-workbench-574d7957b7-xc5xw  --namespace=workbench-dev -- /bin/bash
  [root@workbench-server-workbench-574d7957b7-xc5xw server]# ls
  README.md       app-installone.js  auth-install.js    buildjm  delete_tables.sql  fs-install.js    node_modules  proxy  uninstall.js
  app             app-uninstall.js   auth-uninstall.js  common   emul               fs-uninstall.js  notify        test   unit-manager.js
  app-install.js  auth               build              conf     fs                 install.js       package.json  tests  workbench_tables.sql
  ```

### 상세 보기

- kubectl describe <pod | service | ingress | secret | pvc | pv> <name> --namespace=<namespace name>

- ```bash
  [root@dvpdbp01 ~]kubectl describe service workbench-server-workbench --namespace=workbench-dev
  Name:              workbench-server-workbench
  Namespace:         workbench-dev
  Labels:            app=workbench-server-workbench
                     chart=workbench-0.0.1
                     heritage=Tiller
                     release=workbench-server
  Annotations:       <none>
  Selector:          app=workbench-server-workbench,tier=server
  Type:              ClusterIP
  IP:                10.103.165.47
  Port:              http  80/TCP
  TargetPort:        8080/TCP
  Endpoints:         10.34.0.4:8080
  Session Affinity:  None
  Events:            <none>
  ```

### Name space 생성

- kubectl create namespace <namespace name>

### Image secret 생성 (docker registry auth)

- kubectl create secret **docker-registry** redii-workbench --docker-server="[server_url](http://sds.redii.net/)" --docker-username="dep.dev" --docker-password="test123@2" --docker-email="user-id@mail.addr" --namespace=workbench-dev

