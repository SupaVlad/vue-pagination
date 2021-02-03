<template>
    <div class="v-table">
        <h2>LOGO TYPE</h2>
        <span class="title">Интернет-магазин бытовых и пищевых товаров</span>
        <div class="container-wrapper">
            <aside class="filters">
                <div @click="toggleD" class="filters-block">
                    <span class="menu">Цена (грн)</span>
                    <div :class="{ 'is-open': toggle }" class="filter">
                        <div class="item">
                            <div class="menu-range-filters">
                                <div class="left-range">
                                    <input @change="rangeChange" type="range" v-model="start" :min="min" :max="max">
                                </div>
                                <div class="right-range">
                                    <input @change="rangeChange" type="range" v-model="end" :min="min" :max="max">
                                </div>

                            </div>
                            <div class="menu-range-props">
                                <input class="menu-range-props-left" type="number" v-model="start" :placeholder="start">
                                <input class="menu-range-props-right" type="number" v-model="end" :placeholder="end">
                            </div>

                        </div>
                    </div>
                </div>
                <div @click="toggleD" class="filters-block">
                    <span class="menu">Материал</span>
                    <div :class="{ 'is-open': toggle }"  class="filter">
                        <div class="item">
                            <label><input type="radio" v-model="selectedCategory" value="All"/> All</label>
                            <label><input type="radio" v-model="selectedCategory" value="Резина"/>Резина ({{this.countRubber.length}})</label>
                            <label><input type="radio" v-model="selectedCategory" value="Дерево"/>Дерево ({{this.counterWood.length}})</label>
                            <label><input type="radio" v-model="selectedCategory" value="Пластмасса"/>Пластмасса ({{this.counterPlastic.length}})</label>
                        </div>
                    </div>
                </div>
                <div @click="toggleD" class="filters-block">
                    <span class="menu">Страна производитель</span>
                    <div :class="{ 'is-open': toggle }"  class="filter">
                        <div class="item">
                            <label><input type="radio" v-model="selectedCountry" value="All"/> All</label>
                            <label><input type="radio" v-model="selectedCountry" value="Украина"/>Украина ({{this.counterLocation.length}})</label>
                            <label><input type="radio" v-model="selectedCountry" value="США"/>США ({{this.counterLocationUs.length}})</label>
                        </div>

                    </div>
                </div>
            </aside>
            <div class="content">
                <div class="v-table__body">
                    <v-table-row
                            :key="row.id"
                            :row_data="row"
                            v-for="row in paginatedUsers"
                    />
                </div>

            </div>


        </div>

        <div class="v-table__pagination">
            <div
                    class="page"
                    v-for="page in pages"
                    :key="page"
                    :class="{'page_selected': page === pagesNumber}"
                    @click="pageClick(page)"
            >
                {{page}}
            </div>
        </div>
        <div class="content">
            <form v-on:submit.prevent="submitForm">
                <div class="form-group">
                    <input type="number" class="form-control" id="tel" placeholder="Ваш номер телефона" v-model="form.tel">
                    <label for="tel">Заказать звонок</label>
                </div>
            </form>
        </div>

    </div>
</template>


<script>
    import vTableRow from './v-table-row';
    import {mapActions, mapGetters} from "vuex";
    import {maxBy, minBy} from "lodash";
    import axios from 'axios';

    export default {
        name: "UsersTable",
        data() {
            return {
                usersPerPage: 5,
                pagesNumber: 1,
                color: '',
                selectedCategory: "All",
                selectedCountry: "All",
                min: 0,
                max: 0,
                start: 0,
                end: 0,
                filteredByPrice: false,
                toggle: false,
                form: {
                    tel: ''
                }
            }
        },
        components: {
            vTableRow
        },
        methods: {
            pageClick(page) {
                this.pagesNumber = page;
            },
            ...mapActions([
                'GET_USERS_FROM_API'
            ]),
            rangeChange(){
                this.filteredByPrice = true
            },
            toggleD: function( event ) {
                event.target.classList.toggle('is-open')
            },
            submitForm(){
                axios.post('http://95.217.16.207:1337/forms', this.form)
                    .then((response) => {
                        //Perform Success Action
                        console.log(response)
                    })
                    .catch((error) => {
                        console.log(error)
                        // error.response.status Check status code
                    }).finally(() => {
                    //Perform action in always
                });
            }
        },
        computed: {
            pages() {
                return Math.ceil(this.USERS.length / this.usersPerPage)
            },
            paginatedUsers() {

                let category = this.selectedCategory;
                let country = this.selectedCountry;
                let from = (this.pagesNumber - 1) * this.usersPerPage;
                let to = from + this.usersPerPage;

                if (category === "All" && country === "All") {
                    if (this.filteredByPrice) {
                        return this.USERS.slice(from, to).filter(row => {
                            let price = (row.price >= this.start && row.price <= this.end);
                            return price
                        })
                    } else {
                        return this.USERS.slice(from, to);
                    }
                } else {
                    return this.USERS.filter(row => {
                        let categor = (row.material) ? (row.material.title === category) : (console.log("no material"));
                        let countr = (row.manufacturer) ? (row.manufacturer.title === country) : (console.log("no country"));
                        let price = (row.price >= this.start && row.price <= this.end);
                        if (category === "All") {
                            return countr && price
                        } else if (country === "All") {
                            return categor && price
                        } else return categor && countr && price
                    });
                }
            },
            ...mapGetters([
                'USERS'
            ]),
            countRubber () {
                // itemsFilters is computed
                var counterRubber = 0
                return this.USERS.filter(row => {
                    var countRubber = (row.material && row.material.title === "Резина") ? (counterRubber = counterRubber + 1) : (console.log("no material"));
                    return countRubber
                })
            },
            counterWood () {
                var countWood = 0
                return this.USERS.filter(row => {
                    var counterWood = (row.material && row.material.title === "Дерево") ? (countWood = countWood + 1) : (console.log("no material"));
                    return counterWood
                })
            },
            counterPlastic () {
                // itemsFilters is computed
                var countPlastic = 0
                return this.USERS.filter(row => {
                    var counterPlastic = (row.material && row.material.title === "Пластмасса") ? (countPlastic = countPlastic + 1) : (console.log("no material"));
                    return counterPlastic
                })
            },
            counterLocation () {
                // itemsFilters is computed
                var countUk = 0;
                return this.USERS.filter(row => {
                    var counterLocation = (row.manufacturer && row.manufacturer.title === "Украина") ? (countUk = countUk + 1) : (console.log("no country"));
                    return counterLocation
                })
            },
            counterLocationUs() {
                // itemsFilters is computed
                var countUs = 0;
                return this.USERS.filter(row => {
                    var counterLocation = (row.manufacturer && row.manufacturer.title === "США") ? (countUs = countUs + 1) : (console.log("no country"));
                    return counterLocation
                })
            },
        },
        watch: {},
        async mounted() {
            await this.GET_USERS_FROM_API()
            let max = maxBy(this.USERS, 'price').price,
                min = minBy(this.USERS, 'price').price
            this.start = min
            this.end = max
            this.min = min
            this.max = max
        },
    }
</script>

<style>
    .container-wrapper{
        display: flex;
        max-width: 1254px;
        margin: 82px auto 0 auto;
    }
    @media screen and (max-width: 900px) {
        .container-wrapper{
            display: flex;
           flex-direction: column;
            justify-content: center;
        }
        .filters .filters-block{
            margin-right: 0;
            margin: 0 auto;
        }
        .v-table__body{
            justify-content: center;
            margin-top: 25px
        }
        .v-table__body .card{
            margin-right: 0;
        }
    }

    h2 {
        font-style: normal;
        font-weight: 600;
        font-size: 28px;
        line-height: 34px;
        letter-spacing: 0.05em;
        text-transform: uppercase;
        color: #262329;
        text-align: center;
    }

    .title {
        display: block;
        font-style: normal;
        font-weight: normal;
        font-size: 14px;
        line-height: 17px;
        color: #222222;
        text-align: center;
    }

    .filters {
        min-width: 300px;
    }

    .filter {
        height: 0;
        opacity: 0;
        transition-duration: 0.2s;
        pointer-events: none;
    }
    .is-open ~ .filter{
        height: auto;
        opacity: 1;
        transition-duration: 0.2s;
        pointer-events:auto;
    }

    .filter.active {
        height: auto;
        transition: max-height 1s ease-in-out;
        transition-duration: 0.2s;
    }

    .v-table__body {
        display: flex;
        flex-wrap: wrap;
    }

    .v-table__pagination {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }

    .item{
        background: #F6F6F6;
        margin-bottom: 32px;
    }

    .page {
        padding: 11px 13px;
    }

    .page:hover {
        background: #FBD656;
        cursor: pointer;
    }

    .page_selected {
        background: #FBD656;
        cursor: pointer;
    }
    .filters-block{
        margin-right: 25px;
        max-width: 275px;
    }
    .menu {
        position: relative;
        background: #F6F6F6;
        border-radius: 3px;
        display: block;
        padding: 18px 17px;
        cursor: pointer;
    }
    .menu:after{
        content: "\203A";
        font-size: 25px;
        position: absolute;
        right: 14px;
        top: 11px;
        transform: rotate(90deg);
        transition-duration: 0.2s;
    }
    .menu.is-open:after{
        transform: rotate(-90deg);
        transition-duration: 0.2s;
    }

    .left-range input[type='range'] {
        width: 100px;
        overflow: hidden;
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
        background-color: #FBD656;
    }

    .left-range input[type='range']::-webkit-slider-runnable-track {
        height: 7px;
        border-radius: 10px/100%;
        -webkit-appearance: none;
        color: #13bba4;
        margin-top: -1px;
    }

    .left-range input[type='range']::-webkit-slider-thumb {
        width: 23px;
        -webkit-appearance: none;
        height: 24px;
        appearance: none;
        cursor: ew-resize;
        background: #434343;
        box-shadow: -80px 0 0 80px #fff;
        -webkit-appearance: none;
        margin-top: -4px;
    }

    .left-range input[type="range"]::-moz-range-progress {
        background-color: #43e5f7;
    }
    .left-range input[type="range"]::-moz-range-track {
        background-color: #9a905d;
    }
    .left-range input[type="range"]::-ms-fill-lower {
        background-color: #43e5f7;
    }
    .left-range input[type="range"]::-ms-fill-upper {
        background-color: #9a905d;
    }

    .left-range input[type="range"]::-webkit-slider-thumb {
        -webkit-appearance: none;
        appearance: none;
        width: 14px;
        height: 14px;
        background-size: cover;
        border: 0;
        background: url('~@/assets/range.svg');
        cursor: pointer;
    }

    .left-range input[type="range"]::-moz-range-thumb {
        width: 13px;
        height: 14px;
        border: 0;
        background-size: cover;
        background: url('~@/assets/range.svg');
        cursor: pointer;
    }
    .left-range{
        position: absolute;
        left: 36px;
        top: 30%;
    }
    .menu-range-filters{
        position: relative;
        height: 96px;
    }
    .right-range{
        position: absolute;
        right: 35px;
        top: 30%;
    }
    .right-range input[type='range'] {
        width: 100px;
        overflow: hidden;
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
        background-color: #FFFF;
    }

    .right-range input[type='range']::-webkit-slider-runnable-track {
        height: 7px;
        border-radius: 10px/100%;
        -webkit-appearance: none;
        color: #13bba4;
        margin-top: -1px;
    }

    .right-range input[type='range']::-webkit-slider-thumb {
        width: 23px;
        -webkit-appearance: none;
        height: 24px;
        appearance: none;
        cursor: ew-resize;
        background: #434343;
        box-shadow: -80px 0 0 80px #FBD656;
        -webkit-appearance: none;
        margin-top: -4px;
    }

    .right-range input[type="range"]::-moz-range-progress {
        background-color: #43e5f7;
    }
    .right-range input[type="range"]::-moz-range-track {
        background-color: #9a905d;
    }
    .right-range input[type="range"]::-ms-fill-lower {
        background-color: #43e5f7;
    }
    .right-range input[type="range"]::-ms-fill-upper {
        background-color: #9a905d;
    }

    .right-range input[type="range"]::-webkit-slider-thumb {
        -webkit-appearance: none;
        appearance: none;
        width: 14px;
        height: 14px;
        background-size: cover;
        border: 0;
        background: url('~@/assets/range.svg');
        cursor: pointer;
    }

    .right-range input[type="range"]::-moz-range-thumb {
        width: 13px;
        height: 14px;
        border: 0;
        background-size: cover;
        background: url('~@/assets/range.svg');
        cursor: pointer;
    }
    .right-range input{
        outline: none;
    }
    .left-range input{
        outline: none;
    }
    .menu-range-props-left{
        width: 90px;
        height: 30px;
    }
    .menu-range-props-right{
        width: 90px;
        height: 30px;
    }
    .menu-range-props{
        display: flex;
        justify-content: space-around;
        padding-bottom: 15px;
    }
    .filter .item{
        display: flex;
        flex-direction: column;
    }
    .form-group input{
        background: #F6F6F6;
        margin-right: -4px;
        border-radius: 3px;
        border: none;
        padding: 10.5px;
        outline: none;
    }
    .form-group input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        /* display: none; <- Crashes Chrome on hover */
        -webkit-appearance: none;
        margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
    }
    .form-group label{
        background: #FBD656;
        border-radius: 3px;
        border: none;
        padding: 9px;
        outline: none;
        cursor: pointer;
    }
    .form-group{
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 55px;
    }
</style>
