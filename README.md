# vue_form
vue element表单生成组件
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
