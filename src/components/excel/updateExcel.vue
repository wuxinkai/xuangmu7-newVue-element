<template>
  <div>
    <input ref="input" type="file" style="display:none" accept=".xlsx" @change="change" />
  </div>
</template>
<script>
import Excel from "exceljs";
import FileSaver from "file-saver";
import { isArray, isString } from "@/util/validate.js";
export default {
  name: "meiExcelHelp",
  data() {
    return {
      EXCEL_TYPE: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;charset=UTF-8"
    };
  },
  //外部属性
  props: {},
  //内部方法
  methods: {
    initPage() { },
    change(e) {
      const files = e.target.files;
      let fileReader = new FileReader();
      fileReader.onload = async ev => {
        const data = ev.target.result;
        const workbook = new Excel.Workbook();
        await workbook.xlsx.load(data);
        let newdata = [] //数据 bbody
        let cols = []; //表头header
        workbook.eachSheet((worksheet, sheetId) => {
          //读写 Excel的第一页， 页签在excel的底部。
          if (sheetId == 1) {
            worksheet.eachRow((row, rowNumber) => {
              //第一行是列头
              if (rowNumber == 1) {
                row.eachCell(cell => {
                  cols.push(cell.value);
                });
              } else {
                let newrow = {};
                //第二行 读单元格
                row.eachCell((cell, colNumber) => {
                  Object.assign(newrow, {
                    [cols[colNumber - 1]]: cell.value
                  });
                });
                //加入行
                newdata.push(newrow);
              }
            });
            //触发事件
            this.$refs['input'].value = ""
            this.$emit("readExcel", newdata, cols, worksheet, workbook);
          }
        })
      }

      fileReader.readAsBinaryString(files[0]);
    },
    //触发选择事件
    select() {
      this.$refs.input.click();
    },

    //下载模板
    async downLoad(data, title = 'download') {
      //创建文件
      const workbook = new Excel.Workbook();
      //新建页签
      const sheet = workbook.addWorksheet('Sheet1');
      //如果是数组
      if (!isArray(data)) {
        return;
      }
      sheet.addRow([...data]);
      const buffer = await workbook.xlsx.writeBuffer();
      const blod = new Blob([buffer], { type: this.EXCEL_TYPE });
      FileSaver.saveAs(blod, `${title}.xlsx`)
    }
  },  //组件
  components: {},
  //初始化 异步加async await
  mounted() {
    this.initPage();
  },
  //计算属性
  computed: {},
  //监视
  watch: {}
};
</script>