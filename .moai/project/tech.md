# Composify 기술 스택 문서

## 개요

Composify는 최신 TypeScript와 React 19 기반의 Server Driven UI 라이브러리입니다. 모노레포 구조로 관리되며, 현대적인 빌드 도구와 테스트 프레임워크를 활용합니다.

---

## 핵심 기술 스택

### 언어 및 런타임

| 기술 | 버전 | 용도 |
|-----|------|-----|
| TypeScript | 5.9.3 | 타입 안전성, 개발자 경험 향상 |
| React | 19.1.2 | UI 라이브러리 |
| React DOM | 19.1.2 | DOM 렌더링 |
| Node.js | 18+ (권장) | 빌드 및 개발 환경 |

### 빌드 도구

| 도구 | 버전 | 용도 |
|-----|------|-----|
| tsup | 8.5.0 | TypeScript 번들링 (esbuild 기반) |
| pnpm | 최신 | 패키지 관리, 워크스페이스 |
| Vocs | 1.2.1 | 문서 사이트 빌드 |
| Vite | (Vocs 내장) | 개발 서버, HMR |

### 코드 품질

| 도구 | 버전 | 용도 |
|-----|------|-----|
| Biome | 2.3.0 | 린팅 및 포맷팅 (ESLint/Prettier 대체) |
| Jest | 30.0.4 | 단위 테스트 |
| ts-jest | 29.4.0 | TypeScript Jest 통합 |
| @testing-library/react | 16.3.0 | React 컴포넌트 테스트 |

---

## 주요 의존성

### 핵심 라이브러리 (@composify/react)

#### 에디터 관련

| 라이브러리 | 버전 | 용도 |
|-----------|------|-----|
| @monaco-editor/react | 4.7.0 | 코드 에디터 |
| react-dnd | 16.0.1 | 드래그 앤 드롭 코어 |
| react-dnd-html5-backend | 16.0.1 | HTML5 DnD 백엔드 |
| lucide-react | 0.556.0 | 아이콘 |
| css-box-model | 1.2.1 | CSS 박스 모델 계산 |

#### 파싱 및 변환

| 라이브러리 | 버전 | 용도 |
|-----------|------|-----|
| acorn | 8.15.0 | JavaScript 파서 |
| acorn-jsx | 5.3.2 | JSX 확장 파서 |
| react-element-to-jsx-string | 17.0.1 | React 엘리먼트를 JSX 문자열로 변환 |
| prettier | 3.6.2 | 코드 포맷팅 (JSX 출력) |

#### 유틸리티

| 라이브러리 | 버전 | 용도 |
|-----------|------|-----|
| es-toolkit | 1.40.0 | 모던 유틸리티 함수 (lodash 대체) |

### 문서 사이트 (@composify/docs)

| 라이브러리 | 버전 | 용도 |
|-----------|------|-----|
| vocs | 1.2.1 | 문서 프레임워크 |
| shiki | 3.19.0 | 코드 하이라이팅 |
| tailwindcss | 4.1.15 | CSS 유틸리티 |
| tailwind-variants | 3.2.2 | 스타일 변형 |
| @radix-ui/react-slot | 1.2.4 | 컴포넌트 합성 |
| react-router | 7.9.4 | 라우팅 |

---

## 개발 환경 설정

### 필수 요구사항

- Node.js 18 이상
- pnpm 8 이상
- Git

### 환경 설정 명령

```bash
# pnpm 설치 (없는 경우)
npm install -g pnpm

# 저장소 클론
git clone https://github.com/composify-js/composify.git
cd composify

# 의존성 설치
pnpm install

# 개발 서버 시작
pnpm -F @composify/docs dev
```

### IDE 설정 권장사항

- **VSCode 확장**
  - Biome 확장
  - TypeScript 확장
  - Tailwind CSS IntelliSense

- **설정**
  - `editor.formatOnSave: true`
  - Biome을 기본 포맷터로 설정

---

## 빌드 구성

### tsup 빌드 설정

```typescript
// packages/react/tsup.config.ts 개념
{
  entry: [
    'src/index.ts',
    'src/editor/index.ts',
    'src/renderer/index.ts',
    'src/utils/index.ts'
  ],
  format: ['cjs', 'esm'],
  dts: true,
  splitting: true,
  clean: true,
  external: ['react', 'react-dom']
}
```

### 빌드 출력 구조

```
dist/
├── index.js          # CommonJS 메인
├── index.mjs         # ESM 메인
├── index.d.ts        # 타입 선언
├── index.css         # 스타일
├── editor/
│   ├── index.js
│   ├── index.mjs
│   └── index.d.ts
├── renderer/
│   ├── index.js
│   ├── index.mjs
│   └── index.d.ts
└── utils/
    ├── index.js
    ├── index.mjs
    └── index.d.ts
```

---

## 테스트 전략

### 테스트 프레임워크

- **Jest 30** - 테스트 러너
- **ts-jest** - TypeScript 지원
- **jest-environment-jsdom** - DOM 환경 시뮬레이션
- **@testing-library/react** - React 컴포넌트 테스트

### 테스트 실행

```bash
# 전체 테스트
pnpm test

# 특정 패키지 테스트
pnpm -F @composify/react test

# 워치 모드
pnpm test --watch
```

### 테스트 위치

테스트 파일은 소스 파일과 동일한 디렉토리에 위치:
```
src/renderer/
├── Catalog/
│   ├── Catalog.ts
│   ├── Catalog.spec.ts     # 테스트
│   └── index.ts
├── Parser/
│   ├── Parser.ts
│   ├── Parser.spec.ts      # 테스트
│   └── index.ts
└── NodeManager/
    ├── NodeManager.ts
    ├── NodeManager.spec.ts  # 테스트
    └── index.ts
```

---

## 코드 품질

### Biome 설정

Biome은 ESLint와 Prettier를 대체하는 통합 도구입니다.

```bash
# 린트 검사
pnpm lint

# 포맷팅
pnpm format
```

**주요 규칙**:
- TypeScript 엄격 모드
- React Hooks 규칙
- 일관된 코드 스타일
- 임포트 정렬

### TypeScript 설정

```json
{
  "compilerOptions": {
    "strict": true,
    "jsx": "react-jsx",
    "moduleResolution": "bundler",
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

---

## CI/CD

### GitHub Actions (권장 설정)

```yaml
# .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v3
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'pnpm'
      - run: pnpm install
      - run: pnpm lint
      - run: pnpm test
      - run: pnpm build
```

### npm 배포

```bash
# 빌드
pnpm -F @composify/react build

# npm 배포 (maintainer only)
pnpm -F @composify/react publish
```

---

## 성능 최적화

### 번들 크기 관리

- **Tree Shaking**: ESM 지원으로 미사용 코드 제거
- **코드 스플리팅**: editor/renderer 분리
- **외부 의존성**: react, react-dom을 peerDependencies로

### 렌더링 최적화

- React 19의 새로운 기능 활용
- 메모이제이션 패턴 적용
- 가상화 (긴 리스트)

---

## 보안 고려사항

### JSX 파싱 보안

- acorn을 사용한 안전한 파싱
- 허용된 컴포넌트만 렌더링 (Catalog 기반)
- dangerouslySetInnerHTML 미사용

### 의존성 보안

- 정기적인 의존성 업데이트
- npm audit / pnpm audit 실행
- Elastic License 2.0 준수

---

## 기술 결정 배경

### React 19 선택 이유
- 최신 React 기능 활용 (Server Components 준비)
- 성능 개선
- 미래 호환성

### Biome 선택 이유
- ESLint + Prettier 대비 빠른 속도
- 단일 도구로 린팅/포맷팅 통합
- Rust 기반 고성능

### tsup 선택 이유
- esbuild 기반 빠른 빌드
- TypeScript 선언 파일 자동 생성
- 간단한 설정

### pnpm 선택 이유
- 빠른 설치 속도
- 효율적인 디스크 사용
- 모노레포 워크스페이스 지원

---

## 모니터링 및 분석

### 번들 분석

```bash
# 번들 크기 분석 (추가 도구 필요)
npx bundlesize
```

### 성능 측정

- Lighthouse 스코어 모니터링
- Core Web Vitals 추적
- 사용자 인터랙션 지연 측정

---

## 참고 자료

### 공식 문서
- [TypeScript 문서](https://www.typescriptlang.org/docs/)
- [React 19 문서](https://react.dev/)
- [pnpm 문서](https://pnpm.io/)
- [Biome 문서](https://biomejs.dev/)
- [tsup 문서](https://tsup.egoist.dev/)

### 프로젝트 내부 문서
- [Composify 공식 문서](https://composify.js.org/docs)
- [API 레퍼런스](https://composify.js.org/docs/catalog)

---

*문서 버전: 1.0.0*
*최종 수정: 2025-12-23*
