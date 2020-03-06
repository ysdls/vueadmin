<template>
    <v-card
    outlined
    tile
    elevation="1"
    class="cardSection"
    >
        <div class="subtitle-2">카테고리</div>
        <v-row>
            <v-col cols="12" sm="3">
                <v-autocomplete
                :items="top"
                label="대분류"
                item-text="name"
                item-value="id"
                id="topc"
                outlined
                dense
                @change="topFunc($event)"
                ></v-autocomplete>
            </v-col>

            <v-col cols="12" sm="3">
                <v-autocomplete
                :items="middle"
                label="중분류"
                item-text="name"
                item-value="id"
                id="middlec"
                outlined
                dense
                @change="middleFunc($event)"
                ></v-autocomplete>
            </v-col>

            <v-col cols="12" sm="3">
                <v-autocomplete
                :items="bottom"
                label="소분류"
                item-text="name"
                item-value="id"
                id="bottomc"
                outlined
                dense
                 @change="bottomFunc($event)"
                ></v-autocomplete>
            </v-col>

            <v-col cols="12" sm="3">
                <v-autocomplete
                :items="detail"
                label="세분류"
                item-text="name"
                item-value="id"
                id="detailc"
                outlined
                dense
                 @change="detailFunc($event)"
                ></v-autocomplete>
            </v-col>
        </v-row>
    </v-card>
</template>

<script>
export default {
    props : {
        apiurl:String
    },
    data() {
        return {
            items: ['Foo', 'Bar', 'Fizz', 'Buzz'],
            top: [],
            middle:[],
            bottom:[],
            detail:[],
            topName: "",
        } 
    },
    async created() {
        await this.$axios.get(`${this.apiurl}/erp/category`, {
                'headers': {
                    'Content-Type': "Content-Type: application/json; charset=utf-8",
                    'Authorization' : "token a4f797d9efbdae9d5aef894c2aa4c54621435471"
                }
            })
            .then(r=> {
                if ( r.data.result == true ) {
                    const arr = r.data.data.category_list
                    arr.map(value => {
                        this.top.push(value)
                    })
                } else {
                    console.log(r);
                }
               
            })
            .catch(e=> {
                console.log(e.response);
            })
    },
    methods: {
        async topFunc( e) {
            await this.$axios.get(`${this.apiurl}/erp/category/?category_id=${e}`, {
                'headers': {
                    'Content-Type': "Content-Type: application/json; charset=utf-8",
                    'Authorization' : "token a4f797d9efbdae9d5aef894c2aa4c54621435471"
                }
            })
            .then(r=> {
                this.middle = []
                if ( r.data.result == true ) {
                    const arr = r.data.data.category_list
                    arr.map(value => {
                        this.middle.push(value)
                    })
                } else {
                    console.log(r);
                }
               
            })
            .catch(e=> {
                console.log(e.response);
            })
            let sel = document.getElementById("topc");
            this.$emit("top-category-func-parent", {'name':sel.value, 'value': e } )
        },
        async middleFunc(e) {
            await this.$axios.get(`${this.apiurl}/erp/category/?category_id=${e}`, {
                'headers': {
                    'Content-Type': "Content-Type: application/json; charset=utf-8",
                    'Authorization' : "token a4f797d9efbdae9d5aef894c2aa4c54621435471"
                }
            })
            .then(r=> {
                this.bottom = []
                if ( r.data.result == true ) {
                    const arr = r.data.data.category_list
                    arr.map(value => {
                        this.bottom.push(value)
                    })
                } else {
                    console.log(r);
                }
               
            })
            .catch(e=> {
                console.log(e.response);
            })
            let sel = document.getElementById("middlec");
            this.$emit("middle-category-func-parent", {'name':sel.value, 'value': e } )
        },
        async bottomFunc(e) {
            await this.$axios.get(`${this.apiurl}/erp/category/?category_id=${e}`, {
                'headers': {
                    'Content-Type': "Content-Type: application/json; charset=utf-8",
                    'Authorization' : "token a4f797d9efbdae9d5aef894c2aa4c54621435471"
                }
            })
            .then(r=> {
                this.detail = []
                if ( r.data.result == true ) {
                    const arr = r.data.data.category_list
                    arr.map(value => {
                        this.detail.push(value)
                    })
                } else {
                    console.log(r);
                }
               
            })
            .catch(e=> {
                console.log(e.response);
            })
            let sel = document.getElementById("bottomc");
            this.$emit("bottom-category-func-parent", {'name':sel.value, 'value': e } )
        },
        async detailFunc(e) {
            let sel = document.getElementById("detailc");
            this.$emit("detail-category-func-parent", {'name':sel.value, 'value': e } )
            // await this.$axios.get(`${this.apiurl}/erp/category/?category_id=${e}`, {
            //     'headers': {
            //         'Content-Type': "Content-Type: application/json; charset=utf-8",
            //         'Authorization' : "token a4f797d9efbdae9d5aef894c2aa4c54621435471"
            //     }
            // })
            // .then(r=> {
            //     this.detail = []
            //     if ( r.data.result == true ) {
            //         const arr = r.data.data.category_list
            //         arr.map(value => {
            //             this.detail.push(value)
            //         })
            //     } else {
            //         console.log(r);
            //     }
               
            // })
            // .catch(e=> {
            //     console.log(e.response);
            // })
        },

    }
}
</script>