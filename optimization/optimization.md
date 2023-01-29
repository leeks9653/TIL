# 최적화

### 성능최적화가 필요한 이유?

- 사용자가 떠나지 않도록 하기 위해 = 수익 증대를 위해
- 개발자로서 경쟁력을 갖추기 위해

#### 로딩성능

- html, css, js 같은 파일을 받아오는 성능

#### 렌더링 성능

- 화면에 받아온 데이터를 화면에 보여주는 성능  
  </br>
  </br>
  </br>

# 성능 측정

### lighthouse

```
상단의 점수는 Metrics에서 나온 점수들을 바탕으로 결정된다.

하단의 스크린샷은 페이지가 나타나는 과정을 보여줌

Oppertunities는 로딩성능 최적화와 관련된 항목
Diagnostics는 렌더링성능 최적화와 관련된 항목

붉은색 마크는 최우선으로 해결해야할 문제

Passed audits는 잘 적용되어 있는 항목

Runtime Settings는 성능을 측정하는데 사용된 환경 정보
```

</br>
</br>
</br>

# 최적화 적용

### 이미지 사이즈 최적화

```
파일 내부가 아닌 api처럼 외부 경로를 통하여 불러와지는 이미지는 이미지 CDN을 활용하여 이미지를 최적화 할 수 있음.

이미지 CDN란?
CDN + 이미지 가공
```

### Code Split

```
webpack-bundle-analyzer 라는 라이브러리를 통하여 번들된 파일들의 정보를 블럭형태로 볼 수 있음.

Suspense, lazy를 활용하여 code spliting을 할 수 있음.
lazy가 적용되면 해당 컴포넌트가 바로 불러와지는 것이 아니라 필요한 시점에서 불러와짐.
네트워크 탭에서도 추가적인 다운로드가 진행되는 것을 확인할 수 있음.

lazy를 적용한 컴포넌트를 사용하는 부모 컴포넌트에서는 해당 lazy가 적용된 컴포넌트가 불러와지기 전까지 적용될 fallback ui를 Suspense에 props로 넘겨줘야함.


```

### 텍스트 압축

```
주로 Gzip 이나 Deplate를 활용하여 텍스트 압축을 한다.

Gzip은 내부적으로 Deplate를 사용하고 그 외에 더 많은 기능들을 추가적으로 넣어서 사용하기 때문에 Gzip이 성능이 더 좋다.

2kb이상인 파일에서만 압축을 실행한다.
파일을 압축하는 시간과 압축을 해제하는데도 시간이 필요하기 때문에 모든 파일을 압축하는 것이 아닌 압축이 필요한 파일에만 압축을 실행.

```

### Bottleneck 코드 최적화

```
perfomance탭에 있는 프레임차트를 활용한다.


화면에 페이지가 보여지는 과정

1. 해당 url에 접속
2. html파일을 다운받음
3. bundle.js , chunk.js를 다운받음
4. html파일 다운로드가 끝나면 html파일 파싱
5. js파일들이 다운받아지면 js파일을 실행
6. js파일들이 적용되면 first paint적용


react는 Timings라는 영역에서 추가적인 정보를 볼 수 있음


Main영역

Main영역에서는 실행되는 함수 이름을 볼 수 있음
MinorGC = 가비지 컬렉터


프레임차트를 보고 시간이 오래 걸리는 코드를 수정하여 렌더링 성능을 증가시킬 수 있음
```

### 애니메이션 최적화

```
Reflow, Repaint

Reflow
Critical Rendering Path의 모든 단계를 실행 (width, height, position, padding, margin, border, display, float, font, overflow 등등)

Repaint
Critical Rendering Path에서 Layout 단계를 건너뛰고 실행 (background, 색상, outline 등등)

GPU
Critical Rendering Path에서 Layout, Paint 단계를 건너뛰고 실행 (transform, opacity)
```

### 컴포넌트 Preloading

```
lazy loading의 단점

해당 컴포넌트가 필요할 때 해당 컴포넌트를 다운로드 받음 -> 모달이 열리기 전까지 시간이 걸림

해결방안

해당 컴포넌트가 필요해서 불러오기 직전에 미리 로딩을 해두면 됨 -> 사용자가 언제 해당 컴포넌트를 팰요할 지 모름

해결방안

1. 사용자가 액션을 가하기 직전 ex) 버튼을 눌렀을 때 열리는 모달의 경우에는 버튼 위에 마우스를 올려놨을 때 컴포넌트를 받음.

2. 최초의 페이지가 로드가 되고 모든 컴포넌트의 마운트가 끝났을 때
```

### 이미지 Preloading

```
이미지를 다운받아오는 시점
src = "이미지의 주소" 코드가 읽혀지는 순간.

src = "이미지의 주소" 가 읽혀 이미지가 다운받아진 상태에서 또 src = "이미지의 주소"가 읽혀지면 한번 더 다운받아진다. -> 캐시를 확인

캐시 속성
- max-age = 100  => 100초동안 캐싱한다.
```
