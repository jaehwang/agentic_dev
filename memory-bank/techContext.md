# 기술 컨텍스트 (Tech Context)

## 사용 기술
-   **프로그래밍 언어**:
    -   MCP 서버 및 백엔드: TypeScript (Node.js 런타임, Express.js 프레임워크)
-   **데이터베이스**:
    -   벡터 데이터베이스: Pinecone (주요 선택), Weaviate, Milvus (고려 대상)
    -   메타데이터 데이터베이스: MongoDB
    -   캐싱: Redis
-   **AI 및 머신러닝**:
    -   LLM: OpenAI API (GPT-4, Ada 2 for embeddings), 로컬 LLM (고려 대상)
    -   문서 처리 및 임베딩: LangChain, LlamaIndex, Sentence Transformers (로컬 대안)
-   **문서 형식 및 관리**:
    -   주요 형식: Markdown
    -   다이어그램: Mermaid, PlantUML (CaD)
    -   데이터 교환: JSON, YAML (CaD)
    -   버전 관리: Git, GitHub Actions (CI/CD)
-   **프로토콜 및 인터페이스**:
    -   Model Context Protocol (MCP): 시스템 간 연동, AI 에이전트와의 인터페이스 (RESTful API 기반, JWT 인증)
    -   기존 시스템 연동: codebeamer REST API
-   **인프라 및 배포**:
    -   컨테이너화: Docker
    -   오케스트레이션: Kubernetes (선택적)
    -   클라우드 플랫폼: AWS/Azure (고려 대상)
    -   CI/CD: GitHub Actions

## 개발 설정
-   **IDE**: VSCode, IntelliJ IDEA, Eclipse (플러그인 개발 예정)
-   **AI 코딩 도구**: 개발자 클라이언트로 작동하며, MCP 서버들과 상호작용.
-   **문서 중심 개발 (CaD)**: `requirements.md` (요구사항) → `design.md` (설계, Mermaid 다이어그램 포함) → `code/` (AI 생성 코드) → `test_plan.md` (테스트 계획) 순서의 워크플로우를 지향.
-   **프로젝트별 컨텍스트 관리**: 모든 데이터 및 API 호출에 `project_id`를 사용하여 다중 프로젝트 환경 지원.
-   **아키텍처 정보 표준**: 각 프로젝트의 아키텍처 정보는 웹사이트에 게시되며, AI가 접근할 수 있도록 `/llms.txt` 표준을 따름.

## 기술적 제약 조건
-   **기존 시스템 연동**: codebeamer 등 기존 요구사항 관리 시스템과의 데이터 연동 필요.
-   **임베디드 시스템 컨텍스트**: 최종 개발 대상은 자동차 인포테인먼트 시스템(리눅스 기반 임베디드 시스템)이지만, 본 AI 지원 개발 환경 자체는 웹 기술 기반으로 구축.
-   **데이터 보안 및 접근 제어**: 요구사항 및 설계 문서는 민감 정보를 포함할 수 있으므로, 강력한 인증(JWT) 및 인가 체계 필요.
-   **성능**: 대량의 문서 검색 및 AI 분석/생성 작업 시 실시간 또는 준실시간 응답 성능 확보 필요.
-   **확장성**: 사용자 및 프로젝트 증가에 따른 시스템 확장성 고려.

## 종속성
-   **외부 AI 서비스**: OpenAI API 등 외부 LLM 서비스에 대한 의존성 (비용, 가용성, 정책 변경 등).
-   **벡터 DB 서비스**: Pinecone 등 외부 벡터 DB 서비스에 대한 의존성.
-   **기존 데이터 소스**: codebeamer 등 사내 시스템의 API 가용성 및 데이터 정합성에 의존.

## 도구 사용 패턴
-   **문서 검색 및 조회**:
    -   개발자는 AI 코딩 도구를 통해 자연어 또는 특정 ID로 문서 MCP 서버에 검색/조회 요청.
    -   MCP 서버는 벡터 DB와 메타데이터 DB를 활용하여 관련성 높은 문서를 반환.
-   **아키텍처 정보 조회**:
    -   AI 코딩 도구는 시스템 명세 MCP 서버를 통해 현재 프로젝트의 `/llms.txt` 파일 및 관련 아키텍처 정보를 조회.
-   **요구사항 분석 및 설계 문서 생성**:
    -   개발자가 `requirements.md` 또는 자연어로 초기 요구사항 입력.
    -   AI 시스템(AI 코딩 도구 내 또는 연동된 LLM)이 문서 MCP 서버 및 시스템 명세 MCP 서버를 통해 컨텍스트를 확보.
    -   확보된 컨텍스트를 바탕으로 Markdown 형식의 요구사항 분석 문서 및 `design.md`(Mermaid 다이어그램 포함) 초안 생성.
-   **코드 생성**:
    -   AI 시스템이 `design.md` 및 관련 컨텍스트를 기반으로 소스 코드 초안 생성.
-   **ETL 파이프라인**:
    -   codebeamer 등에서 데이터를 추출(Extract)하고, 분석 및 처리에 적합한 형태로 변환(Transform)한 후, 벡터 DB 및 메타데이터 DB에 적재(Load).
-   **AI Agent 협업**:
    -   MCP를 통해 정의된 명령(예: `mcp://generate-architecture`)을 AI 에이전트가 수행.
