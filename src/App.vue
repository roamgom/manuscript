<template>
    <div id="app">
        <div id="search_bar">
            <form @submit="checkSlang">
                <b-input-group prepend="문장" class="mt-3">
                    <b-form-input id="sentence" name="sentence" v-model="sentence"></b-form-input>
                    <b-input-group-append>
                        <b-button variant="outline-success" @click="sentence=''">지우기</b-button>
                        <b-button variant="info" @click="checkSlang" type="submit">판별</b-button>
                    </b-input-group-append>
                </b-input-group>
            </form>
        </div>

        <div id="result" class="container">
            <div v-if="result && sentence">
                <div id="slang_percent">{{result}} %</div>
                <br>
                <b-button v-show="!is_explain_visible" variant="secondary" @click="is_explain_visible=true"
                          type="submit">explain reason
                </b-button>
                <br>
                <div v-if="is_explain_visible">
                    <b-spinner label="Spinning" v-show="is_loading"></b-spinner>
                    <div class="limer-wrapper">
                        <iframe v-show="!is_loading" id="limer_html" align="left"
                                v-bind:src="`https://manuscript.roamgom.net/limer_html?text=${sentence}`"></iframe>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                sentence: '',
                result: 0,
                is_explain_visible: false,
                is_loading: false
            }
        },
        methods: {
            async checkSlang(e) {
                function sleep(ms) {
                    return new Promise(resolve => setTimeout(resolve, ms));
                }

                this.is_loading = true
                this.is_explain_visible = false
                e.preventDefault()
                const res = await this.axios.get('https://manuscript.roamgom.net', {
                    params: {
                        text: this.sentence
                    }
                })
                this.result = res.data.result * 100
                await sleep(3000)
                this.is_loading = false
            }


        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    #search_bar {
        padding-left: 10vw;
        padding-right: 10vw;

    }

    #limer_html {
        width: 80vw;
        height: 400px;
        border: none;
        /*margin: 0 auto;*/
    }

    .limer-wrapper {
        margin: 0 auto;
    }

    #loading {
        width: 20vw;
        height: 20vw;
    }

    #slang_percent {
        padding-top: 30px;
        padding-bottom: 10px;
        color: rgb(74, 160, 181);
        font-family: Impact;
        font-size: 50px;
    }

    .frame-wrap {
        overflow: auto;
        overflow-x: hidden;
        -webkit-overflow-scrolling: touch;
        width: 100%;
        height: 100px;
    }

    .frame-wrap iframe {
        overflow: auto;
        width: 100%;
        height: 100%;
    }
</style>
