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
            SheetJSFT: _SheetJSFT,
            data: [],
            cols: [],
            submit: false
        }
    },
    methods: {
        onSubmit() {
            for( let i=0; i<this.data.length; i++) {
                let data = {
                    product : {
                        productName: this.data[i][0],  
                        model: this.data[i][18],            
                        sellDate: [],    //array
                        datemanufac: new Date().toISOString().substr(0, 10), //0000-00-00
                    },
                    price: {
                        price: this.data[i][1],          //int
                        salePrice: this.data[i][2],  //int
                        tax: 1
                    },  
                    options: {
                        optionStock: 0,
                        optionValue: null,
                        optionInput: null
                    },
                    delivery: {
                        deliveryMethod: 1,     
                        deliveryFeeType: 1, 
                        deliveryDuration: 3,  //int
                        deliveryFeeDetail: 0,
                        remoteDeliveryFee: false,            //bool
                        prepay: false,        //bool
                        minimumQuantity : this.data[i][5],           //int
                    },
                    info: {
                        productImage: ["5d01238c-786f-4049-b48a-de5ce3a3da73"],
                        detailInfo: this.data[i][21],
                        category: [1],
                        search: [ this.data[i][20] ],
                        memo: ""
                    }
                }
                //원산지
                if ( this.data[i][19] != undefined) {
                    data.product.origin = this.data[i][19]
                }
                if ( this.data[i][4] != undefined ) {
                    data.price.point = this.data[i][4]
                }
                if ( this.data[i][3] != undefined ) {
                    data.price.originPrice = this.data[i][3]
                }
                if ( this.data[i][22] != undefined ) {
                    data.delivery.deliveryFeeDetail = this.data[i][22]  //배송비
                }
                // if ( this.data[i][10] != undefined ) {
                //     data.delivery.deliveryFeeDetail = this.data[i][22]  //배송비
                // }
                //console.log(this.data[i][6]);
                let option = []
                if( this.data[i][6] != undefined ) {
                    let optionElement = ""
                    if ( this.data[i][7] != undefined ) {
                        let optionValue = this.data[i][7].split(",")
                        optionValue.map(el => {
                            if( this.data[i][8] != undefined ) {
                                if ( this.data[i][9] != undefined ) {
                                    let optionValue2 = this.data[i][9].split(",")
                                    optionValue2.map(el2 => {
                                        optionElement = `${this.data[i][6]}=${el}_${this.data[i][8]}=${el2}_999_S_0`
                                        option.push(optionElement)
                                    })
                                }
                            } else {
                                optionElement = `${this.data[i][6]}=${el}_999_S_0`
                                option.push(optionElement)
                            }
                           
                        })
                        data.options.optionValue = option
                    } else {
                        console.log("error");
                    }
                }
                console.log(data);
                this.$axios.post(`${this.apiurl}/erp/product/`, data ,{
                    'headers': {
                        'Content-Type': "application/json;charset=utf-8;",
                        'Authorization' : `token ${this.token}`
                    }
                })
                .then(r=> {
                    // if (r.data.result == true) {
                    //     alert("업로드가 완료되었습니다")
                    //     window.location.reload()
                    // } else {
                    //     alert('필수입력값을 확인해주세요')
                    //     console.log(r);
                    // }

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
