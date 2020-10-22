<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
      <b-dropdown
        split
        split-variant="outline-primary"
        variant="primary"
        text="Kurskod"
        class="m-2"
      >
        <b-dropdown-item href="#">D0000N</b-dropdown-item>
        <b-dropdown-item href="#">A0000N</b-dropdown-item>
        <b-dropdown-item href="#">E0000N</b-dropdown-item>
      </b-dropdown>
      <b-dropdown
        split
        split-variant="outline-primary"
        variant="primary"
        text="Modul i Ladok"
        class="m-2"
      >
        <b-dropdown-item href="#">Modul 1</b-dropdown-item>
        <b-dropdown-item href="#">Modul 2</b-dropdown-item>
        <b-dropdown-item href="#">Modul 3</b-dropdown-item>
      </b-dropdown>
      <b-table striped hover :items="items"></b-table>
    
  </div>
</template>

<script>
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
        ]
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
