<template>
    <div class="header flex flex-col">
        Game | Round: {{numberRounds}}
        <cite>Puoi rinominare entrambi i giocatori, ma il primo gioca autonomamente mentre il secondo sei tu e di conseguenza puoi usare i vari comandi!</cite>
    </div>
</template>

<script>
import {ref} from 'vue'
export default{
    name: 'HeaderComponent',
    props: {
        numberRounds: {type: Number,required: true}
    },
    setup(){
        return {}
    }
}    
</script>


<style>
    
</style>