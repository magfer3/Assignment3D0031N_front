<template>
   <div>
        <div class="form-group">
            <b-form @mouseover="fetchDataAxios">
                <b-form-select  v-model="selected" :options="options" @change="$emit('selectedItem', selected)"></b-form-select>
            </b-form>
        </div>
    </div>
    
</template>

<script>
import axios from 'axios';

export default {
    name: 'Dropdown',
    props: {
        kurskod: String
    },
    created(){

    },
    data() {
        return {
            selected: '',
            options: [
                //{text: 'D0031N', value: 'D0031N'},
                //{text: 'D0025E', value: 'D0025E'},
                //{text: 'D004N', value: 'D004N'},
            ],
            text: ""
        }
    },
    //Hämtar datan till dropdown-options. this.url hämtas via props från helloworld
    methods: {
        fetchDataAxios(){
            axios.get('http://localhost:8080/Assignment3.2_D0031N/resources/epok/' + this.kurskod)
            .then(res => this.options = res.data ) 
          //  .then(res =>console.log(res)) 
            .catch(err => console.log(err));

        }

    }
}
</script>

<style scoped>

</style> 
