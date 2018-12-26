<template>
    <div>
        Home
        <div>
            Board list : 
            <div v-if="loading">Loading...</div>
            <div v-else>
                Api result: {{apiRes}}
            </div>
            <div v-if="error">{{error}}</div>
            <ul>
                <li>
                    <router-link to="/b/1">Board 1</router-link>
                </li>
                <li>
                    <router-link to="/b/2">Board 2</router-link>
                </li>
                <li>
                    <router-link to="/b/3">Board 3</router-link>
                </li>
            </ul>
            
        </div>
    </div>
</template>
<script>
import axios from 'axios';

export default {
    data(){
        return {
            loading: false,
            apiRes: '',
            error: ''
        }
    },
    created(){
        this.fetchData_axios();
    },
    methods: {
        fetchData_old(){
            this.loading = true;

            const req = new XMLHttpRequest();
            req.open('GET', 'http://localhost:3000/health');
            req.send();

            req.addEventListener('load', () => {
                this.loading = false;
                this.apiRes = {
                    status: req.status,
                    statusText: req.statusText,
                    response: JSON.parse(req.response)
                };
            });
        }
        ,fetchData_axios(){
            this.loading = true;

            axios.get('http://localhost:3000/health')
            .then((res) => {
                this.apiRes = res.data;
            })
            .catch((res) => {
                this.error = res.response.data;
            })
            .finally(() => {
                this.loading = false;
            });

        }
    }
}
</script>
<style>
</style>

