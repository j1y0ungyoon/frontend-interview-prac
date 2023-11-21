# useEffect, useState, useRouter, useCallback, useMemo 의 등장배경 / 목적 / 구체적인 사용법 / 주의사항에 대해 작성해주세요.(1000자)

## 1. useState

- 컴포넌트가 가질수 있는 상태 이며 상태를 생성하고 업데이트 할 수 있다. 업데이트 될때마다 상태가 변경되어 화면이 재렌더링 된다.
- useState 는 state와 setState를 배열형태로 리턴해준다.
- state는 현재의 상태 값이들어있고 setState함수로 state를 변경할 수 있다.
- state를 변경할때 이전 state값과 연관 되어 있다면 setState의 인자로 새로운 state 를 리턴하는 콜백함수를 넣어 주는게 좋다
- 만약에 useState의 기본값이 무거운 작업 일경우 콜백함수로 넣어 주면 초기에 무거운 작업을 불러오고 상태가 변경될 때만 업데이트 된다.

### useState 를 사용 하는 이유 !

DOM렌더링과 관련 있는데 일반변수를 사용 하게 되면 렌더링 될때마다 초기화 되지만 state변수를 사용하는 경우 값을 반영하여 업데이트 해주기 때문이다.

함수 컴포넌트는 state변경으로 리렌더링 되면 컴포넌트 내부에 있는 변수들이 모두 초기화 되는데 기존의 컴포넌트가 새로고침 되는 것이 아니기 때문에 state 의 최신상태로 새로운 컴포넌트로 갱신해 주는 것에 더 가깝다.

**기존의 state를 복사하여 새롭게 state를 갱신해 주기 때문에 객체의 불변성을 지킬 수 있고, 발생할 오류들을 미리 방지할 수 있다.**

**state가 변경 되면서 알아서 리렌더링 해주기 때문에 훨씬 간결하고 효율적인 상태 관리가 가능하다.**

```jsx

const heavyWork =() =>{
	console.log('엄청 무거운 작업')
	return ['홍길동', '김민수']
}


function App() {
	const [names, setNames] = useState(()=>{
		return  heavyWork()
	})
	const [input, setInput] = useState('');

	//input value
	const handleInpuChange = (e) =>{
		setInput(e.target.value)
	}
	//이름목록
	const handleUpload =()=>{
		setNames((preveState)=>{
			console.log('이전 state' ,preveState) // 홍길동, 김민수
			return [input, ...preveState]
		})
	}

	return (
		<div>
			<input type="text" value={input} onChange={handleInputChange}/>
			<button onClick={handleUpload}>Upload</button>
			{names.map(name,idx)=>{
				return <p key={idx}>{name}</p>
			}}
		</div>
	)
}

```

## 2. useEffect
