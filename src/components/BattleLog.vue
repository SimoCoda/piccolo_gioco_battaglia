<template>
    <div class="container">
        <ul v-for="log in actions" :key="log" class="text-left">
            <li>{{ log.name }}: <span class="text-red-600 font-bold text-lg">{{ log.damage }}</span> <span class="text-green-600 font-bold text-lg">{{ log.health }}</span></li>
        </ul>
    </div>
</template>

<script>
import {ref,onMounted} from 'vue';
export default{
    name: 'BattleLogComponent',
    props: {
        actions: {
            type: Array,
            required: true
        }
    },
    setup(){
        return{}
    }
}
</script>