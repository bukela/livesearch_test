<template>
 <div>
  <input type="text" placeholder="what are you looking for?" v-model="query" v-on:keyup="autoComplete" class="form-control">

<table>
<tbody>
<tr v-for="result in results" :key="result.id">
        <td class="table-client">{{ result.id }}</td>
        <td class="table-number">{{ result.name }}</td>
        <td class="table-text">{{ result.email }}</td>
        <!-- <td class="table-text">{{ $client->tekuci_racun }}</td> -->
        <!-- <td class="table-text">{{ $client->email }}</td> -->
        <td class="table-text has-text-centered"><a href="#"><i class="fa fa-eye edit-ico"></i></a></td>
    </tr>
</tbody>
</table>

 </div>
</template>
<script>
export default {
  data() {
    return {
      query: "",
      results: [],
      temp: []
    };
  },
  mounted() {
    this.getall();
  },
  methods: {
    autoComplete() {
      this.results = [];
      if (this.query.length > 1) {
        axios
          .get("/api/search", { params: { query: this.query } })
          .then(response => {
            this.results = response.data;
          });
      } else {
        this.results = this.temp;
      }
    },
    getall() {
      axios
        .get("/getusers")
        .then(response => {
          this.results = this.temp = response.data;
        })
        .catch(error => (this.errors = error.response.data.errors));
    }
  }
};
</script>