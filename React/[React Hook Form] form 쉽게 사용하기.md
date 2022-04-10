## [React Hook Form] form 쉽게 사용하기 
React Hook Form은 React에서 Form을 쉽게 만들기 위한 라이브러리로 공식문서에 적혀있는 그대로 성능이 좋고 유연하며 유효성 검사에 아주 탁월하다.

### 1. 설치
```js
npm install react-hook-form
```

### 2. useForm
React Hooks Form을 적용하고 싶다면 먼저 `useForm`이라는 hook을 불러와야 한다.   
React Hooks Form의 거의 모든 기능은 이 hook에서 나오는데 그 중 몇가지만 알아도 React Hooks Form의 대부분의 기능을 사용할 수 있다.
```js
const { register, handleSubmit, formState: { errors } } = useForm();
```

#### (1) register & watch
```js
import React from "react";
import { useForm } from "react-hook-form";

export default function App() {
  const { register, watch } = useForm();
  console.log(watch());
  return (
    <div className="App">
      <form>
        <input type="text" placeholder="Write to Do" {...register("toDo")} />
        <input type="submit" />
      </form>
    </div>
  );
}
```
<img width="378" alt="스크린샷 2022-04-09 오후 10 01 55" src="https://user-images.githubusercontent.com/77538818/162579237-54fe412d-54d2-4bfc-b7dd-8e7f7cc12ac1.png">    

<br/>

- `register`:(name: string, RegisterOptions?) => ({ onChange, onBlur, name, ref })   
  - input 등록시 element를 선택하고 유효성 검사 규칙을 React Hook Form에 적용할 수 있다.    
  - 반환값은 총 4개로 input username의 props로 설정하였다.      
- `watch`: (names?: string | string[] | (data, options) => void) => unknown      
  - form의 `입력값들의 변화를 관찰`하는 함수로 해당 값들을 반환한다.    
  - input 값을 렌더링하고 조건에 따라 무엇을 렌더링할지 결정하는데 유용하다.     
  - 위 코드처럼 toDo input 입력시 watch 함수를 통해 업데이트되는 value 값을 확인할 수 있다.

#### (2) handleSubmit
```js
import { useForm } from "react-hook-form";

function ToDoList() {
    const { register, handleSubmit } = useForm();
    const onSubmit = (data:any) => {
      console.log(data);
    };
    const onError = (error:any) => {
      console.log(error);
    };
    return (
      <div className="App">
        <form onSubmit={handleSubmit(onSubmit, onError)}>
          <input
            type="text"
            placeholder="Write a ToDo"
            {...register("todo", {
              minLength: {
                value: 5,
                message: "ToDo must be longer than 5 characters"
              },
              required: "ToDo must be written"
            })}
          />
          <input type="submit" />
        </form>
      </div>
    );
}
```
[유효성 검사에 성공한 경우 콘솔창]    
<img width="252" alt="스크린샷 2022-04-09 오후 11 57 51" src="https://user-images.githubusercontent.com/77538818/162579577-d13960e2-4f62-4f43-bc17-1886e3aa859b.png">  
[유효성 검사에 실패한 경우 콘솔창]     
<img width="391" alt="스크린샷 2022-04-09 오후 11 59 21" src="https://user-images.githubusercontent.com/77538818/162579579-80166d7b-87bd-4c61-9b4b-a60487922b89.png">       

  
- `handleSubmit`: ((data: Object, e?: Event) => void, (errors: Object, e?: Event) => void) => Function
  - form 유효성 검사가 성공하면 form 데이터를 받는다. 
  - 첫번째 인자는 유효성 검사 성공시 실행할 함수(`onSubmit`), 두번째는 유효성 검사 실패시 실행할 함수(`onError`)로 선택적이다.
  - onSubmit은 form 데이터(`data`)를 전달받을 수 있고, onError는 에러(`error`)를 인자로 받을 수 있다.
  - 유효성 검사에 성공한 경우 입력한 value가 정상적으로 출력된다.
  - 유효성 검사에 실패한 경우 error 객체를 보면 어떤 항목인지와 설정한 메세지를 확인할 수 있다.

### 정규 표현식에 따른 유효성 검사
```js
import React, { useState } from "react";
import { useForm } from "react-hook-form";

function ToDoList() {
    const { register, watch, handleSubmit, formState } = useForm();
    const onValid = (data:any) => {
        console.log(data);
    };

   console.log(formState.errors); // (*)
    return (
        <div>
            <form style={{display: "flex", flexDirection: "column"}} onSubmit={handleSubmit(onValid)}>
                <input 
                    {...register("email", {
                        required:true,
                        pattern: { // (*)
                            value: /^[A-Za-z0-9._%+-]+@naver.com$/,
                            message: "Only naver.com emails allowed",
                        }
                    })} 
                    placeholder="Email" 
                />
                <input 
                    {...register("firstName")} 
                    placeholder="First Name" 
                />
                <input 
                    {...register("lastName")} 
                    placeholder="Last Name" 
                />
                <input 
                    {...register("userName", {required:true, minLength: 10})} 
                    placeholder="Username" 
                />
                <input 
                    {...register("password", {
                        required:"Password is Required", 
                        minLength: {
                            value: 5,
                            message: "Your Password is short"
                        },
                    })} 
                    placeholder="Password" 
                />
                <button>Add</button>
            </form>
        </div>
    );
}
```
[이메일 유효성 검사에 실패한 경우 콘솔창]      
<img width="592" alt="스크린샷 2022-04-10 오전 12 18 28" src="https://user-images.githubusercontent.com/77538818/162580285-0556055a-9cdf-473c-a27d-3ee47f94aa50.png">    
- 유효성 검사 설정으로 required나 minLength 같은 옵션을 설정해도 되지만 특정 유효성 검사를 하고 싶은 경우 정규표현식을 사용할 수 있다.
- email 입력란의 경우 `pattern`을 통해 네이버 메일만 입력하도록 설정하였다.


### (3) setError
```js
import React, { useState } from "react";
import { useForm } from "react-hook-form";

interface IForms {
    email: string;
    firstName?: string;
    lastName?: string;
    userName: string;
    password: string;
    rePassword: string;
    extraError?: string;
}

function ToDoList() {
    const { 
        register, 
        watch, 
        handleSubmit, 
        formState:{errors}, 
        setError // (*)
    } = useForm<IForms>({
        defaultValues: {
            email: "@naver.com",
        }
    });

    const onValid = (data:IForms) => {
        // 특정 조건에 따른 에러 발생
        if(data.password !== data.rePassword) { 
            return setError( // (*)
                "rePassword", 
                { message: "Password are not same" },
                { shouldFocus: true}    // 에러 발생시 해당하는 인풋란에 focus 하고 싶은 경우
            );
        }
        // 특정 조건이 아닌 form 전체에 에러 발생 시키기 ex : 서버 문제 등...
        return setError("extraError", { message: "Server offline" }); // (*)
        console.log(data);
    };

    return (
        <div>
            <form style={{display: "flex", flexDirection: "column"}} onSubmit={handleSubmit(onValid)}>
                <input 
                    {...register("email", {
                        required: "Email required",
                        pattern: {
                            value: /^[A-Za-z0-9._%+-]+@naver.com$/,
                            message: "Only naver.com emails allowed",
                        }
                    })} 
                    placeholder="Email" 
                />
                <span>
                    {errors?.email?.message}
                </span>
                <input 
                    {...register("firstName")} 
                    placeholder="First Name" 
                />
                <span>
                    {errors?.firstName?.message}
                </span>
                <input 
                    {...register("lastName")} 
                    placeholder="Last Name" 
                />
                <span>
                    {errors?.lastName?.message}
                </span>
                <input 
                    {...register("userName", {
                        required: "username reqiured", 
                        minLength: {
                            value: 5,
                            message: "Your Username is short"
                        }
                    })} 
                    placeholder="Username" 
                />
                <span>
                    {errors?.userName?.message}
                </span>
                <input 
                    {...register("password", {
                        required:"Password is Required", 
                        minLength: {
                            value: 5,
                            message: "Your Password is short"
                        },
                    })} 
                    placeholder="password" 
                />
                <span>
                    {errors?.password?.message}
                </span>
                <input 
                    {...register("rePassword", {
                        required:"Write password again", 
                        minLength: {
                            value: 5,
                            message: "Your Password is short"
                        },
                    })} 
                    placeholder="re-password" 
                />
                <span>
                    {errors?.rePassword?.message}
                </span>
                <button>Add</button>
                <span>
                    {errors?.extraError?.message}
                </span>
            </form>
        </div>
    );
}
```
- `setError`:(name: string, error: FieldError, { shouldFocus?: boolean }) => void
  - 오류를 수동으로 설정할 수 있는 함수
  - `shouldFocus` - 오류 발생시 해당 input에 focus가 된다.
- 위 코드에선 입력한 비밀번호들이 일치하지 않다면 오류를 발생시켰고 그와 별개로 서버에 문제가 있다는 가정 하에 특정 조건이 아닌 form 전체에 에러를 발생시켰다.

### (4) validate
```js
import React, { useState } from "react";
import { useForm } from "react-hook-form";

interface IForms {
    email: string;
    firstName?: string;
    lastName?: string;
    userName: string;
    password: string;
    rePassword: string;
    extraError?: string;
}

function ToDoList() {
    const { 
        register, 
        watch, 
        handleSubmit, 
        formState:{errors}, 
        setError 
    } = useForm<IForms>({
        defaultValues: {
            email: "@naver.com",
        }
    });

    const onValid = (data:IForms) => {
        // 특정 조건에 따른 에러 발생
        if(data.password !== data.rePassword) {
            return setError(
                "rePassword", 
                { message: "Password are not same" },
                { shouldFocus: true}    // 에러 발생시 해당하는 인풋란에 focus 하고 싶은 경우
            );
        }
        // 특정 조건이 아닌 form 전체에 에러 발생 시키기 ex : 서버 문제 등...
        return setError("extraError", { message: "Server offline" });
        console.log(data);
    };

    return (
        <div>
            <form style={{display: "flex", flexDirection: "column"}} onSubmit={handleSubmit(onValid)}>
                <input 
                    {...register("email", {
                        required: "Email required",
                        pattern: {
                            value: /^[A-Za-z0-9._%+-]+@naver.com$/,
                            message: "Only naver.com emails allowed",
                        }
                    })} 
                    placeholder="Email" 
                />
                <span>
                    {errors?.email?.message}
                </span>
                <input 
                    {...register("firstName", {
                        required: "firstName required",
                        validate: (value) => !value?.includes("raccoon") || "Don't include raccoon", //(*)
                    })} 
                    placeholder="First Name" 
                />
                <span>
                    {errors?.firstName?.message}
                </span>
                <input 
                    {...register("lastName", {
                        required: "lastName required",
                        validate: { //(*)
                            noApple: (value) => 
                                !value?.includes("apple") || "no apple allowed",
                            noBanana: (value) =>
                                !value?.includes("banana") || "no banana allowed",
                        }
                    })} 
                    placeholder="Last Name" 
                />
                <span>
                    {errors?.lastName?.message}
                </span>
                <input 
                    {...register("userName", {
                        required: "username reqiured", 
                        minLength: {
                            value: 5,
                            message: "Your Username is short"
                        }
                    })} 
                    placeholder="Username" 
                />
                <span>
                    {errors?.userName?.message}
                </span>
                <input 
                    {...register("password", {
                        required:"Password is Required", 
                        minLength: {
                            value: 5,
                            message: "Your Password is short"
                        },
                    })} 
                    placeholder="password" 
                />
                <span>
                    {errors?.password?.message}
                </span>
                <input 
                    {...register("rePassword", {
                        required:"Write password again", 
                        minLength: {
                            value: 5,
                            message: "Your Password is short"
                        },
                    })} 
                    placeholder="re-password" 
                />
                <span>
                    {errors?.rePassword?.message}
                </span>
                <button>Add</button>
                <span>
                    {errors?.extraError?.message}
                </span>
            </form>
        </div>
    );
}
```
- `validate: Function | Object`
  - 원하는 규칙으로 유효성 검사를 할 수 있으며 콜백함수 혹은 객체를 넣을 수 있다.
  - `firstname` validate에서는 raccoon이 포함된다면 쓰지 말라는 문자열을 반환하는 `콜백함수`, `lastname`에서는 2가지의 조건을 포함하는 `객체`를 전달한다.

## 참고 자료(Reference)
[react hook form doc](https://react-hook-form.com/)     

 
