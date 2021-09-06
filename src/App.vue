<template>
  <div class="container">
    <div class="form-group mb-4">
      <select v-model="selectedCountry" class="form-control">
        <option value="">Global</option>
        <option
          v-for="country in countries"
          :key="country"
          :value="country.Slug"
          >{{ country.Country }}</option
        >
      </select>
    </div>
    <div class="row justify-content-around">
      <div class="col-md-4">
        <div class="stats-item btn-danger blue-line">
          <span class="item-title">
            <span>Confirmed</span>
            <i class="fas fa-virus"></i>
          </span>
          <span class="item-count">{{ numberFormat(confirmed) }}</span>
        </div>
      </div>
      <div class="col-md-4">
        <div class="stats-item btn-danger red-line">
          <span class="item-title"
            ><span>Deaths</span>
            <i class="fas fa-bed"></i>
          </span>
          <span class="item-count">{{ numberFormat(deaths) }}</span>
        </div>
      </div>
      <div class="col-md-4">
        <div class="stats-item btn-danger green-line">
          <span class="item-title">
            <span>Recovered</span>
            <i class="fas fa-child"></i>
          </span>
          <span class="item-count">{{ numberFormat(recovered) }}</span>
        </div>
      </div>
    </div>
    <hr />
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Country</th>
          <th scope="col">Confirmed</th>
          <th scope="col">Deaths</th>
          <th scope="col">Recovered</th>
        </tr>
      </thead>
      <tr v-for="(item,index) in data.Countries" :key="index">
        <th scope="row">{{index+1}}</th>
        <td>{{ numberFormat(item.Country) }}</td>
        <td>{{ numberFormat(item.TotalConfirmed) }}</td>
        <td>{{ numberFormat(item.TotalDeaths) }}</td>
        <td>{{ numberFormat(item.TotelRecoverd) }}</td>
      </tr>
    </table>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      selectedCountry: "",
      countries: {},
      data: {},
      confirmed: 0,
      deaths: 0,
      recovered: 0,
    };
  },
  methods: {
    getCountries() {
      axios.get("https://api.covid19api.com/countries").then((response) => {
        this.countries = response.data;
      });
    },
    getSummaryData() {
      axios.get("https://api.covid19api.com/summary").then((response) => {
        this.confirmed = response.data.Global.TotalConfirmed;
        this.deaths = response.data.Global.TotalDeaths;
        this.recovered = response.data.Global.TotalRecovered;
        this.data=response.data
      });
    },
    numberFormat(number) {
      if (number===0 || number==="" || number===null  || number===undefined) {
        number=0
      }
      return number.toLocaleString();
    },
  },
  mounted() {
    this.getCountries();
    this.getSummaryData();
  },
};
</script>
<style>
body {
  background: antiquewhite;
}
table {
  background: #fff;
}
.container {
  margin-top: 40px;
}
.stats-item {
  padding: 20px;
  text-align: center;
  margin-bottom: 20px;
  color: rgb(49, 49, 49);
  border-radius: 15px 0px 0px 15px;
  background-color: #fff;
}
.green-line {
  border-left: 8px solid rgb(42, 221, 111);
}
.green-line:hover {
  background-color: rgb(42, 221, 111);
  border-left: 8px solid rgb(49, 49, 49);
}
.blue-line {
  border-left: 8px solid rgb(42, 105, 221);
}
.blue-line:hover {
  background-color: rgb(42, 105, 221);
  border-left: 8px solid rgb(49, 49, 49);
}
.red-line {
  border-left: 8px solid rgb(226, 59, 59);
}
.red-line:hover {
  background-color: rgb(226, 59, 59);
  border-left: 8px solid rgb(49, 49, 49);
}
.item-title {
  display: block;
  font-size: 25px;
  display: flex;
  justify-content: space-around;
}
.item-title i {
  font-size: 40px;
}
.item-count {
  font-weight: bold;
  font-size: 30px;
}
</style>
