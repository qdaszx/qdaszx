# 오일교의 포트폴리오

토글 ▶︎ 을 클릭하면 자세한 내용을 확인할 수 있습니다.

---

## 주식회사 웨이블 (2022.01 ~ 재직 중)

### [(주)한우리열린교육] 한우리 독서토론 피드백 웹/앱 개발
**2025.01 ~ 2025.11 (11개월)**

**프로젝트 개요**
- 팀 구성: 프론트엔드 2명, 백엔드 1명, 디자이너 1명
- 담당: 프론트엔드 아키텍처 설계, 공통 모듈 개발
- 기술 스택: React, TypeScript, React Query, Emotion

---

### FSD 아키텍처 도입을 통한 개발 생산성 및 유지보수성 증대

다수의 역할(수강생, 학부모, 교사, 센터장)이 얽힌 복잡한 프로젝트에서 FSD(Feature-Sliced Design) 아키텍처를 도입하고 커스텀하여, 코드 복잡도를 낮추고 팀 내 코드 리뷰 문화를 활성화했습니다.

**문제 정의**

**비즈니스 복잡도**
- 3개의 프로젝트(수강생/학부모, 교사/센터장, 어드민)가 동시에 진행되었으며, 디자인은 유사하나 역할(Role)에 따라 로직이 상이한 경우가 많았습니다.

**기술 부채**
- 기존의 단순한 폴더 구조는 비즈니스 로직의 응집도를 낮췄고, 명확한 컨벤션 부재로 인해 타 도메인 개발자의 코드 리뷰 참여가 저조했습니다.

```
기존 폴더 구조
src/
├── components/
├── pages/
├── hooks/
└── utils/
```

**해결 방안**

[FSD 아키텍처(Feature-Sliced Design)](https://feature-sliced.design/kr/docs/get-started/overview) 도입 및 커스텀

팀의 규모와 프로젝트 특성에 맞춰 FSD를 커스텀하여 도입했습니다.

- **명시적 경로**: import 시 `entities/board/api`와 같이 계층(Layer)과 슬라이스(Slice)가 명확히 드러나도록 설정
- **Role 기반 분리**: 예외 처리가 많은 페이지는 Page Layer 또는 Slice 내부에서 `student-board`, `parent-board`로 명확히 분리하여 if/else 복잡도 제거
- **유연성 확보**: 뎁스가 깊은 UI는 Slice 내부 `ui` 세그먼트에서 자율적인 구조를 허용하여 개발 피로도 감소

**성과**

- **코드 리뷰 문화 개선**
  - 통일된 아키텍처 덕분에 다른 도메인 담당자도 로직을 쉽게 파악할 수 있게 되어, 형식적이던 Approve 관행에서 벗어나 실질적인 피드백이 오가는 문화로 변화했습니다.
- **유지보수 효율성 증대**
  - 역할별 예외 로직이 격리(Isolation)되어, 고객사의 긴급한 수정 요청에도 사이드이펙트 없이 신속한 대응이 가능해졌습니다.

---

### Viewport 및 Platform 파편화 해결을 위한 선언적 인터페이스 구축

웹/앱, 데스크톱/모바일이 혼재된 환경에서, CSS 변수와 동기화된 Custom Hook과 선언형 컴포넌트를 통해 뷰포트 및 플랫폼 분기 로직을 추상화하여 개발 일관성을 확보했습니다.

**문제 정의**

**N-Screen 대응**
- 1024px 기준의 반응형 웹과 웹뷰(App) 환경을 동시에 지원해야 했으며, 특히 파일 업로드 등 네이티브 기능 연동 시 환경별 분기 처리가 필수적이었습니다.

**유지보수 위험성**
- CSS의 미디어 쿼리 기준점(Breakpoint)과 JS 내부의 분기 기준점이 하드코딩으로 분리되어 있어, 기준 변경 시 불일치가 발생할 위험이 있었습니다.

**해결 방안**

**CSS 변수와 JS 로직 동기화**
- `getComputedStyle`을 활용해서 CSS에 정의된 글로벌 변수(`--breakpoint-lg`) 값을 직접 참조하는 `useViewportDevice` 훅을 구현했습니다.
- 이를 통해 디자인 시스템의 변경 사항이 별도 코드 수정 없이 JS 로직에도 즉시 반영되도록 설계했습니다.

**정확한 뷰포트 감지**
- `window.innerWidth`와 `resize` 이벤트를 사용하여 실시간 렌더링 환경(Desktop/Mobile)을 정확히 판별했습니다.

**선언적 컴포넌트 제공**
- 비즈니스 로직 내 `if`문 남발을 막기 위해 `<DesktopOnly>`, `<MobileOnly>` 등 직관적인 컴포넌트 인터페이스를 구축했습니다.

**플랫폼 추상화**
- `ReactNativeWebView` 객체 유무 및 UserAgent를 분석하는 `usePlatformDevice` 훅을 통해 웹과 앱(Webview)을 구분하고, 동일한 인터페이스로 네이티브 기능을 호출하도록 했습니다.

**코드 리팩터링 및 적용 사례**

1. Viewport 분기 처리

CSS 변수를 파싱하여 디자인 시스템과 JS 로직을 일치시키고, 사용하기 편한 인터페이스를 제공했습니다.

```tsx
// src/shared/model/device

/**
 * ViewportDeviceResponsive
 * 화면 너비에 따라 적절한 UI를 렌더링하는 HOC 패턴 컴포넌트
 */
export const ViewportDeviceResponsive = ({ desktop, mobile }: ViewportDeviceResponsiveProps) => {
  // 내부에서 getComputedStyle로 --breakpoint-lg를 참조하여 판단
  const device = useViewportDevice(); 
  return device === "desktop" ? desktop : mobile;
};

/**
 * 특정 뷰포트 노출 전용 컴포넌트
 * 비즈니스 로직 내의 불필요한 삼항 연산자를 제거하여 가독성 향상
 */
export const DesktopOnly = ({ children }: { children: ReactNode }) => (
  <ViewportDeviceComponent deviceType="desktop">{children}</ViewportDeviceComponent>
);

export const MobileOnly = ({ children }: { children: ReactNode }) => (
  <ViewportDeviceComponent deviceType="mobile">{children}</ViewportDeviceComponent>
);
```

2. 사용 예시

```tsx
// 실제 페이지 적용 코드
// 미디어 쿼리나 복잡한 조건문 없이 선언적으로 뷰를 구성
return (
  <Layout>
    <DesktopOnly>
       <SideNavigation /> {/* 데스크톱용 네비게이션 */}
    </DesktopOnly>

    <ViewportDeviceResponsive
       desktop={<ComplexDataGrid data={data} />} // 데스크톱: 그리드 뷰
       mobile={<SimpleCardList data={data} />}   // 모바일: 리스트 뷰
    />
  </Layout>
);
```

3. Platform(Web/App) 추상화

```tsx
// 기능 호출부 예시
const handleClickImage = useCallback(async () => {
  try {
    // 플랫폼 감지 로직(UA, RN 객체 확인)을 추상화
    if (platformDevice === "webview") {
       // RN Bridge 통신
       await RNMediaPick();
    } else {
       // Web Input Trigger
       imageInputRef.current?.click();
    }
  } catch (error) {
    handleError(error);
  }
}, [platformDevice]);
```

**성과**

- **유지보수 안정성 확보**
  - CSS 브레이크포인트 변경 시 JS 코드를 수정하지 않아도 자동으로 동기화되는 구조를 만들어 휴먼 에러를 방지했습니다.
- **생산성 향상**
  - 페이지 개발 시 뷰포트 계산 로직을 반복 작성할 필요가 없어져 UI 개발 속도가 단축되었습니다.
- **사용자 경험 통일**
  - 웹, 앱 환경을 가리지 않고 일관된 UX를 제공하며, 플랫폼별 예외 상황을 유연하게 대처했습니다.

---

### 기획/디자인의 모호함 해결을 통한 코드 복잡도 제거

기획 단계의 모호함이 코드의 복잡도(Conditional Rendering)를 높이는 근본 원인임을 파악하고, 역제안을 통해 기능을 재정립하여 코드 품질과 QA 효율을 동시에 개선했습니다.

**문제 정의**

**요구사항의 모호함**
- 자녀 등록과 자녀 변경 기능이 하나의 UI 흐름 안에 혼재되어 있었습니다.

**기술적 부채**
- 기획의 모호함을 코드로 덮으려다 보니, `props`에 따른 과도한 분기 처리와 방어 로직이 중첩되어 유지보수가 불가능한 수준이었습니다.

**커뮤니케이션 비용**
- 해당 기능과 관련된 버그 리포트와 수정 요청이 반복되어 개발 리소스 낭비가 심각했습니다.

**해결 방안**

**데이터 기반 설득 및 기획 재정립**
- 기존 코드의 분기 복잡도와 QA 리포트 데이터를 근거로 팀장 및 기획자를 설득했습니다.
- "등록되지 않은 자녀만 노출(등록)"과 "등록된 자녀만 노출(변경)"로 로직을 명확히 분리할 것을 역제안했습니다.

**컴포넌트 분리 (Separation of Concerns)**
- 하나의 거대 컴포넌트를 `ChildrenRegisterOverlay(등록)`과 `ChildrenUpdateOverlay(변경)`으로 분리하여 각각의 책임(Responsibility)을 명확히 했습니다.

**성과**

- **QA 이슈 Zero 달성**
  - 기능을 명확히 분리한 이후 해당 모듈에서 로직 오류로 인한 QA가 단 한 건도 발생하지 않았습니다.
- **커뮤니케이션 프로세스 개선**
  - "기획의 명확성이 코드 품질을 결정한다"는 인식을 심어주어, 이후 프로젝트에서는 기획 리뷰 단계에서 개발팀이 더 적극적으로 참여하는 문화가 정착되었습니다.

---

**회고**

두 명의 프론트엔드 개발자가 3개 프로젝트를 동시에 진행하는 상황에서, 아키텍처와 추상화에 시간을 투자하는 것이 처음엔 부담스러웠습니다. 하지만 명확한 구조와 재사용 가능한 모듈이 갖춰지니 후반으로 갈수록 생산성이 올라갔고, 유지보수도 훨씬 수월해졌습니다. 기획 단계에서의 커뮤니케이션이 코드 품질에 직접적인 영향을 준다는 것도 배웠습니다.
