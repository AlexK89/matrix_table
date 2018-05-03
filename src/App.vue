<template>
    <div id="app">
        <h2>{{test}}</h2>
        <p v-for="element in dataArray">
            {{element}}
        </p>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                test: 'hello',
                dataArray: []
            }
        },
        mounted() {
            //AJAx request
            axios
                .get('https://demo7244264.mockable.io/matrix')
                .then(response => {
                    this.dataArray = response.data;
                    console.log(this.dataArray);
                })
        },
    }
</script>

<style lang="scss">

</style>
