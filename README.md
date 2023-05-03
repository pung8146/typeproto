# TypeScript와 styped-component

## interface

interface란 object shape(객체 모양)을 TS에 설명해주는 TS의 개념

```tsx
// 기본형태
const x = (a: number, b: number) => a + b;

// 1번
interface CircleProps {
  bgColor: string;
  // interface 안에 어떤 타입인지 설명해준다
}

//  2번
function Circle({ props }: CircleProps) {
  return <div bgColor={props}></div>;
  // props 라고 적어도됨
}
// 3번
function Circle({ bgColor }: CircleProps) {
  return <div bgColor={bgColor}></div>;
}
```

## stlyed Component에 ts 사용법

container 가 받는 props를 TS에게 잘 설명해주는 방식

```tsx
// 1번 방식
// 스타일드 컴포넌트를 사용한다면
// 보통 스타일을 정의할때
interface ContainerProps{
    bgColor: string;
}
const Container = styled.div<ContainerProps>`
    background-color: ${(props) => props.bgColor}
`
// 2번 방식
// 컴포넌트가 props를 받을때 사용하는것이 일반적
interface CircleProps {
    bgColor: string
}

function Circle({bgColor} : CircleProps) {
    return <Container bgColor={bgColor}>
}
```

## 선택적 type

타입은 다 required 속성을 가지고있지만
선택적으로 주어야할때는

```tsx
interface Cirlce {
  borderColor?: string;
  // ? 끝에 붙입니다
}
```

## default type

컴포넌트 뒤에 ?? 두개를 붙여줍니다

```tsx
//  1번째 방법
function Circle({bgColor} : CircleProps) {
    return <Container bgColor={bgColor} borderColor={borderColor ?? "white"}>

    // borderColor undefind일때 bgColor 같게 해준다
    return <Container bgColor={bgColor} borderColor={borderColor ?? bgColor}>
}
```

```tsx
// 2번째 방법
function Circle({bgColor , borderColor = "white"} : CircleProps) {
    return <Container bgColor={bgColor} borderColor={borderColor}>
}

```

## useState<타입1|타입2> 로 타입의 종류를 늘려줄 수 있다.

```tsx
function Circle({bgColor, borderColor}: CircleProps) {
    const[counter,setCounter] = useState<number|string>(1)
    setCounter('hello')

    return <Container bgColor={bgColor} borderColor={borderColor}>
}
```

## Form

함수 onChange 를 inputElement 에 의해서 실행되게 하는 방법
ReactJs Typescript 에선 currentTarget 사용을 택함

```tsx
function App() {
  const [value, setValue] = useState("");
  const onChange = (event: React.FormEvent<HTMLInputElement>) => {
    console.log(event.currentTarget.value);
    // event:React.FormEvent 같이 필요한 이벤트 변수는
    // 공식문서와 구글링으로 필요할때 찾아야한다
    const {
      currentTarget: { value },
    } = event;
    setValue(value);
    // 구조 분해할당과 객체 속성 축약의 조합으로 이루어진 코드
  };
  const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log("hello", value);
  };
}
```

## theme 을 사용하기 위한방법

- styled.d.ts파일을 src폴더내에 만들어준다
  styledComponent 를 import 하고 defaultTheme을 지정해준다
- src내 theme.ts를 만든다
