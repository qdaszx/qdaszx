# 오일교의 포트폴리오

토글 ▶︎ 을 클릭하면 자세한 내용을 확인할 수 있습니다.

---

## 주식회사 웨이블 (2022.01 ~ 재직 중)

### [(주)한우리열린교육] 한우리 독서토론 피드백 웹/앱 개발
**2025.01 ~ 2025.11 (11개월)**

**프로젝트 개요**
- 팀 구성: 프론트엔드 2명, 백엔드 1명, 디자이너 1명
- 담당: 프론트엔드 아키텍처 설계, 공통 모듈 개발
- 기술 스택: React, TypeScript, TanStack Query, Emotion, Tailwind CSS

---

### FSD 아키텍처 도입을 통한 개발 생산성 및 유지보수성 증대

다수의 역할(수강생, 학부모, 교사, 센터장)이 얽힌 복잡한 프로젝트에서 FSD(Feature-Sliced Design) 아키텍처를 도입하고 커스텀하여, 코드 복잡도를 낮추고 팀 내 코드 리뷰 문화를 활성화했습니다.

**문제 정의**

**비즈니스 복잡도**
- 3개의 프로젝트(수강생/학부모, 교사/센터장, 어드민)가 동시에 진행되었으며, 각 프로젝트마다 Github Repository를 별도로 관리했습니다.
- 하나의 프로젝트에 두 역할(Role)이 존재하고 학부모 ↔ 수강생, 센터장 ↔ 교사 계정 전환이 가능했습니다.
- 디자인은 유사하나 역할에 따라 로직이 상이한 경우가 많았고, 때로는 완전히 다른 디자인을 요구하기도 했습니다.

**기술 부채**
- 회사 프론트엔드 폴더 구조에 대한 명확한 가이드가 없어, 큰 단위로만 나누는 형식이었습니다.
- 각 개발자마다 코드 분리 방식이 달랐고, 문서화할 시간이 부족한 상황이었습니다.
- 기존의 단순한 폴더 구조는 비즈니스 로직의 응집도를 낮췄고, 명확한 컨벤션 부재로 인해 타 도메인 개발자의 코드 리뷰가 형식적으로 흘러갔습니다.

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

팀의 규모와 프로젝트 특성에 맞춰 FSD를 커스텀하여 도입했고, 문서 기반으로 팀원들과 공유했습니다.

**커스텀 항목**
- import 시 path 정보가 segment까지 나와야 합니다 (예: `entities/board/api`, `entities/board/model`)
- 역할(Role)에 따른 구분은 pages layer의 slice에서 결정합니다 (예: `student-board`, `parent-board`)
- 예외성이 많은 페이지라면 한 slice 안에서 관리합니다
- 해당 페이지에 뎁스가 있다면 해당 slice의 ui segment 내에 자율적인 폴더 구조를 허용합니다

**성과**

- **코드 리뷰 문화 개선**
  - 통일된 아키텍처 덕분에 다른 도메인 담당자도 로직을 쉽게 파악할 수 있게 되어, 형식적이던 Approve 관행에서 벗어나 실질적인 피드백이 오가는 문화로 변화했습니다.
- **회사 표준 아키텍처로 채택**
  - 프로젝트에서 검증된 FSD 구조가 회사 전체 프론트엔드 폴더 구조로 적용되어, 동일한 시선에서 코드를 바라볼 수 있게 되었습니다.
- **유지보수 효율성 증대**
  - 역할별 예외 로직이 격리(Isolation)되어, 고객사의 긴급한 수정 요청에도 사이드이펙트 없이 신속한 대응이 가능해졌습니다.
  - 동일한 패턴을 유지하면서도 예외적인 부분은 해당 slice에서 모든 것을 해결할 수 있게 되어 코드 복잡도가 크게 낮아졌습니다.

---

### Viewport 및 Platform 파편화 해결을 위한 선언적 인터페이스 구축

웹/앱, 데스크톱/모바일이 혼재된 환경에서, CSS 변수와 동기화된 Custom Hook과 선언형 컴포넌트를 통해 뷰포트 및 플랫폼 분기 로직을 추상화하여 개발 일관성을 확보했습니다.

**문제 정의**

**N-Screen 대응**
- 수강생/학부모, 교사/센터장 프로젝트는 앱 출시까지 필요한 상황이었습니다.
- 기획상 화면 크기 1024px을 기준으로 데스크톱, 모바일 뷰를 제공하기로 결정되었습니다.
- 웹과 앱 환경이 다르기 때문에 환경 차이로 인해 공통으로 적용될 수 없는 부분(파일 저장, 카메라, 앱 푸시 등)에 대한 고려사항이 많았습니다.
- 디자인상 데스크톱, 모바일 뷰가 일관되지 않은 페이지들이 몇몇 존재하여 예외적인 상황도 고려해야 했습니다.

**기술 환경**
- AccessToken에 제공된 사용자 역할(Role)을 판단하여 경로를 설정했습니다.
- 앱은 모든 스택에 WebView를 보여주고, 앱 푸시, 카메라, 파일 업로드/다운로드 같은 특정 상황은 postMessage로 따로 처리하는 환경이었습니다.

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

2. 사용 예시 - 각 페이지마다 동일 패턴 제공

```tsx
// 실제 페이지 적용 코드
// 미디어 쿼리나 복잡한 조건문 없이 선언적으로 뷰를 구성
return (
  <>
    {/* 데스크톱 화면 */}
    <DesktopOnly>
      <SideNavigation /> {/* 데스크톱용 네비게이션 */}
      <ComplexDataGrid data={data} /> {/* 데스크톱: 그리드 뷰 */}
    </DesktopOnly>

    {/* 모바일 화면 */}
    <MobileOnly>
      <SimpleCardList data={data} /> {/* 모바일: 리스트 뷰 */}
    </MobileOnly>
  </>
);
```

예외적으로 얼리 리턴이 필요할 경우에도 처리 가능합니다.

```tsx
const viewportDevice = useViewportDevice();

if (viewportDevice === "desktop") {
  return (
    <DesktopComplexLayout>
      {/* 데스크톱 전용 복잡한 레이아웃 */}
    </DesktopComplexLayout>
  );
}

return (
  <MobileSimpleLayout>
    {/* 모바일 전용 간단한 레이아웃 */}
  </MobileSimpleLayout>
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
- **개발 일관성 확보**
  - 예외성 있는 페이지까지 일관된 패턴을 유지할 수 있었습니다.
  - 페이지 개발 시 뷰포트 계산 로직을 반복 작성할 필요가 없어져 인지적 요소가 감소하고 빠른 대처가 가능해졌습니다.
- **사용자 경험 통일**
  - 웹, 앱 환경을 가리지 않고 일관된 UX를 제공하며, 플랫폼별 예외 상황을 유연하게 대처했습니다.
  - 동일한 mobile 뷰에서 다른 플랫폼이라면 어렵지 않게 대처할 수 있게 되었습니다.

---

### 기획/디자인의 모호함 해결을 통한 코드 복잡도 제거

기획 단계의 모호함이 코드의 복잡도(Conditional Rendering)를 높이는 근본 원인임을 파악하고, 역제안을 통해 기능을 재정립하여 코드 품질과 QA 효율을 동시에 개선했습니다.

**문제 정의**

**비즈니스 상황**
- 학부모는 홈에서 자녀(수강생) 1명의 정보를 갖고 관련 데이터를 볼 수 있습니다.
- 학부모는 꼭 자녀를 등록해야 하며, 자녀 등록은 학부모 로그인 또는 홈에서 유저의 액션에 의해 실행할 수 있습니다.
- 여러 자녀가 있을 경우 자녀 변경 기능이 가능하지만, 학부모 홈 화면에는 자녀 변경을 수행할 수 있는 버튼만 존재했습니다.

**요구사항의 모호함**
- 자녀 등록과 자녀 변경 기능이 하나의 UI 흐름 안에 혼재되어 있었습니다.
- 해당 기능과 관련된 QA가 지속적으로 발생했고, 기획+디자인팀까지 전달하여 진행하면 1~2일 소요가 불가피한 상황이었습니다.

**기술적 부채**
- 명확한 기획이 없다는 점에서 기존 작업자들이 임의로 처리한 코드가 많았습니다.
- 최근 작업자는 기존 코드를 분석하지 않고 QA에 대한 내용만 처리하여 복잡도가 더 올라갔습니다.
- 기획의 모호함을 코드로 덮으려다 보니, `props`에 따른 과도한 분기 처리와 방어 로직이 중첩되어 유지보수가 불가능한 수준이었습니다.

**해결 방안**

**기획 재정립**
- 자녀 등록과 자녀 변경은 다른 기능으로 인식할 수 있게 팀장에게 구두로 전달했습니다.
- 기존 디자인에 존재하는 불분명한 요소를 분리하면서 자녀 등록, 자녀 변경에 대한 기능을 재정립했습니다:
  - **자녀 등록**: 학부모가 등록하지 않은 자녀들만 보여줍니다
  - **자녀 변경**: 학부모가 등록한 자녀들만 보여줍니다
- 기존 코드의 분기 복잡도와 QA 리포트 데이터를 근거로 팀장 및 기획자를 설득했습니다.

**컴포넌트 분리 (Separation of Concerns)**
- 하나의 거대 컴포넌트를 `ChildrenRegisterOverlay(등록)`과 `ChildrenUpdateOverlay(변경)`으로 분리하여 각각의 책임(Responsibility)을 명확히 했습니다.
- 서로 얽혀있던 분기점들이 깔끔하게 정리되었습니다.

**성과**

- **QA 이슈 Zero 달성**
  - 기능을 명확히 분리한 이후 해당 모듈에서 로직 오류로 인한 QA가 단 한 건도 발생하지 않았습니다.
- **설득 경험**
  - 코드 복잡도를 키우는 근본 원인을 찾아내고 재정립해서 고객사, 기획자, 디자이너, 개발 팀장까지 설득할 수 있었습니다.
- **커뮤니케이션 프로세스 개선**
  - "기획의 명확성이 코드 품질을 결정한다"는 인식을 심어주어, 이후 프로젝트에서는 기획 리뷰 단계에서 개발팀이 더 적극적으로 참여하는 문화가 정착되었습니다.

---

**회고**

두 명의 프론트엔드 개발자가 3개 프로젝트를 동시에 진행하는 상황에서, 아키텍처와 추상화에 시간을 투자하는 것이 처음엔 부담스러웠습니다. 하지만 명확한 구조와 재사용 가능한 모듈이 갖춰지니 후반으로 갈수록 생산성이 올라갔고, 유지보수도 훨씬 수월해졌습니다. 기획 단계에서의 커뮤니케이션이 코드 품질에 직접적인 영향을 준다는 것도 배웠습니다.
