<template>
    <button :class="customClass" @click="handleClick" :disabled="isButtonDisabled">{{title}}</button>
</template>

<script>
import {ref} from 'vue'
export default{
    name: 'UiButtonComponent',
    props: {
        isButtonDisabled: {
            type: Boolean,
        },
        title: {
            type: String,
            required: true,
            default: 'Titolo...'
        },
        color: {
            type: String,
            require: false,
            validator(value){
                return ['success','default'].includes(value)
            }
        },
        customClass: {
            type: String,
            required: false,
            default: ''
        },
    },
    emits: ['evtClick'],
    setup(props, ctx){
        const handleClick = () => {
            ctx.emit('evtClick')
        }
        return{handleClick}
    }
}
</script>

<style>
    
</style>