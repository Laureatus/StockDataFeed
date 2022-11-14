<style>
@import url('https://fonts.googleapis.com/css2?family=Manrope&display=swap');body{background-color: #eee;font-family: 'Manrope', sans-serif}.news-card{border-radius: 8px}.news-feed-image{border-radius: 8px;width: 100%}.date{font-size: 12px}.username{color: blue}.share{color: blue}
</style>
<template>
    <div>
      <p v-if="$fetchState.pending">Fetching Data...</p>
      <p v-else-if="$fetchState.error">An error occurred :(</p>
      <div v-else>
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
                <b-list-group-item href="#"><b>Name:</b> {{ symbols.Name }}</b-list-group-item>
                <b-list-group-item href="#"><b>AssetType:</b> {{ symbols.AssetType }}</b-list-group-item>
                <b-list-group-item href="#"><b>Country:</b> {{ symbols.Country }}</b-list-group-item>
                <b-list-group-item href="#"><b>Sector:</b> {{ symbols.Sector }}</b-list-group-item>
                <b-list-group-item href="#"><b>Industry:</b> {{ symbols.Industry }}</b-list-group-item>
                <b-list-group-item href="#"><b v-b-popover.hover.top="'Die Bruttomarge, oder Bruttogewinnmarge, drückt den Vorsteuergewinn eines Unternehmens nach Abzug der direkten Herstellungs- und Verkaufskosten aus. Sie kann aufzeigen, ob das Unternehmen trotz seiner Ausgaben Gewinne erzielt. Bruttogewinn/Ertrag'">Profit Margin:</b> {{ Math.round(symbols.ProfitMargin*100) }}%</b-list-group-item>
                <b-list-group-item href="#"><b v-b-popover.hover.top="'Der Return on Assets (ROA), auch bekannt als „Gesamtkapitalrendite“, „Gesamtkapitalverzinsung“ oder „Unternehmensrentabilität“, ist eine Rentabilitätskennzahl für die Unternehmensbewertung. Investoren können mit dem Return on Assets analysieren, wie hoch der Gewinn eines Unternehmens im Verhältnis zu dem Gesamtvermögen ist. Vereinfacht zeigt die Kennzahl, wie viel Gewinn ein Unternehmen mit jedem Euro Kapital erwirtschaftet.'">Return On Assets TTM:</b> {{ Math.round(symbols.ReturnOnAssetsTTM*100) }}%</b-list-group-item>
                <b-list-group-item href="#"><b v-b-popover.hover.top="'Der ROE (Return on Equity), auch bekannt als „Eigenkapitalrendite“ oder „Eigenkapitalrentabilität“, ist eine Gewinnkennzahl bei der Unternehmensanalyse. Die Kennzahl betrachtet denn Gewinn in Abhängigkeit vom Eigenkapital eines Unternehmens. Anleger können anhand des ROE beispielsweise ableiten, wie gut ein Unternehmen das vorhandene Eigenkapital einsetzt. Dabei kann der ROE keine vollständige Unternehmensanalyse liefern. Zu diesem Zweck sind weitere Kennzahlen als Ergänzung notwendig.'">Return On Equity TTM:</b> {{ Math.round(symbols.ReturnOnEquityTTM*100) }}%</b-list-group-item>
                <b-list-group-item href="#"><b>52 Week High:</b> {{ symbols['52WeekHigh'] }} {{ symbols.Currency }}</b-list-group-item>
                <b-list-group-item href="#"><b>52 Week Low:</b> {{ symbols['52WeekLow'] }} {{ symbols.Currency }}</b-list-group-item>
                <b-list-group-item href="#"><b>50 Day Moving Average:</b> {{ symbols['50DayMovingAverage'] }} {{ symbols.Currency }}</b-list-group-item>
                <b-list-group-item href="#"><b>200 Day Moving Average:</b> {{ symbols['200DayMovingAverage'] }} {{ symbols.Currency }}</b-list-group-item>
                </b-list-group>
                <p class="card-text mt-2">
                    {{ symbols.Description }}
                </p>
            </b-card>
        </b-card-group>
        <div v-for="report in reports">
            {{report}}
        </div>
        <div v-for="article in articles.feed" :key="article.title">
            <div class="container mt-5 mb-5">
                <div class="row d-flex justify-content-center">
                    <div class="col-md-20">
                        <div class="d-flex flex-row"></div>
                        <div class="row news-card p-3 bg-white">
                            <div class="col-md-4">
                                <div class="feed-image"><img class="news-feed-image rounded img-fluid img-responsive" :src="article.banner_image"></div>
                            </div>
                            <div class="col-md-8">
                                <div class="news-feed-text">
                                    <h5>{{article.title}}</h5><span>{{article.summary}}<br></span>
                                    <div class="d-flex flex-row justify-content-between align-items-center mt-2">
                                        <a :href="article.url"><b-button variant="outline-primary">Read more</b-button></a>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
      </div>
    </div>
</template>
  
<script>
    import { Bar } from 'vue-chartjs'
    import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
  
    ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

    export default {
        name: 'ArrivalTable',
        components: { Bar },
        props: {
            chartId: {
                type: String,
                default: 'bar-chart'
            },
            datasetIdKey: {
                type: String,
                default: 'label'
            },
            width: {
                type: Number,
                default: 400
            },
            height: {
                type: Number,
                default: 400
            },
            cssClasses: {
                default: '',
                type: String
            },
            styles: {
                type: Object,
                default: () => {}
            },
            plugins: {
                type: Object,
                default: () => {}
            }
        },
        data() {
            return {
            symbols: [],
            articles:[],
            searchInput: '',
            }
        },
    async fetch() {
        this.symbols = await fetch(
        `https://www.alphavantage.co/query?function=OVERVIEW&symbol=${this.searchInput}&apikey=0ZN6UO9D3R6K3R77`
        ).then(res => res.json())

        this.articles = await fetch(
        `https://www.alphavantage.co/query?function=NEWS_SENTIMENT&tickers=${this.searchInput}&apikey=0ZN6UO9D3R6K3R77`
        ).then(res => res.json())

    },
    }
</script>
