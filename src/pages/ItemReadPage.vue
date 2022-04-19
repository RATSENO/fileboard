<template>
    <div align="center">
        <h2>상품 상세보기</h2>
        <item-read v-if="item" :item="item"></item-read>
        <p v-else>loading...</p>
        <router-link :to="{name:'ItemModifyPage',params: {itemId}}">수정</router-link>
        <button @click="deletePost">삭제</button>
        <router-link :to="{name:'ItemListPage'}">목록</router-link>
    </div>
</template>
<script>
import ItemRead from '../components/ItemRead.vue'
import client from '../modules/client.js'
import {ref} from 'vue'

import {router} from '../router/router.js'

export default {
    name: "ItemReadPage",
    components: { ItemRead },
    props: {
        itemId: {
            type: String,
            required: true
        }
    },
    setup(props, context){
        const item = ref({})

        client.get(`/items/${props.itemId}`)
            .then(function(res){
                item.value=res.data
            })
            .catch(function(err){
                alert(err.response.data.msg)
            })

        
        const deletePost =()=>{
            const {itemId} = item.value
            client.delete(`/items/${itemId}`)
                .then(function(res){
                    alert('삭제되었습니다.')
                    router.push({
                        name: 'ItemListPage'
                    })
                })
                .catch(function(err){
                    alert(err.response.data.message)
                })
        }

        return {
            item,
            deletePost
        }
    }
}
</script>