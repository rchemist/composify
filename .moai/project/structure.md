# Composify 프로젝트 구조 문서

## 개요

Composify는 pnpm 워크스페이스를 활용한 모노레포 구조로 관리됩니다. 핵심 라이브러리, 문서, 예제가 독립적인 패키지로 분리되어 있습니다.

---

## 디렉토리 구조

```
composify/
├── packages/                    # 핵심 패키지들
│   ├── react/                   # @composify/react - 메인 라이브러리
│   │   ├── src/
│   │   │   ├── editor/          # 비주얼 에디터 컴포넌트
│   │   │   ├── renderer/        # JSX 렌더러
│   │   │   ├── ui/              # 공통 UI 컴포넌트
│   │   │   ├── utils/           # 유틸리티 함수
│   │   │   └── preset/          # 프리셋 설정
│   │   └── dist/                # 빌드 결과물
│   │
│   └── docs/                    # @composify/docs - 문서 사이트
│       ├── app/
│       │   ├── pages/           # Vocs 페이지들
│       │   └── public/          # 정적 에셋
│       └── src/                 # 문서 컴포넌트들
│
├── examples/                    # 예제 프로젝트들
│   ├── nextjs/                  # Next.js 통합 예제
│   ├── react-router/            # React Router 예제
│   ├── expo/                    # Expo (React Native) 예제
│   └── api/                     # API 서버 예제
│
├── .moai/                       # MoAI-ADK 설정
│   ├── config/                  # 프로젝트 설정
│   └── project/                 # 프로젝트 문서
│
├── .claude/                     # Claude Code 설정
│   ├── agents/                  # 에이전트 정의
│   ├── commands/                # 커스텀 명령어
│   └── skills/                  # 스킬 정의
│
└── 루트 설정 파일들
    ├── package.json             # 루트 패키지 설정
    ├── pnpm-workspace.yaml      # pnpm 워크스페이스 설정
    ├── tsconfig.json            # 루트 TypeScript 설정
    ├── biome.json               # Biome 린터/포맷터 설정
    └── CLAUDE.md                # Claude Code 지시사항
```

---

## 패키지 상세

### packages/react (@composify/react)

메인 라이브러리로 Editor, Renderer, Catalog를 제공합니다.

**모듈 구조**:

```
src/
├── editor/                      # 비주얼 에디터 (50+ 컴포넌트)
│   ├── Editor/                  # 메인 에디터 컴포넌트
│   ├── VisualEditor/            # 비주얼 편집 영역
│   ├── CodeEditor/              # Monaco 코드 에디터
│   ├── Preview/                 # 미리보기
│   ├── PanelLeft/               # 좌측 패널 (블록 라이브러리)
│   ├── PanelRight/              # 우측 패널 (속성 편집)
│   ├── BlockLibrary/            # 블록 라이브러리
│   ├── PropertyControl*/        # 속성 컨트롤러들
│   ├── Draggable/               # 드래그 기능
│   ├── Droppable/               # 드롭 기능
│   ├── Outline/                 # 아웃라인 뷰
│   ├── ViewportControl/         # 뷰포트 제어
│   └── CloudEditor*/            # Cloud 통합 컴포넌트
│
├── renderer/                    # 렌더러 모듈
│   ├── Renderer/                # 메인 렌더러
│   ├── Catalog/                 # 컴포넌트 카탈로그
│   ├── Parser/                  # JSX 파서 (acorn 기반)
│   ├── Block/                   # 블록 정의
│   ├── NodeManager/             # 노드 관리
│   └── PropertySpec/            # 속성 스펙 정의
│
├── ui/                          # 공통 UI 컴포넌트
│   ├── Button/
│   ├── Input/
│   ├── Select/
│   ├── HStack/
│   ├── VStack/
│   └── ...
│
└── utils/                       # 유틸리티
    ├── Bridge/                  # iframe 통신
    └── createVariants/          # 스타일 변형
```

**엔트리 포인트**:
- `@composify/react` - 메인 익스포트
- `@composify/react/editor` - 에디터 전용
- `@composify/react/renderer` - 렌더러 전용
- `@composify/react/utils` - 유틸리티
- `@composify/react/style.css` - 스타일시트

### packages/docs (@composify/docs)

Vocs 기반 문서 사이트입니다.

**구조**:
```
app/
├── pages/
│   ├── docs/                    # 문서 콘텐츠
│   │   ├── getting-started.md
│   │   ├── catalog/             # Catalog 관련 문서
│   │   ├── editor/              # Editor 관련 문서
│   │   ├── renderer/            # Renderer 관련 문서
│   │   ├── tutorial/            # 튜토리얼
│   │   └── use-cases/           # 사용 사례
│   ├── blog/                    # 블로그 포스트
│   ├── demo.tsx                 # 라이브 데모
│   ├── cloud.tsx                # Cloud 소개
│   └── showcase.tsx             # 쇼케이스
│
└── public/
    └── brand/                   # 브랜드 에셋
```

### examples/

다양한 프레임워크에서의 통합 예제를 제공합니다.

**예제 구조** (공통 패턴):
```
[framework]/
├── components/
│   ├── catalog.ts               # 카탈로그 등록
│   ├── Body/                    # 예제 컴포넌트
│   ├── Heading/
│   ├── HStack/
│   └── VStack/
├── app/ 또는 pages/             # 라우팅
└── package.json
```

---

## 아키텍처 패턴

### 1. 모듈러 컴포넌트 구조

각 컴포넌트는 독립적인 디렉토리로 구성:
```
ComponentName/
├── ComponentName.tsx            # 메인 컴포넌트
├── ComponentName.spec.ts        # 테스트 (있는 경우)
└── index.ts                     # 익스포트
```

### 2. Catalog 패턴

컴포넌트 등록과 메타데이터 분리:
```
Component/
├── Component.tsx                # React 컴포넌트
├── ComponentCatalog.ts          # Catalog 등록 정의
└── index.ts                     # 통합 익스포트
```

### 3. 엔트리 포인트 분리

사용 목적에 따른 번들 분리:
- `/editor` - 에디터 전용 (무거움, 개발/관리 환경용)
- `/renderer` - 렌더러 전용 (가벼움, 프로덕션용)
- `/utils` - 공유 유틸리티

---

## 데이터 흐름

### Editor에서 Renderer로

```
[Editor]
    │
    ├─ 사용자가 비주얼하게 편집
    │
    ├─ 내부 AST 구조로 관리
    │
    └─ onSubmit 시 JSX 문자열로 직렬화
            │
            ▼
    [JSX 문자열] (서버/DB에 저장)
            │
            ▼
[Renderer]
    │
    ├─ JSX 문자열 파싱 (acorn-jsx)
    │
    ├─ Catalog에서 컴포넌트 조회
    │
    └─ React 엘리먼트 생성 및 렌더링
```

### iframe 통신 (Bridge)

Editor는 iframe 내부에서 동작하며 Bridge를 통해 통신:
```
[Parent Window]
    │
    ├─ EditorControl
    │
    └─ Bridge (PostMessage)
            │
            ▼
[iframe]
    │
    ├─ VisualEditor
    │
    └─ InlineFrame 컴포넌트들
```

---

## 외부 통합

### Composify Cloud 통합

CloudEditor 컴포넌트 그룹으로 Cloud 서비스 연동:
- CloudEditor: Cloud 버전 에디터
- CloudEditorControl: Cloud 제어
- CloudEditorEventHandler: 이벤트 처리
- CloudEditorInitializer: 초기화

### 프레임워크 지원

| 프레임워크 | 지원 상태 | 예제 |
|-----------|----------|------|
| Next.js | 완전 지원 | examples/nextjs |
| React Router | 완전 지원 | examples/react-router |
| Expo | 완전 지원 | examples/expo |
| Vanilla React | 완전 지원 | - |
| Remix | 지원 | 문서 참조 |

---

## 빌드 및 번들링

### tsup 빌드 설정

```
출력 형식:
├── dist/index.js          # CommonJS
├── dist/index.mjs         # ESM
├── dist/index.d.ts        # TypeScript 선언
└── dist/index.css         # 스타일
```

### 모듈별 엔트리

```json
{
  "exports": {
    ".": "./dist/index.mjs",
    "./editor": "./dist/editor/index.mjs",
    "./renderer": "./dist/renderer/index.mjs",
    "./utils": "./dist/utils/index.mjs",
    "./style.css": "./dist/index.css"
  }
}
```

---

## 개발 워크플로우

### 로컬 개발

```bash
# 의존성 설치
pnpm install

# 개발 서버 (docs)
cd packages/docs && pnpm dev

# 스토리북 (react)
cd packages/react && pnpm storybook

# 테스트 실행
pnpm test
```

### 예제 실행

```bash
# Next.js 예제
cd examples/nextjs && pnpm dev

# React Router 예제
cd examples/react-router && pnpm dev

# Expo 예제
cd examples/expo && pnpm start
```

---

*문서 버전: 1.0.0*
*최종 수정: 2025-12-23*
