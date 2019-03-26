<template>
  <section class="container">
    <div>
      <WapesLogo />
      <h1>Wapes</h1>
      <div id="app">
        <form>
          <div class="row">
            <label for="total">Total (ml)</label>
            <input id="total" type="number" v-model.number="totalVolume" v-on:keyup="calc(true)">
          </div>
          <div class="row">
            <label for="flavor">Flavor (%)</label>
            <input id="flavor" type="number" max="100" v-model.number="flavorPercent" v-on:keyup="calc">
          </div>
          <div class="row">
            <label for="nico">Nico Strength (mg/ml)</label>
            <select id="nico" v-model.number="nicoStrength" v-on:click="calc">
              <option v-for="nico in nicos" v-bind:value="nico.rate" v-bind:key="nico.id">
                {{ nico.rate }}
              </option>
            </select>
          </div>
          <div class="row">
            <label for="base"> Base (ml)</label>
            <input id="base" type="number" v-model.number="baseVolume" v-on:keyup="calc(false)">
          </div>
          <div class="row">
            <label for="flavcond">Flavor conditionning (ml)</label>
            <input id="flavcond" type="number" v-model.number="flavorCond" v-on:keyup="calc">
          </div>
          <div class="row">
            <p>
              <span class="result">{{nbBooster}}</span>  booster(s) of 10ml for
              <span class="result">{{boostVolume}}</span> ml
            </p>
            <p>
              <span class="result">{{nbFlavor}}</span>  flavor(s) of {{flavorCond}} ml for
              <span class="result">{{flavorVolume}}</span> ml
            </p>
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
    boostVolume:'',
    flavorCond:'',
    nbFlavor:'',
    isTotal:false,
    nicos: [
      {rate:''},
      {rate:3},
      {rate:6},
      {rate:9},
      {rate:12},
      {rate:16}
    ]
  }},

	methods: {
    calcbase : function() {
      this.totalVolume = '';
      this.totalVolume = Math.round(this.baseVolume / (1- (this.flavorPercent/100) - (this.nicoStrength/20)));

      if(this.flavorPercent !='' && !isNaN(this.flavorPercent)) {
        this.flavorVolume = Math.round(this.totalVolume * this.flavorPercent/100);
      }
      /* x = 6b/140 */
      if (!isNaN(this.nicoStrength)) {
        this.boostVolume = Math.round(this.totalVolume * this.nicoStrength / 20);
        this.nbBooster = Math.ceil(this.boostVolume/10);
      }
    },

    calctotal : function() {
      this.baseVolume = '';
      this.baseVolume = this.totalVolume;

      if(this.flavorPercent !='' && !isNaN(this.flavorPercent)) {
        this.flavorVolume = Math.round(this.totalVolume * this.flavorPercent/100);
        this.baseVolume = this.baseVolume - this.flavorVolume;
      }
      /* x = 6b/140 */
      if (!isNaN(this.nicoStrength)) {
        this.boostVolume = Math.round(this.totalVolume * this.nicoStrength / 20);
        this.nbBooster = Math.ceil(this.boostVolume/10);
        this.baseVolume = Math.ceil(this.baseVolume - this.boostVolume);
      }
    },

    calc : function (isTotal) {
      this.nbBooster = '';
      this.flavorVolume = '';
      this.nbFlavor = '';
      if(isTotal==true || isTotal==false) this.isTotal = isTotal;
      if(this.isTotal==true) {
        this.calctotal ();
      } else {
        this.calcbase ();
      }
      if(isFinite(this.flavorVolume/this.flavorCond)) {
        this.nbFlavor = Math.ceil(this.flavorVolume/this.flavorCond);
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
  width: 90%;
  color: white;
  text-align: center;
}

input[type=number]::-webkit-inner-spin-button, 
input[type=number]::-webkit-outer-spin-button { 
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    margin: 0; 
}

select {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  background-color: #3986c4;
  border: 1px solid;
  font-size: 1.3rem;
  width: 90%;
  color: white;
  text-align: center;
  text-align-last: center;
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
