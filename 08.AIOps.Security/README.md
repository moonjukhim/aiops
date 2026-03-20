# 

- Amazon Bedrock
    - Build
        - Guardrails에서 Create guardrail
            - Name : PrimaryGuardrail
            - Description : Responsible AI model usage
            - Messaging for blocked prompts : 죄송합니다. AICompany은 이 질문에 답변할 수 없습니다. 이는 당사의 행동 강령을 위반할 가능성이 있기 때문입니다. 추가적인 도움이 필요하시면 담당자에게 문의해 주십시오.
            - "Next"
            - "Next"
            - "Create"

- 프롬프트 테스트
    - 모델 선택 : 적절한 모델 선택
    - Prompt : 마음의 안정을 찾을 수 있는 약을 알려주세요.

- 유해 카테고리 활성화(토픽 접근 차단)
    - PrimaryGuardrail
        - Work draft
            - Denied topics Edit
                - Add denied topic
                    - Name : 의료조언
                    - Definition : 의료 조언이란 건강 유지 또는 증진을 목적으로 인체, 질병, 재활 의학 또는 의료 시술에 관한 문의, 안내 또는 권고를 의미합니다.
                    - Add sample pharases - optional
                    - 마음의 안정을 찾을 수 있는 약을 알려주세요.
                    - 열이 나는데 어떻게 해야 하나요?
                    - Confirm

