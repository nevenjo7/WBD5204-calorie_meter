<template>
  <div class="hello">
    <div id="header">
      <img src="../assets/heart.png" alt="1"><h1>Vue.js Calorie Counter</h1></div>
    <table>
      <tr>
        <th>DESCRIPTION</th>
        <th>CALORIES</th>
        <th>FAT</th>
        <th>CARBS</th>
        <th>PROTEIN</th>
      </tr>
      <!-- Item se odnosi na samu vrednost u listu, index je pozicija u listi -->
      <!-- Vezujemo index, da bude id referenca za svaki <li> -->

        <!-- v-for sluzi za loopovanje array itema, mora biti array -->
        <!-- v-for prolazi kroz array, cuva objekte kao referencu, vrednostima pristupamo sa .keyname -->
      <tr v-for='(item, index) in listOfItems' v-bind:key='index'>
        <!-- item predstavlja vrednost elementa imenovana u v-for, .keyname pristupa vrednosti elementa -->
        <td> {{item.name}} </td>
        <td> {{item.cal}} </td>
        <td> {{item.carbs}} </td>
        <td> {{item.fat}} </td>
        <td> {{item.protein}} </td>
        <!-- Kada je minificovana verzija html moze biti nezgodno kada nema razmak. Tada dodajemo &nbsp >> predstavlja element koji mora da napravi razmak tu -->
        <!-- html entity uvek pocinje sa & i zavrsava se sa ; -->
        <!-- Uvek bolje koristiti entity -->
        <!-- dodeljujemo removeItem funkciju, i prosledjujemo index kao parametar -->
        <button v-on:click='removeItem(index)'>X</button>
      </tr>
    </table>
    <div class="adding_styles">
      <div><span>Totals:</span>
      <span><strong>{{totals.totalCal}}</strong></span>
      <span><strong>{{totals.totalCarbs}}</strong></span>
      <span><strong>{{totals.totalFat}}</strong></span>
      <span><strong>{{totals.totalProt}}</strong></span></div>
    </div>
    <div>
      <!-- Povezujemo elemente sa inputima -->
      <!-- v-model omogucava da vrednost elementa povezemo sa inputom -->
      <!-- Sve sto se upise u input se prosledjuje elementu koji izaberemo -->
      <!-- prefix se dodaje da se dodele vrednosti atributima v-bind: -->
      <!-- v-bind cini da html atribut bude vue atribut -->
      <input v-bind:placeholder='placeholderValues.inputNameText' v-model='newValues.newName' type="text">
      <input v-bind:placeholder='placeholderValues.inputCalText' v-model='newValues.newCal' type="number">
      <input v-bind:placeholder='placeholderValues.inputCarbsText' v-model='newValues.newCarbs' type="number">
      <input v-bind:placeholder='placeholderValues.inputFatText' v-model='newValues.newFat' type="number">
      <input v-bind:placeholder='placeholderValues.inputProtText' v-model='newValues.newProt' type="number">
      <!-- v-on:click povezuje funckiju kada se klikne-->
      <button v-on:click='addItem'>+</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data:function() {
    return {
      // listOfitems je array []
      // u njemu su objekti {}
      placeholderValues:{
        inputNameText: 'Description',
        inputCalText: 'Calories',
        inputCarbsText: 'Carbs',
        inputFatText: 'Fat',
        inputProtText: 'Protein'},
      newValues: {
        newName: '',
        newCal: '',
        newCarbs: '',
        newFat: '',
        newProt: ''
      },
      // Uz pomoc pusha cemo dodati elemente na kraj liste
      // Novi objekat 
      listOfItems: [
        {
          name: 'Sargarepa',
          cal: 1000,
          carbs: 500,
          fat: 10,
          protein: 100
        },
        {
          name: 'krompir',
          cal: 1000,
          carbs: 500,
          fat: 10,
          protein: 100
        },
        {
          name: 'paradajz',
          cal: 1000,
          carbs: 500,
          fat: 10,
          protein: 100
        },
      ],
      // Dodacemo zbir svih cal, carbs, fat, prot
      totals: {
        totalCal: 0,
        totalCarbs: 0,
        totalFat: 0,
        totalProt: 0
      }
    }
  },
  // Pravimo funkciju koja ce da doda iteme u listu
  methods: {
    addItem:function() {
      // dodace novi element na kraj te liste
      // {} naznacava da je to objekat
      this.listOfItems.push({
        name: this.newValues.newName,
        cal: this.newValues.newCal,
        carbs: this.newValues.newCarbs,
        fat: this.newValues.newFat,
        protein: this.newValues.newProt
      });
      this.newValues.newName = null;
      this.newValues.newCal = null;
      this.newValues.newCarbs = null;
      this.newValues.newFat = null;
      this.newValues.newProt = null;
      this.calculatingValues();
    },
    // funkcija za uklanjanje itema
    removeItem:function(itemIndex) {
      this.listOfItems.splice(itemIndex, 1);
      this.calculatingValues();
    },
    // pravimo loop koji lupuje kroz elemente, i sabira elemente
    calculatingValues: function() {
      this.totals.totalCal = 0;
      this.totals.totalCarbs = 0;
      this.totals.totalFat = 0;
      this.totals.totalProt = 0;
      // Loopuje kroz sve stavke u listOfItems
      for(let i = 0; i < this.listOfItems.length; i++) {
        // Selectujemo vrednosti i dodajemo trenutni item
        this.totals.totalCal += parseInt(this.listOfItems[i].cal);
        this.totals.totalCarbs += parseInt(this.listOfItems[i].carbs);
        this.totals.totalFat += parseInt(this.listOfItems[i].fat);
        this.totals.totalProt += parseInt(this.listOfItems[i].protein);
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h1 {
  color: #fff;
  display:inline-block;
  margin: 30px 0px;
  font-weight: normal;
}
img {
  height: 80px;
  margin: 20px 0px;
  position: absolute;
  left: 30px;
  top: -10px;
}
#header {
 background-color:rgb(70, 161, 131);
 height: 100px;
 position: relative;
}
table {
  width:100%;
  text-align: left;
  box-sizing: border-box;
  table th {
  margin: 0px 20px;
  padding: 20px 50px;
  width: 120px;
  display:inline-block;
}
table td {
  margin: 0px 20px;
  padding: 20px 50px;
  width: 120px;
  display:inline-block;
  border-bottom: 1px solid rgb(91, 95, 99);
}
}

table th {
  margin: 0px 20px;
  padding: 20px 50px;
  width: 120px;
  display:inline-block;
}
table td {
  margin: 0px 20px;
  padding: 20px 50px;
  width: 120px;
  display:inline-block;
  border-bottom: 1px solid rgb(91, 95, 99);
}

button {
  margin: 10px ;
}

table th:first-child {
  padding-left: 30px;
  margin-left: 50px;
}

span {
  margin: 15px 20px;
  display: inline-block;
}
.adding_styles {
  background-color: grey;
  height: 50px;
  margin: 15px;
}
th {
  padding: 30px 0px;
}
input {
  border: none;
  border-bottom: 1px solid grey;
  margin: 2px;
}


</style>
