<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-2 col-sm-2 col-md-2">
        <Demo @filter="filterUpdated"/>
      </div>
      <div class="col-lg-10 col-sm-10 col-md-10">
        <div v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="10">
        <div class="d-flex flex-wrap ">        
          <Products class="ml-2" :price="product.price" :img="product.img"
          :name='product.name' :selling_price='product.selling_price'
          :discount='product.discount'
           v-for="product,index in imgMain" :key="index"/>
        </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import Products from "../components/Products.vue";

import Demo from "../components/Demo.vue";
export default {
  components: {
    Products,
    Demo
  },

  methods: {
    filterUpdated(param){
     var arr = [] 
     arr=(Object.values(param.values))
     console.log(arr)
     if(arr.length==0){
       window.location.reload()
     }
     this.paramHome = arr.toString();
     this.filterDataCallAPI(this.paramHome,1)
    },
    filterDataCallAPI(filter,pagenumber){
      if(filter){
        this.imgMain=[]
      }
      axios.get(`https://pim.wforwoman.com/pim/pimresponse.php/?service=category&store=1&url_key=top-wear-kurtas&page=${pagenumber}&count=20&sort_by=&sort_dir=desc&filter=${filter}`)
    .then(res=>{
      let products=  res.data.result.products;
      for(let item of products){
        this.imgMain.push({
              name: item.name,
              price: item.price,
              img: item.image,
              selling_price: item.selling_price,
              size: item.size,
              discount: item.discount,
              sku: item.sku,})
      }
      this.isLoading= false;

    })
          return this.imgMain
    },
    loadMore: function() {
       this.busy = true;
       this.data = this.filterDataCallAPI('',++this.pagenumber)
       this.busy = false;
    }
  },
  data() {
    return {
      imgMain:[],
      isLoading:false,
      data: [],
      busy: false,
      pagenumber:0,
      paramHome:''
    }
  },
};
</script>

<style lang="scss" scoped>
</style>