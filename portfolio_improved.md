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

### FSD 아키텍처 도입을 통한 코드 복잡도 해결

**문제 상황**

3개의 프로젝트(수강생/학부모, 교사/센터장, 어드민)를 동시에 진행하면서 역할(Role)별로 UI는 비슷하지만 로직이 달라지는 경우가 많았습니다. 기존의 단순한 폴더 구조로는 비즈니스 로직의 응집도가 낮았고, 명확한 컨벤션이 없다 보니 코드 리뷰도 형식적으로 흘러갔습니다.

```
// 기존 구조 예시
src/
├── components/
├── pages/
├── hooks/
└── utils/
```

**해결 방법**

[FSD(Feature-Sliced Design)](https://feature-sliced.design/kr/docs/get-started/overview) 아키텍처를 팀 규모와 프로젝트 특성에 맞게 조정해서 도입했습니다.

- **명시적 경로**: `entities/board/api` 처럼 계층과 슬라이스가 명확히 드러나게 설정
- **Role 기반 분리**: 예외 처리가 많은 페이지는 `student-board`, `parent-board`로 명확히 분리해서 조건문 중첩 제거
- **유연성 확보**: 뎁스가 깊은 UI는 슬라이스 내부에서 자유롭게 구성할 수 있도록 허용

**결과**

- 같은 도메인 담당자만 리뷰하던 관행에서 벗어나, 타 도메인 개발자도 코드 의도를 파악하고 피드백을 주는 문화로 변화했습니다
- 역할별 로직이 격리되어, 한 역할의 긴급 수정이 다른 역할에 영향을 주지 않게 되었습니다

---

### Viewport 및 Platform 파편화 문제 해결

**문제 상황**

1024px 기준 반응형 웹과 웹뷰(App) 환경을 동시에 지원해야 했습니다. 특히 파일 업로드 같은 네이티브 기능 연동 시 환경별 분기가 필수였는데, CSS의 브레이크포인트와 JS의 분기 기준이 하드코딩으로 분리되어 있어 불일치 위험이 있었습니다.

**해결 방법**

CSS 변수와 JS 로직을 동기화하고, 선언적 인터페이스를 제공했습니다.

1. **CSS 변수 동기화**
   - `getComputedStyle`로 CSS의 `--breakpoint-lg` 값을 직접 참조하는 `useViewportDevice` 훅 구현
   - 디자인 시스템 변경 시 JS 코드 수정 없이 자동 반영

2. **선언적 컴포넌트 제공**
   - 비즈니스 로직 내 조건문을 줄이기 위해 `<DesktopOnly>`, `<MobileOnly>` 같은 컴포넌트 제공

```tsx
// src/shared/model/device
export const ViewportDeviceResponsive = ({ desktop, mobile }: Props) => {
  const device = useViewportDevice();
  return device === "desktop" ? desktop : mobile;
};

export const DesktopOnly = ({ children }: { children: ReactNode }) => (
  <ViewportDeviceComponent deviceType="desktop">{children}</ViewportDeviceComponent>
);
```

3. **사용 예시**

```tsx
return (
  <Layout>
    <DesktopOnly>
      <SideNavigation />
    </DesktopOnly>

    <ViewportDeviceResponsive
      desktop={<ComplexDataGrid data={data} />}
      mobile={<SimpleCardList data={data} />}
    />
  </Layout>
);
```

4. **Platform 추상화**

```tsx
const handleClickImage = useCallback(async () => {
  if (platformDevice === "webview") {
    await RNMediaPick();
  } else {
    imageInputRef.current?.click();
  }
}, [platformDevice]);
```

**결과**

- CSS 브레이크포인트 변경 시 JS 코드를 수정하지 않아도 자동으로 동기화되는 구조 확립
- 페이지 개발 시 반복적인 뷰포트 계산 로직을 작성할 필요가 없어짐
- 웹, 앱 환경 모두에서 일관된 UX 제공

---

### 기획 모호성 해결을 통한 코드 품질 개선

**문제 상황**

자녀 등록과 자녀 변경 기능이 하나의 UI 흐름 안에 혼재되어 있었습니다. 기획의 모호함을 코드로 해결하려다 보니 props에 따른 과도한 분기 처리가 중첩되었고, 해당 기능과 관련된 QA 리포트가 반복적으로 발생했습니다.

**해결 방법**

기존 코드의 분기 복잡도와 QA 리포트를 근거로 팀장과 기획자를 설득했습니다. "등록되지 않은 자녀만 노출(등록)"과 "등록된 자녀만 노출(변경)"로 로직을 명확히 분리할 것을 제안했고, 이를 바탕으로 컴포넌트를 `ChildrenRegisterOverlay`와 `ChildrenUpdateOverlay`로 분리했습니다.

**결과**

- 기능 분리 이후 해당 모듈에서 로직 오류로 인한 QA가 발생하지 않음
- 기획 리뷰 단계에서 개발팀이 적극적으로 참여하는 문화가 만들어짐

---

**회고**

두 명의 프론트엔드 개발자가 3개 프로젝트를 동시에 진행하는 상황에서, 아키텍처와 추상화에 시간을 투자하는 것이 처음엔 부담스러웠습니다. 하지만 명확한 구조와 재사용 가능한 모듈이 갖춰지니 후반으로 갈수록 생산성이 올라갔고, 유지보수도 훨씬 수월해졌습니다. 기획 단계에서의 커뮤니케이션이 코드 품질에 직접적인 영향을 준다는 것도 배웠습니다.
