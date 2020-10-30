<template>
    <div class="form-group">
        <b-form-select v-model="selected" :options="options" @change="$emit('selectedItem', selected)"></b-form-select>
    </div>
    
</template>

<script>
export default {
    name: 'Dropdown',
    props: {
        url: String
    },
    data() {
        return {
            selected: '',
            options: [
                {text: 'D0031N', value: 'D0031N'},
                {text: 'D0025E', value: 'D0025E'},
                {text: 'D004N', value: 'D004N'},
            ]
        }
    },
    //Hämtar datan till dropdown-options. this.url hämtas via props från helloworld
    methods: {
        async fetchData() {
        var res = await fetch(
        "\"" + this.url +"\"");
        var val = await res.json();
        this.options = val;
    }
    }
}
</script>

<style scoped>

</style> 
