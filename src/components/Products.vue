<template>
     
           <div class="product_container mb-3 m-2">
               <img  :src="img" alt="img" class="img">
                 <div class="details mt-2"> 
                  <strong>{{ name }}</strong>  
                 <div>   
                    <span class="price"> Rs. <del>{{selling_price}}</del> Rs. {{ price }}</span>
                    <span class="offer"> -{{discount}}%</span>
                    </div>    
               </div>
           </div>
</template>

<script>
    export default {
        props:['img','price','selling_price','name','discount']
    }
</script>
<style scoped>
.img{ 
    width:250px
}

.offer{
    color: red;
}
</style>