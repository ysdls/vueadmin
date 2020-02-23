<template>
  <v-container fluid>
    <v-row align="center">
      <v-col class="d-flex" cols="12" sm="3">
        <v-select
          :items="items"
          label="대분류"
          outlined
        ></v-select>
      </v-col>

      <v-col class="d-flex" cols="12" sm="3">
        <v-select
          :items="items"
          label="중분류"
          outlined
          dense
        ></v-select>
      </v-col>

      <v-col class="d-flex" cols="12" sm="3">
        <v-select
          :items="items"
          label="소분류"
          outlined
          dense
        ></v-select>
      </v-col>

      <v-col class="d-flex" cols="12" sm="3">
        <v-select
          :items="items"
          label="세분류"
          outlined
          dense
        ></v-select>
      </v-col>
    </v-row>
    <v-row align-center>

    </v-row>
    <v-row align="center">
        <v-col cols="12" md="6">
            <v-img src="https://picsum.photos/510/300?random" aspect-ratio="1.4" contain></v-img>
        </v-col>
        <v-col cols="12" md="6">
            <v-row>
                <v-col cols="12">
                    <v-text-field
                        label="상품명"
                        hint="최대 100자까지 가능합니다"
                        placeholder="상품명을 입력하세요"
                        :rules="rules.title"
                        clearable
                        :counter="100"
                        required
                    ></v-text-field>
                </v-col>
                <v-col cols="12">
                    <v-text-field
                        label="모델명"
                        hint="모델명을 입력하시면 일반사용자들에게 공개됩니다. 모델명이 없을시 입력하지마세요"
                        placeholder="모델명을 입력하세요"
                        clearable
                        persistentHint
                    ></v-text-field>
                </v-col>
                <v-col cols="12">
                    <v-text-field
                        type="number"
                        label="판매가"
                        placeholder="판매가를 숫자로 입력하세요"
                        hint="숫자만 가능합니다"
                        suffix="원"
                        :rules="rules.price"
                        clearable
                        required
                    ></v-text-field>
                </v-col>
                <v-col cols="12">
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
                <v-col cols="12">
                    <v-text-field
                        type="number"
                        label="공급가"
                        hint="공급가는 일반사용자들에게 공개되지않습니다. 판매자 관리용입니다"
                        placeholder="공급가 입력하세요"
                        clearable
                        persistentHint
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row>
                <v-col cols="12">
                    <v-select
                    :items="brand"
                    label="브랜드"
                    outlined
                    dense
                    ></v-select>
                    <v-select
                    :items="manufacture"
                    label="제조사"
                    outlined
                    dense
                    ></v-select>
                    <v-select
                    :items="supply"
                    label="공급사"
                    outlined
                    dense
                    ></v-select>
                    <v-select
                    :items="origin"
                    label="원산지"
                    outlined
                    dense
                    ></v-select>
                </v-col>
            </v-row>
        </v-col>
    </v-row>
    <v-row>
        <v-col cols="12">
            <v-switch  class="switch-offset" v-model="swcOption" label="옵션설정"></v-switch>
            <v-divider></v-divider>
        </v-col>
        <v-col cols="12">
            <v-row class="padding-0-30">
                <v-col cols="1" md="1" align-self="center">옵션 구성타입</v-col>
                <v-col cols="2" md="2">
                    <v-radio-group row v-model="optionSelect">
                        <v-radio label="단일옵션" value="val-1"></v-radio>
                        <v-radio label="복수옵션" value="val-2"></v-radio>
                    </v-radio-group>
                </v-col>
            </v-row>
            <v-row class="padding-0-30">
                <v-col cols="1" md="1" >옵션 개수</v-col>
                <v-col cols="2" md="2">
                    <v-select
                        v-model="optionCount[0]"
                        :items="optionCount"
                        outlined
                        dense
                        single-line
                    ></v-select>
                </v-col>
            </v-row>
            <v-row class="padding-0-30">
                <v-col cols="1" md="1" >정렬 순서</v-col>
                <v-col cols="2" md="2">
                    <v-select
                        v-model="optionSort[0]"
                        :items="optionSort"
                        outlined
                        dense
                        single-line
                    ></v-select>
                </v-col>
            </v-row>
            <v-row class="padding-0-30">
                <v-col cols="1" md="1" >옵션입력</v-col>
                <v-col cols="11" class="padding-0">
                    <v-col cols="12" class="padding-0" v-for="n in 3" :key="n">
                        <v-col cols="2" md="2">
                                <v-text-field
                                    label="옵션명"
                                    hint="예시:컬러"
                                    clearable
                                    outlined
                                    persistentHint
                                    single-line
                                ></v-text-field>
                        </v-col>
                    </v-col>
                </v-col>
            </v-row>
        </v-col>        
    </v-row>
  </v-container>
</template>

<script>
export default {
    data() {
        return {
            items: ['Foo', 'Bar', 'Fizz', 'Buzz'],
            brand: ['나이키', '아디다스'],
            manufacture: ['해당없음', '00공장'],
            supply: ['해당없음'],
            origin: ['해당없음', '대한민국'],
            counter: 0,
            rules: {
                title: [
                    value => !!value || '필수입력',
                    value => (value && value.length >= 2 && value.length <= 100) || '최소 2글자 이상 100글자 이하로 적어주세요',
                ],
                price: [value => !!value || '필수입력']
            },
            swcOption: false,
            optionSelect: "val-1",
            optionCount: ["1", "2", "3"],
            optionSort: ["등록순", "가나다순"]
        }
    },
}
</script>

<style scoped>
.switch-offset {
    padding-left: 15px;
}
.padding-0-30 {
    padding: 0px 30px;
}
.padding-0 {
    padding: 0px;
}
</style>