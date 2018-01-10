---
title: 2018년에 더 나은 Node.js App을 만들기 위한 8가지 팁
date : 2018-01-10 19:00:00
author : 황희정
---

# 2018년에 더 나은 Node.js App을 만들기 위한 8가지 팁

2018에 Node.js 개발자들이 해야할 팁들을 소개하겠습니다.

해당글은 번역 글 입니다. 원문은 포스팅 맨 아래 링크에서 확인하실 수 있습니다.



## Tip #1: async - await 를 사용하세요.

Async - await는 Node.js 8에서 소개되었습니다. 비동기 이벤트를 어떻게 처리할지, 이전에 코드 기반에 비해서 간소화하였습니다. async-awit에 관한 자세한 내용은 다음 링크를 참고 하세요.

- Mastering Async Await in Node.js :  https://blog.risingstack.com/mastering-async-await-in-nodejs/
- Node Hero - Understanding Async Programming in Node.js : https://blog.risingstack.com/node-hero-async-programming-in-node-js/



## Tip #2: import and import()에 익숙해지세요

ES 모듈은 이미 transpilers 와 @std/esm library에서 널리 사용하고 있습니다. 이것은 Node.js 8.5 부터 `--experimental-modules` 옵션을 통해서 내장적으로 사용할 수 있지만, 실무(production)에서 사용하려면 여전히 기다리긴 해야합니다. 

- Using ES modules natively in Node.js : http://2ality.com/2017/09/native-esm-node.html

## Tip #3: HTTP/2와 친숙해지세요

Node.js 8.8에서 옵션없이 HTTP/2 를 사용할 수 있습니다. server push와 multiplexing을 가지고 있어서 브라우저에서 네이티브 모듈을 로딩할 때 효과적입니다. Koa나 Hapi와 같은 프레임워크는 부분적으로 지원하고, Express나 Meteor는 지원합니다.

HTTP/2는 아직 Node.js에서는 실험적이지만, 2018년에는 많은 라이브러리에서 널리 채택될 것으로 예상됩니다.

* HTTP/2 Server Push with Node.js : https://blog.risingstack.com/node-js-http-2-push/

## Tip #4: 논란이 있는 코드 스타일을 제거하세요.

Prettier는 2017년에 대단히 인기가 있었습니다. Prettier는 단순한 스타일의 경고 메시지 대신에 코드를 변환하는 엄격한 코드 포맷팅을 적용합니다. 하지만, no-unused-vars나 no-implicit-globals 같은 코드는 자동으로 치환해주지 않는 코드 퀄리티 에러는 아직 존재합니다.

결국, 새로하는 프로젝트에는 Prettier와 예전의 좋은 linter 를 함께 써야합니다.

- Prettier : https://github.com/prettier/prettier

## Tip #5: Secure your Node.js applications

매년 새롭게 발견되는 취약점과, 보안을 위배하는 것들이 있고, 2017년 역시 예외가 아니었습니다. 보안은 이슈로 급격하게 바뀌고, 절대 무시할 수 없습니다.  Node.js security를 시작하려면, Node.js Security Checklist(아래)를 읽어보세요

만약, 당신의 application이 보호가 되어 있다면, 교활한 취약점을 찾는 Snyk 와 Node Security Platform을 사용할 수 있습니다.

* Node.js Security Checklist : https://blog.risingstack.com/node-js-security-checklist/
* Snyk : https://snyk.io/
* Node Security Platform : https://nodesecurity.io/

## Tip #6: microservices를 받아들이세요

만약 규모가 큰 프로젝트를 개발한다면, microservices 아키텍쳐를 받아들야 한니다. 아래에 2가지 테크닉을 소개하겠습니다.

* Docker
  컨테이너를 제공하는 소프트웨어 기술이고, 코드와 런터임, 시스템 툴과 시스템 라이브러리를 포함한 모든 완벽한 파일시스템 소프트웨어를 하나로 감싸줍니다.

* Kubernetes
  자동으로 개발, 확장(scaling), 컨터이너화된 어플리케이션을 자동으로 관리하는 오픈소스 시스템입니다.

컨테이너와 orchestration에 대해서 더 깊게 알아보기 전에, 가지고 있는 소스를 발전시켜야 합니다. 12-factor app 방법(아래 링크 참조)으로, 당신의 서비스는 컨테이너화하고 서비스를 배포하는데 더 쉬울 것 입니다.

- 12-factor app methodology : https://12factor.net/

## Tip #7: 서비스를 모니터링 하세요.

사용자들이 인지하기전에 이슈들을 수정하세요. 모니터링와 알림은 실제 배포에서 중요한 부분이고, 복잡한 microservices 시스템에서 하기는 쉽지가 않습니다. 다행히도, 급격하게 변화하는 필드에서 발전하게 하는 툴이 있습니다. 아래 링크를 참고하세요.

- future of monitoring holds : https://blog.risingstack.com/the-future-of-microservices-monitoring-and-instrumentation/
- OpenTracing standard : https://blog.risingstack.com/distributed-tracing-opentracing-node-js/

만약, 실험적인 것을 하고 싶다면, Prometheus 를 사용해볼 수 있습니다.

* Node.js Performance Monitoring with Prometheus : https://blog.risingstack.com/node-js-performance-monitoring-with-prometheus/

## Tip #8:  open-source project에 기여하세요.

좋아하는 Node.js 프로젝트가 있나요? 당신의 도움이 더 나아지게 하는 기회가 많습니다. 이슈를 찾아보고, 당신의 관심있어하는 부분을 찾고, 코딩을 하세요.

어떻게 시작할지 모른다면, 아래 링크를 참고하세요.

- Get Started Contributing to JavaScript Open Source : https://egghead.io/articles/get-started-contributing-to-javascript-open-source

- How to Contribute to an Open Source Project on GitHub : https://egghead.io/courses/how-to-contribute-to-an-open-source-project-on-github


---


### 원문 링크

[8 Tips to Build Better Node.js Apps in 2018](https://blog.risingstack.com/node-js-development-tips-2018/?utm_source=RisingStack+Engineering&utm_campaign=86728bcc79-EMAIL_CAMPAIGN_2018_01_09&utm_medium=email&utm_term=0_02a6a69990-86728bcc79-474894449)



