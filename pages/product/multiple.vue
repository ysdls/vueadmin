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
                    <v-btn type="submit" v-show="submit">submit</v-btn>
                </v-form>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12">
                <div class="table-responsive">
                    <table class="table table-striped">
                        <tbody>
                            <tr v-for="(r, key) in data" :key="key">
                                <td v-for="c in cols" :key="c.key"> {{ r[c.key] }}</td>
                            </tr>
                        </tbody>
                    </table>
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
            SheetJSFT: _SheetJSFT,
            data: [],
            cols: [],
            submit: false
        }
    },
    methods: {
        onSubmit() {

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
				const data = XLSX.utils.sheet_to_json(ws, {header:1});
				/* Update state */
				this.data = data;
                this.cols = make_cols(ws['!ref']);
                console.log(data);
			};
			reader.readAsBinaryString(file);
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
