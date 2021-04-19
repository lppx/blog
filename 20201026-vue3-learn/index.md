# Vue3学习


####  生命周期函数

*只能在setup里面使用*

```
setup() {
    onBeforeMount(() => {
      console.log('onBeforeMount!');
    });
    onMounted(() => {
      console.log('mounted!');
    });
    onUpdated(() => {
      console.log('updated!');
    });
    onUnmounted(() => {
      console.log('unmounted!');
    });

    return {};
  }
```

####  ref,reactive,isRef,isReactive

ref()是用来根据给定的值（number、string、bool等）创建一个响应式的数据对象，ref()函数调用的返回值是个对象，这对象只包含一个value属性，只在setup函数内容访问ref函数需要添加`.value`

```vue
<template>
  <p>{{ name }}</p>
  <p>{{ refName }}</p>
  <van-button @click="changeValue" type="primary">click</van-button>
  <p>用户信息：{{ user.name }}-{{ user.age }}-{{ user.refName }}</p>
  <van-button @click="changeUser('张三', 100)" type="primary">changeUser</van-button>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue';
export default defineComponent({
  name: 'HOME',

  setup() {
    let name = 'lppx';
    const refName = ref<string>('refName');
    const changeValue = () => {
      name = '123'; // 非响应式数据 ，不会更新到dom中
      refName.value = 'lppx';
      console.log(name, refName.value);
    };
    const user = reactive({
      name: 'lppx',
      age: 22,
      refName, // reactive中使用ref值
    });

    const changeUser = (name: string, age: number) => {
      user.name = name;
      user.age = age;
      //通过reactive 来获取或设置ref 的值时,不需要使用.value属性
      console.log(user.refName);
      user.refName = 'reactive中使用ref值不需要添加.value';
    };
      
    console.log('是否为ref()创建出来的对象', isRef(name)); // false
    console.log('是否为ref()创建出来的对象', isRef(refName)); // true
    console.log('是否为ref()创建出来的对象', isRef(user)); // false
    console.log('是否为reactive()创建出来的对象', isReactive(user)); // true

    return {
      name,
      refName,
      changeValue,
      user,
      changeUser,
    };
  },
});
</script>
<style lang="scss" scoped></style>
```

####  toRefs

toRefs()将reactive()创建出来的响应式对象转为普通对象，但对象的属性都是ref()类型的响应式对象

在使用不需要通过`user.name`访问

```vue
<template>
  <p>用户信息：{{ name }}-{{ age }}</p>
  <p>用户信息：{{ name1 }}-{{ age1 }}</p>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from 'vue';

function useUser() {
  const state = reactive({
    name: 'use-lppx',
    age: 99,
  });
  return toRefs(state);
}

export default defineComponent({
  name: 'HOME',

  setup() {
    const user = reactive({
      name1: 'lppx',
      age1: 22,
    });
    const { name, age } = useUser();
    return {
      ...toRefs(user),
      name,
      age,
    };
  },
});
</script>

<style lang="scss" scoped></style>


```



####  watchEffect  watch

```vue
<template>
  <h1>home</h1>
  <div>
    {{ count }}
    <van-button @click="setCount" type="primary">+</van-button>
    <van-button @click="stopWatchEffect" type="primary">stopWatchEffect</van-button>
    <van-button @click="stopWatch" type="primary">stopWatch</van-button>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, reactive, ref, toRefs, watch, watchEffect } from 'vue';
export default defineComponent({
  name: 'HOME',
  setup() {
    const count = ref(1);
    const setCount = () => {
      count.value++;
    };
    // watchEffect执行就监听
    const stopWatchEffect = watchEffect(() => {
      console.log('watchEffect count:', count.value);
    });
    // watch 懒监听，当value改变才触发
    const stopWatch = watch(
      () => count.value,
      (newValue, oldValue) => {
        console.log('watch new:', newValue);
        console.log('watch old:', oldValue);
      }
    );
    return {
      count,
      setCount,
      stopWatchEffect,
      stopWatch,
    };
  },
});
</script>

<style lang="scss" scoped></style>

```


