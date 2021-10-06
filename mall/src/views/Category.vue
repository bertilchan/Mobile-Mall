<template>
    <div id="category">
        <van-search v-model="searchData" placeholder="搜索商品，共30000+款好物" input-align="center"/>
        <van-tree-select 
            :items="items" 
            :main-active-index.sync="activeIndex"
            height="calc(100vh - 104px)"
            @click-nav="changeRight">
            <template #content>
                <div class="imgbanner">
                    <img :src="bannerImg" width="100%">
                </div>
                <van-grid :column-num="3">
                    <van-grid-item v-for="(item1,index1) in subCategoryList" :key="index1" :icon="item1.wap_banner_url" :text="item1.name" :to="'categorylist/'+item1.id"/>
                </van-grid>
            </template>
        </van-tree-select>
        <tab-btn></tab-btn>
    </div>
</template>

<script>
import tabBtn from '../components/tabBtn'
import axios from 'axios'
import api from '../assets/config/api'

export default {
    name: 'Category',
    components: {
        tabBtn
    },
    data() {
        return {
            searchData: '',
            data: {},
            activeIndex: 0,
            subCategoryList: []
        }
    },
    computed: {
        // 格式  items: [{ text: '分组 1' }, { text: '分组 2' }],
        items: function() {
            if(this.data.categoryList == undefined) {
                return []
            }else {
                let arr = []
                this.data.categoryList.forEach((item,index)=>{
                    item.text = item.name
                    arr.push(item)
                })  
                return arr
            }
        },
    },
    methods: {
        changeRight: function(index) {
            this.activeIndex = index
        },
    },
    watch: {
        activeIndex: async function() {
            if(this.items.length !== 0){
                let id = this.items[this.activeIndex].id
                await axios.get(api.CatalogCurrent,{params:{id}}).then((res)=>{
                    this.subCategoryList = res.data.data.currentCategory.subCategoryList
                    console.log(this.subCategoryList);
                })
                this.bannerImg = this.items[this.activeIndex].img_url
            }else {
                this.subCategoryList = []
            }
        }
    },
    async created() {
        let res = await axios.get(api.CatalogList)
        this.data = res.data.data
        this.subCategoryList = this.data.currentCategory.subCategoryList
        this.bannerImg = this.data.currentCategory.img_url
    }
    
}
</script>

<style scoped lang="less">
    #category {
        .imgbanner {
            width: 100%;
            box-sizing: border-box;
            padding: 10px;
        }
    }
</style>