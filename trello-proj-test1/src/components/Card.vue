<template>
    <div>
        Card
        <div v-if="loading">Loading Card ...</div>
        <div v-else>cid : {{cid}}</div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            cid:0,
            loading:false
        }
    },
    created(){
        this.loading = true;
        this.fetchData();
    },
    watch: {
        $route(){
            this.fetchData();
        }
    },
    methods: {
        fetchData(){
            setTimeout(() => {
                this.cid = this.$route.params.cid;
                this.loading = false;
            },500);
        }
    }
}
</script>

<style>

</style>
