## 개요

- css 학습을 위한 프로젝트

## 디자인 4규칙

- fluid layout : % vh 사용, max-width 사용
  - float : 오래된 다자인 기술이지만 많은 웹 페이지가 이 방식으로 작성 되 있으므로 알아야함
    - calc 로 넘비와 간격을 싹다 계산해 주는 방식으로 접근해서 코드를 계속 재활용 해야함
  - flex, grid : 현재 많이 사용하는 디자인 기술
- response unit : rem 사용
- flexible image : %, max-width 사용,
- media query : 기기별 다른 크기

## 몰랐던 css 및 디자인 방법

- scale 이나 translate 할 경우

  - 가까워 지면 그림자가 진하고 더 넓은 offset
  - 멀어지면 연하고 짧은 offset

- z-index 는 상대적인 것이다.

  - 무엇보다 높이고 싶다. 면 그 무엇에도 z-index 를 넣어줘야 한다.
  - 띡 높이고 싶다고 높일 거에만 z-index 붙이면 나중에 오작동 한다.

- 호버되지 않는 나머지 요소 선택

  ```
      &:hover &__photo:not(:hover) {
    transform: scale(0.95);
  }
  ```

- 글자 색상에 그라이데이션 주는 트릭은 다음과 같다

```
  .feature-box-icon {
    //블록 안되면 약간 텍스트나 아이콘이 씹힌다.
    display: inline-block;
    background-image: linear-gradient(
      to right,
      rgba($light-green, 0.8),
      rgba($darkgreen, 0.8)
    );
    -webkit-background-clip: text;
    font-size: 6rem;
    color: transparent;
  }
```

- 레이아웃을 기울이는 트릭은 다음과 같다.

방법1

```
  //주의사항 : 꼭 직계자식만 적용해야 제대로 기울기가 적용됨
  transform: skewY(-10deg);

  & > * {
    transform: skewY(10deg);
  }
```

방법2

```
  clip-path: polygon(0 0, 100% 0, 100% 75vh, 0 100%);
```

- 카드를 회전하는 데 자연스럽게 진짜 뒤집어 지는 것 처럼 만드는 트릭

```
  .card {
  //perspective 를 사용하면된다
  //rem 수치에 따라 엄청난 변화가 있다 낮을 수록 나에게 가까이 멀 수록 멀리
  perspective: 150rem;
  -moz-perspective: 150rem;

    &-side {
      padding: 3rem 1.3rem;
      text-align: center;
      background-color: #f7f7f7;
      border-radius: 5px;
      box-shadow: 0 1.2rem 4rem rgba($color-black, 0.2);
      transition: all 0.8s linear;
    }

    &-icon {
      font-size: $icon-size;
    }

    &:hover &-side {
      transform: rotateY(180deg);
    }
  }

```

- 카드를 회전할때 뒷면을 숨겼다가 다시 살리는 트릭

```
.card {
  perspective: 150rem;
  -moz-perspective: 150rem;

  &-side {
    position: absolute;
    width: 100%;
    padding: 3rem 1.3rem;
    text-align: center;
    background-color: #f7f7f7;
    border-radius: 5px;
    box-shadow: 0 1.2rem 4rem rgba($color-black, 0.2);
    transition: all 0.8s;

    //이 것이 핵심임 평상시에는 뒤에 있는 요소를 숨기다가 애니메이션이 종료된 후
    backface-visibility: hidden;

    &--back {
      background-color: red;
      transform: rotateY(180deg);
    }

    &--front {
      background-color: green;
    }

    &--icon {
      font-size: $icon-size;
    }
  }

  &:hover &-side--back {
    transform: rotateY(0deg);
  }

  &:hover &-side--front {
    transform: rotateY(180deg);
  }
}

```

- 줄이 바뀌면 알아서 요소가 클론된다.

  ```
      &__heading {
        &-1 {
          padding: 1rem 1.5rem;

          //바로 이 속성
          -webkit-box-decoration-break: clone;
          background-image: linear-gradient(
            to right bottom,
            rgba($color-orange-dark, 0.5),
            rgba($color-orange-dark, 0.5)
          );
        }
      }
  ```

- 애니메이션이 약간 흔들리면 이 코드를 추가한다(뒷 요소 숨김)
- 애니메이션 발생할때 뒷 배경이 조금 보이거나 약간 깜박인다 그러면 사용한다.
- 혹시 이게 안먹히면 overflow:hidden 까지 추가하면 작동하더라

```
  backface-visibility: hidden;
  overflow: hidden;
```

- blur 는 흐림 효과 px 단위로 작게 주면 좋다.
- brightness 는 백라이트 효과 100 보다 작으면 어두워지고 크면 밝아진다.

```
  filter: blur(3px) brightness(80%);
```

- video 는 배경으로도 사용 할 수 있다.
- 구 exproler 에서는 작동 안하므로 소스 마지막에 오류 시 메세지를 남긴다.

```
//index.html
  <div class="bg-video">
    <video class="bg-video-content" autoplay muted loop>
      <source src="img/video.mp4" type="video/mp4" />
      <source src="img/video.webm" type="video/webm" />
      지원하지 않는 브라우저 입니다.
    </video>
  </div>

//video.scss
.bg-video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  user-select: none;
  opacity: 0.45;
  overflow: hidden;
  &-content {
    height: 100%;
    width: 100%;

    //이전에는 넓은 쪽으로 100% 줬는데 그냥 이렇게 해도 좋다.(비율 유지하며 꽉채움(커버 효과)
    object-fit: cover;
  }
}



```

- 배경화면을 사선으로 하는 방법
- 위에 있는 것 처럼 clip-path:로 할 수도 있고
- 아래처럼 간단한 가운데 사선 정도는 그라디에이션으로도 가능하다

```
  .book {
    height: 50rem;
    background-image: linear-gradient(
        // 기울기 각도
        105deg,

        //색상과 색상이 차지하는 %
        rgba($color-white, 0.8) 0%,

        //가운데 사선을 위해서 작성하는 트릭 50% 는 흰색으로 가자
        rgba($color-white, 0.8) 50%,

        //나머지 50%는 orangered 로 꽉 채워라.
        orangered 50%
      ),
      url("/img/nat-10.jpg");
    background-size: 100%;

    border-radius: 3px;
    box-shadow: 0 1.5rem 4rem rgba($color-black, 0.15);
  }
```

- 레이아웃 팁 하나 height 가 고정되면 size 를 % 단위로 조정해서 이미지를 표현 가느앟다

```
  //메트릭스 영화에 보면 tv에 화면이 몇백개로 분할되는 장면 나오는데 그게 가능해 진다.
  //1. height 정하기   2. background 정하기  3. 그 baackground 를 몇%로 몇분할 할건지 정하기

  - 배경화면을 사선으로 하는 방법
- 위에 있는 것 처럼 clip-path:로 할 수도 있고
- 아래처럼 간단한 가운데 사선 정도는 그라디에이션으로도 가능하다
  .book {
    height: 50rem;
    background-image: linear-gradient(
        105deg,
        rgba($color-white, 0.8) 0%,
        rgba($color-white, 0.8) 50%,
        orangered 50%
      ),
      url("/img/nat-10.jpg");
    background-size: 100%;

    border-radius: 3px;
    box-shadow: 0 1.5rem 4rem rgba($color-black, 0.15);
  }
```

- 형제 선택자 는 ~(위치 상관없이 같은 레벨), +(바로 요소 바로 다음에 있어야만 작동함) 이 있다.
- javascript 없이 일부 유효성 검사 가 제법 그럴듯 하게 가능해 진다

```
  &-input:placeholder-shown + .form-label {
    opacity: 0;
    visibility: hidden;
    transform: translateY(-10px);
  }
```
