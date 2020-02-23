<template>
    <v-card
        outlined
        tile
        elevation="1"
        class="cardSection"
        >
            <div class="subtitle-2">옵션</div>
            <v-checkbox label="옵션설정함" v-model="optionCheck" @click="optionCheckFunc"></v-checkbox>
            <v-row v-show="optionStock">
                <v-col cols="12" md="6" >
                        <v-text-field
                            type="number"
                            label="재고량"
                            placeholder="재고량을 숫자로 입력하세요."
                            hint="옵션설정시 옵션재고상태로 변경됩니다."
                            clearable
                            required
                            persistentHint
                        ></v-text-field>
                </v-col>

            </v-row>
            <v-row v-show="optionDetail" >
                <v-col cols="12" md="2" style="margin-top:9px;">
                    <v-select
                    :items="type"
                    label="옵션타입"
                    outlined
                    dense
                    @change="optionType($event)"
                    ></v-select>
                </v-col>
                <v-col cols="12" md="9" v-show="typeInput">
                    <v-text-field
                        label="사용자 입력 옵션명"
                        placeholder=""
                        clearable
                        required
                    ></v-text-field>
                </v-col>
                <v-col cols="12" md="1" v-show="typeInput" style="margin-top:10px;">
                    <v-btn
                        color="dark"
                        dark
                        small
                        fab
                    >
                        <v-icon>mdi-plus</v-icon>
                    </v-btn>
                </v-col>
                <v-col cols="12" md="2" v-show="typeDefault">
                    <v-text-field
                        label="옵션명"
                        placeholder=""
                        clearable
                        required
                    ></v-text-field>
                </v-col>
                <v-col cols="12" md="7" v-show="typeDefault" style="margin-top:6px;">
                    <v-combobox
                        v-model="chips"
                        chips
                        clearable
                        multiple
                        clear-icon
                        label="입력하세요"
                        dense
                    >
                        <template v-slot:selection="{ attrs, item, select, selected }">
                            <v-chip
                                v-bind="attrs"
                                :input-value="selected"
                                close
                                @click="select"
                                @click:close="remove(item)"
                            >
                                <strong>{{ item }}</strong>&nbsp;
                            </v-chip>
                        </template>
                    </v-combobox>
                </v-col>
                <v-col cols="12" md="1" v-show="typeDefault" style="margin-top:10px;">
                    <v-btn
                        color="dark"
                        dark
                        small
                        fab
                    >
                        <v-icon>mdi-plus</v-icon>
                    </v-btn>
                </v-col>
            </v-row>
        </v-card>
</template>

<script>
export default {
    data() {
        return {
            optionCheck: false,
            optionDetail: false,
            optionStock: true,
            typeInput: false,
            typeDefault: false,
            type:[
                {text: '기본', value: 'D'},
                {text: '직접입력', value: 'T'}
            ],
            chips: []
        }
    },
    methods: {
        optionCheckFunc() {
            if ( this.optionCheck === true ) {
                this.optionDetail = true
                this.optionStock = false
            } else {
                this.optionDetail = false
                this.optionStock = true
            }
        },
        optionType(val) {
            if ( val === 'T' ) {
                this.typeInput = true
                this.typeDefault = false
            } else {
                this.typeInput = false
                this.typeDefault = true
            }
        },
        remove (item) {
            console.log(item);
            this.chips.splice(this.chips.indexOf(item), 1)
            this.chips = [...this.chips]
        },
    }
    
}
</script>