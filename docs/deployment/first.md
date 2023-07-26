---
layout: page
#title: About
#permalink: /test.h
---

# 프로젝트 배포하기

: 레일스 프로젝트를 서버로 배포하기는 생각보다는 쉽지 않다. 그러나 `Capistrano`를 이용하면 레일스 프로젝트의 배포를 자동화할 수 있어 전체적인 배포의 관리운영이 매우 용이하다. 최근에는 도커를 이용한 레일스 프로젝트의 손쉬운 배포가 시도되고 있어 조만간 레일스 포로젝트의 배포에 대한 부담이 줄어들 것으로 생각한다.

### Capistrano란?

루비로 작성된 원격 서버 배포 자동화 도구로서, 레일스 뿐만 아니라 다른 언어로 작성된 코드도 쉽게 배포할 수 있다. 최근에는 버전 3가 릴리스되어 기존의 Capistrano 전용 DSL의 사용을 중지하고 `rake` DSL로만 `task`를 작성할 수 있도록 하였다.


---

_**References:**_

1. [capistranorb.com](http://capistranorb.com)
