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
            ></v-text-field>
            <v-text-field
                label="브랜드"
                placeholder="브랜드를 입력하세요"
                clearable
                persistentHint
            ></v-text-field>
            <v-text-field
                label="제조사"
                placeholder="제조사를 입력하세요"
                clearable
                persistentHint
            ></v-text-field>
            <v-text-field
                label="원산지"
                placeholder="원산지를 입력하세요"
                clearable
                persistentHint
            ></v-text-field>
            <v-text-field
                label="공급사"
                placeholder="공급사를 입력하세요"
                clearable
                persistentHint
            ></v-text-field>
            <div>
                <p>제조일자</p>
                <v-date-picker
                v-model="picker"
                show-current="true"
                locale="ko-kr"
                ></v-date-picker>
            </div>
    </v-card>
</template>

<script>
export default {
    data() {
        return {
            picker :new Date().toISOString().substr(0, 10) 
        }
    }
}
</script>