<template>
    <div class="row">
        <div class="col-md-7  search-bar">
            <div class="input-group mb-3 mt-3">
                <input type="text" class="form-control" placeholder="Search"
                       v-model="typingWord"
                       @keyup="searching"
                       @keyup.down="down"
                       @keyup.up="up"
                       @keyup.enter="chooseByEnter()"
                >
            </div>
            <div class="search-options" v-if="typingWord">
                <div v-if="searchingInProgress">
                    <Spinner></Spinner>
                </div>
                <div class="search-options__item"
                     v-else
                     :class="[index === activeItem ? 'search-options__item-active' : '']"
                     v-for="(item, index) in data" :key="index"
                     @click="chooseByClick({index, $event})"
                >
                    {{item}}
                </div>
                <div v-if="resultArrayLength > 10" class="search-options__item">{{resultArrayLength - 10}} more words
                </div>
            </div>
        </div>
        <div class="col-md-7  search-bar" v-if="chosenItem">
            <div class="card border-secondary  mb-3 shadow">
                <div class="card-header">{{chosenItem}}</div>
                <div class="close" @click="closeResultWindow"></div>
                <div class="card-body text-secondary ">
                    <h5 class="card-title">{{chosenItem}}</h5>
                    <p v-if="!chosenItem.startsWith('No data')" class="card-text">
                        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Exercitationem, velit.
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import dummyData from "../assets/dummyData";
    import Spinner from "./Spinner";

    export default {
        name: "SearchBar",
        components: {
            Spinner
        },
        data() {
            return {
                data: [],
                dummyData,
                typingWord: "",
                activeItem: -1,
                chosenItem: "",
                resultArrayLength: 0,
                selectionWindowIsOpen: false,
                searchingInProgress: true,
            }
        },
        methods: {
            down: function () {
                this.activeItem < this.data.length ? this.activeItem++ : this.activeItem = 0
            },
            up: function () {
                this.activeItem < 0 ? this.activeItem = this.data.length -1 : this.activeItem--
            },
            chooseByClick: function (event) {
                this.chosenItem = this.data[event.index]
                this.resetAll()
            },
            chooseByEnter: function () {
                if (this.data[this.activeItem]) {
                    this.chosenItem = this.data[this.activeItem]
                }else if (this.data.length !== 0) {
                    this.chosenItem = this.data[0]
                } else {
                    this.chosenItem = "No data matching your request " + "'" + this.typingWord + "'"
                }

                this.resetAll()
            },
            searching: async function () {
                this.closeResultWindow()
                this.searchingInProgress = true
                let tempData = await this.dummyData.filter(item => item.startsWith(this.typingWord)).sort()
                this.resultArrayLength = tempData.length
                this.data = tempData.slice(0, 10)
                this.searchingInProgress = false
            },
            resetAll: function () {
                this.selectionWindowIsOpen = false
                this.typingWord = ""
                this.activeItem = -1
            },
            closeResultWindow: function () {
                this.chosenItem = ""
            }
        },
    }
</script>

<style scoped>
    .search-options {
        border: 1px solid #a7a7a7;
        border-radius: 5px;
        -webkit-box-shadow: 10px 10px 5px -5px rgba(0, 0, 0, 0.39);
        -moz-box-shadow: 10px 10px 5px -5px rgba(0, 0, 0, 0.39);
        box-shadow: 10px 10px 5px -5px rgba(0, 0, 0, 0.39);
    }

    .search-options__item {
        padding: 8px 20px;
        border-bottom: 1px solid #b8b7b7;
    }

    .search-options__item-active {
        background-color: #a0a0a0;
    }
    .shadow{
        -webkit-box-shadow: 10px 10px 5px -5px rgba(0, 0, 0, 0.39);
        -moz-box-shadow: 10px 10px 5px -5px rgba(0, 0, 0, 0.39);
        box-shadow: 10px 10px 5px -5px rgba(0, 0, 0, 0.39);
    }

    .search-options__item:hover {
        background-color: #e5e4e4;
        cursor: pointer;
    }

    .close {
        position: absolute;
        right: 10px;
        top: 10px;
        width: 32px;
        height: 32px;
        opacity: 0.3;
        cursor: pointer;
    }
    .close:hover {
        opacity: 1;
    }
    .close:before, .close:after {
        position: absolute;
        left: 15px;
        content: ' ';
        height: 33px;
        width: 2px;
        background-color: #333;
    }
    .close:before {
        transform: rotate(45deg);
    }
    .close:after {
        transform: rotate(-45deg);
    }


</style>