<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  <div class="container">
    <div class="row">
      <!-- Input för kurskod-->
      <div class="col-sm">
        <label for="kursKodInput">Kurskod</label>
        <input class="form-control input-sm" id="kursKodInput" type="text" v-model="selected_kursKod" placeholder="Skriv in kurskoden här">
      <code>{{selected_kursKod}}</code>
      </div>
      <!-- Dropdown för modul-->
      <div class="col-sm">
          <div class="col-sm">
          <label for="modul" > Modul i Ladok </label>
          <Dropdown :url="URL_modul" @selectedItem="getSelectedItem" />
      </div>
      <code>{{selected_modul}}</code>
    </div>
    </div>
  </div>

      <!-- tabellen-->
      <!-- itererar nu över items-arrayen i data()-->
      <!-- ska uppdateras till lista med studenter och omdöme i Canvas efter att kurskoden är vald-->
      <b-table striped hover :items="items"></b-table>
    
  </div>

</template>

<!-- lägg till dataobjekt för kursmoment-->
<!-- lägg till dataobjekt för personnummer-->
<!-- lägg till fetch-metod för import av kursmoment-->
<!-- lägg till fetch-metod för import av personnummer-->
<script>
import Dropdown from './Dropdown.vue'

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
      return {
        items: [
          { first_name: 'Dickerson', last_name: 'Macdonald',  omdome_canvas: '', examinations_datum: '', betyg_ladok: '', status: '', information: ''},
          { first_name: 'Larsen', last_name: 'Shaw', omdome_canvas: '', examinations_datum: '', betyg_ladok: '', status: '', information: ''},
          { first_name: 'Geneva', last_name: 'Wilson', omdome_canvas: '', examinations_datum: '', betyg_ladok: '', status: '', information: ''},
          { first_name: 'Jami', last_name: 'Carney', omdome_canvas: '', examinations_datum: '', betyg_ladok: '', status: '', information: ''}
        ],
        URL_kursKod: "urltest",
        URL_modul: "",
        selected_kursKod: '',
        selected_modul: ''
      }

    },
    components: {
    Dropdown
  }, 
  methods: {
    getSelectedItem(item){
      this.selected_modul = item; 
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
