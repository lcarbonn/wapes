<template>
  <section class="container">
    <div>
      <WapesLogo />
      <h1 class="title">
        Wapes
      </h1>
      <div id="app">
        <form>
          <div class="labels">
            <p><label for="base">Base :</label></p>
            <p><label for="flavor">Flavor :</label></p>
            <p><label for="nico">Nico Strength :</label></p>
            <p><label for="flavcond">Flavor cond :</label></p>
            <p>You need :</p>
            <p>You need :</p>
            <p>for total :</p>
          </div>
          <div class="inputs">
            <p><input class="field" id="base" v-model.number="baseVolume" v-on:keyup="calc"></p>
            <p><input class="field" id="flavor" v-model.number="flavorPercent" v-on:keyup="calc"></p>
            <p><input class="field" id="nico" v-model.number="nicoStrength" v-on:keyup="calc"></p>
            <p><input class="field" id="flavcond" v-model.number="flavorCond" v-on:keyup="calc"></p>
            <p><span class="result">{{nbBooster}}</span></p>
            <p><span class="result">{{nbFlavor}}</span></p>
            <p><span class="result">{{totalVolume}}</span></p>
          </div>
          <div class="metrics">
            <p>ml</p>
            <p>%</p>
            <p>mg/ml</p>
            <p>ml</p>
            <p>booster(s)</p>
            <p>flavor(s) of <span class="result">{{flavorCond}}</span> ml for <span class="result">{{flavorVolume}}</span> ml</p>
            <p>ml</p>
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

.links {
  padding-top: 15px;
}
</style>
