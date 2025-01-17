---
title: ! '[Kubernetes Basic] 쿠버네티스란?'
author: cawcaw253
date: 2022-10-19 22:26:00 +0900
categories:
  - Kubernetes
tags:
  - devops
  - kubernetes
  - basic
description: |
  요즘 많이 주목받으며 사용되고 있는 Kubernetes란 무엇인가? 기존에 사용되던 전통적인 방식에서 가상화, 그리고 컨테이너로 넘어온 과정과 각각의 특징에 대해서 정리하고 Kubernetes가 무엇이며 왜 필요하게 되었는지를 설명한 글입니다. 
published: true
---

---
# 쿠버네티스란 무엇인가

> 쿠버네티스는 컨테이너화된 워크로드와 서비스를 관리하기 위한 이식성이 있고, 확장가능한 오픈소스 플랫폼입니다. 쿠버네티스는 선언적 구성과 자동화를 모두 용이하게 해줍니다. 쿠버네티스는 크고, 빠르게 성장하는 생태계를 가지고 있으며, 쿠버네티스 서비스, 기술 지원 및 도구는 어디서나 쉽게 이용할 수 있습니다.

쿠버네티스는 전통적인 Platform as a Service(PaaS) 처럼 보이지만 쿠버네티스는 하드웨어 수준보다는 컨테이너 수준에서 운영되기 때문에, PaaS가 일반적으로 제공하는 배포, 로드 밸런싱과 같은 기능을 제공하고 사용자가 로깅, 모니터링등의 솔루션을 통합할 수 있습니다. 하지만, 쿠버네티스는 모놀리식(monolithic)이 아니어서, 앞서 말한 다양한 솔루션이 선택적이며 추가나 제거가 용이합니다. 따라서 쿠버네티스는 개발자 플랫폼을 만드는 구성 요소를 제공하며 사용자에게 더 폭 넓은 선택권과 유연성을 준 확장가능한 플랫폼입니다.

> 쿠버네티스란 명칭은 키잡이(helmsman)나 파일럿을 뜻하는 그리스어에서 유래했으며, K8s는 kubernetes의 약식 표기입니다.
{: .prompt-tip }

# 쿠버네티스까지의 변화

![쿠버네티스까지의 변화](posts/20221019/container_evolution.svg)
_ref : [https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/](https://kubernetes.io/ko/docs/concepts/overview/what-is-kubernetes/)_

쿠버네티스라는 플랫폼으로 오기까지 기존의 배포 방식은 위의 그림처럼 다양한 변화를 거쳐왔습니다. 

## Traditional Deployment

초창기에는 하나의 물리서버에서 여러 애플리케이션을 동작시켰습니다. 이러한 배포 환경에서는 여러 애플리케이션에 리소스 제한을 두지 못했으며 이로 인해서 특정 어플리케이션에 리소스가 몰리는 리소스 할당의 문제가 발생하였습니다.
또한 하나의 OS와 라이브러리를 모든 어플리케이션의 요구사항에 맞춰야했으므로 하나의 서버에 여러 애플리케이션이 동작하게 될 경우, 이로인한 버전 관리나 라이브러리등의 관리가 복잡해져 각 애플리케이션의 설계에도 영향을 미치게 되었습니다. 

## Virtualized Deployment

위의 리소스 할당, 버전 관리 문제등에 대한 해결책으로 가상화가 도입되었습니다.
가상화를 사용하면 VM간에 애플리케이션을 격리함으로서 일정 수준의 보안을 제공할 수 있으며, 서버에서 리소스를 보다 효율적으로 활용할 수 있었습니다.
또한 손쉽게 새로운 어플리케이션을 추가하거나 업데이트하는것이 가능해졌습니다.

하지만 이러한 가상화에서도 단점은 존재했습니다. 그것은 VM을 구성하기위한 하이퍼바이저의 존재와 각 VM이 무거운 OS를 기반으로 프로비저닝 된다는 점이었습니다.

## Container Deployment

2013년 도커가 컨테이너 기술을 본격적으로 퍼뜨리면서 VM에서 컨테이너 기반의 배포 방식으로 많은 변화가 생기게 되었습니다.
컨테이너는 VM과 유사하지만 격리 속성을 완화하여 애플리케이션 간에 운영체제(OS)를 공유합니다. 또한 컨테이너의 이미지 레이어를 공유하므로 일반적으로 컨테이너는 VM보다 더욱 가볍습니다.
![VM에서의 애플리케이션](posts/20221019/vm.png){: style="max-width: 48%" .left}
![컨테이너에서의 애플리케이션](posts/20221019/container.png){: style="max-width: 49%" .normal}

또한 컨테이너는 다음과 같은 추가적인 혜택을 제공하기 때문에 인기가 많습니다.
- 기민한 애플리케이션 생성과 배포: VM 이미지를 사용하는 것에 비해 컨테이너 이미지 생성이 보다 쉽고 효율적임.
- 지속적인 개발, 통합 및 배포: 안정적이고 주기적으로 컨테이너 이미지를 빌드해서 배포할 수 있고 (이미지의 불변성 덕에) 빠르고 효율적으로 롤백할 수 있다.
- 개발과 운영의 관심사 분리: 배포 시점이 아닌 빌드/릴리스 시점에 애플리케이션 컨테이너 이미지를 만들기 때문에, 애플리케이션이 인프라스트럭처에서 분리된다.
- 가시성(observability): OS 수준의 정보와 메트릭에 머무르지 않고, 애플리케이션의 헬스와 그 밖의 시그널을 볼 수 있다.
- 개발, 테스팅 및 운영 환경에 걸친 일관성: 랩탑에서도 클라우드에서와 동일하게 구동된다.
- 클라우드 및 OS 배포판 간 이식성: Ubuntu, RHEL, CoreOS, 온-프레미스, 주요 퍼블릭 클라우드와 어디에서든 구동된다.
- 애플리케이션 중심 관리: 가상 하드웨어 상에서 OS를 실행하는 수준에서 논리적인 리소스를 사용하는 OS 상에서 애플리케이션을 실행하는 수준으로 추상화 수준이 높아진다.
- 느슨하게 커플되고, 분산되고, 유연하며, 자유로운 마이크로서비스: 애플리케이션은 단일 목적의 머신에서 모놀리식 스택으로 구동되지 않고 보다 작고 독립적인 단위로 쪼개져서 동적으로 배포되고 관리될 수 있다.
- 리소스 격리: 애플리케이션 성능을 예측할 수 있다.
- 자원 사용량: 리소스 사용량: 고효율 고집적.

이러한 컨테이너의 도입으로 기존의 VM이 가진 단점을 보완하여 지금의 컨테이너 기반의 배포 환경을 구축할 수 있게 되었습니다.

# 정리

컨테이너는 애플리케이션을 포장하고 실행하는 좋은 방법입니다. 하지만, 단순히 컨테이너를 사용하는 것뿐이라면 부족합니다.
우리가 운용하고자 하는 환경에서는 애플리케이션이 가동 중인 컨테이너를 관리하고 가동 중지 시간이 없는지 확인해야 합니다. 예를 들어 컨테이너가 다운되면 다른 컨테이너를 다시 시작하는 자동화된 복구(self-healing)등의 작업이 필요합니다.

이러한 고민을 쿠버네티스가 해결해줄 수 있습니다! 쿠버네티스는 분산 시스템을 탄력적으로 실행하기 위한 프레임 워크를 제공하며 아래와 같은 기능들 또한 제공합니다.

- **서비스 디스커버리와 로드 밸런싱** 쿠버네티스는 DNS 이름을 사용하거나 자체 IP 주소를 사용하여 컨테이너를 노출할 수 있습니다. 컨테이너에 대한 트래픽이 많으면, 쿠버네티스는 네트워크 트래픽을 로드밸런싱하고 배포하여 배포가 안정적으로 이루어지게 할 수 있습니다.
- **스토리지 오케스트레이션** 쿠버네티스를 사용하면 로컬 저장소, 공용 클라우드 공급자 등과 같이 원하는 저장소 시스템을 자동으로 탑재 할 수 있습니다.
- **자동화된 롤아웃과 롤백** 쿠버네티스를 사용하여 배포된 컨테이너의 원하는 상태를 서술할 수 있으며 현재 상태를 원하는 상태로 설정한 속도에 따라 변경할 수 있습니다. 예를 들어 쿠버네티스를 자동화해서 배포용 새 컨테이너를 만들고, 기존 컨테이너를 제거하고, 모든 리소스를 새 컨테이너에 적용시킬 수 있습니다.
- **자동화된 빈 패킹(bin packing)** 컨테이너화된 작업을 실행하는데 사용할 수 있는 쿠버네티스 클러스터 노드를 제공합니다. 또한 할당 가능한 리소스를 확인하여 각 컨테이너가 필요로 하는 CPU와 메모리(RAM)를 쿠버네티스에게 지시하며, 쿠버네티스는 컨테이너를 노드에 맞추어서 리소스를 가장 잘 사용할 수 있도록 할당 해줍니다.
- **자동화된 복구(self-healing)** 쿠버네티스는 실패한 컨테이너를 다시 시작하고, 컨테이너를 교체하며, '사용자 정의 상태 검사(health check)'에 응답하지 않는 컨테이너를 죽이고, 서비스 준비가 끝날 때까지 그러한 과정을 클라이언트에 백그라운드에서 지원합니다.
- **시크릿과 구성 관리** 쿠버네티스를 사용하면 암호, OAuth 토큰 및 SSH 키와 같은 중요한 정보를 저장하고 관리 할 수 있습니다. 컨테이너 이미지를 재구성하지 않고 스택 구성에 시크릿을 노출하지 않고도 시크릿 및 애플리케이션 구성을 배포 및 업데이트 할 수 있습니다.

# 참고

- **[Kubernetes Overview]** [https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)
