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
      <button @click="pushToLadok">Pusha till Ladok</button>
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
        <b-form-datepicker v-model="row.item.examinationsdatum"/>
      </template>
      <template v-slot:cell(ladok_grade)="row">
        <b-form-select v-model="row.item.ladok_grade" :options="grades_options"></b-form-select>
      </template>

      
      <template v-slot:cell(PNmbr)="row">
        <b-form v-model="row.item.PNmbr" ></b-form>
      </template>


      <template v-slot:cell(grades)="row">
        <div v-for="items in row.item" :key="items.grade">
          <div v-for="grade in items" :key="grade">
            {{grade.nameOfAssignment}} {{grade.grade}}
          </div>
        </div>
      </template>
      
    </b-table>

    {{selected_items}}
  </div>

</template>

<script>
import Dropdown from './Dropdown.vue';
import axios from 'axios';
import queryString from 'query-string';

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
          {key: 'Information'},
          {key: 'PNmbr'}],
        items: [/*{"firstName":"Lukas","grades":[{"grade":"U","nameOfAssignment":"Inlämningsuppgift1"},{"grade":"G","nameOfAssignment":"Inlämningsuppgift2"},{"grade":"U","nameOfAssignment":"Inlämningsuppgift3"},{"grade":"VG","nameOfAssignment":"Projekt"}],"lastName":"Skog","userName":"luksok-8"},{"firstName":"Magdalena","grades":[{"grade":"VG","nameOfAssignment":"Inlämningsuppgift1"},{"grade":"VG","nameOfAssignment":"Inlämningsuppgift2"},{"grade":"G","nameOfAssignment":"Inlämningsuppgift3"},{"grade":"VG","nameOfAssignment":"Projekt"}],"lastName":"Fernlund","userName":"magrad-2"},{"firstName":"Simon","grades":[{"grade":"G","nameOfAssignment":"Inlämningsuppgift1"},{"grade":"G","nameOfAssignment":"Inlämningsuppgift2"},{"grade":"VG","nameOfAssignment":"Inlämningsuppgift3"},{"grade":"VG","nameOfAssignment":"Projekta"}],"lastName":"Sterner","userName":"simste-6"}
         */],
        selected_kursKod: '',
        selected_modul: '',
        selected_items: [],
        grades_options: ['U', 'G', 'VG'],
        pushArray: [/*{"pNmr": "9603169876", "course": "D0031N", "module": "0002", "date": "2020-12-25", "grade": "MVG"},
        {"pNmr": "9408315678", "course": "D0031N", "module": "0005", "date": "2020-12-25", "grade": "G"}*/],
        requestBody: '',
        failedPush: []
      }

    },
    components: {
    Dropdown,
  }, 
  methods: {
    getSelectedItem(item){
        this.selected_modul = item;
        axios.get('http://localhost:8080/Assignment3.2_D0031N/resources/canvas/' + this.selected_kursKod)
            .then(res => this.items = res.data) 
            .catch(err => console.log(err));
   },
   pushToLadok(){
     this.failedPush = [];
     Array.prototype.forEach.call(this.pushArray, stud =>{
     this.requestBody = stud;
     const config = {
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
      }
    }
    axios.post('http://localhost:8080/Assignment3.2_D0031N/resources/ladok', queryString.stringify(this.requestBody), config).then(res => console.log(res)).catch(err => {
    console.log(err);
    this.failedPush.push(this.requestBody)
    }
    );
     })},
    submitKursKod(){
      this.URL_kursKod = "" + this.selected_kursKod
      console.log(this.URL_kursKod)
    },
    onRowSelected(items) {
        this.selected_items = items
        this.pushArray = this.selected_items.map(obj => {
        let rObj = {}
        axios.get('http://localhost:8080/Assignment3.2_D0031N/resources/studentits/' + obj.userName)
            .then(res => rObj["pNmr"] = res.data) 
            .catch(err => console.log(err))
          rObj["course"] = this.selected_kursKod
          rObj["module"] = this.selected_modul
          rObj["date"] = obj.examinationsdatum
          rObj["grade"] = obj.ladok_grade
          return rObj 
      })
      }
  },
  filters: {
       // Filter definitions
    toArray(value) {
      return value.toUpperCase();
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
