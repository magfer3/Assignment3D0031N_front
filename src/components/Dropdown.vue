<template>
    <div class="form-group">
        <b-form-select v-model="selected" :options="options" @change="$emit('selectedItem', selected)"></b-form-select>
        {{url}}
    </div>
    
</template>

<script>
import axios from 'axios'

export default {
    created() {
    this.fetchDataAxios();
    },
    name: 'Dropdown',
    props: {
        url: String
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
        async fetchData() {
        const res = await fetch(
        "http://localhost:8080/Assignment3.2_D0031N/resources/epok/D0031N");
        const val = await res.text();
        this.text = val;
        console.log(this.text);
        console.log(this.url);
        },
        fetchDataAxios(){
            axios.get('http://192.168.1.246:8080/Assignment3_d0031N/resources/canvas/D0031N')
            .then(res => console.log(res)) 
            .catch(err => console.log(err));

        }

    }
}
</script>

<style scoped>

</style> 
