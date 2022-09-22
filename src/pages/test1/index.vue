<template>
    <div class="tdesign-demo-block-column tdesign-demo__table" style="width: 100%">
        <!-- :expanded-row-keys="expandedRowKeys" 通过keys指定默认会展开的行-->
        <t-table
            :row-key="data.id"
            :columns="columns"
            :data="data"
            :expanded-row="expandedRow"
            :expandIcon="expandIcon"
            :expandOnRowClick="expandOnRowClick"
            :horizontalScrollAffixedBottom="true"
            table-layout="auto"
            tableContentWidth="1200"
            @expand-change="rehandleExpandChange"
        >
        </t-table>
    </div>
</template>

<script lang="jsx">
import Line1 from "./components/line1.vue"

const getColumns = (isFixedColumn) => [{ colKey: "instance", title: " ", fixed: isFixedColumn ? "left" : "" }]

const data = [
    {
        id: 100,
        instance: "纳税人基本信息"
    },
    {
        id: 101,
        instance: "异常信息显示"
    },
    {
        id: 102,
        instance: "基本信息"
    },
    {
        id: 103,
        instance: "附送资料"
    }
]
// const list = []

export default {
    data() {
        return {
            expandIcon: true,
            expandOnRowClick: true,
            fixedColumns: false,
            emptyData: false,
            data,
            list:[],
            dataLoading: false,
            expandedRow: (h, { row }) => (row.id === 100 ? <Line1 list={this.list} /> : "")
        }
    },
    computed: {
        columns() {
            return getColumns(this.fixedColumns)
        }
    },
    mounted() {
        this.dataLoading = true
        this.$request
            .get("/api/get-info")
            .then((res) => {
                if (res.code === 0) {
                    const { list = [] } = res.data
                    console.log("succeed", list)
                    this.list = list
                }
            })
            .catch((e) => {
                console.log(e)
            })
            .finally(() => {
                this.dataLoading = false
            })
    },
    methods: {
        rehandleClickOp(data) {
            console.log("data", data)
        },
        rehandleExpandChange(value, params) {
            this.expandedRowKeys = value
            console.log("rehandleExpandChange", params)
        }
    }
}
</script>

<style lang="less" scoped>
/deep/ [class*="t-table-expandable-icon-cell"] .t-icon {
    background-color: transparent;
}
.link {
    cursor: pointer;
    margin-right: 15px;
}
.status {
    position: relative;
    color: #00a870;
    margin-left: 10px;
    &::before {
        position: absolute;
        top: 50%;
        left: 0;
        transform: translateY(-50%);
        content: "";
        background-color: #00a870;
        width: 6px;
        height: 6px;
        margin-left: -10px;
        border-radius: 50%;
    }
}
.status.unhealth {
    color: #e34d59;
    &::before {
        background-color: #e34d59;
    }
}
.more-detail {
    > p {
        display: inline-block;
        margin: 5px;
    }
    > p.title {
        width: 100px;
    }
}
</style>
