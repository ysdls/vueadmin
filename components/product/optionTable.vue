<template>
    <div>
        <v-col v-if="optionArray == 1" >
            <v-row no-gutters>
                <v-col cols="12" md="6" class="text-center">{{ optionData[0].title }}</v-col>
                <v-col cols="12" md="2" class="text-center">옵션가격</v-col>
                <v-col cols="12" md="2" class="text-center">재고량</v-col>
                <v-col cols="12" md="2" class="text-center">상태</v-col>
            </v-row>
            
            <v-row v-for="(item) in optionData[0].chips" :key="item" no-gutters >
                    <v-col cols="12" md="6" style="padding:0 2px">{{ item }}</v-col>
                    <v-col cols="12" md="2" style="padding:0 2px">
                        <v-text-field
                            type="number"
                            placeholder=""
                        ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="2" style="padding:0 2px">
                        <v-text-field
                            type="number"
                            placeholder=""
                        ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="2" style="padding:0 2px">
                        <v-select
                            :items="status"
                        ></v-select>
                    </v-col>
            </v-row  >
        </v-col>
        <v-col v-else-if="optionArray == 2" >
            <v-row no-gutters>
                <v-col cols="12" md="3" class="text-center">{{ optionData[0].title }}</v-col>
                <v-col cols="12" md="3" class="text-center">{{ optionData[1].title }}</v-col>
                <v-col cols="12" md="2" class="text-center">옵션가격</v-col>
                <v-col cols="12" md="2" class="text-center">재고량</v-col>
                <v-col cols="12" md="2" class="text-center">상태</v-col>
            </v-row>
            
            <v-row v-for="item in optionData[0].chips" :key="item" no-gutters>
                <v-row  v-for="item1 in optionData[1].chips" :key="item1" no-gutters>
                    <v-col cols="12" md="3" style="padding:0 2px">{{ item }}</v-col>
                    <v-col cols="12" md="3" style="padding:0 2px">{{ item1 }}</v-col>
                    <v-col cols="12" md="2" style="padding:0 2px">
                        <v-text-field
                            type="number"
                            placeholder=""
                        ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="2" style="padding:0 2px">
                        <v-text-field
                            type="number"
                            placeholder=""
                        ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="2" style="padding:0 2px">
                        <v-select
                            :items="status"
                        ></v-select>
                    </v-col>
                </v-row >
            </v-row  >
        </v-col>
        <v-col v-else-if="optionArray == 3" >
            <v-row no-gutters>
                <v-col cols="12" md="2" class="text-center">{{ optionData[0].title }}</v-col>
                <v-col cols="12" md="2" class="text-center">{{ optionData[1].title }}</v-col>
                <v-col cols="12" md="2" class="text-center">{{ optionData[2].title }}</v-col>
                <v-col cols="12" md="2" class="text-center">옵션가격</v-col>
                <v-col cols="12" md="2" class="text-center">재고량</v-col>
                <v-col cols="12" md="2" class="text-center">상태</v-col>
            </v-row>
            
            <v-row v-for="item in optionData[0].chips" :key="item" no-gutters>
                <v-row  v-for="item1 in optionData[1].chips" :key="item1" no-gutters>
                    <v-row  v-for="item2 in optionData[2].chips" :key="item2" no-gutters>
                        <v-col cols="12" md="2" style="padding:0 2px">{{ item }}</v-col>
                        <v-col cols="12" md="2" style="padding:0 2px">{{ item1 }}</v-col>
                        <v-col cols="12" md="2" style="padding:0 2px">{{ item2 }}</v-col>
                        <v-col cols="12" md="2" style="padding:0 2px">
                            <v-text-field
                                type="number"
                                placeholder=""
                            ></v-text-field>
                        </v-col>
                        <v-col cols="12" md="2" style="padding:0 2px">
                            <v-text-field
                                type="number"
                                placeholder=""
                            ></v-text-field>
                        </v-col>
                        <v-col cols="12" md="2" style="padding:0 2px">
                            <v-select
                                :items="status"
                            ></v-select>
                        </v-col>
                    </v-row>
                </v-row >
            </v-row  >
        </v-col>
        <div >
            <v-btn small @click="optionFinish">생성완료</v-btn>
        </div>
    </div>
    
</template>

<script>
export default {
    props: {
        optionArray: Number,
        optionData: Array,
    },
    data() {
        return {
            height:300,
            defaultPrice:0,
            status: [
                {"text": "판매중", value: "S"},
                {"text": "품절", value: "O"},
                {"text": "숨기기", value: "H"}
            ],
            optionValue: {
                title:"",
                options:[]
            },
            option: {
                name: "",
                price: "",
                stock: "",
                status: ""
            }
        }
    },
    methods: {
        check() {
            console.log("dkafkjs");
        },
        optionFinish() {
            if( this.optionArray == 1) {
                let len = this.optionData[0].chips.length
                for(let i=0; i < len; i++) {
                    this.optionValue.options = []
                    this.optionValue.options.push(this.option)
                    // this.optionValue.options[i].name = this.optionData[0].chips[i]
                    // console.log(this.optionValue.options[i].name);
                }
                console.log(this.optionValue.options[0]);
                console.log(this.optionValue);
            }
        },
        priceFunc() {
            this.price.push(this.defaultPrice)
            console.log(this.price);
        }

    }
}
</script>