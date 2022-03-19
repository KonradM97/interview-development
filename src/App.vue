<template>
    <div id="app">
        <div class="header">
            <button @click="switchVisibility">{{ isListVisible ? 'Schowaj listę pracowników' : 'Pokaż listę pracowników'}}</button>
        </div>
        <workers-list v-if="isListVisible" :workers-list="workers"/>
    </div>
</template>
<script>
import Vue from 'vue';
import { Component } from 'vue-property-decorator';
import WorkersList from './components/WorkersList.vue';
import WorkerListFactory from './models/WorkerListFactory';

@Component({
    name: 'App',
    components: {WorkersList},
})
export default class App extends Vue {

    data(){
        return {
            workers: [],
            buttonName: 'Pokaż listę pracowników',
            isListVisible: false,
        };
    }

    mounted(){
        this.createWorkers();
    }
    switchVisibility(){
        this.isListVisible = !this.isListVisible;
    }
    async createWorkers(){
        let factory = new WorkerListFactory();
        this.workers = await factory.getWorkers();
    }

}
</script>

<style lang="scss" scoped>
@import "./assets/global.css";

#app {
    .header {
        padding: 0.5rem;
    }
}
</style>
