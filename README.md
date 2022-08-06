# react-pinia-store一个跟vue pinia类似的react状态管理 

## 1.Useage
```
npm i react-pinia-store 
or
yarn add react-pinia-store
```
## 2.Demo
```typescript jsx
import {Button, Text, View} from "@tarojs/components";
import {defineStore} from "react-pinia-store";

const userStore = defineStore({
  store() {
    return {
      username: 'aaa',
      password: '1111',
      password1: '1111',
    }
  },
  actions: {
    setUserInfo(userInfo) {
      this.$setStoreState(userInfo)
    },
    reset() {
      this.$reset()
    }
  }
});

setInterval(() => {
  userStore.setUserInfo({username: '我是来自组件外部更改的状态', username1: new Date().getTime()})
}, 3000)

const ComponentA = () => {
  const [{username}] = userStore.useStore();
  return (
    <View>
      {username}
      <View>
        ComponentA
      </View>
    </View>
  )
}

const ComponentB = () => {
  const [userInfo, actions] = userStore.useStore();
  return (
    <View>
      {JSON.stringify(userInfo)}
      <Button onClick={() => actions.setUserInfo({username: new Date().getTime()})}>
        ComponentB
      </Button>
    </View>
  )
}

const ComponentC = () => {
  const [userInfo, actions] = userStore.useStore();

  return (
    <View>
      {JSON.stringify(userInfo)}
      <Button onClick={() => actions.setUserInfo({username: new Date().getTime()})}>
        ComponentB
      </Button>
    </View>
  )
}


export default () => {
  return (
    <View className='index'>
      <Text>Hello world!</Text>
      <ComponentA />
      <ComponentB />
      <ComponentC />
      <Button onClick={() => userStore.$reset()}>
        状态重置
      </Button>
      <Button onClick={() => userStore.$patch({username:'$patch username' })}>
        $patch 更改局部状态
      </Button>
    </View>
  )
}

```