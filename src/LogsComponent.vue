<template>
    <div class="logs-container">

        <h2>{{ title }}</h2>
        <div>
            <!-- Display only the first 10 logs -->
            <!-- <ul>
                <li v-for="log in logs.slice(0,)" :key="log">{{ log.message }}</li>
            </ul> -->
            <ul>
                <li v-for="log in displayedLogs" :key="log">{{ log.message }}</li>
            </ul>
            <!-- Paging -->
            <p v-if="logs.length < 5 || showAllLogs" class="paging">{{ logs.length }}/{{ logs.length }}</p>
            <p v-else class="paging">5 / {{ logs.length }}</p>
            <!-- Show more button -->
            <!-- @click="this.showAllLogs = !this.showAllLogs" in line calculation -->
            <button @click="toogleLogs" v-if="logs.length > 5">{{ showAllLogs ? 'Hide' : 'Show ' +
                "logs"
                }}</button>
        </div>

    </div>
</template>

<script>
export default {
    name: 'LogsComponent',
    props: ['title', 'logs'],
    data() {
        return {
            showAllLogs: false,
        }
    },
    methods: {
        toogleLogs() {
            this.showAllLogs = !this.showAllLogs;
        }
    },
    computed: {
        displayedLogs() {
            return this.showAllLogs ? this.logs : this.logs.slice(0, 5);
        }
    }
}
</script>

<!-- scoped is used to make the css only available to this component -->
<style scoped>
.logs-container {
    margin-top: 20px;
    max-width: 400px;
    text-align: center;
}

ul {
    padding: 0;
    list-style: none;
}

ul li {
    background: rgba(255, 255, 255, 0.2);
    margin: 5px 0;
    padding: 10px;
    border-radius: 5px;
}

button {
    margin-top: 10px;
    padding: 10px 15px;
    background-color: #44b3d7;
    color: white;
    border-radius: 5px;
    cursor: pointer;
    border: 1px solid #fff;
}

button:hover {
    background-color: rgba(68, 179, 215, 0.7);
}

.paging {
    color: black;
    font-size: 12px;
    font-weight: 500;
}
</style>