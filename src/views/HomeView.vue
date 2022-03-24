<template>
   <v-form>
    <v-container>
      <v-row>
        <v-col
          cols="12"
          sm="6"
          md="3"
        >
          <v-text-field
            label="Ime" @change="getAll($event)"
          ></v-text-field>
        </v-col>
      </v-row>

  <v-data-table
    :headers="headers"
    :items="commit"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>
    </v-container>
  </v-form>
</template>

<script>
  export default {
    name: 'Home',

    data: function(){
      return{
        godine: {},
        spol: {},
        naci: {},
        commit: [],
        headers: [],


         headers: [{
        text: "Ime",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Država", value: "country" },
      { text: "Probability", value: "probability of country" },
      { text: "Godine", value: "age" },
      { text: "Spol", value: "gender" }, 
      { text: "Probability of Gender", value: "probability of gender" },  
    ],
      }
    },

    methods: {
      async getGodine(value){
        let age = await fetch("https://api.agify.io?name=" + value) 
        this.godine = await age.json()  
      },

      async getSpol(value){
        let sex = await fetch ("https://api.genderize.io?name=" + value)
        this.spol = await sex.json()  
      },

      async getNacionalnost(value){
        let nac = await fetch("https://api.nationalize.io?name=" + value)
        this.naci = await nac.json()          
      },

      async getAll(value){
        await this.getGodine(value)
        await this.getSpol(value)
        await this.getNacionalnost(value)

        this.commit.push({
          name: value,
          age: this.godine.age,
          gender: this.spol.gender,
          "probability of gender": this.spol.probability,
          country: this.naci.country[0].country_id,
          "probability of country": this.naci.country[0].probability,
        })

        console.log(this.commit)
      }
    }
  }
</script>
