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
                            v-model="optionStockValue"
                            @change="optionStockValueFunc"
                        ></v-text-field>
                </v-col>

            </v-row>
            <optionAdd v-show="optionDetail" @option-value-default-parent="optionValueDefaultParent" @option-value-input-parent="optionValueInputParent" />
        </v-card>
</template>

<script>
import optionAdd from "~/components/product/optionAdd.vue";

export default {
    data() {
        return {
            optionCheck: false,
            optionStock: true,
            optionDetail: false,  
            optionStockValue: "",
            optionValue: "",
            optionInput: "",
        }
    },
    components: {
        optionAdd,
    },
    methods: {
        optionCheckFunc() {
            if ( this.optionCheck === true ) {
                this.$emit("option-check-parent", true)
                this.optionDetail = true
                this.optionStock = false
            } else {
                this.$emit("option-check-parent", false)
                this.optionDetail = false
                this.optionStock = true
            }
            
        },
        optionStockValueFunc() {
            this.$emit("option-stock-func-parent", this.optionStockValue)
        },
        optionValueDefaultParent(val) {
            this.optionValue = val
            this.$emit("option-value-func-parent", this.optionValue)
        },
        optionValueInputParent(val) {
            this.optionInput = val
            this.$emit("option-input-func-parent", this.optionInput)
        }
 
    }
    
}
</script>