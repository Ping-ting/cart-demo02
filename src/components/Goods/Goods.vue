<template>
    <div class="goods-container">
        <!-- 左侧图片 -->
        <div class="thumb">
            <div class="custom-control custom-checkbox">
                <!-- 复选框 -->
                <input type="checkbox" class="custom-control-input" :id="'cb' + id" :checked="state" @change="stateChange" />
                <label class="custom-control-label" :for="'cb' + id">
                <!-- 商品的缩略图 -->
                <img :src="pic" alt="" />
                </label>
            </div>
        </div>
        <!-- 右侧信息区域 -->
        <div class="goods-info">
            <!-- 商品标题 -->
            <h6 class="goods-title">{{ title }}</h6>
            <div class="goods-info-bottom">
                <!-- 商品价格 -->
                <span class="goods-price">￥{{ price }}</span>
                <!-- 商品的数量 -->
                <slot></slot>
            </div>
        </div>
    </div>
</template>

<script>
import Count from '@/components/Counter/Counter.vue'
export default {
    components: {
        Count
    },
    props: {
        // id
        // 子组件中商品的勾选状态发送改变时
        // 需要子传父 根据 id修改勾选状态数据
        id: {
            require: true,
            type: Number
        },
        // 标题
        title:{
            default: '',
            type: String
        },
        // 图片
        pic:{
            default: '',
            type: String
        },
        // 单价
        price: {
            default: 0,
            type: Number
        },
        // 复选框
        state: {
            default: true,
            type: Boolean
        },
        count: {
            default: 1,
            type: Number
        }

    },
    methods: {
        stateChange(e){
            const newState = e.target.checked
            // console.log(newState , this.id)
            this.$emit('state-change' , {
                id:this.id,
                value: newState
            })
        }
    }

}
</script>

<style lang="less" scoped>
.goods-container {
+ .goods-container {
    border-top: 1px solid #efefef;
}
padding: 10px;
display: flex;
.thumb {
    display: flex;
    align-items: center;
    img {
    width: 100px;
    height: 100px;
    margin: 0 10px;
    }
}

.goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
    font-weight: bold;
    font-size: 12px;
    }
    .goods-info-bottom {
    display: flex;
    justify-content: space-between;
    .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
    }
    }
}
}
</style>