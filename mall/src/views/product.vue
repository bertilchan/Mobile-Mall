<template>
    <div id="product">
        <van-nav-bar
            title="商品详情"
            left-text="返回"
            left-arrow
            @click-left="onClickLeft"
        />
        <van-swipe :autoplay="3000">
            <van-swipe-item v-for="(image, index) in images" :key="index">
                <img v-lazy="image" />
            </van-swipe-item>
        </van-swipe>
        
    </div>
</template>

<script>
import axios from 'axios'

export default {
    props: ['id'],
    name: 'product',
    data() {
        return {
            data: {}
        }
    },
    
    methods: {
        onClickLeft:function() {
            // console.log(this.id);
            this.$router.go(-1)
        }
    },
    async created() {
        let res = await axios.get(this.$root.api.GoodsDetail,{params: {id:this.id}})
        this.data = res.data.data
    }

}
</script>

<style scoped>

</style>