<template>
    <div class="container">
        <span class="text-xl font-bold italic">{{ whoWin }}</span> ha vinto in {{rounds}} rounds!
        <div>
            <button @click="restartGame" class="bg-green-800 w-48 mt-7 hover:bg-green-600 transition-all duration-300 hover:rounded-none">Ricomincia il gioco</button>
        </div>
    </div>
</template>

<script>
export default{
    name: 'GameOverComponent',
    props: ['whoWin','rounds'],
    emits: ['resetGame'],
    setup(props,ctx){
        const restartGame = () => {
            ctx.emit('resetGame');    
        }
        return {restartGame}
    }
}
    
</script>