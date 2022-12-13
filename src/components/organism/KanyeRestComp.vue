<template>
    <div>Kanye says: {{ quote }} | <button class=" hover:bg-gray-900 hover:text-white px-1 rounded-md py-1" @click="getQuotes">New Quote</button></div>
</template>

<script lang="ts">
import axios from 'axios'
import { defineComponent } from 'vue'

export default defineComponent({
    setup() {
        
    },
    data(){
        return{
            quote: ""
        }
    },
    async mounted(){
        await this.getQuotes()
    },
    methods: {
        async getQuotes(){
            const url = "https://api.kanye.rest"
            const response = await axios.get(url)
            this.quote = response.data.quote
        }
    }
})
</script>
