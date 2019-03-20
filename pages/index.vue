<template>
  <section class="container">
    <div>
      <logo />
      <h1 class="title">
        Vape Calculator
      </h1>
      <div id="app">
        <div class="labels">
          <p>Base :</p>
          <p>Flavor :</p>
          <p>Nicotine strength :</p>
          <p>Total volume :</p>
          <p>You nedd :</p>
          <p>Flavor conditionning :</p>
          <p>You need :</p>
        </div>
        <div class="inputs">
          <p><input class="field" v-model.number="baseVolume" v-on:keyup="calc"></p>
          <p><input class="field" v-model.number="flavorPercent" v-on:keyup="calc"></p>
          <p><input class="field" v-model.number="nicoStrength" v-on:keyup="calc"></p>
          <p><span class="result">{{totalVolume}}</span></p>
          <p><span class="result">{{nbBooster}}</span></p>
          <p><input class="field" v-model.number="flavorCond" v-on:keyup="calc"></p>
          <p><span class="result">{{nbFlavor}}</span></p>
        </div>
        <div class="metrics">
          <p>ml</p>
          <p>% : Volume : <span class="result">{{flavorVolume}}</span> ml</p>
          <p>mg/ml</p>
          <p>ml</p>
          <p>booster(s)</p>
          <p>ml</p>
          <p>flavor(s) of <span class="result">{{flavorCond}}</span> ml</p>
        </div>
      </div>      
    </div>
  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },

  data() {return {
    totalVolume: '',
    baseVolume: '',
    flavorPercent: '',
    flavorVolume:'',
    nicoStrength:'',
    nbBooster:'',
    flavorCond:'',
    nbFlavor:''
  }},

	methods: {
  	calc : function (event) {
      this.totalVolume = '';
      this.nbBooster = '';
      this.nbFlavor = '';

      this.totalVolume = this.baseVolume;
      if(!isNaN(this.flavorPercent)) {
        this.flavorVolume = this.baseVolume * this.flavorPercent / 100;
        this.totalVolume = this.totalVolume + this.flavorVolume;
      }
      /* x = 6b/140 */
      if (!isNaN(this.nicoStrength)) {
        var tempbooster = this.totalVolume * this.nicoStrength / 140;
        this.nbBooster = Math.round(tempbooster);
        this.totalVolume = Math.round(this.totalVolume + 10*tempbooster);
      }
      if(isFinite(this.flavorVolume/this.flavorCond)) {
        this.nbFlavor = Math.round(this.flavorVolume/this.flavorCond);
      }
		}
	}      
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-color:#3986c4;
}

p {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 25px;
  color:white;
}

.labels {
  /*border:solid;
  border-color: blue;*/
  float: left;
  text-align: right;
  width: 40%;
}

.inputs {
  /*border:solid;
  border-color:yellow;*/
  float: left;
  text-align: right;
  width: 15%;
}

.metrics {
  /*border:solid;
  border-color:green;*/
  float: right;
  text-align: left;
  width: 43%;
}

.result {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-weight: bold;
  font-size: 25px;
}

.field {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  background-color: #3986c4;
  border-style: none;
  font-size: 20px;
  width: 100%;
  color: white;
  text-align: right;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color:white;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color:white;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
