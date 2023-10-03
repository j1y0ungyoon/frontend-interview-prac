# tailwind에서 tailwind.config.js 파일의 목적이 무엇인가요?

이름처럼 tailwind의 css 설정 관련한 파일로, 프로젝트에 맞게 tailwind css를 커스터마이징할 수 있도록 합니다

content(purge): Tailwind className이 포함된 모든 HTML 템플릿, JS 컴포넌트, 그리고 다른 파일들의 경로를 설정하는 곳임.
즉, tailwind css를 사용할 파일들이 존재하는 경로를 적어줌

유틸리티 css 특성 상 모든 스타일 클래스를 미리 생성함. 때문에 파일크기가 큼 -> 그래서 content에 지정된 파일들 내에서 실제로 사용된 클래스만 빌드할 때 포함시키고 나머지 미사용 클래스들은 삭제함 -> css 번들 크기 줄임
purge에서 content로 이름 바뀜

theme: 시각적 디자인과 관련된 색상, 폰트, 브레이크 포인트 등을 정의하는 옵션

# border와 outline의 차이점을 설명해주세요

### border

- 어느 브라우저에서나 값이 동일하게 적용됨
- 특정 테두리(상,하,좌,우)에만 효과를 줄 수 있음
- width: 400, height: 300 인 div가 있을 때 border: 3px solid black을 한다면, 테두리가 덧붙여지기 때문에 총 크기는 400 _ 400이 아니라 406 _ 306이 됨
- 즉, border의 두께만큼 가로, 세로 사이즈가 증가함
- 따라서 css 렌더링의 과정인 layout-paint-composite 과정 중 layout에 영향을 미치고, 특정한 상태에 border-width에 transition을 준다면 layout이 매번 변경되어 화면이 다시 repaint되므로 성능에 좋지 않은 영향 줌

### outline

- IE7 이하에서 지원x
- border와 달리 상하좌우 모든 테두리에 적용됨
- - width: 400, height: 300 인 div가 있을 때 outline: 3px solid black을 한다면, border와 달리 총 크기는 400 \* 300 임
- border와 다르게 테두리만 생김(가로, 세로 사이즈에 영향x -> 레이아웃 변경 x)
- 하지만 세밀한 스타일링을 할 수 없음(상하좌우 다 적용되기 때문에)

=> 이를 개선하기 위해선 어떻게 해야할까?
outline으로 css를 만지기엔 너무 세밀한 작업을 할 수 없으므로
border를 width를 설정한 후 색상을 tranparent로 두고, 특정 상태가 변했을 때 색깔만 변경시켜주면 됨

```
border-bottom: 2px, solid, transparent

&:focus {
    border-bottom-color: red
}
```

# tailwind에서 accent color에 대해서 설명해주세요

사용자 인터페이스 컨트롤에서 강조하는 색깔을 변경하는 css 속성임.
예를 들면 체크박스를 눌렀을 때 체크박스 속 색이 변하는 등이 있음

여기서 말하는 사용자 인터페이스 컨르롤이란 아래의 HTML 요소를 말함

- '<input type="checkbox">'
- '<input type="radio">'
- '<input type="range">'
- '<progress>'

```
<style type="text/css">
    .test {
        accent-color: red;
    }
</style>
<input type="radio" checked />
<input type="radio" class="test" checked />
```

그런데 이 속성을 사용하다보면 원하는 색상이 안 나오는 경우가 있음
이럴 경우에는 [여기](https://accent-color.glitch.me/)에서 체크 표시가 흰색인 색상을 골라서 사용하면 배경이 까맣게 되지 않음

# 부모요소에 호버할 때, 자식요소도 hover효과가 나타나게 하려면 tailwind 로 어떻게 하면 되나요?

group이라는 속성을 부모 요소에게 주고 자식 요소에 group-hover:text-red 이렇게 설정하면 부모요소를 호버했을 때 자식요소도 같이 호버효과가 나타남

```
// 예시
<div class="group flex items-center">
  <img class="shrink-0 h-12 w-12 rounded-full" src="..." alt="" />
  <div class="ltr:ml-3 rtl:mr-3">
    <p class="text-sm font-medium text-slate-700 group-hover:text-slate-900">...</p>
    <p class="text-sm font-medium text-slate-500 group-hover:text-slate-700">...</p>
  </div>
</div>
```

# tailwind 속성 중 aspect-ratio에 대해서 설명해주세요

[여기](https://www.google.com/search?sca_esv=570220637&rlz=1C5CHFA_enKR1067KR1067&sxsrf=AM9HkKkFH_WImtthCPCr9WtlYmwHLtzSAw:1696301879356&q=Tailwind+aspect+example&sa=X&ved=2ahUKEwj2pc338NiBAxWBp1YBHWaGD30Q1QJ6BAg9EAE&biw=1512&bih=827&dpr=2#fpstate=ive&ip=1&vld=cid:6a427ad9,vid:NX_NW6bt6_s,st:363)를 참고!

**참고**

- https://blog.naver.com/PostView.naver?blogId=iyakiggun&logNo=100159740947
- https://velog.io/@awesome-hong/border-vs-outline-%EC%B0%A8%EC%9D%B4%EC%95%8C%EA%B3%A0-%EC%9D%B4%EC%8A%88%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0
- https://developer.mozilla.org/en-US/docs/Web/CSS/accent-color
- https://ksyy.tistory.com/180
- https://tailwindcss.com/docs/accent-color
- https://tailwindcss.com/docs/hover-focus-and-other-states#differentiating-nested-groups
-
