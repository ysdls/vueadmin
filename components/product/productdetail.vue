<template>
    <v-card
    outlined
    tile
    elevation="1"
    class="cardSection"
    >
    <div class="subtitle-2">상품 기타정보</div>
    <div>
        <p>모두 선택사항입니다. 입력시 일반사용자들에게 공개되며, 미입력시에는 공개되지않습니다</p>
    </div>
            <v-text-field
                label="모델명"
                placeholder="모델명을 입력하세요"
                clearable
                persistentHint
                v-model="model"
                @change="modelFunc"
            ></v-text-field>
            <v-autocomplete
                :items="brand"
                label="브랜드"
                item-text="name"
                item-value="id"
                outlined
                dense
                clearable
                id="brand"
                @change="brandFunc($event)"
            ><v-icon slot="prepend" @click="brandAddOpen">mdi-plus</v-icon></v-autocomplete>
                <v-dialog v-model="brandAdd" max-width="500">
                    <v-card>
                        <v-container>
                            <v-row>
                                <v-col cols="12">
                                <v-text-field
                                    label="브랜드"
                                    placeholder="추가할 브랜드명을 입력하세요"
                                    clearable
                                    persistentHint
                                    v-model="brandNew"
                                ></v-text-field>
                                </v-col>                            
                            </v-row>
                        </v-container>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue darken-1" text @click="brandAdd = false">취소</v-btn>
                            <v-btn color="blue darken-1" text @click="addFunc('brand')">저장</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            <v-autocomplete
                :items="manufacture"
                label="제조사"
                item-text="name"
                item-value="id"
                outlined
                dense
                clearable
                @change="manuFunc($event)"
            ><v-icon slot="prepend" @click="manuAddOpen">mdi-plus</v-icon></v-autocomplete>
                <v-dialog v-model="manuAdd" max-width="500">
                    <v-card>
                        <v-container>
                            <v-row>
                                <v-col cols="12">
                                <v-text-field
                                    label="제조사"
                                    placeholder="추가할 제조사를 입력하세요"
                                    clearable
                                    persistentHint
                                    v-model="manuNew"
                                ></v-text-field>
                                </v-col>                            
                            </v-row>
                        </v-container>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue darken-1" text @click="manuAdd = false">취소</v-btn>
                            <v-btn color="blue darken-1" text @click="addFunc('manufacturer')">저장</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            <v-autocomplete
                :items="supply"
                label="공급사"
                item-text="name"
                item-value="id"
                outlined
                dense
                clearable
                @change="supplyFunc($event)"
            ><v-icon slot="prepend" @click="supplyAddOpen">mdi-plus</v-icon></v-autocomplete>
                <v-dialog v-model="supplyAdd" max-width="500">
                    <v-card>
                        <v-container>
                            <v-row>
                                <v-col cols="12">
                                <v-text-field
                                    label="공급사"
                                    placeholder="추가할 공급사를 입력하세요"
                                    clearable
                                    persistentHint
                                    v-model="supplyNew"
                                ></v-text-field>
                                </v-col>                            
                            </v-row>
                        </v-container>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue darken-1" text @click="supplyAdd = false">취소</v-btn>
                            <v-btn color="blue darken-1" text @click="addFunc('supplier')">저장</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            <v-text-field
                label="원산지"
                placeholder="원산지를 입력하세요"
                clearable
                persistentHint
                v-model="origin"
                @change="originFunc"
            ></v-text-field>
            <div>
                <p>제조일자</p>
                <v-date-picker
                v-model="picker"
                show-current="true"
                locale="ko-kr"
                @change="pickerFunc"
                ></v-date-picker>
            </div>
    </v-card>
</template>

<script>
export default {
    props : {
        apiurl:String
    },
    data() {
        return {
            picker :new Date().toISOString().substr(0, 10) ,
            model: "",
            brand: [],
            manufacture: [],
            origin: "",
            supply: [],
            brandAdd:false,
            manuAdd: false,
            supplyAdd: false,
            brandNew:"",
            manuNew:"",
            supplyNew:""
        }
    },
    async created() {
        await this.$axios.get(`${this.apiurl}/erp/product/option/`, {
                'headers': {
                    'Content-Type': "Content-Type: application/json; charset=utf-8",
                    'Authorization' : "token a4f797d9efbdae9d5aef894c2aa4c54621435471"
                }
            })
            .then(r=> {
                if ( r.data.result == true ) {
                    const bd = r.data.data.brand
                    const mf = r.data.data.manufacturer
                    const sp = r.data.data.supplier
                    bd.map(value => {
                        this.brand.push(value)
                    })
                    mf.map(value => {
                        this.manufacture.push(value)
                    })
                    sp.map(value => {
                        this.supply.push(value)
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
        modelFunc() {
            this.$emit('model-func-parent', this.model)
        },
        brandFunc(e) {
            let sel = document.getElementById("brand");
            this.$emit('brand-func-parent',  {'name':sel.value, 'value': e })
        },
        manuFunc(e) {
            this.$emit('manufacture-func-parent', e)
        },
        originFunc() {
            this.$emit('origin-func-parent', this.origin)
        },
        supplyFunc(e) {
            this.$emit('supply-func-parent', e)
        },
        pickerFunc() {
            this.$emit('picker-func-parent', this.picker)
        },
        brandAddOpen() {
            this.brandAdd = true
        },
        manuAddOpen() {
            this.manuAdd = true
        },
        supplyAddOpen() {
            this.supplyAdd = true
        },
        addFunc(value) {
            let form = new FormData()

            if ( value == "brand") {
                form.append("name", this.brandNew )
            } else if ( value == "manufacturer") {
                form.append("name", this.manuNew) 
            } else if ( value == "supplier") {
                form.append("name", this.supplyNew)
            }
            form.append("option_type", value)

            this.$axios.post(`${this.apiurl}/erp/product/option/`, form,{
                'headers': {
                    'Content-Type': "Content-Type: application/json; charset=utf-8",
                    'Authorization' : "token a4f797d9efbdae9d5aef894c2aa4c54621435471"
                }
            })
            .then(r=> {
                if ( r.data.result == true) {
                    const re = r.data.data
                    let obj = new Object()
                        obj.id = re.id
                        obj.name = re.name
                    if ( re.option_type == "brand") {
                        this.brand.push(obj)
                        this.brandAdd = false
                    } else if ( re.option_type == "manufacturer") {
                        this.manufacture.push(obj)
                        this.manuAdd= false
                    } else if ( re.option_type == "supplier") {
                        this.supply.push(obj)
                        this.supplyAdd = false
                    }
                } else {
                    console.log(r);
                }
                
            })
            .catch(e=> {
                console.log('error');
            })
        },

    }
}
</script>