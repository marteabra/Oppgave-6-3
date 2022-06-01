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
      <div v-if="error" class="error">{{ error }}</div>
    </section>    
  </main>
</template>

<script>
import Header from './../components/Header.vue';

export default {
  components: {
    Header,
  },

  data(){
    return {
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
      } catch (error) {                     //error message when site crashes -> could use some styling but wasn`t able to get error message on screen?
        this.error = error.message;
      }
    },

    async handleResponse(response) {
      if (response.status >= 199 && response.status < 300) {
        const result = await response.json();
        this.data = result;
      } else {
        throw new Error("Ops! Noe gikk galt..");
      }
    },
  },

    computed: {
      todaysDate(){
        let today = new Date().toISOString().slice(0, 10);       //  Using Date() to create and use todays date to display on site
        return today; 

      }
    }
}
</script>

<style>
.el-prices{
  background: var(--background);
  height: 100vh;
  margin: 0 auto;
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
  font-size: var(--font-size-big-mobile);
}

.el-prices__heading {
  font-size: var(--font-size-medium);
  display: flex;
  justify-content: space-evenly;
}

.el-prices__info {
  display: grid;
  margin: 0 auto;
  grid-template-columns: repeat(2, 1fr);
  font-size: var(--font-size-small-mobile);
  padding-top: 40px;
  width: 60%;
  border: 1px solid white;
  text-align: center;
}

.error {
  position: absolute;
  height: 100vh;
  width: 100vw;
  background: black;
}

@media screen and (min-width: 1100px) {
  .el-prices__date {
    margin-bottom: 40px;
    height: 25vh;
  }

  .el-prices__date div {
    font-size: var(--font-size-big-mobile);
  }

  .el-prices__info {
    display: grid;
    margin: 0 auto;
    grid-template-columns: repeat(2, 1fr);
    font-size: var(--font-size-small);
    padding-top: 40px;
    width: 60%;
    border: 1px solid white;
    text-align: center;
  } 
}
</style>