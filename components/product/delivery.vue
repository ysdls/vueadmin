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
            <v-checkbox label="착불일시 체크해주세요" v-model="cashOnDelivery" @click="cashOnDeliveryFunc"></v-checkbox>
            <v-text-field
                type="number"
                label="배송비를 입력하세요"
                clearable
                persistentHint
                v-model="deliveryPay"
                v-show="deliveryPayShow"
                @change="deliveryPayFunc"
            ></v-text-field>
            <v-text-field
                type="text"
                label="배송정보관리를 입력하세요"
                clearable
                persistentHint
                v-model="deliveryInfo"
                @change="deliveryInfoFunc"
            ></v-text-field>
            <v-checkbox label="도서산간 추가운임" v-model="mountain" @click="mountainFunc"></v-checkbox>
            <!-- <v-checkbox label="배송비 선결제" v-model="paygoahead" @click="paygoaheadFunc"></v-checkbox> -->
            <v-text-field
                type="number"
                label="배송기간을 입력하세요"
                clearable
                persistentHint
                v-model="deliveryRange"
                @change="deliveryRangeFunc"
            ></v-text-field>
            <v-autocomplete
                :items="provider"
                label="묶음배송 그룹"
                item-text="name"
                item-value="id"
                outlined
                dense
                clearable
                @change="providerFunc($event)"
            ><v-icon slot="prepend" @click="providerAddOpen">mdi-plus</v-icon></v-autocomplete>
                <v-dialog v-model="providerAdd" max-width="500">
                    <v-card>
                        <v-container>
                            <v-row>
                                <v-col cols="12">
                                <v-text-field
                                    label="묶음배송 그룹"
                                    placeholder="추가할 그룹을 입력하세요"
                                    clearable
                                    persistentHint
                                    v-model="providerNew"
                                ></v-text-field>
                                </v-col>                            
                            </v-row>
                        </v-container>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue darken-1" text @click="providerAdd = false">취소</v-btn>
                            <v-btn color="blue darken-1" text @click="asdfFunc('provider')">저장</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
                <v-checkbox label="배송프로모션 제외" v-model="promotion" @click="promotionFunc"></v-checkbox>
    </v-card>
</template>

<script>
export default {
    data() {
        return {
            deliveryDefault:1,
            deliveryFeeDefault:1,
            deliveryRange:"3",
            deliveryPay: 3000,
            delivery: [
                {"text":"기타", value:0}, 
                {"text":"일반배송", value:1}, 
                {"text":"퀵/화물", value:2},
                // {"text":"배송없음", value:3}
            ],
            deliveryFee: [
                {"text":"기타", value:0},
                {"text":"무료", value:1},
                {"text":"고정배송비 사용", value:2},
                {"text":"구매금액에 따른부과", value:3},
                {"text":"상품 수량별 차등 배송료 사용", value:4},
            ],
            cashOnDelivery: false,
            mountain: true,
            paygoahead: false,
            deliveryPayShow: true,
            deliveryInfo: "상세정보참고",
            provider: [],
            providerAdd: false,
            providerNew: "",
            promotion: false
        }
    },
    methods: {
        deliveryFunc(e) {
            this.$emit('delivery-func-parent', e)
        },
        //착불여부
        cashOnDeliveryFunc() {
            this.$emit('delivery-cash-func-parent', this.cashOnDelivery)
            if ( this.cashOnDelivery == false) {
                this.deliveryPayShow = true
            } else {
                this.deliveryPayShow = false
                this.$emit('delivery-pay-func-parent', 0);
            }
        },
        //배송비
        deliveryPayFunc() {
            this.$emit('delivery-pay-func-parent', this.deliveryPay);
        },
        //배송정보
        deliveryInfoFunc() {
            this.$emit('delivery-info-func-parent', this.deliveryInfo)
        },
        //배송기간
        deliveryRangeFunc() {
            this.$emit('delivery-range-func-parent', this.deliveryRange)
        },
        //도서산간
        mountainFunc() {
            this.$emit("delivery-mountain-func-parent", this.mountain)
        },
        //배송그룹
        providerFunc(e) {
            this.$emit('delivery-provider-func-parent', e)
        },
        providerAddOpen() {
            this.providerAdd = true
        },
        asdfFunc(value) {
            console.log(value);
        },
        promotionFunc() {
            this.$emit("delivery-promotion-func-parent", this.promotion)
        }
        // paygoaheadFunc() {
        //     this.$emit('paygoahead-func-parent', this.paygoahead)
        // },

        
    }
}
</script>