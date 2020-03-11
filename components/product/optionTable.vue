<template>
    <v-row>
        <v-col cols="12" md="12">
            <v-row>
                <v-col cols="12" md="2">
                    <div style="text-align:center;margin-top: 9px;">기본형</div>
                </v-col>
                <v-col cols="12" md="1">
                    <v-switch v-model="optionDefault" inset hide-details style="margin-top:6px;"></v-switch>
                </v-col>
                <v-col cols="12" md="2" v-show="optionDefault">
                    <v-select
                    :items="optionCount"
                    v-model="optionArray"
                    label="옵션개수"
                    outlined
                    dense
                    hide-details
                    ></v-select>
                </v-col>
            </v-row>
            <v-row v-for="(item, i) in optionArray" :key="item" v-show="optionDefault">
                <v-col cols="12" md="3">
                    <v-text-field
                        label="옵션명"
                        placeholder=""
                        clearable
                        required
                        v-model="optionData[i].title"
                    ></v-text-field>
                </v-col>
                <v-col cols="12" md="9" style="margin-top:4px;">
                    <v-combobox
                        v-model="optionData[i].chips"
                        chips
                        clearable
                        multiple
                        clear-icon
                        label="엔터 또는 tab키를 이용해 연속 입력"
                        dense
                        @change="comboFunc(i)"
                    >
                        <template v-slot:selection="{ attrs, item, select, selected }">
                            <v-chip
                                v-bind="attrs"
                                :input-value="selected"
                                close
                                @click="select"
                                @click:close="remove(item, i)"
                            >
                                <strong>{{ item }}</strong>&nbsp;
                            </v-chip>
                        </template>
                    </v-combobox>
                </v-col>
            </v-row>
            <v-row v-show="optionDefault">
                <!-- <optionTable :optionArray="optionArray" :optionData="optionData" /> -->
                <v-col v-if="optionArray == 1" >
                    <v-row>
                        <v-col cols="12" md="6" class="text-center">{{ optionData[0].title }}</v-col>
                        <v-col cols="12" md="2" class="text-center">옵션가격</v-col>
                        <v-col cols="12" md="2" class="text-center">재고량</v-col>
                        <v-col cols="12" md="2" class="text-center">상태</v-col>
                    </v-row>
                    
                    <v-row v-for="(item, i) in optionData[0].chips" :key="item" no-gutters>
                            <v-col cols="12" md="6" style="padding:0 2px">
                                <p style="text-align: center;padding-top: 15px;">{{ item }}</p>
                            </v-col>
                            <v-col cols="12" md="2" style="padding:0 2px">
                                <v-text-field
                                    type="number"
                                    placeholder=""
                                    @change="optionPrice($event, i)"
                                ></v-text-field>
                            </v-col>
                            <v-col cols="12" md="2" style="padding:0 2px">
                                <v-text-field
                                    type="number"
                                    placeholder=""
                                    @change="optionStock($event, i)"
                                ></v-text-field>
                            </v-col>
                            <v-col cols="12" md="2" style="padding:0 2px">
                                <v-select
                                    :items="status"
                                    @change="optionStatus($event, i)"
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
                    
                    <v-row v-for="(item, i) in optionData[0].chips" :key="item" no-gutters>
                        <v-row  v-for="(item1, j) in optionData[1].chips" :key="item1" no-gutters>
                            <v-col cols="12" md="3" style="padding:0 2px">
                                <p style="text-align: center;padding-top: 15px;">{{ item }}</p>
                            </v-col>
                            <v-col cols="12" md="3" style="padding:0 2px">
                                <p style="text-align: center;padding-top: 15px;">{{ item1 }}</p>
                            </v-col>
                            <v-col cols="12" md="2" style="padding:0 2px">
                                <v-text-field
                                    type="number"
                                    placeholder=""
                                    @change="optionPrice($event, i, j)"
                                ></v-text-field>
                            </v-col>
                            <v-col cols="12" md="2" style="padding:0 2px">
                                <v-text-field
                                    type="number"
                                    placeholder=""
                                    @change="optionStock($event, i, j)"
                                ></v-text-field>
                            </v-col>
                            <v-col cols="12" md="2" style="padding:0 2px">
                                <v-select
                                    :items="status"
                                    @change="optionStatus($event, i, j)"
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
                    
                    <v-row v-for="(item, i) in optionData[0].chips" :key="item" no-gutters>
                        <v-row  v-for="(item1, j) in optionData[1].chips" :key="item1" no-gutters>
                            <v-row  v-for="(item2, k) in optionData[2].chips" :key="item2" no-gutters>
                                <v-col cols="12" md="2" style="padding:0 2px">
                                    <p style="text-align: center;padding-top: 15px;">{{ item }}</p>
                                </v-col>
                                <v-col cols="12" md="2" style="padding:0 2px">
                                    <p style="text-align: center;padding-top: 15px;">{{ item1 }}</p>
                                </v-col>
                                <v-col cols="12" md="2" style="padding:0 2px">
                                    <p style="text-align: center;padding-top: 15px;">{{ item2 }}</p>
                                </v-col>
                                <v-col cols="12" md="2" style="padding:0 2px">
                                    <v-text-field
                                        type="number"
                                        placeholder=""
                                        @change="optionPrice($event, i, j, k)"
                                    ></v-text-field>
                                </v-col>
                                <v-col cols="12" md="2" style="padding:0 2px">
                                    <v-text-field
                                        type="number"
                                        placeholder=""
                                        @change="optionStock($event, i, j, k)"
                                    ></v-text-field>
                                </v-col>
                                <v-col cols="12" md="2" style="padding:0 2px">
                                    <v-select
                                        :items="status"
                                        @change="optionStatus($event, i, j, k)"
                                    ></v-select>
                                </v-col>
                            </v-row>
                        </v-row >
                    </v-row  >
                </v-col>
            </v-row>
            <div v-show="optionDefault">
                <v-btn small @click="optionFinish">생성완료</v-btn>
            </div>
        </v-col>
        <v-col cols="12" md="12">
            <v-row>
                <v-col cols="12" md="2">
                    <div style="text-align:center;margin-top:9px;">입력형</div>
                </v-col>
                <v-col cols="12" md="1">
                    <v-switch v-model="optionInput" inset hide-details style="margin-top:6px;"></v-switch>
                </v-col>
                <v-col cols="12" md="2" v-show="optionInput">
                    <v-select
                    :items="optionCountInput"
                    v-model="optionArrayInput"
                    label="옵션개수"
                    outlined
                    dense
                    hide-details
                    ></v-select>
                </v-col>
            </v-row>
            <v-row v-for="(item, i) in optionArrayInput" :key="item"  v-show="optionInput">
                <v-col cols="12" md="12">
                    <v-text-field
                        label="사용자 입력 옵션명"
                        placeholder=""
                        clearable
                        required
                        v-model="optionDataInput.data[i].title"
                    ></v-text-field>
                </v-col>
            </v-row>
            <div v-show="optionInput" >
                <v-btn small @click="optionFinishInput" >생성완료</v-btn>
            </div>
        </v-col>
    </v-row>
</template>

<script>

export default {
    props: {
        optionIndex: Number
    },
    data() {
        return {
            optionDefault : true,   //기본형
            optionInput: false,     //입력형
            optionCount: [          //기본형 select
                {"text": 1, "value": 1},
                {"text": 2, "value": 2},
                {"text": 3, "value": 3},
            ],
            optionCountInput: [     //입력형 selelct
                {"text": 1, "value": 1},
                {"text": 2, "value": 2},
                {"text": 3, "value": 3},
                {"text": 4, "value": 4},
                {"text": 5, "value": 5},
            ],
            optionArray: 1,         //기본형 초기숫자
            optionArrayInput: 1,    //입력형 초기숫자
            optionData: [
                {title: "",chips: [],Default: "D"},
                {title: "",chips: [],Default: "D"},
                {title: "",chips: [],Default: "D"}
            ],
            optionDataInput: {
                array:"",
                data: [
                    {title: "",Default: "T"},
                    {title: "",Default: "T"},
                    {title: "",Default: "T"},
                    {title: "",Default: "T"},
                    {title: "",Default: "T"}
                ]
            },
            status: [
                {"text": "판매중", value: "S"},
                {"text": "품절", value: "O"},
                {"text": "숨기기", value: "H"}
            ],
            optionValue: {
                title1:"",
                title2:"",
                title3:"",
                options: []
            },
            secondLength: "",
            optionValues: []
        }
    },
    methods: {
        remove (item, i) {
            console.log(i);
            this.optionData[i].chips.splice(this.optionData[i].chips.indexOf(item), 1)
            this.optionData[i].chips = [...this.optionData[i].chips]
            if ( this.optionArray == 1) {
                this.optionValue.options.pop()
            } else if ( this.optionArray == 2) {
                if ( i == 0 ) {
                    this.optionValue.options.pop()
                } else {
                    let len = this.optionData[1].chips.length
                    for(let i=0; i<len+1; i++) {
                        this.optionValue.options[i].values.pop()
                    }
                }
            } else if ( this.optionArray == 3)  {
                if ( i == 0 ) {
                    this.optionValue.options.pop()
                } else if ( i == 1) {
                    let len = this.optionData[1].chips.length
                    for(let i=0; i<len+1; i++) {
                        this.optionValue.options[i].values.pop()
                    }
                } else if ( i == 2 ) {
                    let len = this.optionData[1].chips.length
                    let len2 = this.optionData[2].chips.length
                    for(let i=0; i<len; i++) {
                        for(let j=0; j<len2+1; j++) {
                            this.optionValue.options[i].values[j].element.pop()
                        }
                    }
                }
            }            
        },
        optionFinish() {
            this.optionValues = []
            this.$set(this.optionValues, 'option', this.optionArray)
            if( this.optionArray == 1) {
                let len = this.optionData[0].chips.length
                for(let i=0; i < len; i++) {
                    let title =  this.optionData[0].title
                    let chips = this.optionData[0].chips[i]
                    let stock = this.optionValue.options[i].stock
                    let price = this.optionValue.options[i].price
                    let status = this.optionValue.options[i].status
                    let pushdata = `${title}=${chips}_${stock}_${status}_${price}`
                    this.optionValues.push(pushdata)
                }

            } else if ( this.optionArray == 2) {
                let len = this.optionData[0].chips.length
                let len2 = this.optionData[1].chips.length
                for(let i=0; i < len; i++) {
                    for(let j=0; j < len2; j++) {
                        let title1 =  this.optionData[0].title
                        let title2 =  this.optionData[1].title
                        let chips1 = this.optionData[0].chips[i]
                        let chips2 = this.optionData[1].chips[j]
                        let stock = this.optionValue.options[i].values[j].stock
                        let price = this.optionValue.options[i].values[j].price
                        let status = this.optionValue.options[i].values[j].status
                        let pushdata = `${title1}=${chips1}_${title2}=${chips2}_${stock}_${status}_${price}`
                        this.optionValues.push(pushdata)
                    }
                }
            } else if ( this.optionArray == 3) {
                let len = this.optionData[0].chips.length
                let len2 = this.optionData[1].chips.length
                let len3 = this.optionData[2].chips.length
                for(let i=0; i < len; i++) {
                    for(let j=0; j < len2; j++) {
                        for(let k=0; k < len3; k++) {
                            let title1 =  this.optionData[0].title
                            let title2 =  this.optionData[1].title
                            let title3 =  this.optionData[2].title
                            let chips1 = this.optionData[0].chips[i]
                            let chips2 = this.optionData[1].chips[j]
                            let chips3 = this.optionData[2].chips[k]
                            let stock = this.optionValue.options[i].values[j].element[k].stock
                            let price = this.optionValue.options[i].values[j].element[k].price
                            let status = this.optionValue.options[i].values[j].element[k].status
                            let pushdata = `${title1}=${chips1}_${title2}=${chips2}_${title3}=${chips3}_${stock}_${status}_${price}`
                            this.optionValues.push(pushdata) 
                        }
                    }
                }
            }
            console.log(this.optionValues)
            if ( this.optionDefault == true ) {
                alert("옵션설정이 완료되었습니다.")
                this.$emit("option-value-default-parent", this.optionValues);
            } else {
                alert("옵션설정이 취소되었습니다.")
                this.$emit("option-value-default-parent", null);
            }

        },
        optionFinishInput () {
            if ( this.optionInput == true ) {
                alert("옵션설정이 완료되었습니다.")
                this.$set(this.optionDataInput, 'array', this.optionArrayInput)
                this.$emit("option-value-input-parent", this.optionDataInput);
            } else {
                alert("옵션설정이 취소되었습니다.")
                this.$emit("option-value-input-parent", null);
            }

        },
        comboFunc(num) {
            if ( this.optionArray == 1 ) {
                this.optionValue.options.push({
                    name1: "",
                    name2: "",
                    name3: "",
                    price: "0",
                    stock: "999",
                    status: "S",
                })
            } else if ( this.optionArray == 2 ) {
                let first = 0
                let sec = 0 

                first = this.optionData[0].chips.length
                sec = this.optionData[1].chips.length

                let len = first * sec
                this.secondLength = len
                this.optionValue.options = []
                for(let i=0; i<first; i++) {
                    this.optionValue.options.push({
                        values: []
                    })
                    for(let j=0; j<sec; j++) {
                        this.optionValue.options[i].values.push({
                            name1: "",
                            name2: "",
                            name3: "",
                            price: "0",
                            stock: "999",
                            status: "S",
                        })
                    }
                }
            } else if ( this.optionArray == 3) {
                let first = 0
                let sec = 0 
                let thd = 0

                first = this.optionData[0].chips.length
                sec = this.optionData[1].chips.length
                thd = this.optionData[2].chips.length

                let len = first * sec * thd
                this.secondLength = len
                this.optionValue.options = []
                for(let i=0; i<first; i++) {
                    this.optionValue.options.push({
                        values: []
                    })
                    for(let j=0; j<sec; j++) {
                        this.optionValue.options[i].values.push({
                            element: []
                        })
                        for(let k=0; k<thd; k++) {
                            this.optionValue.options[i].values[j].element.push({
                                name1: "",
                                name2: "",
                                name3: "",
                                price: "0",
                                stock: "999",
                                status: "S",
                            })
                        }
                    }
                }
            }
            

        },
        optionPrice(event, i, j, k) {           
            if ( this.optionArray == 1 ) {
                this.$set(this.optionValue.options[i], 'price', event);
            } else if ( this.optionArray == 2) {
                this.$set(this.optionValue.options[i].values[j], 'price', event);
            } else if ( this.optionArray == 3) {
                this.$set(this.optionValue.options[i].values[j].element[k], 'price', event);
            }
                       
        },
        optionStock(event, i, j, k) {
            if ( this.optionArray == 1 ) {
                this.$set(this.optionValue.options[i], 'stock', event);
            } else if ( this.optionArray == 2) {
                this.$set(this.optionValue.options[i].values[j], 'stock', event);
            } else if ( this.optionArray == 3) {
                this.$set(this.optionValue.options[i].values[j].element[k], 'stock', event);
            }
        },
        optionStatus(event, i, j, k) {
            if ( this.optionArray == 1 ) {
                this.$set(this.optionValue.options[i], 'status', event);
            } else if ( this.optionArray == 2) {
                this.$set(this.optionValue.options[i].values[j], 'status', event);
            } else if ( this.optionArray == 3) {
                this.$set(this.optionValue.options[i].values[j].element[k], 'status', event);
            }
        }
        
    }
}
</script>