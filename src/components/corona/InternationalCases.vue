<template>
    <div>
        <h2>국제현황</h2>
        <div class="Buttons">
         <el-radio-group v-model="displayType" size="small">
            <el-radio-button 
            v-for="(item, index) in displayTypes"
            :key="index" 
            :label="index"
            >{{ item.alias }}</el-radio-button>
        </el-radio-group>
        <input type="text" 
        v-model="keyword" />
        </div>
        <ul class="List">
            <li 
            class="Country" 
            v-for="Country in sortedCountries" 
            :key="Country.CountryCode"
            >
            <img 
            :src="require(`@/assets/flags/${Country.CountryCode}.svg`)" 
            :alt="Country.Country" />
            <span class="name">{{ Country.Country }}</span>
            <span class="cases">{{ Country.NewConfirmed }}/{{ Country.TotalConfirmed }}</span>
        </li>
        </ul>
    </div>
</template>

<script>
//https://github.com/purecatamphetamine/country-flag-icons
import coronaMixin from '@/mixins/coronaMixin';
export default {
    name: "InternationalCases",
    mixins: [coronaMixin],
    data() {
        return {
            keyword:"",
            Countries: [],
            displayType: 0,
            displayTypes: [
                {alias: "Daily Worst", key: "NewConfirmed", compare: -1},
                {alias: "Daily Best", key: "NewConfirmed", compare: 1},
                {alias: "Total Worst", key: "TotalConfirmed", compare: -1},
                {alias: "Total Best", key: "TotalConfirmed", compare: 1},
            ]
        };
    },
    computed: {
        sortedCountries() {
            const { compare, key } = this.displayTypes[this.displayType];
            let list = [...this.Countries];
            //filter
            list = list.filter(
                (li) => li.Country.toUpperCase().includes(this.keyword.toUpperCase())
            );
            // sort
            return  list.sort((a,b)=> a[key] > b[key] ? compare : -compare )
            }
        },
    mounted() {
        this.fetchCases();
    },
    methods: {
        async fetchCases() {
            const url = "https://api.covid19api.com/summary"
            const summary = await this.fetchData("get", url);
            console.log(summary, "res");
            this.Countries = summary.Countries;
        },
    },
}
</script>

<style scoped>
.List {
    list-style: none;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 0.5em;
    margin-top: 1em;
}
.Country {
    border: 1px solid #efefef;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 0.5em;
    font-size: 12px;
}
.Country img {
    max-width: 1em;
    margin-right: 1em;
}
.Country .name {
    margin-right: 1em;
    font-weight: bold;
}
.Buttons {
    margin-top: 1em;
    display: flex;
    justify-content: space-between;
}
</style>