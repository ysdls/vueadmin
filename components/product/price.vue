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
                        :rules="rules.price"
                        clearable
                        required
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
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row>
                <v-col cols="12" md="6">
                    <v-text-field
                        type="number"
                        label="적립금"
                        hint=""
                        placeholder="적립금을 입력하세요"
                        suffix="원"
                        clearable
                        persistentHint
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
                    ></v-text-field>
                </v-col>
            </v-row>
        </v-card>
</template>

<script>
export default {
    data() {
        return {
            rules: {
                price: [value => !!value || '필수입력']
            },
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
    }
}
</script>