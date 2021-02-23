<template>
  <b-row class="mt-3 mb-3">
    <b-col xl="8" offset-xl="2">
      <b-alert
        :show="dismissCountDown"
        dismissible
        variant="success"
        @dismissed="dismissCountDown = 0"
        @dismiss-count-down="countDownChanged"
      >
        <p>土地补偿费：{{ tudibuchangfee }}万元</p>
        <p>安置补助费：{{ anzhibuzhufee }}万元</p>
        <p>
          合计：<strong>{{ finalfee }}万元</strong>
        </p>
        <p>
          计算依据：<a href="../../static/fee.jpeg" target="_blank"
            >《广州市征收农用地区区片综合地价表》(点击查看)</a
          >
        </p>
        <hr />

        <p>钱拿少了？</p>
        <p>马上联系 ☎ 福律阁土地诉讼团队：</p>
        <a href="tel:18802035347">188 0203 5347</a>
        <b-progress
          variant="warning"
          :max="dismissSecs"
          :value="dismissCountDown"
          height="6px"
        ></b-progress>
      </b-alert>

      <b-alert v-model="showDismissibleAlert" variant="danger" dismissible>
        {{ message }}
      </b-alert>

      <b-form-group id="input-group-1" label="请选择征地范围：">
        <el-select v-model="province" placeholder="--选择省份--">
          <el-option
            v-for="item in areaOptions"
            :key="item.value"
            :label="item.label"
            :value="item"
          >
          </el-option>
        </el-select>

        <el-select
          v-if="province && province.children"
          v-model="city"
          placeholder="--选择城市--"
        >
          <el-option
            v-for="item in province.children"
            :key="item.value"
            :label="item.label"
            :value="item"
          >
          </el-option>
        </el-select>
        <el-select
          v-if="city && city.children"
          v-model="district"
          placeholder="--选择区县--"
        >
          <el-option
            v-for="item in city.children"
            :key="item.value"
            :label="item.label"
            :value="item"
          >
          </el-option>
        </el-select>
        <el-select
          v-if="district && district.children"
          v-model="street"
          placeholder="--选择街道--"
        >
          <el-option
            v-for="item in district.children"
            :key="item.value"
            :label="item.label"
            :value="item"
          >
          </el-option>
        </el-select>
      </b-form-group>

      <b-form-group
        id="input-group-3"
        label="请选择土地类型："
        label-for="input-2"
      >
        <b-form-select
          v-model="selected"
          :options="options2"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group
        id="input-group-2"
        label="请输入征收面积（亩）："
        label-for="input-2"
        description="1亩≈666.7㎡"
      >
        <b-form-input
          id="input-2"
          placeholder="精确到小数点后两位"
          required
          v-model="area"
          type="number"
        ></b-form-input>
      </b-form-group>

      <b-form @reset="onReset">
        <b-button variant="success" block @click="showAlert"
          >立即计算征地补偿标准</b-button
        >
        <b-button variant="outline-secondary" block type="reset"
          >重新填写</b-button
        >
      </b-form>
    </b-col>
  </b-row>
</template>

<script>
export default {
  name: "forminfo",
  data() {
    return {
      dismissSecs: 60,
      dismissCountDown: 0,
      showDismissibleAlert: false,
      area: "",
      tudibuchangfee: null,
      anzhibuzhufee: null,
      finalfee: "",
      selected: null,
      message: "",
      value2: [],

      province: null, // 省份
      city: null, // 城市
      district: null, //区县
      street: null, // 街道

      options2: [
        { value: null, text: "请选择" },
        { value: "a", text: "耕地" },
        { value: "b", text: "园地" },
        { value: "c", text: "林地" },
        { value: "d", text: "牧草地" },
        { value: "e", text: "养捕水面" },
        { value: "f", text: "农田水利设施用地" },
      ],
      areaOptions: [
        {
          value: "广东省",
          label: "广东省",
          children: [
            {
              value: "广州市",
              label: "广州市",
              children: [
                {
                  value: "越秀区",
                  label: "越秀区",
                  children: [
                    {
                      value: "白云街道",
                      label: "白云街道",
                    },
                    {
                      value: "黄花岗街道",
                      label: "黄花岗街道",
                    },
                    {
                      value: "北京街道",
                      label: "北京街道",
                    },
                    {
                      value: "大东街道",
                      label: "大东街道",
                    },
                    {
                      value: "大塘街道",
                      label: "大塘街道",
                    },
                    {
                      value: "登峰街道",
                      label: "登峰街道",
                    },
                    {
                      value: "东山街道",
                      label: "东山街道",
                    },
                    {
                      value: "光塔街道",
                      label: "光塔街道",
                    },
                    {
                      value: "洪桥街道",
                      label: "洪桥街道",
                    },
                    {
                      value: "华乐街道",
                      label: "华乐街道",
                    },
                  ],
                },
                {
                  value: "海珠区",
                  label: "海珠区",
                  children: [
                    {
                      value: "赤岗街道",
                      label: "赤岗街道",
                    },
                    {
                      value: "新港街道",
                      label: "新港街道",
                    },
                    {
                      value: "滨江街道",
                      label: "滨江街道",
                    },
                    {
                      value: "凤阳街道",
                      label: "凤阳街道",
                    },
                    {
                      value: "龙凤街道",
                      label: "龙凤街道",
                    },
                    {
                      value: "沙园街道",
                      label: "沙园街道",
                    },
                    {
                      value: "江海街道",
                      label: "江海街道",
                    },
                  ],
                },
              ],
            },
          ],
        },
      ],
    };
  },
  methods: {
    countDownChanged(dismissCountDown) {
      this.showDismissibleAlert = false;
      if (this.value2[0] == "越秀区") {
        this.tudibuchangfee = parseFloat((this.area * 25.5).toFixed(2));
        this.anzhibuzhufee = parseFloat((this.area * 25.5).toFixed(2));
        this.finalfee = this.tudibuchangfee + this.anzhibuzhufee;
        this.dismissCountDown = dismissCountDown;
      } else {
        this.tudibuchangfee = parseFloat((this.area * 25.09).toFixed(2));
        this.anzhibuzhufee = parseFloat((this.area * 25.09).toFixed(2));
        this.finalfee = this.tudibuchangfee + this.anzhibuzhufee;
        this.dismissCountDown = dismissCountDown;
      }
    },
    showAlert() {
      // 获取省市区街的值
      // this.province.value
      // this.city.value
      // this.district.value
      // this.street.value

      if (this.area && this.selected) {
        this.dismissCountDown = this.dismissSecs;
        console.log();
      } else if (this.value == "") {
        this.message = "请选择征地范围！";
        this.showDismissibleAlert = true;
      } else if (this.selected == "") {
        this.message = "请选择土地类型！";
        this.showDismissibleAlert = true;
      } else if (this.area == "") {
        this.message = "要记得填面积！";
        this.showDismissibleAlert = true;
      } else {
        this.message = "啥都没填，算不出来？";
        this.showDismissibleAlert = true;
      }
    },
    handleChange(value) {
      console.log(value);
    },
    onReset(evt) {
      evt.preventDefault();
      // Reset our form values
      this.value2 = "";
      this.area = "";
      this.selected = null;
    },
  },
};
</script>

<style scoped>
.el-cascader {
  width: 100%;
}

.el-select {
  width: 100%;
}

.el-select + .el-select {
  margin-top: 16px;
}
</style>
