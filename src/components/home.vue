<script>
export default {
    name:'home_app',
    data() {
      return {
        Creature_Boss:'',
        Creature_Boss_img:'',
        Boosted_Boss: '',
        Boosted_Boss_img: '',
        news:'',
        tickers:''
      }
    },
    mounted(){
      BoostedBoss().then((data)=>{
        this.Creature_Boss = data.boostable_bosses.boosted.name
        this.Creature_Boss_img = data.boostable_bosses.boosted.image_url
      })
      CreatureBoos().then((data)=>{
        this.Boosted_Boss = data.creatures.boosted.name
        this.Boosted_Boss_img = data.creatures.boosted.image_url
      })

      News().then((data)=>{
        let arrayNews = []
        for (let i = 0; i < 2; i++) {
          arrayNews.push(data.news[i])
        }
        this.news = arrayNews
      })

      Ticker().then((data)=>{
        console.log(data.news)
        let arrayTickers = []
        for (let i = 0; i < 10; i++) {
          arrayTickers.push(data.news[i])
        }
        this.tickers = arrayTickers
      })

    }
}
async function BoostedBoss() {
  let url = 'https://api.tibiadata.com/v3/boostablebosses';
  let respuesta = fetch(url)
  let boostedBoos = (await respuesta).json()
  return boostedBoos
}

async function CreatureBoos() {
  let url = 'https://api.tibiadata.com/v3/creatures';
  let respuesta = fetch(url)
  let creatureBoos = (await respuesta).json()
  return creatureBoos
}

async function News() {
  let url = 'https://api.tibiadata.com/v3/news/latest';
  let respuesta = fetch(url)
  let news = (await respuesta).json()
  return news
}

async function Ticker() {
  let url = 'https://api.tibiadata.com/v3/news/newsticker';
  let respuesta = fetch(url)
  let ticker = (await respuesta).json()
  return ticker
}
</script>
<template>
<main class="mdl-layout__content barraOpciones contenedor">
    <br><br><br>
<div class="mdl-grid">

<div class="mdl-cell mdl-cell--4-col contenedorTarjetas">
  <div class="demo-card-wide mdl-card mdl-shadow--2dp">
    <div class="mdl-card__title">
    <h2 class="mdl-card__title-text">Creature Boss</h2>
  </div>
      <div class="mdl-card__title">
        <img :src="Boosted_Boss_img" alt="">
      </div>
      <div class="mdl-card__supporting-text" style="color: white">
        {{Boosted_Boss}}
      </div>
    </div>
</div>

<div class="mdl-cell mdl-cell--4-col contenedorTarjetas">
  <div class="demo-card-wide mdl-card mdl-shadow--2dp">
    <div class="mdl-card__title">
    <h2 class="mdl-card__title-text">Boosted Boss</h2>
  </div>
      <div class="mdl-card__title">
        <img :src="Creature_Boss_img" alt="">
      </div>
      <div class="mdl-card__supporting-text" style="color: white">
        {{Creature_Boss}}
      </div>
    </div>
</div>

<div class="mdl-cell mdl-cell--12-col contenedorTarjetas" v-for="newsData in news" :key="newsData">
  <div class="demo-list-three mdl-list slider-child" style="width: 900px; margin: 20px;">
  <li class="mdl-list__item mdl-list__item--three-line">
    <span class="mdl-list__item-primary-content" style="text-align: center;">
      <div class="contenedor_titulos">
        <span class="elementos">{{newsData.category}}</span>
        <span class="elementos">{{newsData.type}}</span>
        <span class="elementos">{{newsData.date}}</span>
    </div>
    <span class="mdl-list__item-text-body" style="color: white">
            {{newsData.news}}
      </span>
    </span>
  </li>
</div>
</div>

<!--Listas-->
<div class="slider">
  <div v-for="Ticker in tickers" :key="Ticker" class="demo-list-three mdl-list slider-child" style="width: 900px; margin: 20px;">
    <li class="mdl-list__item mdl-list__item--three-line">
      <span class="mdl-list__item-primary-content" style="text-align: center;">
        <div class="contenedor_titulos">
          <span class="elementos">{{Ticker.category}}</span>
          <span class="elementos">{{Ticker.type}}</span>
          <span class="elementos">{{Ticker.date}}</span>
      </div>
      <span class="mdl-list__item-text-body" style="color: white">
        {{Ticker.news}}
        </span>
      </span>
    </li>
  </div>
</div>

</div>
</main> 
</template>
<style scoped>
@import '../assets/css/experienciacompartida.css';

.slider {
    display: flex;
    width: 100%;
    height: 23vh;
    color: aliceblue;
    scroll-snap-type: x mandatory;
    overflow-x: scroll;
    overflow-y: hidden;
}

.slider-child {
  flex: 0 0 100%;
  width: 100%;
  object-fit: cover;
  scroll-snap-align: center;
}
/**para los elementos de las tarjetas */
.contenedor_titulos {
  text-align: center; /* centra horizontalmente los elementos */
  display: inline;
}

.elementos {
  color: white;
  width: 50px;
  margin-bottom: 15px;
  height: 10px;
  margin: 30px;
}


.elementos-contenedor {
  color: blanchedalmond;
  margin: 30px;
}

/**Estilo del scroll */
.slider::-webkit-scrollbar {
    -webkit-appearance: none;
}

.slider::-webkit-scrollbar:vertical {
    width:10px;
}

.slider::-webkit-scrollbar-button:increment,.slider::-webkit-scrollbar-button {
    display: none;
} 

.slider::-webkit-scrollbar:horizontal {
    height: 10px;
}

.slider::-webkit-scrollbar-thumb {
    background-color: #37474F;
    border-radius: 20px;
    border: 2px solid #263238;
}

.slider::-webkit-scrollbar-track {
    border-radius: 10px;  
}
</style>