<template>
    <v-card
    outlined
    tile
    elevation="1"
    class="cardSection"
    >
        <div class="subtitle-2">배송</div>
            <v-select
                :items="delivery"
                label="배송구분"
                outlined
                dense
                v-model="deliveryDefault"
                @change="deliveryFunc($event)"
            ></v-select>
            <v-select
                :items="deliveryFee"
                label="배송비 타입"
                outlined
                dense
                v-model="deliveryFeeDefault"
                @change="deliveryFeeFunc($event)"
            ></v-select>
            <v-text-field
                type="number"
                label="배송기간을 입력하세요"
                clearable
                persistentHint
                v-model="deliveryRange"
                @change="deliveryRangeFunc"
            ></v-text-field>
            <v-text-field
                type="number"
                label="배송비를 입력하세요"
                clearable
                persistentHint
                v-model="deliveryPay"
                v-show="deliveryPayShow"
                @change="deliveryPayFunc"
            ></v-text-field>
            <v-checkbox label="도서산간 추가운임" v-model="mountain" @click="mountainFunc"></v-checkbox>
            <v-checkbox label="배송비 선결제" v-model="paygoahead" @click="paygoaheadFunc"></v-checkbox>
    </v-card>
</template>

<script>
export default {
    data() {
        return {
            deliveryDefault:1,
            deliveryFeeDefault:1,
            deliveryRange:"3",
            deliveryPay: 0,
            delivery: [
                {"text":"기타", value:0}, 
                {"text":"택배", value:1}, 
                {"text":"퀵/화물", value:2},
                {"text":"배송없음", value:3}
            ],
            deliveryFee: [
                {"text":"기타", value:0},
                {"text":"무료", value:1},
                {"text":"고정배송비 사용", value:2},
                {"text":"구매금액에 따른부과", value:3},
                {"text":"상품 수량별 차등 배송료 사용", value:4},
            ],
            mountain: false,
            paygoahead: false,
            deliveryPayShow: false
        }
    },
    methods: {
        deliveryFunc(e) {
            this.$emit('delivery-func-parent', e)
        },
        deliveryFeeFunc(e) {
            this.$emit('delivery-fee-func-parent', e)
            if ( e != 1) {
                this.deliveryPayShow = true
            } else {
                this.deliveryPayShow = false
            }
        },
        deliveryRangeFunc() {
            this.$emit('delivery-range-func-parent', this.deliveryRange)
        },
        mountainFunc() {
            this.$emit("mountain-func-parent", this.mountain)
        },
        paygoaheadFunc() {
            this.$emit('paygoahead-func-parent', this.paygoahead)
        },
        deliveryPayFunc() {
            this.$emit('delivery-pay-func-parent', this.deliveryPay);
        },
        
    }
}
</script>