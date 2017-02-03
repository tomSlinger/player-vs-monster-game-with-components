<template>
    <div class="small-6 columns">
        <h1 class="text-center">MONSTER</h1>
        <div class="healthbar">
            <div class="healthbar text-center" style="margin: 0; color: white;" :style="{backgroundColor: monstercolor, width: monsterhealth + '%'}">
                {{ monsterhealth }}
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        props: {
            'monsterhealth': {
                type: Number
            },
            'monstercolor': {
                type: String
            }
        }
    }
</script>