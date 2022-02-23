<template>
    <div id="app" class="app-container">
        <!-- Header 头部 -->
        <Header :title="'标题'"></Header>
        <!-- 循环渲染每个商品的信息 -->
        <Goods 
        v-for="item in list" 
        :key="item.id" 
        :id="item.id"
        :title="item.goods_name" 
        :pic="item.goods_img" 
        :price="item.goods_price" 
        :state="item.goods_state"
        :count="item.goods_count"
        @state-change="getNewState"
        >
        <Count 
        :num="item.goods_count" 
        @num-change="getNewNum(item,$event)"
        ></Count>
        </Goods>

        <Footer 
        :isfull="fullState" 
        :amount="amount"
        :total="total"
        @full-change="getFullState"
        ></Footer>
    </div>
</template>

<script>
// 导入 axios 
import axios from 'axios'

// 导入需要的组件
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
import Count from '@/components/Counter/Counter.vue'


import bus from '@/components/eventBus.js'

export default {
    data(){
        return {
            list: []
        }
    },
    components: {
        Header,
        Goods,
        Footer,
        Count
    },
    computed:{
        // 全选框状态
        fullState(){
            return this.list.every(item => item.goods_state)
        },
        // 合计价格
        amount(){
            // 1. 先 filter 过滤
            // 2. 再 reduce 累加
            return this.list
                .filter(item => item.goods_state)
                .reduce((total , item) => (total += item.goods_price * item.goods_count) , 0)
        },
        // 勾选商品总数量
        total(){
            return this.list
                .filter(item => item.goods_state)
                .reduce((t , item) => ( t += item.goods_count),0)
        }
    },
    methods: {
        // 封装请求数据的方法
        async initCartList(){
            const {data: result} = await axios.get('https://www.escook.cn/api/cart')
            if(result.status === 200){
                this.list = result.list
            }
        },
        // 获取商品复选框状态改变后的新状态
        getNewState(e){
            // console.log(e)
            this.list.some(item => {
                if(item.id === e.id){
                    item.goods_state = e.value
                    return true
                }
            })
        },
        getFullState(e){
            this.list.forEach(item => item.goods_state = e)
        },
        getNewNum(item,e){
            item.goods_count = e
        }
    },
    created(){
        // 调用请求购物车数据的方法
        this.initCartList()

        // 接收 count 的数据
        bus.$on('share' , (e) => {
            // console.log('App 收到数据：',e)
            this.list.some(item => {
                if(item.id === e.id){
                    item.goods_count = e.value
                    return true
                }
            })
        })
    },

}
</script>

<style lang="less" scoped>
.app-container{
    padding-top: 45px;
    padding-bottom: 50px;
}
</style>
