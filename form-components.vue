/** 表单组件 by 丘秋_JXT **/
/** 使用说明
1.引入该组件与element

2.属性  :formList  在父组件传入要渲染的表单列表
formList:[
    {
        label: '姓名',               //label名称
        type: 'input',              // 表单类型 【input、date、select、switch】
        key: 'name',                // 绑定的（v-model）data值名称
        disabled:"nameDisabled",    //是否禁用，值为对应的data名称，切换状态仅需修改对应的data值
        list:[{}],                  // type值为 【select】 时传入的绑定数据
        radio_label:'',             // type值为 【radio】 时传入的radio的label数据
        filter:'trimAll-upper'      // 对值进行过滤处理，可选值【trim（去除空格）】、【trimAll(去除全部空格)】、【lower(转小写)】 、【upper(转大写)】 ，可用 '-' 进行多个拼接
        ……                          //支持其它element的属性，当element的属性存在 ‘-’ 时，需改为 ‘_’
    }
]

属性  :formData.sync  传入接收数据的对象
object 类型 , key需与formList所对应，表单元素值改变 formData也将改变 :formData.sync="formData"
formData:{
    "name":"张三"
}

属性  :column 表单列数 （默认 4）

属性  :labelWidth  label宽度    （默认 100）

属性  :lowerSpacing 每行间距    （默认 15）


3.方法 @formChange 表单元素值发生变化时触发  返回 key值与val值
formChange(key, val) {
    console.log(key, val)
    console.log(this.formData)
},
**/
<template>
    <div class="form-components">
        <span :monitor="dynamicMonitoring()" v-show="false"></span>
        <div  class="start-quote" :style="{ marginBottom: lowerSpacing + 'px' }"  v-for="(item, index) in List" :key="index" >
            <div  class="start-quote-item" v-for="(item2, index2) in item" :key="index2">
                <template v-if="item2">
                    <div class="from-label" :style="{ width: labelWidth + 'px' }">{{item2.label}}</div>
                    <div class="from-item-content">
                        <!-- 文本框 -->
                        <template v-if="item2.type == 'input'">
                            <el-input
                                v-model="item2.data"
                                @input="val => formChange(item2,val)"
                                :disabled="obtainDisabled(item2.disabled)"
                                :placeholder="item2.placeholder ? item2.placeholder : '请填写' + item2.label"
                                :maxlength="item2.maxlength"
                                :minlength="item2.minlength"
                                :show-word-limit="item2.show_word_limit"
                                :clearable="item2.clearable ? item2.clearable : true"
                                :show-password="item2.show_password"
                                :prefix-icon="item2.prefix_icon"
                                :suffix-icon="item2.suffix_icon"
                                :readonly="item2.readonly"
                                :autofocus="item2.autofocus"
                                :validate-event="item2.validate_event">
                            </el-input>
                        </template>
                        <!-- 日期选择 -->
                        <template v-else-if="item2.type == 'date'">
                            <el-date-picker
                                @change="val => formChange(item2,val)"
                                v-model ="item2.date"
                                slot="registerDate"
                                type="date"
                                value-format="yyyy-MM-dd"
                                :disabled="obtainDisabled(item2.disabled)"
                                :placeholder="item2.placeholder ? item2.placeholder : '请选择' + item2.label"
                                :readonly="item2.readonly"
                                :editable="item2.editable"
                                :clearable="item2.clearable"
                                :start-placeholder="item2.start_placeholder"
                                :end-placeholder="item2.end_placeholder"
                                :format="item2.format"
                                :align="item2.align"
                                :default-value="item2.default_value"
                                :prefix-icon="item2.prefix_icon"
                                :clear-icon="item2.clear_icon"
                                :validate-event="item2.validate_event">
                            </el-date-picker>
                        </template>
                        <!-- 时间选择 -->
                        <template v-else-if="item2.type == 'time'">
                            <el-time-select
                                @change="val => formChange(item2,val)"
                                v-model ="item2.date"
                                :disabled="obtainDisabled(item2.disabled)"
                                :placeholder="item2.placeholder ? item2.placeholder : '请选择' + item2.label"
                                :readonly="item2.readonly"
                                :editable="item2.editable"
                                :clearable="item2.clearable"
                                :start-placeholder="item2.start_placeholder"
                                :end-placeholder="item2.end_placeholder"
                                :arrow-control="item2.arrow_control"
                                :align="item2.align"
                                :range-separator="item2.range_separator"
                                :value-format="item2.value_format"
                                :default-value="item2.default_value">
                            </el-time-select>
                        </template>
                        <!-- 下拉框 -->
                        <template v-else-if="item2.type == 'select'">
                            <el-select
                                :disabled="obtainDisabled(item2.disabled)"
                                :placeholder="item2.placeholder ? item2.placeholder : '请选择' + item2.label"
                                v-model="item2.data"
                                @change="val => formChange(item2,val)"
                                :multiple="item2.multiple"
                                :clearable="item2.clearable"
                                :collapse-tags="item2.collapse_tags"
                                :multiple-limit="item2.multiple_limit"
                                :filterable="item2.filterable">
                                    <el-option
                                        v-for="option in item2.list"
                                        :key="option.id"
                                        :label="option.name"
                                        :value="option.id">
                                    </el-option>
                            </el-select>
                        </template>
                        <!-- 开关 -->
                        <template v-else-if="item2.type == 'switch'">
                            <el-switch
                                v-model="item2.data"
                                :disabled="obtainDisabled(item2.disabled)"
                                @change="val => formChange(item2,val)"
                                :width="item2.width"
                                :active-icon-class="item2.active_icon_class"
                                :inactive-icon-class="item2.inactive_icon_class"
                                :active-text="item2.active_text"
                                :inactive-text="item2.inactive_text"
                                :active-value="item2.active_value"
                                :inactive-value="item2.inactive_value"
                                :active-color="item2.active_color"
                                :inactive-color="item2.inactive_color"
                                :validate-event="item2.validate_event">
                            </el-switch>
                        </template>
                        <!-- 单选框 -->
                        <template v-else-if="item2.type == 'radio'">
                            <el-radio
                                v-model="radio[item2.key]"
                                :disabled="obtainDisabled(item2.disabled)"
                                @change="val => formChange(item2,val)"
                                :label="item2.radio_label"
                                :border="item2.border"
                                :name="item2.name">
                            </el-radio>
                        </template>
                        <!-- 多选框 -->
                        <template v-else-if="item2.type == 'check'">
                            <el-checkbox
                                v-model="item2.data"
                                :disabled="obtainDisabled(item2.disabled)"
                                @change="val => formChange(item2,val)"
                                :label="item2.check_label"
                                :true-label	="item2.true_label"
                                :false-label="item2.false_label"
                                :border="item2.border"
                                :checked="item2.checked">
                            </el-checkbox>
                        </template>
                        <!-- 计数器 -->
                        <template v-else-if="item2.type == 'number'">
                            <el-input-number
                                v-model="item2.data"
                                :disabled="obtainDisabled(item2.disabled)"
                                @change="val => formChange(item2,val)"
                                :label="item2.check_label"
                                :min="item2.min"
                                :max="item2.max"
                                :step="item2.step"
                                :step-strictly="item2.step_strictly"
                                :precision="item2.precision"
                                :controls="item2.controls"
                                :controls-position="item2.controls_position"
                                :placeholder="item2.placeholder ? item2.placeholder : '请输入' + item2.label">
                            </el-input-number>
                        </template>
                    </div>
                </template>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'DialogAgent',
    props: {
        column: { //列数
            type: Number,
            default: 4
        },
        labelWidth: { // label宽度
            type: Number,
            default: 100
        },
        lowerSpacing: { //下间距
            type: Number,
            default: 15
        },
        formList: Array, //表单渲染列表
        formData: Object //表单绑定数据
    },
    data() {
        return {
            List: [],
            Data: {},
            radio: {}
        }
    },
    mounted() {

    },
    created() {
        this.format()
    },
    watch: {
        column() {
            this.format()
        }
    },
    methods: {
        //处理formData
        format() {
            this.defaultValue()
            this.List = this.formList
            const copyFormData = []
            let minArray = []
            let num = 0
            for (let i = 0; i < this.formList.length; i++) {
                minArray.push(this.formList[i])
                num++
                if (num == this.column) {
                    num = 0
                    copyFormData.push(minArray)
                    minArray = []
                }
            }
            if (minArray.length != 0) {
                for (let i = minArray.length; i < this.column; i++) {
                    minArray.push(null)
                }
                copyFormData.push(minArray)
            }
            this.List = copyFormData
        },
        //默认值绑定
        defaultValue() {
            this.Data = JSON.parse(JSON.stringify(this.formData))
            this.formList.forEach((item, index) => {
                if (this.Data[item.key]) {
                    this.$set(this.formList[index], 'data', this.Data[item.key])
                }
                else {
                    this.$set(this.formList[index], 'data', "")
                    this.$set(this.formData, item.key, "")
                }
                if (item.type == "radio") {
                    this.$set(this.radio, item.key, this.Data[item.key])
                }
            })
        },
        //动态改变值
        dynamicMonitoring() {
            this.List.forEach((item) => {
                if (item) {
                    item.forEach((item2) => {
                        if (item2 && this.formData[item2.key] && item2.data != this.formData[item2.key]) {
                            item2.data = this.formData[item2.key]
                            if (item2.type == "date" || item2.type == "time") {
                                item2.date = this.formData[item2.key]
                            }
                        }
                    })
                }
            })
        },
        //表单改变
        formChange(item, val) {
            const key = item.key
            if (item.filter) {
                const filters = item.filter.split("-")
                filters.forEach((filterItem) => {
                    val = item.data = this.filter(filterItem, item.data)
                })
            }
            this.formData[key] = val
            this.$emit('formChange', key, val)
        },
        //过滤处理
        filter(e, val) {
            if (e == "trim") {
                val = val.toString().trim()
            }
            else if (e == "trimAll") {
                val = val.replace(/\s+/g, "")
            }
            else if (e == "lower") {
                val = val.toLowerCase()
            }
            else if (e == "upper") {
                val = val.toUpperCase()
            }
            return val
        },
        //修改表单的值
        modifyValue(key, val) {
            if (key && val) {
                this.List.forEach((item) => {
                    if (item) {
                        item.forEach((item2) => {
                            if (item2 && item2.key == key) {
                                item2.data = val
                            }
                        })
                    }
                })
            }
        },
        //禁用响应
        obtainDisabled(key) {
            return this.$parent[key]
        }
    }
}
</script>
<style scoped lang="scss">
    .form-components{
        padding: 15px;
        .start-quote {
            width: 100%;
            display: -webkit-box;
            display: -ms-flexbox;
            display: flex;
            -webkit-box-orient: horizontal;
            -webkit-box-direction: normal;
            -ms-flex-direction: row;
            flex-direction: row;
            -ms-flex-wrap: wrap;
            flex-wrap: wrap;
            font-size: 12px;
            margin-bottom: 15px;
            .start-quote-item, .start-quote-item02 {
                float: left;
                -webkit-box-flex: 0;
                -ms-flex: 0 0 auto;
                flex: 1;
                display: flex;
                flex-wrap: wrap;
                flex-direction: row;
                font-size: 12px;
                align-items: center;
                .from-label {
                    text-align: right;
                    vertical-align: middle;
                    float: left;
                    font-size: 13px;
                    color: #4d5565;
                    line-height: 1;
                    padding: 10px 12px 10px 0;
                    -webkit-box-sizing: border-box;
                    box-sizing: border-box;
                    width: 100px;
                }
                .from-item-content {
                    flex: 1;
                    min-width: 120px;
                    position: relative;
                    line-height: 32px;
                    font-size: 12px;
                    display: flex;
                    .el-select{
                        width: 100%;
                    }
                    .el-date-editor{
                        width: 100%;
                    }
                }
            }
        }
        .start-quote:last-of-type{
            margin-bottom: 0px !important;
        }
    }
</style>


