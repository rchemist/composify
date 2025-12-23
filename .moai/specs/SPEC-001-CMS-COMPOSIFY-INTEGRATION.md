# SPEC-001: CMS-Composify 통합

## 메타데이터

| 항목 | 값 |
|------|-----|
| SPEC ID | SPEC-001 |
| 제목 | CMS-Composify 통합 |
| 상태 | 분석 완료 (Analysis Complete) |
| 생성일 | 2025-12-23 |
| 작성자 | R2-D2 |
| 우선순위 | 높음 |
| 예상 복잡도 | 높음 (Large) |

---

## 1. 개요

### 1.1 배경
CMS 백엔드 프로젝트에서 WYSIWYG 에디터로 GrapesJS를 계획하고 있었으나, Composify 비주얼 에디터가 React 네이티브로 더 나은 통합 가능성을 제공합니다.

### 1.2 목적
CMS 백엔드와 Composify 프론트엔드를 통합하여 완전한 Server-Driven UI(SDUI) 솔루션을 구축합니다.

### 1.3 범위
- CMS 백엔드의 Page/Block 데이터 모델과 Composify Node 모델 매핑
- API 연동 어댑터 레이어 개발
- CMS 커스텀 블록 컴포넌트 개발
- 발행 워크플로우 통합

---

## 2. 요구사항 (EARS 형식)

### 2.1 데이터 모델 통합

**REQ-001**: 데이터 매핑
- When CMS stores page content, the system shall store Composify JSX string in the `composifyContent` field.
- CMS 페이지 콘텐츠 저장 시, 시스템은 Composify JSX 문자열을 `composifyContent` 필드에 저장해야 합니다.

**REQ-002**: 컴포넌트 추적
- When page content is saved, the system shall extract and store used component types in `usedComponents` field for dependency analysis.
- 페이지 콘텐츠 저장 시, 시스템은 의존성 분석을 위해 사용된 컴포넌트 타입을 `usedComponents` 필드에 추출하여 저장해야 합니다.

**REQ-003**: 버전 관리
- When page content is modified, the system shall create a new version record with the previous content.
- 페이지 콘텐츠 수정 시, 시스템은 이전 콘텐츠로 새 버전 레코드를 생성해야 합니다.

### 2.2 에디터 통합

**REQ-004**: 에디터 로드
- When user navigates to page edit route, the system shall load Composify Editor with the page's composifyContent.
- 사용자가 페이지 편집 라우트로 이동 시, 시스템은 해당 페이지의 composifyContent로 Composify Editor를 로드해야 합니다.

**REQ-005**: 자동 저장
- While user is editing, the system shall auto-save content to local storage every 30 seconds.
- 사용자 편집 중, 시스템은 30초마다 로컬 스토리지에 콘텐츠를 자동 저장해야 합니다.

**REQ-006**: 저장 기능
- When user clicks save button, the system shall send composifyContent to CMS API and update the page record.
- 사용자가 저장 버튼 클릭 시, 시스템은 composifyContent를 CMS API로 전송하고 페이지 레코드를 업데이트해야 합니다.

### 2.3 블록 시스템

**REQ-007**: 블록 등록
- When CMS frontend initializes, the system shall register all CMS custom blocks to Composify Catalog.
- CMS 프론트엔드 초기화 시, 시스템은 모든 CMS 커스텀 블록을 Composify Catalog에 등록해야 합니다.

**REQ-008**: 필수 블록
- The system shall provide at minimum: TextBlock, Heading, Image, Button, Section, Container blocks.
- 시스템은 최소 TextBlock, Heading, Image, Button, Section, Container 블록을 제공해야 합니다.

**REQ-009**: 속성 편집
- When user selects a block, the system shall display PropertySpec-defined editors for all block props.
- 사용자가 블록 선택 시, 시스템은 모든 블록 props에 대해 PropertySpec으로 정의된 에디터를 표시해야 합니다.

### 2.4 발행 워크플로우

**REQ-010**: 발행 기능
- When user clicks publish, the system shall update page status to 'published' and trigger deployment workflow.
- 사용자가 발행 클릭 시, 시스템은 페이지 상태를 'published'로 업데이트하고 배포 워크플로우를 트리거해야 합니다.

**REQ-011**: 미리보기
- When user requests preview, the system shall generate a temporary preview URL valid for 1 hour.
- 사용자가 미리보기 요청 시, 시스템은 1시간 유효한 임시 미리보기 URL을 생성해야 합니다.

**REQ-012**: 버전 복원
- When user selects a previous version, the system shall restore that version's content and create a new version record.
- 사용자가 이전 버전 선택 시, 시스템은 해당 버전의 콘텐츠를 복원하고 새 버전 레코드를 생성해야 합니다.

### 2.5 에셋 관리

**REQ-013**: 에셋 선택
- When user edits Image block, the system shall display asset picker connected to CMS Asset API.
- 사용자가 Image 블록 편집 시, 시스템은 CMS Asset API에 연결된 에셋 선택기를 표시해야 합니다.

**REQ-014**: 에셋 업로드
- When user drops file onto editor, the system shall upload to CMS Asset API and insert appropriate block.
- 사용자가 에디터에 파일을 드롭하면, 시스템은 CMS Asset API에 업로드하고 적절한 블록을 삽입해야 합니다.

### 2.6 동적 블록 시스템

**REQ-015**: 블록 계층 구조
- The system shall support a 3-tier block hierarchy: Core Blocks (code-based), Template Blocks (DB-based), and Schema Blocks (DB-based).
- 시스템은 3단계 블록 계층을 지원해야 합니다: 코어 블록(코드 기반), 템플릿 블록(DB 기반), 스키마 블록(DB 기반).

**REQ-016**: 코어 블록
- The system shall provide code-based Core Blocks that serve as fundamental building blocks: Container, Section, Text, Heading, Image, Button, Form, Accordion, Tabs, Carousel.
- 시스템은 기본 빌딩 블록 역할을 하는 코드 기반 코어 블록을 제공해야 합니다: Container, Section, Text, Heading, Image, Button, Form, Accordion, Tabs, Carousel.

**REQ-017**: 템플릿 블록 저장
- When admin creates a template block, the system shall store the block definition in `block_templates` table with composifySource (JSX template) and propsSchema (property definitions).
- 관리자가 템플릿 블록 생성 시, 시스템은 composifySource(JSX 템플릿)와 propsSchema(속성 정의)를 `block_templates` 테이블에 저장해야 합니다.

**REQ-018**: 템플릿 블록 로드
- When CMS frontend initializes, the system shall load all active template blocks from DB and register them to Composify Catalog dynamically without requiring a build.
- CMS 프론트엔드 초기화 시, 시스템은 DB에서 모든 활성 템플릿 블록을 로드하고 빌드 없이 동적으로 Composify Catalog에 등록해야 합니다.

**REQ-019**: 템플릿 블록 편집기
- The system shall provide a visual Template Block Editor in CMS admin panel that allows non-developers to create blocks by combining Core Blocks.
- 시스템은 CMS 관리자 패널에 비주얼 템플릿 블록 편집기를 제공하여 비개발자가 코어 블록 조합으로 블록을 생성할 수 있어야 합니다.

**REQ-020**: 템플릿 블록 속성 정의
- When creating a template block, the system shall allow defining custom props with PropertySpec types (text, number, boolean, select, radio, array, object, node).
- 템플릿 블록 생성 시, 시스템은 PropertySpec 타입(text, number, boolean, select, radio, array, object, node)으로 커스텀 props 정의를 허용해야 합니다.

**REQ-021**: 템플릿 블록 즉시 적용
- When a template block is created or modified, the system shall make it available in the editor immediately without requiring frontend rebuild or redeployment.
- 템플릿 블록 생성 또는 수정 시, 시스템은 프론트엔드 리빌드나 재배포 없이 즉시 에디터에서 사용 가능하도록 해야 합니다.

**REQ-022**: 블록 카테고리 관리
- The system shall allow organizing blocks into custom categories that can be managed through CMS admin panel.
- 시스템은 CMS 관리자 패널에서 관리 가능한 커스텀 카테고리로 블록을 구성할 수 있어야 합니다.

**REQ-023**: 사이트별 블록 설정
- The system shall allow enabling/disabling specific blocks per site through block configuration settings.
- 시스템은 블록 설정을 통해 사이트별로 특정 블록을 활성화/비활성화할 수 있어야 합니다.

**REQ-024**: 글로벌 템플릿 블록
- The system shall support global template blocks that can be shared across all sites within the CMS.
- 시스템은 CMS 내 모든 사이트에서 공유 가능한 글로벌 템플릿 블록을 지원해야 합니다.

**REQ-025**: 스키마 블록 (선택적)
- The system may support Schema Blocks defined by JSON schema for simple style variations without requiring template composition.
- 시스템은 템플릿 조합 없이 간단한 스타일 변형을 위해 JSON 스키마로 정의된 스키마 블록을 지원할 수 있습니다.

---

## 3. 기술 명세

### 3.1 아키텍처

```
CMS Frontend (Next.js 15)
├── Composify Editor Integration
│   ├── CMSAdapter (API Bridge)
│   ├── DynamicBlockLoader (블록 동적 로드)
│   ├── Block Components
│   │   ├── Core Blocks (코드 기반)
│   │   └── Template Blocks (DB 기반, 동적 로드)
│   └── Custom Hooks
└── CMS Backend (Spring Boot 3.2)
    ├── Page Service (Extended)
    ├── Version Service
    ├── Asset Service
    └── BlockTemplate Service (신규)
```

**블록 계층 구조:**
```
┌─────────────────────────────────────┐
│  Level 1: Core Blocks (코드)         │
│  - 개발자가 코드로 관리               │
│  - Container, Text, Image, Button   │
└─────────────────────────────────────┘
              ↓ 조합
┌─────────────────────────────────────┐
│  Level 2: Template Blocks (DB)      │
│  - CMS 관리자가 UI로 생성            │
│  - 빌드 없이 즉시 적용               │
└─────────────────────────────────────┘
              ↓ 커스터마이징
┌─────────────────────────────────────┐
│  Level 3: Schema Blocks (DB, 선택)  │
│  - JSON으로 스타일 변형              │
└─────────────────────────────────────┘
```

### 3.2 데이터 모델 확장

**Page Entity 확장:**
```java
@Entity
public class Page {
    // 기존 필드들...

    @Column(columnDefinition = "text")
    private String composifyContent;  // JSX 문자열

    @ElementCollection
    private Set<String> usedComponents;  // 사용된 컴포넌트
}
```

**BlockTemplate Entity (신규):**
```java
@Entity
@Table(name = "block_templates")
public class BlockTemplate {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    private Site site;

    private String name;              // 블록 이름
    private String category;          // 카테고리
    private String description;       // 설명

    @Column(columnDefinition = "text")
    private String composifySource;   // JSX 템플릿

    @Column(columnDefinition = "jsonb")
    private String propsSchema;       // 속성 정의 (PropertySpec)

    private Boolean isActive;         // 활성화 여부
    private Boolean isGlobal;         // 글로벌 블록 여부

    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
}
```

**BlockConfig Entity (신규):**
```java
@Entity
@Table(name = "block_configs")
public class BlockConfig {
    @Id
    private Long id;

    @ManyToOne
    private Site site;

    private String blockName;         // 블록 이름
    private Boolean isEnabled;        // 활성화 여부
    private String customCategory;    // 커스텀 카테고리

    @Column(columnDefinition = "jsonb")
    private String defaultProps;      // 기본 속성값
}
```

### 3.3 API 엔드포인트

**Page API:**
| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | /pages/{id} | 페이지 조회 |
| PUT | /pages/{id}/content | 콘텐츠 저장 |
| POST | /pages/{id}/publish | 발행 |
| GET | /pages/{id}/versions | 버전 목록 |
| POST | /pages/{id}/versions/{vId}/restore | 버전 복원 |

**Block Template API (신규):**
| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | /block-templates | 템플릿 블록 목록 |
| GET | /block-templates/{id} | 템플릿 블록 상세 |
| POST | /block-templates | 템플릿 블록 생성 |
| PUT | /block-templates/{id} | 템플릿 블록 수정 |
| DELETE | /block-templates/{id} | 템플릿 블록 삭제 |

**Block Config API (신규):**
| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | /sites/{siteId}/block-configs | 사이트 블록 설정 조회 |
| PUT | /sites/{siteId}/block-configs | 블록 설정 업데이트 |
| GET | /sites/{siteId}/available-blocks | 사용 가능 블록 전체 목록 |

---

## 4. 구현 계획

### 4.1 Phase 구성

| Phase | 내용 | 산출물 |
|-------|------|--------|
| Phase 1 | 기반 구축 | 패키지 구조, 라우트 |
| Phase 2 | 핵심 통합 | API 연동, 버전 관리 |
| Phase 3 | 코어 블록 개발 | 10+ 코어 블록 컴포넌트 |
| Phase 4 | 동적 블록 시스템 | 템플릿 블록 저장/로드, 동적 로더 |
| Phase 5 | 템플릿 편집기 | 블록 관리 UI, 템플릿 에디터 |
| Phase 6 | 고급 기능 | 발행, 에셋, 페이지 템플릿 |
| Phase 7 | 최적화 | 성능, 테스트, 문서 |

### 4.2 Phase 4 상세: 동적 블록 시스템

| 작업 | 설명 | 우선순위 |
|------|------|----------|
| BlockTemplate Entity | DB 스키마 및 Entity 생성 | P0 |
| BlockTemplate API | CRUD API 구현 | P0 |
| DynamicBlockLoader | 프론트엔드 동적 로더 | P0 |
| 템플릿 렌더러 | 템플릿 → React 컴포넌트 변환 | P0 |
| BlockConfig API | 사이트별 블록 설정 | P1 |
| 글로벌 블록 지원 | 전체 사이트 공유 블록 | P1 |

### 4.3 Phase 5 상세: 템플릿 편집기

| 작업 | 설명 | 우선순위 |
|------|------|----------|
| 블록 라이브러리 관리 UI | 블록 목록/검색/필터 | P0 |
| 템플릿 에디터 UI | 코어 블록 조합 편집기 | P0 |
| Props 정의 UI | 속성 추가/편집 인터페이스 | P0 |
| 미리보기 기능 | 템플릿 실시간 프리뷰 | P1 |
| 카테고리 관리 | 커스텀 카테고리 CRUD | P1 |

### 4.4 의존성

**Composify:**
- @composify/react

**CMS:**
- 기존 Page/Block Entity 확장
- 신규 Version Service
- 신규 BlockTemplate Service
- 신규 BlockConfig Service

---

## 5. 검증 계획

### 5.1 테스트 시나리오

**기본 통합 테스트:**
| ID | 시나리오 | 예상 결과 |
|----|----------|-----------|
| TC-001 | 에디터에서 페이지 로드 | composifyContent가 에디터에 표시됨 |
| TC-002 | 블록 추가 및 저장 | DB에 JSX 저장, 버전 증가 |
| TC-003 | 버전 복원 | 이전 콘텐츠 복원, 새 버전 생성 |
| TC-004 | 페이지 발행 | status=published, 배포 실행 |
| TC-005 | 미리보기 생성 | 유효한 preview URL 반환 |

**동적 블록 시스템 테스트:**
| ID | 시나리오 | 예상 결과 |
|----|----------|-----------|
| TC-006 | 템플릿 블록 생성 | DB에 템플릿 저장, API 응답 성공 |
| TC-007 | 템플릿 블록 로드 | 에디터 초기화 시 Catalog에 등록됨 |
| TC-008 | 템플릿 블록 사용 | 에디터에서 드래그&드롭 가능 |
| TC-009 | 템플릿 블록 수정 | 수정 후 즉시 에디터에 반영 (빌드 불필요) |
| TC-010 | 템플릿 블록 Props | 템플릿 props가 에디터 패널에 표시됨 |
| TC-011 | 사이트별 블록 설정 | 비활성화된 블록이 에디터에서 숨김 |
| TC-012 | 글로벌 블록 | 모든 사이트에서 글로벌 블록 사용 가능 |

### 5.2 성능 기준

| 메트릭 | 목표 |
|--------|------|
| 에디터 로드 | < 3초 |
| 저장 응답 | < 500ms |
| 블록 추가 | < 100ms |
| 동적 블록 로드 | < 1초 (50개 템플릿 기준) |
| 템플릿 저장 | < 300ms |

---

## 6. 관련 문서

### 6.1 생성된 문서

| 문서 | 경로 | 설명 |
|------|------|------|
| 통합 개요 | /documents/프론트엔드/00.통합개요.md | 타당성 분석 |
| 데이터 매핑 | /documents/프론트엔드/01.데이터모델매핑.md | 모델 매핑 상세 |
| 아키텍처 | /documents/프론트엔드/02.통합아키텍처.md | 시스템 설계 |
| API 설계 | /documents/프론트엔드/03.API설계.md | 엔드포인트 정의 |
| 로드맵 | /documents/프론트엔드/04.구현로드맵.md | 구현 계획 |
| 동적 블록 시스템 | /documents/프론트엔드/05.동적블록시스템.md | 동적 블록 계층 설계 |

### 6.2 참조 문서

| 문서 | 프로젝트 | 설명 |
|------|----------|------|
| 16.AI사이트빌더.md | CMS | AI 사이트 생성 |
| 17.WYSIWYG에디터.md | CMS | 에디터 요구사항 |
| 21.콘텐츠발행빌드.md | CMS | 발행 워크플로우 |
| C.CMS데이터구조가이드.md | CMS | 7-Layer 모델 |

---

## 7. 이력

| 날짜 | 버전 | 변경 내용 | 작성자 |
|------|------|----------|--------|
| 2025-12-23 | 1.0 | 초기 작성 | R2-D2 |
| 2025-12-23 | 1.1 | 동적 블록 시스템 추가 (REQ-015~REQ-025) | R2-D2 |

---

## 8. 승인

| 역할 | 이름 | 승인일 | 서명 |
|------|------|--------|------|
| PM | - | - | - |
| Tech Lead | - | - | - |
| QA Lead | - | - | - |
