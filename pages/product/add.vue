<template>
    <v-form ref="form">
        <v-row>
            <v-col cols="12" md="6">
                <category />
                <product-info />
                <price />
                <option-setting />
                <terms />
            </v-col>
            <v-col cols="12" md="6">
                <detail-image />
                <detail-info />
                <v-row>
                    <v-col cols="12" md="6">
                        <product-detail />
                    </v-col>
                    <v-col cols="12" md="6">
                        <delivery />
                    </v-col>
                </v-row>
            </v-col>
        </v-row> 
    </v-form>
</template>

<script>
import category from "~/components/product/category.vue";
import productInfo from "~/components/product/productInfo.vue";
import price from "~/components/product/price.vue";
import terms from "~/components/product/terms.vue";
import optionSetting from "~/components/product/option.vue";
import detailImage from "~/components/product/detailImage.vue";
import detailInfo from "~/components/product/detailInfo.vue";
import productDetail from "~/components/product/productDetail.vue";
import delivery from "~/components/product/delivery.vue";

export default {
    data() {
        return {
            vaild: true,                

        }
    },
    components: {
        category,
        productInfo,
        price,
        terms,
        optionSetting,
        detailImage,
        detailInfo,
        productDetail,
        delivery
    }

}
</script>

<style scoped>
.cardSection {
    padding: 15px;
    margin-bottom: 20px;
}
</style>

