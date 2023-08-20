<script lang="js">

export default {
  data: () => ({
    cityData: "",
    currentCity: "",
    dataRows: 50,
    pignationStart: 50,
    dataSections: [1,2,3,4,5,6,7,8,9,10],
    activeSection: 1
    
  }),
  methods: {
    async fetchCity(cityQuery) {

      const query = cityQuery
      const wskey = "rgeallep"
      const rows = this.dataRows
      const media = true
      const thumbnail = true
      const start = this.pignationStart
      const landingpage = true
  
      const response = await fetch(`https://api.europeana.eu/record/v2/search.json?query=${query}&wskey=${wskey}&rows=${rows}&media=${media}&thumbnail=${thumbnail}&start=${start}&landingpage=${landingpage}&theme=photography`)
      let data  = await response.json()
      console.log(data.items)
      this.currentCity = cityQuery
      this.cityData = data.items
    }, 
    changePignationStart(dataSection) {
      this.pignationStart = this.dataRows * dataSection
      this.fetchCity(this.currentCity)
      this.activeSection = dataSection
    }
    
  },
  created() {
    this.fetchCity('Vilnius')

  }
}

</script>

<template>

  <header>

    
  <h1 class="app-heading">Miestų archyvas</h1>

  <nav>
    <ul class="cities">
      <li><button @click="fetchCity(`Vilnius`)">Vilnius</button></li>
      <li><button @click="fetchCity(`Kaunas`)">Kaunas</button></li>
      <li><button @click="fetchCity(`Klaipėda`)">Klaipėda</button></li>
      <li><button @click="fetchCity(`Šiauliai`)">Šiauliai</button></li>
      <li><button @click="fetchCity(`Panevėžys`)">Panevėžys</button></li>

    </ul>
  </nav>

  <div class="pignation-wrap">
    <ul class="pignation">
      <li :key="item" v-for="item in dataSections">

        <button  @click="changePignationStart(item)"
        :class="{ 'active-button': item === activeSection, 'notActive': item !== activeSection }"
        >
          {{ item }}
        </button>


  
      </li>
    </ul>
  </div>

  </header>


  <section>

    <span class="current-city">{{ currentCity }}.</span>

  </section>


  <section class="city-section">

    
  <div v-for="(item, index) in cityData" :key="index" class="card">

  <img class='image' :src="item.edmIsShownBy[0]" alt="" />


  <p v-if="item.title !== undefined">
  Pavadinimas: {{ item.title[0] }}
  </p>

 
  <p v-if="item.edmTimespanLabel !== undefined">
   Metai: {{ item.edmTimespanLabel[0].def }}
  </p>

  <p v-if="item.dcDescription !== undefined && item.dcDescription[0].length < 300">
  Aprašymas: {{ item.dcDescription[0] }}
  </p>








  </div>

  </section>


</template>

<style scoped>

.cities {
  display: flex;
  flex-wrap: wrap;
  list-style-type: none;
  gap: 10px;
  justify-content: center;
  margin-bottom: 75px;
}

.cities li button {
  border: azure;
  padding: 20px 40px;
  background-color: #006a44;
  color: white;
  border-radius: 15px;
  transition: opacity 0.2s;
}

.cities li button:hover {
  opacity: 0.8;
  cursor: pointer;
}

.image {
  height: 100%;
  width: 100%;

  object-fit: contain;
}

.city-section {

  display: grid;
  grid-template-columns: 1fr 1fr;
  justify-content: center;
  align-items: center;
  justify-items: center;
}

.city-section div {
  max-width: 500px;
  max-width: 500px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.city-section div p {
 display: flex;
 align-self: start;
}





@media (max-width: 999px) {
      
    .city-section {
      display: grid;
      grid-template-columns: 1fr;


    }


}

.current-city {
  font-size: large;
  padding-left: 10px;

  
  
}

.app-heading {
  text-align: center;
}

.pignation {

  display: flex;
  list-style-type: none;

}

.pignation-wrap{
  display: flex;
  justify-content: center;
}
.notActive {
  
  border: none;
  background-color: transparent;
  font-size: 1.2rem;
  color: rgb(129, 129, 254);
  cursor: pointer;
}

.active-button {
  border: none;
  background-color: transparent;
  font-size: 1.2rem;
  cursor: pointer;
  color: rgb(255, 2, 2);
}

</style>
