# Tech Stack Policy

Planterior AI의 공개 포트폴리오 문서에서 기술 스택을 표기하는 기준입니다.

## 기준

1. 프로젝트에서 실제 역할이 있는 기술만 작성합니다.
2. 기술명만 나열하지 않고, 담당한 기능을 함께 설명합니다.
3. 구현 영역, 설계 영역, 향후 확장 영역을 구분합니다.
4. runtime에서 호출하지 않는 offline tool은 offline tool로 표시합니다.
5. 공개 저장소에는 소스코드, credential, private endpoint, model weight, private dataset을 포함하지 않습니다.

## AWS 표기

AWS는 SaaS 백엔드 운영을 위한 cloud infrastructure layer로 표기합니다.

공개 문서에서는 credential, account id, private endpoint, 배포 스크립트 같은 민감한 정보는 제외하고, 인프라 역할만 설명합니다.
