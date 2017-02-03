<template>
    <div class="small-12 columns" v-if="!gameStatus">
        <button id="start-game" @click="startGameFn">START NEW GAME</button>
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
            'startGameFn': Function,
            'attackFn': Function,
            'specAttackFn': Function,
            'healFn': Function,
            'giveUpFn': Function,
            'gameStatus': Boolean
        }
    }
</script>