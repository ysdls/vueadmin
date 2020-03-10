<template>
    <v-form ref="form" @submit.prevent="onSubmit">
        <v-row>
            <v-col cols="12" md="7">
                <category :apiurl="apiurl" :token="token" @top-category-func-parent="topCategoryFuncParent" @middle-category-func-parent="middleCategoryFuncParent" @bottom-category-func-parent="bottomCategoryFuncParent" @detail-category-func-parent="detailCategoryFuncParent" />
                <product-info @product-name-func-parent="productNameFuncParent" @date-range-func-parent="dateRangeFuncParent" />
                <detail-info :apiurl="apiurl" :token="token" @detail-info-func-parent="detailInfoFuncParent" />
                <option-setting @option-check-parent="optionCheckParent" @option-stock-func-parent="optionStockFuncParent" @option-value-func-parent="opionValueFuncParent" @option-input-func-parent="optionInputFuncParent" />
                <terms @min-count-func-parent="minCountFuncParent" />
                <v-card outlined tile elevation="1" class="cardSection" >
                    <div class="subtitle-2">검색어</div>
                    <v-row>
                        <v-col cols="12"  style="margin-top:4px;">
                            <v-combobox
                                v-model="searchword"
                                chips
                                clearable
                                multiple
                                clear-icon
                                label="엔터 또는 tab키를 이용해 연속 입력"
                                dense
                            >
                                <template v-slot:selection="{ attrs, item, select, selected }">
                                    <v-chip
                                        v-bind="attrs"
                                        :input-value="selected"
                                        close
                                        @click="select"
                                        @click:close="remove(item)"
                                    >
                                        <strong>{{ item }}</strong>&nbsp;
                                    </v-chip>
                                </template>
                            </v-combobox>
                        </v-col>
                    </v-row>
                </v-card>
                <v-card outlined tile elevation="1" class="cardSection" >
                    <div class="subtitle-2">운영메모</div>
                    <v-textarea
                    clearable
                    clear-icon="cancel"
                    label="Memo"
                    v-model="memo"
                    ></v-textarea>
                </v-card>

            </v-col>
            <v-col cols="12" md="5">
                <detail-image :apiurl="apiurl" :token="token" @product-image-func-parent="productImageFuncParent" />
                <price @price-func-parent="priceFuncParent" @sale-func-parent="saleFuncParent" @point-func-parent="pointFuncParent" @origin-price-func-parent="originPriceFuncParent" @tax-func-parent="taxFuncParent" />
                <v-row>
                    <v-col cols="12" md="6">
                        <product-detail  :apiurl="apiurl" :token="token" @model-func-parent="modelFuncParent" @brand-func-parent="brandFuncParent" @manufacture-func-parent="manufactureFuncParent" @origin-func-parent="originFuncParent" @supply-func-parent="supplyFuncParent" @picker-func-parent="pickerFuncParent" />
                    </v-col>
                    <v-col cols="12" md="6">
                        <delivery @delivery-func-parent="deliveryFuncParent" @delivery-fee-func-parent="deliveryFeeFuncParent" @delivery-range-func-parent="deliveryRangeFuncParent" @mountain-func-parent="mountainFuncParent" @paygoahead-func-parent="paygoaheadFuncParent" @delivery-pay-func-parent="deliveryPayFuncParent" />
                    </v-col>
                </v-row>
            </v-col>
        </v-row> 
        <v-btn type="submit">submit</v-btn>
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
import search from "~/components/product/search.vue";


export default {

    data() {
        return {
            // apiurl: "https://dev-core.airsupply.kr",
            // token: "b67a8df0c64c99465b866a3b8ee25293bd5e1139",
            apiurl: "https://dev-core.airsupply.kr",
            token: "cdd57477b0a7316c60e6050bb1db72112c601bdf",
            vaild: true,
            productName:null,  //제품명     
            sellDate: [],        //판매기간  
            price: "",
            salePrice: "",
            point:"",
            originPrice: "",    
            deliveryData: 1,     //배송구분
            deliveryType: 1,     //배송비 타입
            deliveryRange: 3,
            mountain: false,
            paygoahead: false,
            
            minCount : 1,        //최소구매수량
            model:null,             //모델명
            brand:{
                name:"",
                value:""
            },
            manufacture: "",
            origin: "",
            supply: "",
            datemanufac: new Date().toISOString().substr(0, 10),           //제조일자
            
            optionCheck: false,
            optionStock: 0,
            optionValue: null,
            optionInput: null,
            tax:1,
            productImage: [],
            detailInfo: null,
            
            topc: {
                name:"",
                value:null
            },
            middlec:{
                name:"",
                value:null
            },
            bottomc:{
                name:"",
                value:null
            },
            detailc:{
                name:"",
                value:null
            },
            searchword:[],
            deliveryPay: 0,
            memo: ""
            
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
        delivery,
        search
    },
    methods: {
        //카테고리
        topCategoryFuncParent(val) {
            this.topc = val
            if(this.topc.name != "" ) {
                this.searchword.push(this.topc.name)
            }
        },
        middleCategoryFuncParent(val) {
            this.middlec = val
            if(this.middlec.name != "" ) {
                this.searchword.push(this.middlec.name)
            }
        },
        bottomCategoryFuncParent(val) {
            this.bottomc = val
            if(this.bottomc.name != "" ) {
                this.searchword.push(this.bottomc.name)
            }
        },
        detailCategoryFuncParent(val) {
            this.detailc = val
            if(this.detailc.name != "" ) {
                this.searchword.push(this.detailc.name)
            }
        },
        //상품명
        productNameFuncParent(val) {
            this.productName = val
            if(this.productName != null ) {
                this.searchword.push(this.productName)
            }
        },
        //판매기간
        dateRangeFuncParent(val) {
            this.sellDate = val
        },
        //가격
        priceFuncParent(val) {
            this.price = val
        },
        saleFuncParent(val) {
            this.salePrice = val
        },
        pointFuncParent(val) {
            this.point = val
        },
        originPriceFuncParent(val) {
            this.originPrice = val
        },
        taxFuncParent(val) {
            this.tax = val
        },
        //배송구분
        deliveryFuncParent(val) {
            this.deliveryData = val
        },
        //배송비타입
        deliveryFeeFuncParent(val) {
            this.deliveryType = val
        },
        deliveryRangeFuncParent(val) {
            this.deliveryRange =val
        },
        mountainFuncParent(val) {
            this.mountain = val
        },
        paygoaheadFuncParent(val) {
            this.paygoahead = val
        },
        deliveryPayFuncParent(val) {
            this.deliveryPay = val
        },
        //옵션 체크 재고
        optionCheckParent(val) {
            this.optionCheck = val
        },
        optionStockFuncParent(val) {
            this.optionStock = val
        },
        opionValueFuncParent(val) {
            this.optionValue = val
        },
        optionInputFuncParent(val) {
            this.optionInput = val
        },
        //최소구매수량
        minCountFuncParent(val) {
            this.minCount = val
        },
        //모델명
        modelFuncParent(val) {
            this.model = val
            if(this.model != null ) {
                this.searchword.push(this.model)
            }
        },
        //브랜드명
        brandFuncParent(val) {
            console.log(val);
            this.brand = val
            if(this.brand.name != "" ) {
                this.searchword.push(this.brand.name)
            }
        },
        //제조사
        manufactureFuncParent(val) {
            this.manufacture = val
        },
        //원산지
        originFuncParent(val) {
            this.origin = val
        },
        //공급사
        supplyFuncParent(val) {
            this.supply = val
        },
        //제조일자
        pickerFuncParent(val) {
            this.datemanufac = val
        },
        //이미지
        productImageFuncParent(val) {
            this.productImage =val
        },
        detailInfoFuncParent(val) {
            this.detailInfo = val
        },
        onSubmit() {
            let pointKey = ""
            let originPriceKey = ""
            let brandKey = ""
            let originKey = ""
            let supplyKey = ""
            if ( this.optionCheck == false ) {
                this.optionValue = null
                this.optionInput = null
            } 
            if ( this.point != "" ) {
                pointKey = `this.point: ${this.point}`
            }
            if ( this.originPrice != "" ) {
                originPriceKey = `this.originPrice: ${this.originPrice}`
            }
            if ( this.brand.value != "" ) {
                brandKey = `this.brand: ${this.brand.value}`
            }
            if ( this.origin != "" ) {
                originKey = `this.origin: ${this.origin}`
            }
            if ( this.supply != "" ) {
                supplyKey = `this.supply: ${this.supply}`
            }
            const data = {
                product : {
                    productName: this.productName,  
                    model: this.model,            
                    brandKey,
                    manufacutrer: this.manufacture,
                    originKey,
                    supplyKey,
                    sellDate: this.sellDate,    //array
                    datemanufac: this.datemanufac, //0000-00-00
                },
                price: {
                    price: this.price,          //int
                    salePrice: this.salePrice,  //int
                    pointKey,          //int
                    originPriceKey,      //int
                    tax: this.tax,          //tax1,tax2,tax3
                },
                options: {
                    optionStock: this.optionStock,
                    optionValue: this.optionValue,
                    optionInput: this.optionInput
                },
                delivery: {
                    deliveryMethod: this.deliveryData,     
                    deliveryFeeType: this.deliveryType, 
                    deliveryDuration: this.deliveryRange,  //int
                    deliveryFeeDetail: null,
                    remoteDeliveryFee: this.mountain,            //bool
                    prepay: this.paygoahead,        //bool
                    minimumQuantity : this.minCount,           //int
                },
                info: {
                    productImage: this.productImage,
                    detailInfo: this.detailInfo,
                    category: [
                       this.bottomc.value
                    ],
                    search: this.searchword,
                    memo: this.memo
                }
            }
 
            this.$axios.post(`${this.apiurl}/erp/product/`, data ,{
                'headers': {
                    'Content-Type': "application/json;charset=utf-8;",
                    'Authorization' : `token ${this.token}`
                }
            })
            .then(r=> {
                if (r.data.result == true) {
                    alert("업로드가 완료되었습니다")
                    window.location.reload()
                } else {
                    alert('필수입력값을 확인해주세요')
                }
                
            })
            .catch(e=> {
                console.log(e.response);
            })
        },
        remove (item) {
            this.searchword.splice(this.searchword.indexOf(item), 1)
            this.searchword = [...this.searchword]
        }
    }

}
</script>

<style scoped>
.cardSection {
    padding: 15px;
    margin-bottom: 20px;
}
</style>

