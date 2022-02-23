<template>
    <div class="number-container d-flex justify-content-center align-items-center">
        <!-- 减 1 的按钮 -->
        <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
        <!-- 购买的数量 -->
        <span class="number-box">{{ num }}</span>
        <!-- 加 1 的按钮 -->
        <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
    </div>
</template>

<script>
import bus from '@/components/eventBus.js'
export default {
    props: {
        // 接收id
        // 使用 EventBus 方案将 id，count 传给 App
        id: {
            require: true,
            type: Number
        },
        num: {
            type: Number,
            default: 1
        }
    },
    methods: {
        sub(){
            if(this.num - 1 == 0) return
            const obj = {
                id: this.id,
                value: this.num - 1
            }
            bus.$emit('share' , obj)
        },
        add(){
            // 发送给 App 的数据 {id,value} 商品 id 购买数量
            const obj = {
                id: this.id,
                value: this.num + 1
            }
            bus.$emit('share' , obj)
        }
    }
}
</script>

<style lang="less" scoped>
.number-box {
    min-width: 30px;
    text-align: center;
    margin: 0 5px;
    font-size: 12px;
}

.btn-sm {
    width: 30px;
}
</style>