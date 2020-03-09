<template>
    <v-card
        outlined
        tile
        elevation="1"
        class="cardSection"
        >
            <div class="subtitle-2">가격</div>
            <v-row>
                <v-col cols="12" md="6">
                    <v-text-field
                        type="number"
                        label="판매가*"
                        placeholder="판매가를 숫자로 입력하세요"
                        hint="숫자만 가능합니다"
                        suffix="원"
                        :rules="rules.priceCheck"
                        clearable
                        required
                        v-model="price"
                        @change="priceFunc"
                    ></v-text-field>
                </v-col>
                <v-col cols="12" md="6">
                    <v-text-field
                        type="number"
                        label="할인가"
                        hint="할인가를 입력하시면 할인된 가격으로 판매됩니다. 할인 미적용시 입력하지마세요"
                        placeholder="할인가격을 숫자로 입력하세요"
                        suffix="원"
                        clearable
                        persistentHint
                        v-model="salePrice"
                        @change="saleFunc"
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row>
                <v-col cols="12" md="6">
                    <v-text-field
                        type="number"
                        label="적립금"
                        hint="적립금 미적용시 입력하지마세요"
                        placeholder="적립금을 입력하세요"
                        suffix="원"
                        clearable
                        persistentHint
                        v-model="point"
                        @change="pointFunc"
                    ></v-text-field>
                </v-col>
                <v-col cols="12" md="6">
                    <v-text-field
                        type="number"
                        label="공급가"
                        hint="공급가는 일반사용자들에게 공개되지않습니다. 판매자 관리용입니다"
                        placeholder="공급가 입력하세요"
                        suffix="원"
                        clearable
                        persistentHint
                        v-model="originPrice"
                        @change="originPriceFunc"
                    ></v-text-field>
                </v-col>
            </v-row>
            <div>
                <p>부가세</p>
                <v-radio-group row v-model="tax" @change="taxFunc">
                <v-radio label="과세" value="1"></v-radio>
                <v-radio label="면세" value="3"></v-radio>
                <v-radio label="영세" value="2"></v-radio>
                <v-radio label="기타" value="0"></v-radio>
                </v-radio-group>
            </div>
        </v-card>
</template>

<script>
export default {
    data() {
        return {
            rules: {
                priceCheck: [value => !!value || '필수입력']
            },
            price: "",
            salePrice: "",
            point: "",
            originPrice: "",
            tax: "1"
        }
    },
    filters: {
        numberWithDelimiter(value) {
            if(!value) {
                return '0'
            } else {
                return value.toString().replace(/(\d)(?=(\d{3})+$)/g, '$1,')
            }
        }
    },
    methods: {
        priceFunc() {
            this.$emit("price-func-parent", this.price);
        },
        saleFunc() {
            this.$emit("sale-func-parent", this.salePrice);
        },
        pointFunc() {
            this.$emit("point-func-parent", this.point);
        },
        originPriceFunc() {
            this.$emit("origin-price-func-parent", this.originPrice);
        },
        taxFunc() {
            this.$emit("tax-func-parent", this.tax);
        }
    }
}
</script>