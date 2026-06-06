# Tech Stack Policy

이 문서는 FloorplanSP 포트폴리오에서 기술 스택을 표기하는 기준입니다.

## 기준

1. 단순히 알고 있는 기술을 나열하지 않습니다.
2. 해당 프로젝트에서 맡은 역할을 함께 적습니다.
3. 실제 구현 완료, 설계 완료, 향후 확장 후보를 구분합니다.
4. runtime에서 호출하지 않는 offline tool은 offline tool로 표시합니다.
5. 공개 저장소에는 소스코드, credential, private endpoint, model weight, private dataset을 포함하지 않습니다.

## AWS 표기

사용자 요청에 따라 AWS를 Tech Stack과 README에 포함했습니다.

현재 README에서는 AWS를 특정 세부 서비스명이 아니라 cloud infrastructure layer로 설명합니다.
이는 portfolio 공개 저장소에서 private deployment detail, account id, endpoint, credential을 노출하지 않기 위한 방식입니다.

세부 서비스명을 실제로 공개해도 되는 경우에는 다음처럼 확장할 수 있습니다.

- AWS EC2 / ECS: backend runtime
- AWS S3: uploaded image and artifact storage
- AWS RDS: production database
- AWS SQS: async AI job queue
- AWS CloudWatch: logging and monitoring
