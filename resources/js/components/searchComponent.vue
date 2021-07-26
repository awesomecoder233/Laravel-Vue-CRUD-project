<template>

<div class="card">

  <div class="card-header" id="hide">
    <div class="row">
       <div class="col-md-2">
          <input type="text" @keyup="searchUnit" placeholder="Search" v-model="search" class="form-control form-control-sm">
        </div>
    </div>
  </div>


  <div class="card-body" id="hide_again">
    <table id="" class="table table-bordered table-striped">
      <thead>
      <tr>
         <th>Name</th>
         <th>Status</th>
       </tr>
      </thead>
     <tbody>
        <tr v-for="unit in units.data" :key="unit.id">
          <td>{{ unit.name }}</td>
          <td>{{ unit.is_active == 1 ? 'Active' : 'InActive' }}</td>
        </tr>
     </tbody>
     <pagination :data="units" @pagination-change-page="getResults"></pagination>
    </table>

  </div>
</div>
</template>

<script>

    import _ from "lodash";

    export default {

    data() {
        return {
            units: {},
            search: '',
        }
    },
    methods: {

        fetchingAllUnit() {
          axios.get("api/unit").then( data => (this.units = data.data));
        },
        getResults(page = 1) {
              axios.get('api/unit?page=' + page)
                .then(response => {
                  this.customers = response.data;
              });
        },

        searchUnit:_.debounce(function(){
          axios.get('/search_unit?q='+this.search)
                .then((response)=>{
                this.units.data = response.data.unit
          })
        }),
    },
    created(){

        this.fetchingAllUnit();

    },

}
</script>