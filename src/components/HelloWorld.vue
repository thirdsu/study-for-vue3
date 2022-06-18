<template>
<div class="hello">
    <div></div>
</div>
</template>

<script>
import {
    ref
} from "vue"
export default {
    name: 'HelloWorld',
    props: {
        msg: String
    },
    setup(props) {
        const count = ref(0)
        console.log(count)
        //响应式系统实现,通过拦截proxy对象的get,set方法来监听属性的变化，
        // 当读取属性时，将副作用函数放入一个副作用函数桶中，
        // 当值发生改变时，在set里面值发生改变后调用副作用函数，实现响应。
        const bucket=new Set()
        const data={text:'hello world'}
        const obj=new Proxy(data,{
          get(target,key){
            bucket.add(effect)
            return target[key]
          },
          set(target,key,newVal){
            target[key]=newVal
            bucket.forEach(fn=>fn())
            return true
          }

        })
        function effect(){
          document.body.innerText=obj.text
        }
        effect()
        setTimeout(()=>{
          obj.text='hello vue3'
        },1000)
        return {
            props
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
h3 {
    margin: 40px 0 0;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}
</style>
