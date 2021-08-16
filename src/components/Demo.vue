<template>
  <div>
    <div class="row " v-for="item,index in totalfilters" :key="index">
      <div>
        <!-- Using value -->
        <b-button v-b-toggle="item.category"  class="m-1">{{item.category}}</b-button>
        <!-- Element to collapse -->
        <b-collapse :id='item.category' class="ml-4" v-for="subcategory,index in item.options" :key="index">
          <b-card>
            <input
              class="form-check-input"
              type="checkbox"          
              :value="subcategory.value"
              :id="subcategory.value"
              @change="setFilters($event,subcategory.code,subcategory.value)"
            />
            <label class="form-check-label" for="flexCheckDefault">
              {{subcategory.value }} ( {{ subcategory.total}}) 
            </label><br/>
          </b-card>
        </b-collapse>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      totalfilters:[],
      filters: {
        kurta: false,
        pant: false,
        patloon: false,
      },
      values:{}
    };
  },
  methods: {
    setFilters(event,code,value) {
      const inputId = event.target.id;
      const isActive = event.target.checked;
      console.log(inputId,isActive)
      const name = code+"-"+value
      if(isActive){
      this.values[inputId] = name 
      }
      else{
      delete  this.values[inputId] 
      }
      const updatedFilters = {
        ...this.filters,
        [inputId]: isActive,values:this.values
      };
      this.filters = updatedFilters;
      this.$emit('filter', updatedFilters )
       console.log( this.filters);
    },
       loadFiltersAction(){
      axios
        .get(
          "https://pim.wforwoman.com/pim/pimresponse.php/?service=category&store=1&url_key=top-wear-kurtas&page=1&count=20&so"
        )
        .then((res) => {
          let filters = res.data.result.filters;
          for (let filterItem of filters) {
            let filter = {
              category: filterItem.filter_lable,
              options: filterItem.options,
              timeStamp: Math.random().toString(),
            };
            this.totalfilters.push(filter);
          }
        });
    },
  },
  created(){
    this.loadFiltersAction()
  }
};
</script>

<style  scoped>
button{

    color: black;
    width: 150px;
}
</style>