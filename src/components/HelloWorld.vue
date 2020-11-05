<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  <div class="container">
    <div class="row">
      <!-- Input för kurskod-->
      <div class="col-sm">
        <label for="kursKodInput">Kurskod</label>
        <input class="form-control input-sm" v-on:keyup.enter="submitKursKod" id="kursKodInput" type="text" v-model="selected_kursKod" placeholder="Skriv in kurskoden här">
      <code>{{selected_kursKod}}</code>
      </div>
      <!-- Dropdown för modul-->
      <div class="col-sm">
          <div class="col-sm">
          <label for="modul" > Modul i Ladok </label>
          <Dropdown :kurskod="selected_kursKod" @selectedItem="getSelectedItem" />
      </div>
      <code>{{selected_modul}}</code>
    </div>
    </div>
  </div>

      <!-- tabellen-->
      <!-- itererar nu över items-arrayen i data()-->
      <!-- ska uppdateras till lista med studenter och omdöme i Canvas efter att kurskoden är vald-->
      <b-table striped hover :items="items" :fields="fields"></b-table>
    
  </div>

</template>

<script>
import Dropdown from './Dropdown.vue';
import axios from 'axios';
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
      return {
        fields: ['firstName', 'lastName', 'grades' ],
        items: [{"firstName":"Lukas","grades":[{"grade":"U","nameOfAssignment":"Inlämningsuppgift1"},{"grade":"G","nameOfAssignment":"Inlämningsuppgift2"},{"grade":"U","nameOfAssignment":"Inlämningsuppgift3"},{"grade":"VG","nameOfAssignment":"Projekt"}],"lastName":"Skog","userName":"luksok-8"},{"firstName":"Magdalena","grades":[{"grade":"VG","nameOfAssignment":"Inlämningsuppgift1"},{"grade":"VG","nameOfAssignment":"Inlämningsuppgift2"},{"grade":"G","nameOfAssignment":"Inlämningsuppgift3"},{"grade":"VG","nameOfAssignment":"Projekt"}],"lastName":"Fernlund","userName":"magrad-2"},{"firstName":"Simon","grades":[{"grade":"G","nameOfAssignment":"Inlämningsuppgift1"},{"grade":"G","nameOfAssignment":"Inlämningsuppgift2"},{"grade":"VG","nameOfAssignment":"Inlämningsuppgift3"},{"grade":"VG","nameOfAssignment":"Projekta"}],"lastName":"Sterner","userName":"simste-6"}
         ],
        selected_kursKod: '',
        selected_modul: ''
      }

    },
    components: {
    Dropdown,
  }, 
  methods: {
    getSelectedItem(item){
        this.selected_modul = item;
        axios.get('http://localhost:8080/Assignment3_d0031N/resources/canvas/' + this.selected_kursKod)
            .then(res => this.items = res.data) 
            .catch(err => console.log(err));
    },
    submitKursKod(){
      this.URL_kursKod = "" + this.selected_kursKod
      console.log(this.URL_kursKod)
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
