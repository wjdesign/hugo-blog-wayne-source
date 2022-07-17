+++
author = "Wayne"
title = "尚硅谷 Vue3 視頻筆記"
date = "2022-07-17"
description = "Vue是一套用於構建用戶界面的漸進式JavaScript框架，是目前最火的一個前端框架，是前端工程師的必備技能。"

tags = [
    "前端",
    "vue3"
]

categories = [
    "前端",
    "vue3"
]

series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
image = "143400003_253834612811965_4864474600445741482_n.jpeg"
+++

<style>
.focus {
    background: #f1e2e2;
    color: #d62c2c;
    padding: 0 5px;
}
</style>

[參考網站 - 尚硅谷Vue3技術](https://youtu.be/08UqvGfE1_M)  

---

## 創建 Vue 3.0 工程  
### 使用 vue cli 創建  

- 官方文檔：[https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create](https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create)  

---

```shell
## 查看 @vue/cli 版本，確保 @vue/cli 版本在 4.5.0 以上
vue --version

## 安裝或升級你的 @vue/cli
npm install -g @vue/cli

## 創建
vue create vue_test

## 啟動
cd vue_test
npm run serve
```

### 使用 vite 創建(Vue作者的團隊開發)  

- 官方文檔：[https://v3.cn.vuejs.org/guide/installation.html#vite](https://v3.cn.vuejs.org/guide/installation.html#vite)  
- vite官網：[https://vitejs.cn/](https://vitejs.cn/)  
- 優勢：  
    - 開發環境中，無需打包操作，可快速的冷啟動。  
    - 輕量快速的熱重載(HMR)。  
    - 真正的按需編譯，不再等待整個應用編譯完成。  
- 傳統 grunt、gulp、webpack 與 vite 構建對比圖：  

![](https://i.imgur.com/xegPukx.png)  

![](https://i.imgur.com/ItzEkfY.png)  

```shell
## 創建工程
npm init vite-app <project-name>

## 進入工程目錄
cd <project-name>

## 安裝依賴
npm install

## 啟動
npm run dev
```

---

## 安裝 Vue 開發者工具  

![](https://i.imgur.com/eEzqgl9.jpg)  

[Vue.js devtools](https://chrome.google.com/webstore/search/vue)  

---

## 拉開序幕的 Setup  
1. Vue3.0 中一個新的配置項，值為一個函數。  
2. 是所有 `Composition API (組合式API)` 的表演舞台。  
3. 組件中所用到的數據、方法等等，均要配置在 setup 中。  
4. setup 函數的：  
    - 若返回一個對象，則對象中的屬性、方法，在模板中均可直接使用。(重點關注!)  
    - 若返回一個渲染函數，則可以自定義渲染內容。(了解即可)  

###### 返回對象  
```javascript
export default {
    setup() {
        const name = "測試"
        return {
            name
        }
    }
}
```

###### 返回渲染函數(需引入 `h` )  
```javascript
import { h } from "vue"
export default {
    setup() {
        return () => { return h('h1', '尚硅谷')}
    }
}
```

5. 注意：  
- 不要與Vue2.x配置混用。  
    - Vue2.x配置(data、methods、computed...)中<span class="focus">可以訪問到</span>setup中的屬性、方法，但在`setup`中<span class="focus">不能訪問到</span>Vue2.x配置(data、methods、computed...)。  
    - 如果有重名，setup優先。  
- setup 不能是一個 `async` 函數，因為返回值不再是 return 的對象，而是一個 `promise`，模板看不到 return 對象中的屬性；後期可以返回一個 Promise 實例，但需要 `Suspense` 與 `異步組件(動態組件)` 的配合：[點我前往 Suspense](###Suspense)  

---

## ref 函數  
- 作用：定義一個響應式的數據。  
- 語法：  

```javascript
const xxx = ref(initValue)
```
- 將數據加工成一個 `RefImpl` (Reference: 引用；Implete: 實現) = (引用實現的實例對象)。  
- js 中操作數據：`xxx.value`。  
- 模板中讀取數據：`<div>{{xxx}}</div>`

### 備註：  
- 接收的數據可以是基本類型，也可以是對象類型。  
- 基本類型的數據：響應式依然是靠 `Object.defineProperty()` 的 `get` 與 `set` 完成的。  
- 對象類型的數據：內部<span class="focus">求助</span>了 Vue3.0 中的一個新函數---- `reactive`  

---

## reactive 函數  
- 作用：定義一個<span class="focus">對象類型</span>的響應式數據(基本類型別用他，用`ref`函數)。  
- 語法：  

```javascript
const xxx = reactive({
    name: "測試",
    age: 18
})
```
- 接收一個對象或數組，返回一個<span class="focus">代理對象(Proxy對象)</span>。  
- reactive 定義的響應式數據是`深層次的`。  
- 內部基於 ES6 的 Proxy 實現，通過代理對象操作源對象內部數據都是響應式的，並通過 `Reflect` 操作源對象內部的數據。  
- js、模板中操作數據均不需要 `.value`

---

## Vue 2.0 中的響應式原理  
### 實現原理：  
- 對象類型：通過 `Object.defineProperty()` 對屬性的讀取、修改進行攔截(數據劫持)。  
- 數組類型：通過重寫更新數組的一系列方式來實現攔截。(對數組的變更方法進行了包裹)。  

```javascript
Object.defineProperty(data, "count", {
    get() {
        
    },
    set() {
        
    }
})
```

#### 原理模擬：  
```javascript
let person = {
    name: "張三",
    age: 18
}

// 模擬 Vue2 中實現響應式
let p = {}
Object.defineProperty(p, "name", {
    configurable: true, // 允許刪除，但捕獲不到
    get() {
        // 有人讀取 name 時調用
        return person.name
    },
    set(value) {
        console.log("有人修改了 name 屬性，我發現了ㄛ，我要去更新介面！")
        person.name = value
    }
})
Object.defineProperty(p, "age", {
    configurable: true, // 允許刪除，但捕獲不到
    get() {
        // 有人讀取 age 時調用
        return person.age
    },
    set(value) {
        console.log("有人修改了 age 屬性，我發現了ㄛ，我要去更新介面！")
        person.age = value
    }
})
```

- 存在問題：  
    - 新增屬性、刪除屬性，介面不會更新，需使用 `$set`、`$delete`。  
    - 直接通過下標修改數組，介面不會更新，需使用 `$set`、`$delete`。  

#### 問題情況演示：  
```javascript
export default {
    data() {
        return {
            person: {
                name: "張三",
                age: 18,
                hobby: ["學習", "吃飯"]
            }
        }
    },
    methods: {
        addSex() {
            this.person.sex = "女" // 此時畫面不會更新
            this.$set(this.person, "sex", "女") // 需使用 $set 畫面才會更新
            // 或是使用 Vue.set()
            // Vue.set(this.person, "sex", "女")
        },
        deleteName() {
            delete this.person.name // 此時畫面不會更新
            this.$delete(this.person, "name") // 需使用 $delete 畫面才會更新
            // 或是使用 Vue.delete()
            // Vue.delete(this.person, "name")
        },
        updateHobby() {
            this.person.hobby[0] = "逛街" // 此時畫面不會更新
            this.$set(this.person.hobby, 0, "逛街") // 需使用 $set 畫面才會更新
            // 或是使用 splice()
            // this.person.hobby.splice(0, 1, "逛街")
        }
    }
}
```

---

## Vue 3.0 中的響應式原理  
### 實現原理：  
- 通過 Proxy(代理)：攔截對象中任意屬性的變化，包含屬性值的讀寫、屬性的新增、屬性的刪除等。  
- 通過 Reflect(反射)：對被代理對象的屬性進行操作。  

#### 原理模擬：  
```javascript
let person = {
    name: "張三",
    age: 18
}

// 模擬 Vue3 中實現響應式
const p = new Proxy(person, {
    // 有人讀取p的某個屬性時調用
    get(target, propName) {
        console.log(`有人讀取了p身上的${propName}屬性`)
        return Reflect.get(target, propName)
    },
    // 有人新增或修改p的某個屬性時調用
    set(target, propName, value) {
        console.log(`有人修改了p身上的${propName}屬性，我要去更新介面了！`)
        Reflect.set(target, propName, value)
    },
    // 有人刪除p的某個屬性時調用
    deleteProperty(target, propName) {
        console.log(`有人刪除了p身上的${propName}屬性，我要去更新介面了！`)
        return Reflect.deleteProperty(target, propName)
    }
})
```

---

## setup 的兩個注意點  
- setup 執行的時機：在 beforeCreate 之前執行一次，this 是 undefined。  
- setup 的參數
    - `props`：值為對象，包含：組件外部傳遞過來，且組件內部聲明接收了的屬性。  
    - `context`：上下文對象：  
        - `attrs`：值為對象，包含：組件外部傳遞過來，但沒有在 props 配置中聲明的屬性，相當於 Vue 2.0 的 `this.$attrs`。  
        - `slots`：接收的插槽內容，相當於 Vue 2.0 的 `this.$slots`。  
        - `emit`：分發自定義事件的函數，相當於 Vue 2.0 的 `this.$emit`。  

---

## watch 函數  
- 與 Vue 2.0 中的 watch 配置功能一致。  
- 兩個小"坑"：  
    - 監視 ref 定義的響應式數據時，不需加 `.value`。  
    - 監視 reactive 定義的響應式數據時，`oldValue` 無法正確捕獲、強制開啟了深度監視(deep配置失效)。  
    - 監視 reactive 定義的響應式數據中的某個屬性時，deep 配置有效。  

```javascript
import { ref, reactive, watch } from "vue"
export default {
    setup() {
        const sum = ref(10)
        const msg = ref("測試")
        const person = reactive({
            name: "張三",
            age: 18,
            job: {
                job1: {
                    salary: 20
                }
            }
        })

        // 情況一：監視 ref 定義的響應式數據
        watch(sum, (newValue, oldValue) => {
            console.log("sum變化了", newValue, oldValue)
        }, { immediate: true })

        // 情況二：同時監視多個 ref 定義的響應式數據
        watch([sum, msg], (newValue, oldValue) => {
            console.log("sum或msg變化了", newValue, oldValue)
        }, { immediate: true })

        /*
         * 情況三：監視 reactive 定義的響應式數據的全部屬性
         *     1. 注意: 此數無法正確的獲取 oldValue
         *     2. 注意: 強制開啟了深度監視(deep配置無效)
         */
        watch(person, (newValue, oldValue) => {
            console.log("person變化了", newValue, oldValue)
        }, { immediate: true, deep: false }) // 此處的 deep 配置無效

        // 情況四：監視 reactive 定義的響應式數據的某個屬性
        watch(() => person.name, (newValue, oldValue) => {
            console.log("person的name變化了", newValue, oldValue)
        }, { immediate: true })

        // 情況五：監視 reactive 定義的響應式數據的某些屬性
        watch([() => person.name, () => person.age], (newValue, oldValue) => {
            console.log("person的name或age變化了", newValue, oldValue)
        }, { immediate: true })

        // 特殊情況：監視 reactive 定義的響應式數據的某些對象屬性
        watch(() => person.job, (newValue, oldValue) => {
            console.log("person的job變化了", newValue, oldValue)
        }, { immediate: true, deep: true }) // 此處由於監視的是 reactive 所定義的對象中的某個屬性，所以 deep 配置有效

        return {
            sum,
            msg
        }
    }
}
```

---

## watch 時 value 的問題  
- 若監視的數據為 ref 求助 reactive 生成的響應式數據，則可使用以下兩種方式進行監視：  

```javascript
import { ref, watch } from "vue"
export default {
    setup() {
        const sum = ref(0)
        const person = ref({
            name: "張三",
            name: 18,
            job: {
                job1: {
                    salary: 20
                }
            }
        })

        watch(sum, (newValue, oldValue) => {
            // 監視的是 sum 這個 RefImpl 數據，因此不需要 .value
            console.log("sum的值變化了", newValue, oldValue)
        })

        // 方法一:
        watch(person.value, (newValue, oldValue) => {
            // 監視 person.value 的 Proxy 對象
            console.log("person的值變化了", newValue, oldValue)
        })

        // 方法二:
        watch(person, (newValue, oldValue) => {
            // 深度監視 person 的 Proxy 對象的屬性
            console.log("person的值變化了", newValue, oldValue)
        }, { deep: true })

        return {
            person
        }
    }
}
```

---

## watchEffect  
- 智能版 watch，不用指名監視哪個屬性，監視的回調中用到哪個屬性，就監視哪個屬性(而且是深層次的)。  
- watchEffect 有點像 computed：  
    - 但 computed 注重計算出來的值(回調函數的返回值)，所以必須要寫返回值。  
    - 而 watchEffect 更注重的是過程(回調函數的函數體)，所以不用寫返回值。  

```javascript
import { ref, reactive, watchEffect } from "vue"
export default {
    setup() {
        const sum = ref(0)
        const person = reactive({
            name: "張三",
            age: 18,
            job: {
                job1: {
                    salary: 20
                }
            }
        })
        watchEffect(() => {
            const x1 = sum.value
            const x2 = person.job.job1.salary

            console.log("watchEffect 配置的回調執行了")
        })
    }
}
```

---

## 自定義 hook 函數
- hook 本質是一個函數，把 setup 函數中使用的 Composition API 進行了封裝。  
- 類似於 vue 2.0 中的 mixin。  
- 自定義 hook 的優勢：重複使用代碼，讓 setup 中的邏輯更清楚易懂。  
- 命名通常建議以 "use" 開頭，例如：  

#### 一個獲取鼠標點擊位置的 hook  
###### src/hooks/usePoint.js  
```javascript
import { reactive, onMounted, onBeforeUnmount} from "vue"
export default function() {
    // 獲取鼠標點擊位置 相關的數據
    let point = reactive({
        x: 0,
        y: 0
    })
    
    // 獲取鼠標點擊位置 相關的方法
    function savePoint(event) {
        console.log(event.pageX, event.pageY)
        point.x = event.pageX
        point.y = event.pageY
    }
    
    // 獲取鼠標點擊位置 相關的生命週期鉤子
    onMounted(() => {
        window.addEventListener("click", savePoint)
    })
    
    onBeforeUnmount(() => {
        window.removeEventListener("click", savePoint)
    })
    
    return point
}
```

###### Demo.vue  
```javascript
import usePoint from "@/hooks/usePoint"

export default {
    name: "Demo",
    setup() {
        // 使用自定義的 hook
        const point = usePoint()
        
        return {
            point
        }
    }
}
```

---

## toRef  
- 作用：創建一個 ref 對象，其 value 值指向(引用)另一個對象中的某個屬性(返回值為一個 `ObjectRefImpl` 對象，為響應式)。  
- 語法：  

```javascript
const name = toRef(person, "name")
```

- 應用：要將響應式對象中的某個屬性單獨提供給外部使用時。  
- 擴展：`toRefs` 與 `toRef` 功能一致，但可以批量創建多個 ref 對象，語法：  

```javascript
toRefs(person)
```

#### 使用範例：  
```javascript
import { reactive, toRef, toRefs} from "vue"
export default {
    setup() {
        let person = reactive({
            name: "張三",
            age: 18,
            job: {
                job1: {
                    salary: 20
                }
            }
        })
        
        const name1 = person.name // name1 僅為賦值，無響應式
        const name2 = toRef(person, "name") // name2 的值會指向(引用) person 的 name
        
        return {
            // errors：
            // name1: person.name, // 僅為賦值，無響應式
            // name2: ref(person.name) // 初始值正常，但修改時不會改到 person 的 name，因為此寫法僅是將 "ref(pserson.name)" 賦值給 name2，而非將 name2 指向 person 的 name
            
            // success：
            // 模板中使用 {{ person.name }}...等：
            // person,
            
            // 一個一個給出，模板中使用 {{ name }}...等：
            // name: toRef(person, "name"),
            // age: toRef(person, "age"),
            // salary: toRef(person.job.job1, "salary")
        
            // 一次全給出，模板中可直接使用 {{ name }}、{{ age }}、{{ job.job1.salary }}
            ...toRefs(person)
        }
    }
}
```

---

## 其他的 Composition API  
### 1. shallowReactive 與 shallowRef  
- shallow：淺層的  
- shallowReactive：只處理對象最外層屬性的響應式(淺響應式)。  
- shallowRef：只處理基本數據類型的響應式，不進行對象的響應式處理。  
- 什麼時候使用？  
    - 如果有一個對象數據，結構比較深，但變化時只是外層屬性變化 => `shallowReactive`。  
    - 如果有一個對象數據，後續功能不會修改該對象中的屬性，而是生成新的對象來替換 => `shallowRef`。  

```javascript
import { shallowRef, shallowReactive, toRefs} from "vue"
export default {
    setup() {
        let person = shallowReactive({ // 只考慮第一層數據的響應式
            name: "張三", // 響應式
            age: 18, // 響應式
            job: { // 非響應式
                job1: {
                    salary: 20
                }
            }
        })
        
        let x = shallowRef({ // 基本類型時同 ref，但對象類型不是響應式(value 會變成一般的 Object 而不是 Proxy )
            y: 0
        })
        
        return {
            x,
            ...toRefs(person)
        }
    }
}
```

---

### 2. readonly 與 shallowReadonly  
- readonly：讓一個響應式數據變為唯讀的(深層唯讀)。  
- shallowReadonly：讓一個響應式數據變為唯讀的(淺層唯讀)。  
- 應用場景：不希望數據被修改時。  

```javascript
import { ref, reactive, toRefs, readonly, shallowReadonly} from "vue"
export default {
    setup() {
        let sum = ref(0)
        let person = reactive({
            name: "張三",
            age: 18,
            job: {
                job1: {
                    salary: 20
                }
            }
        })
        
        // 將 sum 變為唯讀，保護數據不被修改
        sum = readonly(sum)
        
        // 將 person 的所有屬性變為唯讀，保護所有屬性數據不被修改
        person = readonly(person)
        
        // 將 person 的"第一層屬性數據"變為唯讀(name、age無法修改，但 job 可以)
        person = shallowReadonly(person)
        
        return {
            sum,
            ...toRefs(person)
        }
    }
}
```

---

## toRaw 與 markRaw  
- raw：原始。  
- toRaw：  
    - 作用：將一個由 `reactive` 生成的`響應式對象`轉為`普通對象`。  
    - 應用場景：用於讀取響應式對象對應的普通對象，對這個普通對象的所有操作，不會引起頁面更新。  
- markRaw：  
    - 作用：標記一個對象，使其永遠不會再成為響應式對象。  
    - 應用場景：  
        - 有些值不應被設置為響應式的，例如複雜的第三方類庫等。  
        - 當渲染具有不可變數據源的大列表時，跳過響應式轉換可以提高性能。  

```javascript
import { ref, reactive, toRaw, markRaw} from "vue"
export default {
    setup() {
        let sum = ref(0) 
        let person = reactive({
            name: "張三",
            age: 18,
            job: {
                job1: {
                    salary: 20
                }
            }
        })
        
        function showRawPerson() {
            const p = toRaw(person)
            console.log(p) // 返回的不再是 Proxy，而是 Object
        }
        
        function addCar() {
            let car = { name: "奔馳", price: 40}
            person.car = markRaw(car) // 標記 person.car 不是響應式的數據(數據依舊可修改，但畫面不會變)
        }
        
        return {
            sum,
            ...toRefs(person),
            showRawPerson,
            addCar
        }
    }
}
```

---

### customRef  
- 作用：創建一個自定義的 ref，並對其依賴項跟蹤和更新觸發進行顯示控制。  
- 實現防抖效果：  

```html
<template>
    <input type="text" v-model="keyWord" />
    <h3>
        {{ keyWord }}
    </h3>
</template>
<script>
import { ref, customRef } from "vue"
export default {
    setup() {
        // 使用 vue 提供的 ref
        // let keyWord = ref("hello")
        
        // 自定義的一個 ref
        function myRef(value, delay) {
            let timer
            return customRef((track, trigger) => {
                return {
                    get() {
                        console.log(`有人從 myRef 這個容器中讀取數據了，我把${value}給他了`)
                        track() // 通知 Vue 追蹤數據的變化(提前與 get 商量一下，讓它認為這個 value 是有用的)
                        return value
                    },
                    set(newValue) {
                        console.log(`有人把 myRef 這個容器中的數據改為了${newValue}`)
                        clearTimeout(timer)
                        timer = setTimeout(() => {
                            value = newValue
                            trigger() // 通知 Vue 去重新解析模板，以便觸發 get
                        }, delay)
                    }
                }
            })
        }
        // 使用自定義的防抖 ref
        let keyWord = myRef("hello", 500)
        
        return {
            keyWord
        }
    }
}
</script>
```

---

### provide 與 inject  
- 作用：實現祖孫組件間通信。  
- 套路：父組件有一個 `provide` 選項來提供數據，後代組件有一個 `inject` 選項來開始使用這些數據。  
- 具體寫法：  

#### 1. 祖組件中：  
```javascript
import { reactive, toRefs, provide } from "vue"
export default {
    name: "App",
    setup() {
        let car = reactive({
            name: "奔馳",
            price: "40W"
        })
        provide("car", car) // 給自己的後代組件傳遞數據
        return { ...toRefs(car) }
    }
}
```

#### 2. 後代組件中：  
```javascript
import { inject } from "vue"
export default {
    name: "Son",
    setup() {
        let car = inject("car")
        console.log(car)
        return { car }
    }
}
```

---

### 響應式數據的判斷  
- isRef：檢查一個值是否為一個 `ref` 對象。  
- isReactive：檢查一個對象是否是由 `reactive` 創建的響應式代理。  
- isReadonly：檢查一個對象是否是由 `readonly` 創建的唯讀代理。  
- isProxy：檢查一個對象是否是由 `reactive` 或是 `readonly` 方法創建的代理。  

---

## Teleport  
- teleport：傳送、瞬間移動。  
- 作用：能夠將我們的 `組件 html 結構` 移動到指定的位置。  
- 具體寫法：  

```html
<teleport to="body">
    <!-- to 也能寫 css select，例如 to="#app" -->
    <div v-if="isShow" class="mask">
        <div class="dialog">
            <h3>
                我是一個彈窗
            </h3>
            <button @click="isShow = false">
                關閉彈窗
            </button>
        </div>
    </div>
</teleport>
```

---

## Suspense  
- suspense：懸疑、懸而未決的。  
- 作用：等待異步組件時，渲染一些額外內容，讓使用者有更好的用戶體驗。  
- 使用步驟：  

#### 異步引用組件  
```javascript
import { defineAsyncComponent } from "vue" // 宣告異步組件時使用
const Child = defineAsyncComponent(() =>. import ("./components/Child.vue"))
```

#### 使用 `Suspense` 包裹組件，並配置好 `default` 與 `fallback`  
```html
<template>
    <div id="app">
        <h3>我是App組件</h3>
        
        <Suspense>
            <template v-slot:default>
                <Child />
            </template>
            <template v-slot:fallback>
                <h3>加載中......</h3>
            </template>
        </Suspense>
    </div>
</template>
```

---

## 全局 API 的轉移  
- Vue 2.0 有許多全局 API 和配置。  
    - 例如：註冊全局組件、註冊全局指令等。  
    
```javascript
// 註冊全局組件
Vue.component("MyButton", {
    data() {
        return {
            count: 0
        }
    },
    template: "<button @click='count++'>Clicked {{ count }}</button>"
})

// 註冊全局指令
Vue.directive("focus", {
    inserted: el => el.focus()
})
```

- Vue 3.0 中對這些 API 做出了調整：  
    - 將全局的 API，即： `Vue.xxx` 調整到應用實例(`app`)上  

| 2.0 全局 API(`Vue`) | 3.0 實例 API(`app`) |
|:--|:--|
| Vue.config.xxxx | app.config.xxxx |
| Vue.config.productionTip | **移除** |
| Vue.component | app.component |
| Vue.directive | app.directive |
| Vue.mixin | app.mixin |
| Vue.use | app.use |
| Vue.prototype | app.config.globalProperties |

---