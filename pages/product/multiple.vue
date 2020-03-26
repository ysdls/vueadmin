<template>
    <v-card    
    outlined
    tile
    elevation="1"
    class="cardSection">
        <v-row>
            <v-col cols="12">
                <v-form ref="form" @submit.prevent="onSubmit">
                    <div class="form-group">
                        <label for="file">파일 선택</label>
                        <v-file-input type="file" label="엑셀파일을 올려주세요" id="file" :accept="SheetJSFT" @change="_change"></v-file-input>
                    </div>
                    <v-btn type="submit" v-show="submit">업로드</v-btn>
                </v-form>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12">
                <div class="table-responsive">
                    
                    <div class="table table-striped">
                        <div>
                            <div v-for="(r, key) in data" :key="key">
                                <span >
                                    <v-checkbox v-if="key != 0" :value="key"></v-checkbox>
                                </span>
                                <span v-for="c in cols" :key="c.key"> 
                                    {{ r[c.key] }}
                                    c: {{ c.key }}
                                </span>
                            </div>
                        </div>
                    </div>
                </div>
            </v-col>
        </v-row>
    </v-card>
</template>

<script>
import XLSX from 'xlsx';

const make_cols = refstr => 
    Array(XLSX.utils.decode_range(refstr).e.c + 1).fill(0).map((x,i) => (
        {name:XLSX.utils.encode_col(i), key:i}
    ));

const _SheetJSFT = [
	"xlsx", "xlsb", "xlsm", "xls", "xml", "csv", "txt", "ods", "fods", "uos", "sylk", "dif", "dbf", "prn", "qpw", "123", "wb*", "wq*", "html", "htm"
].map(function(x) { return "." + x; }).join(",");

export default {
    data() {
        return {
            apiurl: "https://dev-core.airsupply.kr",
            token: "a4f797d9efbdae9d5aef894c2aa4c54621435471",
            // apiurl: "https://core.airsupply.kr",
            // token: "1cdcd1ef0c508a48d80f10c6d9d43eabac29b635",
            SheetJSFT: _SheetJSFT,
            data: [],
            cols: [],
            submit: false
        }
    },
    methods: {
        onSubmit() {
            let result = ""
            for( let i=0; i<this.data.length; i++) {
                let data = {
                    product : {
                        productName: this.data[i][0],  
                        model: this.data[i][19],            
                        sellDate: [],    //array
                        datemanufac: new Date().toISOString().substr(0, 10), //0000-00-00
                    },
                    price: {
                        price: this.data[i][2],          //int
                        salePrice: this.data[i][3],  //int
                        tax: 1
                    },  
                    options: {
                        optionStock: 0,
                        optionValue: null,
                        optionInput: null
                    },
                    delivery: {
                        deliveryMethod: this.data[i][26],       //배송구분
                        deliveryFeeType: this.data[i][27],      //배송비 타입
                        deliveryDuration: 3,  //int
                        remoteDeliveryFee: true,            //bool
                        prepay: false,        //bool
                        minimumQuantity : this.data[i][6],           //int
                    },
                    info: {
                        detailInfo: this.data[i][25],
                        memo: "",
                    }
                }
                //공급가
                if ( this.data[i][4] != undefined ) {
                    data.price.originPrice = this.data[i][4]
                }
                //적립금
                if ( this.data[i][5] != undefined ) {
                    data.price.point = this.data[i][5]
                }
                //브랜드
                if ( this.data[i][20] != undefined) {
                    data.product.brand = this.data[i][20]
                }
                //제조사
                if ( this.data[i][21] != undefined) {
                    data.product.manufacturer = this.data[i][21]
                }
                //공급사
                if ( this.data[i][22] != undefined) {
                    data.product.supply = this.data[i][22]
                }
                //원산지
                if ( this.data[i][23] != undefined) {
                    data.product.origin = this.data[i][23]
                }
                //검색어
                if ( this.data[i][24] != undefined ) {
                    let word = this.data[i][24].split(",")
                    data.info.search = word
                }
                if ( this.data[i][27] == 2 ) {
                    data.delivery.deliveryFeeDetail = {
                        fixed_fee : parseInt(this.data[i][28])
                    }  
                }
                //상품 이미지
                let obj = []
                for ( let j=11; j<19; j++) {
                    if ( this.data[i][j] != undefined ) {
                        obj.push( this.data[i][j] )
                    }
                }
                data.info.productImageUrl = obj
                //카테고리
                if ( this.data[i][1] != undefined ) {
                    let str = String(this.data[i][1])
                    if ( str.indexOf(",") != -1 ) {
                        data.info.category = str.split(",")
                    } else {
                        data.info.category = [this.data[i][1]]
                    }
                }
                //옵션 설정
                let option = []
                if( this.data[i][7] != undefined ) {
                    let optionElement = ""
                    if ( this.data[i][8] != undefined ) {
                        let optionValue = this.data[i][8].split(",")
                        optionValue.map(el => {
                            if( this.data[i][9] != undefined ) {
                                if ( this.data[i][10] != undefined ) {
                                    let optionValue2 = this.data[i][10].split(",")
                                    optionValue2.map(el2 => {
                                        optionElement = `${this.data[i][7]}=${el}_${this.data[i][9]}=${el2}_999_S_0`
                                        option.push(optionElement)
                                    })
                                }
                            } else {
                                optionElement = `${this.data[i][7]}=${el}_999_S_0`
                                option.push(optionElement)
                            }
                           
                        })
                        data.options.optionValue = option
                    } else {
                        console.log("옵션 에러");
                    }
                }
 //               console.log(data);
                this.$axios.post(`${this.apiurl}/erp/product/`, data ,{
                    'headers': {
                        'Content-Type': "application/json;charset=utf-8;",
                        'Authorization' : `token ${this.token}`
                    }
                })
                .then(r=> {
                    console.log(r);

                })
                .catch(e=> {
                    console.log(e.response);
                })
            }
        },
        _change(evt) {
			// const files = evt.target.files;
            //if(files && files[0]) 
            this._file(evt);
            this.submit = true
        },
        _file(file) {
			/* Boilerplate to set up FileReader */
			const reader = new FileReader();
			reader.onload = (e) => {
				/* Parse data */
				const bstr = e.target.result;
				const wb = XLSX.read(bstr, {type:'binary'});
				/* Get first worksheet */
				const wsname = wb.SheetNames[0];
                const ws = wb.Sheets[wsname];
                
				/* Convert array of arrays */
                let data = XLSX.utils.sheet_to_json(ws, {header:1});
                data.shift();
                /* Update state */
				this.data = data;
                this.cols = make_cols(ws['!ref']);
               console.log(data);

			};
			reader.readAsBinaryString(file);
        },

    
    }
}
</script>

<style scoped>
.cardSection {
    padding: 15px;
    margin-bottom: 20px;
}
</style>
