# pinia-for-react

```
一个跟vue pinia类似的react状态管理 
```

## 1.Useage

```
npm i pinia-for-react
or
yarn add pinia-for-react
```

## 2.Demo

```typescript jsx
import {defineStore} from "pinia-for-react";

const userStore = defineStore({
    state() {
        return {
            username: 'aaa',
            password: '1111',
            password1: '1111',
        }
    },
    actions: {
        setUserInfo(userInfo) {
            const state = this.$getState();
            //可以访问内部提供的方法
            this.$setState({
                ...state,
                ...userInfo
            })
        },
        //异步调用
        async syncUserInfo() {
            await new Promise(resolve => setTimeout(resolve, 2000));
            this.$patch({
                username: 'syncUserInfoaaa',
                password: 'syncUserInfo1111',
                password1: 'syncUserInfo1111',
            })
        },
        reset() {
            this.$reset()
        }
    }
});

//满足hook以use开头规范
const useUserStore = userStore;

//外部更新状态
setInterval(() => {
    userStore.setUserInfo({username: '我是来自组件外部更改的状态', username1: new Date().getTime()})
}, 3000)

const ComponentA = () => {
    // 直接调用返回值
    const [{username}] = useUserStore();
    return (
        <div>
            {username}
            <div>
                ComponentA
            </div>
        </div>
    )
}

const ComponentB = () => {
    //也可以直接使用useStore方法hook
    const [userInfo, actions] = userStore.useStore();
    return (
        <div>
            {JSON.stringify(userInfo)}
            <button onClick={() => actions.setUserInfo({username: new Date().getTime()})}>
                ComponentB
            </button>
        </div>
    )
}

const ComponentC = () => {
    const [userInfo, actions] = userStore.useStore();

    return (
        <div>
            {JSON.stringify(userInfo)}
            <button onClick={() => actions.setUserInfo({username: new Date().getTime()})}>
                ComponentB
            </button>
        </div>
    )
}


export default () => {
    return (
        <div>
            <span>Hello world!</span>
            <ComponentA/>
            <ComponentB/>
            <ComponentC/>
            {/*也可以直接调用返回的userStore变量的方法*/}
            <button onClick={() => userStore.$reset()}>
                状态重置
            </button>
            {/*也可以直接调用返回的userStore变量的方法*/}
            <button onClick={() => userStore.$patch({username: '$patch username'})}>
                $patch 更改局部状态
            </button>
        </div>
    )
}

```
