<template>
    <div class="progress-edit">
        <furniture-steps>
            <furniture-step :hiddenLine="flowData.length === (i + 1)" v-for="item, i in flowData" :key="i" :data="item"
                :title="item.nodeName"
                :childData="item.subFlowData && item.subFlowData.length ? { products: [], purchases: item.subFlowData && item.subFlowData.length ? item.subFlowData[0] : [] } : undefined">
                <template #desc="{ data }">
                    <p>提交时间：{{ data.nodeDate }}</p>
                    <p>{{ data.principal }}</p>
                </template>
            </furniture-step>
        </furniture-steps>
        <!-- 生产订单步骤条  -->
        <div class="time_line_box">
            <div class="time_line" style="100%;">
                <ol>
                    <li v-for="(item, index) in nodes">
                        <div class="order_item" :style="getLeft(index)"
                            :class="{ 'finished': item.status == 'completed', 'selected': item.status == 'active' }">
                            <p @click="showDetail(item)" class="title" :class="{ 'len': item.length }"
                                v-if="item.title">
                                {{ item.title }} <span class="tip" v-if="item.length">{{ item.length }}</span></p>
                            <p>{{ item.statusName }}</p>
                            <p>{{ item.desc }}</p>

                        </div>
                    </li>
                </ol>
                <span class="filling_line" style="transform: scaleX(0.28);"></span>
            </div>
        </div>
        <el-dialog :visible="open" @close="open = false">
            <div style="max-height:60vh;overflow:auto">
                <el-steps :active="currentList.length" direction="vertical">
                    <el-step active v-for="(it, idx) in currentList" :title="it.nodeName + '(' + it.lineCode + ')'"
                        :key="idx">
                        <template slot="description">
                            <p>{{ it.nodeDate }}</p>
                            <p>{{ it.status }}</p>
                        </template>
                    </el-step>
                </el-steps>
            </div>

        </el-dialog>
    </div>
</template>

<script>
import FurnitureSteps from './components/furniture-steps.vue';
import FurnitureStep from './components/furniture-step.vue';
const mock = {
    "code": "ok",
    "message": "获取成功",
    "data": [
        {
            "nodeName": "创建",
            "lineCode": null,
            "status": null,
            "nodeDate": "2022-08-19",
            "principal": "负责人：null",
            "dateDsc": "创建时间: ",
            "list": null,
            "subFlowData": null
        },
        {
            "nodeName": "提交",
            "lineCode": null,
            "status": null,
            "nodeDate": "2022-07-02",
            "principal": "负责人：null",
            "dateDsc": "提交时间：",
            "list": null,
            "subFlowData": null
        },
        {
            "nodeName": "审核情况",
            "lineCode": null,
            "status": "自然关闭",
            "nodeDate": "2022-08-22",
            "principal": "负责人：null",
            "dateDsc": "审核时间：",
            "list": [
                {
                    "nodeName": "销售订单行",
                    "lineCode": "10",
                    "status": "自然关闭",
                    "nodeDate": "2022-09-24",
                    "principal": null,
                    "dateDsc": null,
                    "list": null,
                    "subFlowData": null
                }
            ],
            "subFlowData": null
        },
        {
            "nodeName": "备产/采购",
            "lineCode": null,
            "status": null,
            "nodeDate": null,
            "principal": null,
            "dateDsc": null,
            "list": null,
            "subFlowData": [
                [
                    {
                        "nodeName": "采购订单",
                        "lineCode": null,
                        "status": "自然关闭",
                        "nodeDate": "2022-08-22",
                        "principal": "负责人：邓慧萍、欧认平",
                        "dateDsc": "审核时间：",
                        "list": [
                            {
                                "nodeName": "采购订单",
                                "lineCode": "61-PO22082200046",
                                "status": "已审核",
                                "nodeDate": "2022-08-22",
                                "principal": null,
                                "dateDsc": null,
                                "list": null,
                                "subFlowData": null
                            }
                        ],
                        "subFlowData": null
                    },
                    {
                        "nodeName": "采购入库",
                        "lineCode": null,
                        "status": "已完成",
                        "nodeDate": "2022-09-24",
                        "principal": "负责人：邓慧萍、欧认平",
                        "dateDsc": "审核时间：",
                        "list": [
                            {
                                "nodeName": "采购",
                                "lineCode": "61-PO22082200046",
                                "status": "业务关闭",
                                "nodeDate": "2022-09-24",
                                "principal": null,
                                "dateDsc": null,
                                "list": null,
                                "subFlowData": null
                            }
                        ],
                        "subFlowData": null
                    }
                ]
            ]
        },
        {
            "nodeName": "出货计划",
            "lineCode": null,
            "status": "全部认款",
            "nodeDate": "2022-09-24",
            "principal": "负责人：刘秀珠",
            "dateDsc": "审核时间：",
            "list": [
                {
                    "nodeName": "出货计划",
                    "lineCode": "61-SH2209240067",
                    "status": "全部认款",
                    "nodeDate": "2022-09-24",
                    "principal": null,
                    "dateDsc": null,
                    "list": null,
                    "subFlowData": null
                }
            ],
            "subFlowData": null
        },
        {
            "nodeName": "出货",
            "lineCode": null,
            "status": "已核准",
            "nodeDate": "2022-09-24",
            "principal": "负责人：精一西樵-程敏华、精一西庆-陈嘉敏",
            "dateDsc": "审核时间：",
            "list": [
                {
                    "nodeName": "出库单",
                    "lineCode": "61-SH2209240067",
                    "status": "已核准",
                    "nodeDate": "2022-09-24",
                    "principal": null,
                    "dateDsc": null,
                    "list": null,
                    "subFlowData": null
                }
            ],
            "subFlowData": null
        },
        {
            "nodeName": "应收",
            "lineCode": null,
            "status": "",
            "nodeDate": null,
            "principal": "负责人：李倩妮",
            "dateDsc": null,
            "list": [],
            "subFlowData": null
        },
        {
            "nodeName": "关闭",
            "lineCode": null,
            "status": "",
            "nodeDate": null,
            "principal": null,
            "dateDsc": null,
            "list": null,
            "subFlowData": null
        }
    ],
    "requestid": "b063c32ec057472297beea4559d17f97"
}
export default {
    name: 'ProgressEdit',
    props: ['orderCode', 'url',],
    components: {
        FurnitureSteps,
        FurnitureStep
    },
    created() {
        this.url = 'http://cscrm.sitzone.cn:30611';
        this.orderCode = '147';
        this.$request({
            method: 'get',
            url: this.url + '/custom/crm/getOrderCurrentProgress',
            params: {
                orderCode: this.orderCode
            }
        })
            .asyncThen(res => {
                if (res.data) {
                    this.flowData = res.data;
                    var list = [];
                    res.data.forEach((item, index) => {
                        if (this.finishStatus.indexOf(item.status) > -1) {
                            item.checkStatus = 1;
                        }
                        else {
                            item.checkStatus = 0;
                        }
                    })
                    let initNodes = [
                        {
                            title: '订单创建',
                            desc: '订单创建',
                            status: 'completed',
                        },
                        {
                            title: '提交',
                            desc: '业务员提交',
                            status: 'completed',
                        }];
                    this.nodes = initNodes
                    res.data.forEach((item, index) => {
                        if (this.finishStatus.indexOf(item.status) > -1) {
                            item.checkStatus = 'completed';
                        }
                        else if (this.waitStatus.indexOf(item.status) > -1) {
                            item.checkStatus = 'active';
                        }
                        else {
                            item.checkStatus = 'unactive';
                        }
                        let len = 0;
                        if (item.list) {
                            len = item.list.length;
                        }
                        var node = {
                            title: item.nodeName,
                            desc: item.nodeDate,
                            status: item.checkStatus,
                            statusName: item.status,
                            children: item.list,
                            length: len
                        }
                        this.nodes.push(node);
                    });



                }
            })
    },
    data() {
        return {
            flowData: [],
            finishStatus: ["审核", "自然关闭", "短缺关闭", "已完成", "已核销", "月结客户", "完工", "已核准", "已认款"],
            waitStatus: ['审核中', '进行中', '部分认款', '部分核销'],
            orderStyle: {},
            orders: [

            ],
            currentList: [],
            nodes: [],
            open: false
        }
    },
    methods: {
        showDetail: function (item) {
            if (item.children && item.children.length > 0) {
                this.open = true;
                this.currentList = item.children;
            }
        },
        handleClose() {
            this.open = false;
            this.currentList = [];
        }
    },

    computed: {
        getLeft() {
            return function (index) {
                let it = 100 / this.nodes.length;
                return 'left:' + (it * index + 5) + '%';
            }
        }
    }
}
</script>

<style lang="scss">
.progress-edit {
    position: relative;
    margin: 5%;

    li {
        list-style: none;
    }

    .time_line_box {
        position: relative;
        height: 60px;
        // overflow: hidden;
    }

    .time_line {
        width: 100%;
        position: absolute;
        z-index: 1;
        left: 0;
        top: 49px;
        height: 2px;
        background: #027AFF;
        -webkit-transition: -webkit-transform 0.4s;
        -moz-transition: -moz-transform 0.4s;
        transition: transform 0.4s;
    }

    .order_item {
        position: absolute;
        top: 0;
        width: 10%;
        z-index: 2;
        text-align: center;
        font-size: 13px;
        padding-top: 15px;
        color: #027AFF;

        .len {
            cursor: pointer;
        }

        .title {
            font-weight: bolder;
            position: relative;

            .tip {
                position: absolute;
                color: white;
                font-size: 6px;
                font-weight: bolder;
                border: 1rpx solid #999999;
                background: #027AFF;
                width: 10px;
                height: 10px;
                line-height: 10px;
                border-radius: 50%;
            }
        }
    }

    .order_item:after {
        content: '';
        position: absolute;
        left: 50%;
        right: auto;
        transform: translateX(-50%);
        top: -10px;
        height: 20px;
        width: 20px;
        border-radius: 50%;
        border: 2px solid #dfdfdf;
        background-color: #f8f8f8;
        align: center;
        line-height: 20px;
        font-weight: bolder;
    }

    .finished:after {
        content: '✓';
        border-color: #027AFF;

    }

    .selected:after {
        background-color: #027AFF;
        border-color: #027AFF;
    }

    .filling_line {
        position: absolute;
        z-index: 1;
        left: 0;
        top: 0;
        height: 100%;
        background-color: #027AFF;
        transform-origin: left center;
        transition-property: transform;
        transition-duration: 0.3s;
        transition-timing-function: initial;
        transition-delay: initial;
    }

    .time_tip {
        height: 100px;
        line-height: 70px;
        text-align: center;
        color: #151BFD;
        border-bottom: 1px solid #ddd;
    }
}
</style>