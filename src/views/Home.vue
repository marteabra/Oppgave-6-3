<template>
<Header/>
  <main class="el-prices">
    <section class="el-prices__date">
      <div>Dagens Dato</div>
        <div>
          {{  todaysDate }}
        </div>
    </section>
   <div class="el-prices__heading">
     <span>Klokkeslett</span>
     <span>Pris i kr</span>
   </div>
    <section class="el-prices__info" v-for="data in data">
      <div class="el-prices__time">
           {{ data.valid_from.slice(11, 16)}}
              -
          {{ data.valid_to.slice(11, 16)}}
      </div>
      <div class="el-prices__price" >
        <div class="price">
          {{data.NOK_per_kWh}}
        </div>
      </div>
    </section>
    
  </main>

</template>

<script>
import Header from './../components/Header.vue';

export default {
  components: {
    Header
  },

  data(){
    return {
      appName: import.meta.env.VITE_APP_NAME,
      data: [],
      error: "",
    }
  },

  created() {
    this.fetchData(); 
  },

  methods: {
     async fetchData() {
      const url = `https://playground-norway-power.ffail.win/?zone=NO2&date=2022-01-31&key=2yywtyet`;   //api url containing data
      const response = await fetch(url);
      try {
        await this.handleResponse(response);
      } catch (error) {
        this.error = error.message;
      }
    },

    async handleResponse(response) {
      if (response.status >= 199 && response.status < 300) {
        const result = await response.json();
        this.data = result;
      }
    },
  },

    computed: {
      todaysDate(){
        let today = new Date().toISOString().slice(0, 10)
        return today; 
      }
    }
}
</script>

<style>
.el-prices{
  background: var(--background);
  height: 100vh;
}

.el-prices__date {
  display: grid;
  padding: 20px;
  background: var(--foreground);
  height: 15vh;
  margin-bottom: 40px;
}

.el-prices__date div {
  margin: 0 auto;
  font-size: var(--font-size-big);
}

.el-prices__heading {
  font-size: 40px;
  display: flex;
  justify-content: space-evenly;
}

.el-prices__info {
  display: grid;
  margin: 0 auto;
  grid-template-columns: repeat(2, 1fr);
  font-size: var(--font-size-small);
  padding-top: 40px;
  width: 80%;
  border: 1px solid white;
  text-align: center;
}


</style>