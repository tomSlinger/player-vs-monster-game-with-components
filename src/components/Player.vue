<template>
    <div class="small-6 columns">
        <h1 class="text-center">YOU</h1>
        <div class="healthbar">
            <div class="healthbar text-center" style="margin: 0; color: white;" :style="{backgroundColor: playercolor, width: playerhealth + '%'}">
                {{ playerhealth }}
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        props: {
            'playerhealth': {
                type: Number
            },
            'playercolor': {
                type: String
            }
        }
    }
</script>