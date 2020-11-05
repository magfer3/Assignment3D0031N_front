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
      <b-table
      ref="selectableTable"
      selectable
      :select-mode="selectMode"
      :items="items"
      :fields="fields"
      @row-selected="onRowSelected"
      responsive="sm"
    >
      <!-- Example scoped slot for select state illustrative purposes -->
      <!--<template #cell(selected)="{ rowSelected }">
        <template v-if="rowSelected" >
          <span aria-hidden="true">&check;</span>
          <span class="sr-only">Selected</span>
        </template>
        <template v-else>
          <span aria-hidden="true">&nbsp;</span>
          <span class="sr-only">Not selected</span>
        </template>
      </template>
      -->
      <template v-slot:cell(examinationsdatum)="row">
        <b-form-input v-model="row.item.examinationsdatum"/>
      </template>
            <template v-slot:cell(ladok_grade)="row">
        <b-form-select v-model="row.item.ladok_grade" :options="grades_options"></b-form-select>
      </template>
    </b-table>

    {{selected_items}}
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
        fields: [ 
          {key: 'firstName', label: 'Förnamn'}, 
          {key: 'lastName', label: 'Efternamn'},
          {key: 'grades', label: 'Betyg'},
          {key: 'examinationsdatum'},
          {key: 'ladok_grade'},
          {key: 'Status'},
          {key: 'Information'}],
        items: [{"firstName":"Lukas","grades":[{"grade":"U","nameOfAssignment":"Inlämningsuppgift1"},{"grade":"G","nameOfAssignment":"Inlämningsuppgift2"},{"grade":"U","nameOfAssignment":"Inlämningsuppgift3"},{"grade":"VG","nameOfAssignment":"Projekt"}],"lastName":"Skog","userName":"luksok-8"},{"firstName":"Magdalena","grades":[{"grade":"VG","nameOfAssignment":"Inlämningsuppgift1"},{"grade":"VG","nameOfAssignment":"Inlämningsuppgift2"},{"grade":"G","nameOfAssignment":"Inlämningsuppgift3"},{"grade":"VG","nameOfAssignment":"Projekt"}],"lastName":"Fernlund","userName":"magrad-2"},{"firstName":"Simon","grades":[{"grade":"G","nameOfAssignment":"Inlämningsuppgift1"},{"grade":"G","nameOfAssignment":"Inlämningsuppgift2"},{"grade":"VG","nameOfAssignment":"Inlämningsuppgift3"},{"grade":"VG","nameOfAssignment":"Projekta"}],"lastName":"Sterner","userName":"simste-6"}
         ],
        selected_kursKod: '',
        selected_modul: '',
        selected_items: [],
        grades_options: ['U', 'G', 'VG']
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
    },
    onRowSelected(items) {
        this.selected_items = items
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
