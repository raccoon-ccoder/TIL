## [React + React-Query] QueryClientProvider에 IntrinsicAttributes 에러가 발생할 때 해결 방법
`21.04.16`일자 기준으로 netflex 클론 코딩 프로젝트에선 React 18, React-Query 3.34.17 을 사용하고 있다.    
React 17과 React 18의 FunctionComponent의 인터페이스가 달라졌기 때문에 발생하는 에러다. 

### React 17에서 Function Component 인터페이스
```js
interface FunctionComponent<P = {}> {
        (props: PropsWithChildren<P>, context?: any): ReactElement<any, any> | null;
        propTypes?: WeakValidationMap<P> | undefined;
        contextTypes?: ValidationMap<any> | undefined;
        defaultProps?: Partial<P> | undefined;
        displayName?: string | undefined;
    }
```

### React 18에서 Function Component 인터페이스
```js
interface FunctionComponent<P = {}> {
        (props: P, context?: any): ReactElement<any, any> | null;
        propTypes?: WeakValidationMap<P> | undefined;
        contextTypes?: ValidationMap<any> | undefined;
        defaultProps?: Partial<P> | undefined;
        displayName?: string | undefined;
    }
```
QueryClientProvider의 인터페이스가 React 17 기준으로 맞춰서 있기 때문에 발생한다.

<br/>

방법은 React 17버전으로 낮추거나 React Query v4를 사용하는 방법도 있지만 아직은 베타 버전이기에 Function Component 인터페이스를 다시 React 17 기준으로 변경한다.

### 1. 프로젝트 root 위치에 `index.d.ts`파일 생성
### 2. 아래 코드 삽입
```js
import "react";

declare module "react" {
  export type FC<P = {}> = FunctionComponent<P>;
  export interface FunctionComponent<P = {}> {
    (props: PropsWithChildren<P>, context?: any): ReactElement<any, any> | null;
    propTypes?: WeakValidationMap<P> | undefined;
    contextTypes?: ValidationMap<any> | undefined;
    defaultProps?: Partial<P> | undefined;
    displayName?: string | undefined;
  }
}
```
그러면 해당 타입에 대해 업데이트 할 것인지에 대한 팝업창이 뜨고 허용하면 오류가 해결되며 `tsconfig.json`파일은 다음과 같이 코드가 추가된다.
```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": ["index.d.ts"] /// (*)
}
```

### 참고 자료
[QueryClientProvider type issue after upgrading to React 18](https://github.com/tannerlinsley/react-query/issues/3476)





