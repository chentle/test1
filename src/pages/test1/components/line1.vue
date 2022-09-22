<template>
    <t-space direction="vertical" size="32px">
        <t-form
            :data="formData"
            labelWidth="calc(2em + 128px)"
            :layout="formData.layout"
            ref="form"
            @reset="onReset"
            @submit="onSubmit"
            scrollToFirstError="smooth"
        >
            <t-form-item
                label="纳税人识别号"
                name="id"
                :rules="[{ required: true, message: '纳税人识别号不可为空' }, { validator: this.nameValidator }]"
            >
                <t-input v-model="formData.id" clearable placeholder="格式: 001xxx"></t-input>
            </t-form-item>
            <t-form-item label="纳税人名称" name="name">
                <t-input v-model="formData.name" :disabled="formDisabled" placeholder=""></t-input>
            </t-form-item>
            <t-form-item label="登记注册类型" name="type">
                <t-select v-model="formData.type" :options="COLLEGE_OPTIONS" clearable></t-select>
            </t-form-item>
            <t-form-item label="主管税务机关" name="government">
                <t-input v-model="formData.government" :disabled="formDisabled" placeholder=""></t-input>
            </t-form-item>
            <t-form-item label="纳税人生产经营地址" name="address">
                <t-input v-model="formData.address" :disabled="formDisabled" placeholder=""></t-input>
            </t-form-item>
            <t-form-item :statusIcon="false" style="margin-left: 80px">
                <t-space size="10px">
                    <t-button theme="primary" type="submit">查询</t-button>
                    <t-button theme="default" variant="base" type="reset">重置</t-button>
                </t-space>
            </t-form-item>
        </t-form>
    </t-space>
</template>
<script>
const INITIAL_DATA = {
    layout: "vertical",
    id: "",
    name: "",
    type: 1,
    government: "",
    address: ""
}
const COLLEGE_OPTIONS = [
    { label: "类型 A", value: 1 },
    { label: "类型 B", value: 2 },
    { label: "类型 C", value: 3 }
]

// const state = []
const mark = ""

export default {
    props:[
        'list'
    ],
    data() {
        return {
            formData: { ...INITIAL_DATA },
            formDisabled: true,
            COLLEGE_OPTIONS,
            dataLoading: false
        }
    },
    mounted() {
        console.log("rowIIIId",this.$props)
    },
    methods: {
        onReset() {
            this.$message.success("重置成功")
        },
        onSubmit({ validateResult, firstError }) {
            const { $props:{list}, mark } = this
            if (validateResult === true) {
                const result = list.filter((i) => i.id === mark)
                if (result.length){
                    console.log(result)
                    const {name,type,government,id,address} = result[0]
                    this.formData = {
                        layout:"vertical",
                        id,
                        name,
                        type,
                        government,
                        address
                    }
                    // console.log(result)
                    this.$message.success("查询成功")
                }
                else  this.$message.warning("无结果")
            } else {
                console.log("Errors: ", validateResult)
                this.$message.warning(firstError)
            }
        },
        nameValidator(val) {
            // eslint-disable-next-line no-restricted-globals
            if (isNaN(val * 1)) return { result: false, message: "纳税人识别号应为数字组合！", type: "error" }
            if (val.length < 5) return { result: false, message: "纳税人识别号应为大于4位数的组合", type: "error" }
            this.mark = val
            return { result: true, message: "c成功", type: "succeed" }
            // if () return { result: false, message: "识别号长度错误！", type: "error" }
        }
    }
}
</script>
<style lang="scss" scoped></style>
