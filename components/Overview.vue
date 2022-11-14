<template>
     <b-card-group deck>
            <b-card bg-variant="default">
                <b-input-group prepend="Symbol" class="mt-3">
                    <b-form-input type="text" id="input" @keyup.enter="$fetch" v-model="searchInput"></b-form-input>
                    <b-input-group-append>
                    <b-button @click="$fetch" variant="outline-primary">Search</b-button>
                    </b-input-group-append>
                </b-input-group>
                <p></p>
                <b-list-group>
                <b-list-group-item href="#">Name: {{ symbols.Name }}</b-list-group-item>
                <b-list-group-item href="#">AssetType: {{ symbols.AssetType }}</b-list-group-item>
                <b-list-group-item href="#">Currency: {{ symbols.Currency }}</b-list-group-item>
                <b-list-group-item href="#">Country: {{ symbols.Country }}</b-list-group-item>
                <b-list-group-item href="#">Sector: {{ symbols.Sector }}</b-list-group-item>
                <b-list-group-item href="#">Industry: {{ symbols.Industry }}</b-list-group-item>
                <b-list-group-item href="#">ProfitMargin: {{ symbols.ProfitMargin }}</b-list-group-item>
                <b-list-group-item href="#">ReturnOnAssetsTTM: {{ symbols.ReturnOnAssetsTTM }}</b-list-group-item>
                <b-list-group-item href="#">ReturnOnEquityTTM: {{ symbols.ReturnOnEquityTTM }}</b-list-group-item>
                <b-list-group-item href="#">52WeekHigh: {{ symbols['52WeekHigh'] }}</b-list-group-item>
                <b-list-group-item href="#">52WeekLow: {{ symbols['52WeekLow'] }}</b-list-group-item>
                </b-list-group>
                <p class="card-text mt-2">
                    {{ symbols.Description }}
                </p>
            </b-card>
        </b-card-group>
</template>

<script>
    export default {
        name: 'Overview',
        data() {
        return {
            symbols: [],
            searchInput: "",
        };
    },
        async fetch() {
        this.symbols = await fetch(`https://www.alphavantage.co/query?function=OVERVIEW&symbol=${this.searchInput}&apikey=0ZN6UO9D3R6K3R77`).then(res => res.json());
    },
    }
</script>