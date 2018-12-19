<template>
<div class="red">
  <b-container class="bv-example-row">
    <!-- <b-row>      
      <b-col><img src="https://i.wpimg.pl/O/644x428/d.wpimg.pl/406973798-2142198828/lpg-lpg-stag-instalacja.jpg" alt=""></b-col>
    </b-row> -->
    <b-row>
      <b-col class="title">OBLICZENIE RENTOWNOŚCI MONTAŻU INSTALACJI GAZOWEJ</b-col>
    </b-row>
    <br>
    <b-row>
      <b-col cols="8">
        <b-row>
          <b-col class="text">Przewidywany czas posiadania auta</b-col>
          <b-col>
            <b-input-group size="lg" append="lat(a)">
              <b-form-input  v-model.number="years"></b-form-input>
            </b-input-group>
          </b-col>
        </b-row>
        <br>
        <b-row>
          <b-col class="text">Cena montażu instalacji</b-col>
          <b-col>
            <b-input-group size="lg" append="zł">
              <b-form-input  v-model.number="totalCost"></b-form-input>
            </b-input-group>
          </b-col>
        </b-row>
        <br>
        <b-row>
          <b-col class="text">Aktualna cena benzyny</b-col>
          <b-col>
            <b-input-group size="lg" append="zł">
              <b-form-input  v-model.number="petrolCost"></b-form-input>
            </b-input-group>
          </b-col>
        </b-row>
        <br>
        <b-row>
          <b-col class="text">Aktualna cena LPG</b-col>
          <b-col>
            <b-input-group size="lg" append="zł">
              <b-form-input  v-model.number="gasCost"></b-form-input>
            </b-input-group>
          </b-col>
        </b-row>
        <br>
        <b-row>
          <b-col class="text">Zuzycie benzyny</b-col>
          <b-col>
            <b-input-group size="lg" append="L/100km">
              <b-form-input  v-model.number="petrolCombustion"></b-form-input>
            </b-input-group>
          </b-col>
        </b-row>
        <br>
        <b-row>
          <b-col class="text">Roczny przebieg</b-col>
          <b-col>
            <b-input-group size="lg" append="km">
              <b-form-input  v-model.number="avrKm"></b-form-input>
            </b-input-group>
          </b-col>
        </b-row>
        <br>
        <b-row>
          <b-col class="text">Dopłata za przegląd techniczny</b-col>
          <b-col>
            <b-input-group size="lg" append="zł">
              <b-form-input  v-model.number="inspection"></b-form-input>
            </b-input-group>
          </b-col>
        </b-row>
        <b-row><b-button @click="totalPetrolCost(), totalGasCost(), totalInspectionCost(), totalFiltrCost(),km(), kmGas(),difference(),kmNeeded(), yearsNeeded() " class="btn btn-outline-success btn-lg">OBLICZ</b-button></b-row>
      </b-col>
      <b-col class="summary">
        <b-row>
          <p><strong>Koszt instalacji gazowej</strong> </p>
        </b-row>
        <b-row>
          <p>{{totalCost}} <span>zł</span></p>
        </b-row>
        <b-row>
          <p><strong>Koszty paliwa</strong></p>
        </b-row>
        <b-row>
          <p>Benzyna - {{totalPetrol}} <span>zł</span></p>
        </b-row>
        <b-row>
          <p>GAZ - {{totalGas}} <span>zł</span></p>
        </b-row>        
        <b-row>
          <p><strong>Wydatki dodatkowe</strong></p>
        </b-row>
        <b-row>
          <p>Przegląd techniczny samochodu {{totalInspection}} <span>zł</span></p>
        </b-row>
        <b-row>
          <p>Filtry + wymiana {{totalFiltrCosts}} <span>zł</span></p>
        </b-row>
        <b-row class="totalSummary"><strong><p>Podsumowanie</p></strong></b-row>
        <b-row class="totalSummaryP"><p>Instalacja zwróci się po przejechaniu <strong>{{fullKm}} <span> km</span></strong> lub <strong>{{yearsNeed}} <span>miesiącach</span></strong></p></b-row>
      </b-col>      
    </b-row>
  </b-container>
  </div>
</template>
<script>
  export default {

    name: 'HelloWorld',
    data() {
      return {
        years: 1,
        totalCost: 3500,
        petrolCost: 5,
        gasCost: 2.20,
        petrolCombustion: 7,
        gasCombustion: 0,
        avrKm: 10000,
        returnKm: 0,
        returnTime: 0,
        filtrOne: 30,        
        inspection: 65,
        totalPetrol: 0,
        totalGas: 0,
        petrolPerYear: 0,
        totalInspection: 0,
        totalFiltrCosts: 0,
        kmCostPetrol: 0,
        kmCostGas: 0,
        differencePrice: 0,
        kmNeed: 0,
        yearsNeed: 0,        
      }
    },
    methods: {
      totalPetrolCost() {        //Całkowity koszt benzyny
        return this.totalPetrol = this.years * this.petrolCost * (this.avrKm /100 * this.petrolCombustion);
      },
      totalGasCost() {          //Całkowity koszt gazu
        return this.totalGas = Math.round(this.years * this.gasCost * (this.avrKm /100 * this.petrolCombustion * 1.1))
      },
      totalInspectionCost() {  //Całkowity koszt przeglądu
        return this.totalInspection = this.years * this.inspection;
      },
      totalFiltrCost() {    //Całkowity koszt filtrów z wymianą
        return this.totalFiltrCosts = this.filtrOne * this.years;
      },   
      km() {              
        return this.kmCostPetrol = this.petrolCombustion/100 * this.petrolCost  //koszt 1km benzyny
      },
      kmGas() {  //Koszt 1km gazu
        return this.kmCostGas = this.gasCombustions/100 *this.gasCost
      },
      difference() { //Różnica w cenie między gazem a benzyną na 1km
        return this.differencePrice = this.kmCostPetrol - this.kmCostGas
      },   
      kmNeeded() {
        return this.kmNeed = (this.totalCost + this.filtrOne + this.inspection)/this.differencePrice
      },
      yearsNeeded() {
        return this.yearsNeed = Math.round(this.kmNeed / this.avrKm *12)
      },
    },
    computed: {
      gasCombustions() {
        return (this.petrolCombustion *1.1)
      },
      fullKm() {
        return Math.round(this.kmNeed)
      },      
    }
  }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .summary {
    background: #e9ecef;
    border-radius: 5px;
    border-top: 2px solid lightskyblue;
    padding: 30px;
    color: balck
  }

  .summary p {
    padding-left: 10px;
    text-align: left
  }
  .title {
    color: white;
    font-weight: 700;
    font-size: 32px;
    padding-top: 30px;
  }
  .bv-example-row.container {
    padding-bottom: 100px;
    
}
button.btn.btn.btn-outline-success.btn-lg.btn-secondary {
    width: 96%;
    margin-top: 30px;
    margin: 30px auto 0;
}
.totalSummary {
  font-size: 20px;
}
.totalSummary p {
  margin-bottom: 0
}
.totalSummaryP {
  font-size: 20px
}
.text {
  margin: auto;
  text-align: left;
  color: white;
}
.red {
  background: url(../assets/background-instalacje-gazowe-LPG.jpg);
  background-repeat: no-repeat; 
  height: 100vh;
  width: 100vw;  
}

</style>