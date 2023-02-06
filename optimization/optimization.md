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

### IntersectionObserver를 활용한 이미지 로딩최적화

```
IntersectionObserver를 활용하여 화면에 이미지 영역이 보여질 시점에 이미지를 로드한다.

<img data-src="이미지경로">
위 처럼 src 대신 data-src를 넣어서 영역만 잡게 한 뒤 isIntersecting일 경우에 dataset의 src를 이미지의 src값에 넣어준다

image.src = image.dataset.src
```

### 이미지 사이즈 최적화2

```
이미지 포맷
 png, jpg, webp

squoosh.app에서 webp형식의 이미지로 변환 가능

webp는 지원하지 않는 브라우저도 존재

picture태그를 활용하여 webp와 다른 포맷의 이미지를 분기처리할 수 있음

<picture>
  <source srcset="이미지경로" type"image/webp" >
  <img src="대체이미지경로" alt="">
</picture>
```

### 웹폰트 최적화

```
웹 폰트의 문제점

FOUT(Flash of Unstyled Text)
기본 폰트스타일에서 폰트 스타일이 적용되면서 깜빡이는 현상
FOIT(Flash of Invisible Text)
아무것도 없던 화면에서 스타일이 적용된 폰트가 나타나는 현상
```

최적화 방법

1. 폰트 적용 시점 컨트롤
2. 폰트 사이즈 줄이기

```
폰트 적용 시점 컨트롤

font-display 속성을 사용함
- auto
- block(FOIT)
- swap(FOUT)
- optional(FOIT)
- fallback(FOIT)
```

```
폰트 사이즈 줄이기

파일 크키
EOT > TTF/OTF > WOFF > WOFF2

@font-face {
  font-family : "폰트이름";
  src : url("폰트 경로") fotmat("포맷 형식"), url("폰트 경로") fotmat("포맷 형식");
  // 첫 번째 폰트가 안될 경우 콤마 이후의 폰트가 적용됨.

  src : local("폰트명"), url("폰트 경로") fotmat("포맷 형식");
  // local을 사용하게 되면 내 컴퓨터에 해당 폰트명이 있을 경우 폰트를 다운로드 받지 않고 적용됨.
}

Subset

일부 단어들에만 적용되는 폰트스타일은 해당 단어들만 폰트가 적용될 수 있게 해서 폰트파일의 사이즈를 줄일 수 있다.


Unicode-range

@font-face {
  // ...폰트 스타일
  unicode-range : "단어의 유니코드"
}

해당 단어가 unicode-range에 포함되지 않는 경우 폰트를 다운로드 받지 않음.

Data-uri

폰트의 값을 직접 font-face의 url값에 넣음

@font-face {
  font-family : "폰트이름";
  src : url("data-uri값") fotmat("포맷 형식")
}

Preload

webpack을 통하여 폰트를 미리 다운받을 수 있게 한다.
preload가 적용이 되면 perfomance탭에서 폰트파일이 먼저 다운로드가 진행되는 것을 확인할 수 있음.
```

### 캐시최적화

- 메모리 캐시
  - RAM
- 디스크 캐시
  - file
    네트워크 탭 -> size에서 확인 가능

```
캐시 option

no-cache: 캐시를 사용하기 전에 서버에서 검사 후, 사용 결정 (기존의 데이터를 사용할 수 있으면 기존의 데이터를 사용하고 데이터가 변경이 됐으면 변경된 데이터를 사용)
no-store: 캐시 사용 안함. (max-age: 0과 같다.)
public: 모든 환경에서 캐시 사용가능
private: 브라우저 환경에서만 캐시 사용, 외부 캐시 서버에서는 사용 불가
max-age: 캐시의 유효기간

브라우저에서 요청을 보낸 파일에 캐시를 적용하고 싶으면 서버에서 적용을 해서 내려줘야함.
해당 데이터가 캐시가 만료되면 다음 동일데이터 요청을 보낼 때 해당 데이터가 변경이 되었는지를 확인을 함.
동일 데이터일 경우 304 Not Modified(size 탭에서도 용량이 작은 것을 확인 가능), 아닐 경우 다운로드를 받음.

데이터가 변경이 되었는지 ETag를 이용하여 확인 가능.
```

### 사용하지 않는 css 제거

```
Pergecss 실행으로 build된 css파일에서 사용이 되지 않는 css를 제거한다.
```

### Layout Shift

```
원래 위치하고 있던 곳에서 다른 데이터의 로드로 인해 아래로 밀려나는 현상 -> layout, paint단계가 다시 실행됨. -> 효율이 떨어지고 유저가 다른 데이터를 클릭할 수 있는 상황이 발생함.

원인

- 사이즈가 정해져 있지 않은 이미지
- 사이즈가 정해져 있지 않은 광고
- 동적으로 삽인된 콘텐츠
- Web font
```

### Redux/CreateSelector

```
const {array} = useSelector((rootState) => ({
  array : rootState.type.type === true ?
    rootState.array.data.filter((data) => data.state === true) :
    rootState.array.data.filter((data) => data.state === false)
}),shallowEqual);

useSelector를 통해서 type이 true일 때 type이 true인 데이터만 필터링해서 사용하려고 한다.

하지만 filter함수는 새로운 함수를 리턴하므로 리덕스의 데이터가 변경되는 순간 무조건 렌더링이 된다.

그때 사용할 수 있는 방법이 createSelector이다.

npm install reselect
reselect 라이브러리를 받은 후

import {createSelector} from "reselect";
를 실행하여 createSelector를 가져온다.

const selectData = createSelector(
  [(rootState) => state.type.type, ]

  , )

```
