# React
> 사용자 정의 태그(컴포넌트)를 만드는 기술

리액트의 핵심적 역할은 사용자 정의 태그(부품)을 만드는 것이다.

## props
> 컴포넌트가 가진 속성

```javascript
function Nav(props) {
    const lis = []
    for(let i=0; i<props.topics.length; i++>) {
        let t = props.topics[i];
        lis.push(<li key={t.id}><a href={'/read/'+t.id}>{t.title}</a></li>)
    }
}
...
function App() {
    const topics = [
        {id:1, title:'html', body:'html is...'},
        {id:2, title:'css', body:'css is...'},
        {id:3, title:'javascript', body:'javascript is...'}
    ]
    return (
        <div>
            <Header title="WEB"></Header>
            <Nav topics={topics}></Nav>
            <Article title="Welcome" body="Hello, WEB"></Article>
    )
}
```

위의 코드에서 props가 인수로 전달되어 for문을 거쳐 랜더링 된다. 이때 리액트는 자동으로 생성한 태그의 경우 태그를 추적해야 하는데 key라는 약속된 prop을 부여함으로 리액트가 성능을 높이고 정확한 동작을 하는데 도움을 준다

## event
> 어떤 이벤트가 발생했을대 사용자가 추가적 작업을 처리할 수 있도록 한다

```javascript
function Header(props) {
    return <header>
    <h1><a href="/" onClick={function(event)=>{
        event.preventDefault();
        props.onChangeMode();
    }}>{props.title}</a></h1>
}
...
function App() {
    const topics = [
        {id:1, title:'html', body:'html is...'},
        {id:2, title:'css', body:'css is...'},
        {id:3, title:'javascript', body:'javascript is...'}
    ]
    return (
        <div>
            <Header title="WEB" onChangeMode={function(){
                alert('Header');
            }}></Header>
            <Nav topics={topics}></Nav>
            <Article title="Welcome" body="Hello, WEB"></Article>
    )
}
```

prop을 통해 onChangeMode함수를 전달하고 이후 props.onChangeMode();로 실행한다.  
event.preventDefault(); -> 기본동작 방지  
target = 이벤트를 유발시킨 태그를 가리킨다

## state
> 컴포넌트를 만드는 내부자를 위한 데이터

prop과의 차이 = prop은 컴포넌트를 사용하는 외부자를 위한 데이터이다.

```javascript
const [mode, setMode] = useState('WELCOME');
...
function App(){
    ...
    return (
        <div>
            <Header title="WEB" onChangeMode={()=>{
                setMode('WELCOME');
            }}></Header>
            <Nav topics={topics} onChangeMode={()=>{
                setMode('READ');
            }></Nav>
            {contnet}
    )
}
```

- useState는 배열을 리턴한다.  

- useState의 0번째 원소 = 상태의 값을 읽을때 쓰는 데이터.  

- useState의 1번째 원소 = 그 상태의 값을 변경할때 쓰는 함수.  

- const [mode, setMode] = useState("WELCOME");와 같은 형태로 0번째 원소와 1번째 원소를 정의한다.  
- useState의 인자값 = 초기값 ex) useState("WELCOME");의 초기값은 "WELCOME"  

- 값을 바꿀때 = setMode('READ');와 같은 형태로 넣는다 --> App()을 다시 실행하며 mode의 값을 READ로 변경한다.  

- 원시타입이 아닌 state 변경 = 객체의 경우 {...x} 배열의 경우 [...x]로 복사한 이후 추가할 값을 넣고 setState한다.

Reference: [생활코딩 React](https://www.youtube.com/watch?v=AoMv0SIjZL8&list=PLuHgQVnccGMCOGstdDZvH41x0Vtvwyxu7&index=1)