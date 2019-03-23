<template>
  <section class="container">
    <div>
      <WapesLogo />
      <h1>Wapes</h1>
      <div id="app">
        <form>
          <div class="row">
            <label for="base"> Base (ml)</label>
            <input id="base" type="text" v-model.number="baseVolume" v-on:keyup="calc">
          </div>
          <div class="row">
            <label for="flavor">Flavor (%)</label>
            <input id="flavor" type="number" max="100" v-model.number="flavorPercent" v-on:keyup="calc">
          </div>
          <div class="row">
            <label for="nico">Nico Strength (mg/ml)</label>
            <input id="nico" v-model.number="nicoStrength" v-on:keyup="calc">
          </div>
          <div class="row">
            <label for="flavcond">Flavor conditionning (ml)</label>
            <input id="flavcond" v-model.number="flavorCond" v-on:keyup="calc">
          </div>
          <h4>You need</h4>
          <div class="row">
            <p><span class="result">{{nbBooster}}</span>  booster(s) of 10ml</p>
            <p><span class="result">{{nbFlavor}}</span>  flavor(s) of {{flavorCond}} ml for</span>
            <span class="result">{{flavorVolume}}</span> ml</p>
            <p>for a total of <span class="result">{{totalVolume}}</span> ml</p>
          </div>
        </form>
      </div>
    </div>
  </section>
</template>

<script>
import WapesLogo from '~/components/Logo.vue'

export default {
  components: {
    WapesLogo
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
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-color:#3986c4;
}

.row {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 1px;
}

p, label {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 1.5rem;
  color:white;
  font-weight: 300;
}

.result {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-weight: bold;
  font-size: 1.5rem;
}

input {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  background-color: #3986c4;
  border: 1px solid;
  font-size: 1.3rem;
  width: 10em;
  color: white;
  text-align: center;
  height: 1.5rem;  
}

h1 {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 3rem;
  color:white;
  letter-spacing: 1px;
}

h4 {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 1.5rem;
  color:white;
  letter-spacing: 1px;
}

.links {
  padding-top: 15px;
}
</style>
