---
layout: post
title: "Minikube에서 docker driver를 사용했을 때 실행방법"
date: 2021-05-17 14:58:08 +0900
categories: k8s
comments: true 
---

`Minikube`를 사용할 때 기존에 `Docker Desktop`이 설치되어 있을 경우 `driver`의 기본값으로 `docker driver`가 설정된다.

![Minikube-1](https://github.com/mkshin96/mkshin96.github.io/blob/master/images/Minikube-1.png?raw=true)

이 때, `kubectl apply -f {이름}.yml`  적용 후 `minukube` 의 `ip` +  `k8s`의 `NodePort`의 `port`가 동작하지 않았다.

![Minikube-2](https://github.com/mkshin96/mkshin96.github.io/blob/master/images/Minikube-4.png?raw=true)													

![Minikube-3](https://github.com/mkshin96/mkshin96.github.io/blob/master/images/Minikube-2.png?raw=true)

(위의 경우 192.168.49.2:30214가 동작할 줄 알았다.)

`TCP` 연결 상태를 확인해보니 `SYS_SENT` 였고, 원인을 몰라 검색 도중 아래와 같이 실행하면 되는 것을 알게되었다.

~~~bash
minikube service {동작시킬 서비스명}
~~~

그리고 `minikube`의 `driver`를 `docker driver`가 아닌 `hyperkit`으로 설정할 경우 `minukube` 의 `ip` +  `k8s`의 `NodePort`의 `port`가 잘 동작한다.

~~~bash
# 앞서 실행시킨 minikube 삭제
minikube delete

# driver를 hyperkit으로 설정
minikube start --driver=hyperkit
~~~

![Minikube-4](https://github.com/mkshin96/mkshin96.github.io/blob/master/images/Minikube-3.png?raw=true)

(driver가 hyperkit으로 설정되었다.)

