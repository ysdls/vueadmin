<template>
    <v-card
    outlined
    tile
    elevation="1"
    class="cardSection"
    >
        <div class="subtitle-2">상품정보</div>
        <v-text-field
            label="상품명*"
            hint="최대 100자까지 가능합니다"
            placeholder="상품명을 입력하세요"
            :rules="rules.title"
            clearable
            :counter="100"
            required
            color="grey darken-4"
            class="headline marginText"
        ></v-text-field>
        <div class="d-flex flex-column ">
            <div class="d-flex flex-column caption">상품정보제공공시</div>
            <div class="d-flex flex-column caption">
                    <v-checkbox label="설정함" ></v-checkbox>
            </div>
        </div>
        <v-divider></v-divider>
        <div class="d-flex flex-column">
            <div class="d-flex flex-column caption">판매기간설정</div>
            <div class="d-flex flex-column">
                <v-checkbox label="설정함" v-model="saleCheck" @click="saleCheckFunc"></v-checkbox>
                <v-row v-show="saleDate">
                    <v-col cols="12" sm="6" style="text-align:center;">
                        <v-date-picker locale="ko-kr" v-model="dates" range></v-date-picker>
                    </v-col>
                    <v-col cols="12" sm="6">
                        <v-text-field v-model="dateRangeText" label="판매기간" readonly></v-text-field>
                    </v-col>
                </v-row>
                
            </div>
        </div>
    </v-card>
</template>
<script>
export default {
    data() {
        return {
            rules: {
                title: [
                    value => !!value || '최소 2글자 이상 100글자 이하로 적어주세요',
                    value => (value && value.length >= 2 && value.length <= 100) || '최소 2글자 이상 100글자 이하로 적어주세요',
                ],
            },
            counter: 0,
            saleDate: false,    
            saleCheck: false,
            dates: [ new Date().toISOString().substr(0, 10), new Date().toISOString().substr(0, 10)],
        }
    },
    methods: {
        saleCheckFunc() {
            if ( this.saleCheck === true ) {
                return this.saleDate = true
            } else {
                return this.saleDate = false
            }
        }
    },
    computed: {
        dateRangeText () {
            return this.dates.join(' ~ ')
        },
    },
}
</script>

<style>
.marginText input{
    margin: 7px 0px;
}
</style>