<template>
    <div class="small-12 columns" v-if="!gameStatus">
        <button id="start-game" @click="startNewGame">START NEW GAME</button>
    </div>
    <div class="small-12 columns" v-else>
        <button id="attack" @click="attackFn">ATTACK</button>
        <button id="special-attack" @click="specAttackFn">SPECIAL ATTACK</button>
        <button id="heal" @click="healFn">HEAL</button>
        <button id="give-up" @click="giveUpFn">GIVE UP</button>
    </div>
</template>
<script>
    import { eventBus } from '../main.js';

    export default {
        props: {
            'attackFn': Function,
            'specAttackFn': Function,
            'healFn': Function,
            'giveUpFn': Function,
            'gameStatus': Boolean
        },
        data(){
            return {

            }
        },
        methods: {
            startNewGame(){
                this.gameStatus = true;
                eventBus.$emit('gameHasStarted', this.gameStatus);
                eventBus.$emit('clearLog', []);
                eventBus.$emit('resetPlayerHealth', 100);
                eventBus.$emit('resetMonsterHealth', 100);
                
            }
        }
    }
</script>