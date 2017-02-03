<template>
    <div class="small-12 columns">
        <ul>
            <li v-for="logItem in log">
                {{ logItem.message }}
            </li>
        </ul>
    </div>
</template>
<script>
    export default {
        props: {
            'log': Array
        }
    }
</script>