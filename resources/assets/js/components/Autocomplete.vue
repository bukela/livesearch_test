<template>
 <div>
  <input type="text" placeholder="what are you looking for?" v-model="query" v-on:keyup="autoComplete" class="form-control">
  <div class="panel-footer" v-if="results.length">
   <ul class="list-group">
    <li class="list-group-item" v-for="result in results" :key="result.id">
     {{ result.name }}
    </li>
   </ul>
  </div>
 </div>
</template>
<script>
 export default{
  data(){
   return {
    query: '',
    results: [],
    temp: []
   }
  },
  mounted() {
    this.getall();
  },
  methods: {
   autoComplete(){
    this.results = [];
    if(this.query.length > 2){
     axios.get('/api/search',{params: {query: this.query}}).then(response => {
      this.results = response.data;
     });
    } else {
         this.results = this.temp;
    }
   },
   getall() {
        axios.get('/api/search')
        .then((response) => {
            this.results = this.temp = response.data;
            })
        .catch((error) => this.errors = error.response.data.errors)

    }
  }
 }
</script>