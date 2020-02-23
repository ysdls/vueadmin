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
            ></v-select>
            <v-select
                :items="deliveryFee"
                label="배송비 타입"
                outlined
                dense
            ></v-select>
            <v-text-field
                placeholder="배송기간을 입력하세요"
                clearable
                persistentHint
            ></v-text-field>
            <v-checkbox label="도서산간 추가운임" ></v-checkbox>
            <v-checkbox label="배송비 선결제" ></v-checkbox>
    </v-card>
</template>

<script>
export default {
    data() {
        return {
            delivery: ["택배", "퀵/화물", "배송없음"],
            deliveryFee: ["무료", "고정배송비사용", "구매금액에 따른부과", "상품 수량별 차등 배송료 사용"],
            picker :new Date().toISOString().substr(0, 10) 
        }
    }
}
</script>