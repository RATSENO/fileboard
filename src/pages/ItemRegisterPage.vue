<template>
    <div align="center">
        <h2>상품등록</h2>
        <item-register-form @add-post="addPost"></item-register-form>
    </div>
</template>
<script>
import ItemRegisterForm from '../components/ItemRegisterForm.vue'
import client from '../modules/client.js'
import {router} from '../router/router.js'

export default {
    components: { ItemRegisterForm },
    name: "ItemRegisterPage",
    setup(){
        const addPost = (payload) =>{
            const {itemName, price, description, file} = payload
            const itemObject = {
                itemName: itemName,
                price: price,
                description: description
            }

            const formData = new FormData();
            formData.append("file", file)
            formData.append("item", JSON.stringify(itemObject))

            client.post('/items',
                formData,
                {
                    headers: {
                        'Content-Type':'multipart/form-data'
                    }
                }
            )
            .then(function(res){
                alert('등록되었습니다.')
                router.push({
                    name: 'ItemReadPage',
                    params: {itemId:res.data.itemId}
                })
            })
            .catch(function(err){
                alert(err.response.data.msg)
            })
        }

        return {
            addPost
        }
    }
}
</script>