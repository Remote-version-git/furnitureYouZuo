<template>
    <div class="progress-edit">
        <!-- 生产订单步骤条  -->
        <el-steps ref="order" :style="orderStyle" direction="vertical" v-if="orders" class="order-progress"
            :active="orderActive">
            <el-step v-for="item, i in orders" :key="i" :title="item.title" :description="item.desc">
                <template #icon>
                    <!-- 待办节点图标 -->
                    <svg v-if="item.status === 'unactive'" xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink" fill="none" version="1.1" width="16" height="16"
                        viewBox="0 0 16 16">
                        <g style="mix-blend-mode:passthrough">
                            <g style="mix-blend-mode:passthrough">
                                <rect x="0" y="0" width="16" height="16" rx="8" fill="#FFFFFF" fill-opacity="1" />
                                <rect x="0.5" y="0.5" width="15" height="15" rx="7.5" fill-opacity="0"
                                    stroke-opacity="1" stroke="#E0E0E0" fill="none" stroke-width="1" />
                            </g>
                            <g style="mix-blend-mode:passthrough">
                                <path
                                    d="M7.81771,4.86133C7.580080000000001,5.12175,7.27734,5.34961,6.91927,5.57096C6.55794,5.76953,6.229167,5.90951,5.906901,5.99089L5.906901,7.07161C6.56771,6.87956,7.11784,6.58008,7.54753,6.16992L7.54753,12L8.638020000000001,12L8.638020000000001,4.86133L7.81771,4.86133Z"
                                    fill="#999999" fill-opacity="1" />
                            </g>
                        </g>
                    </svg>
                    <!-- 当前步骤图标 -->
                    <svg v-if="item.status === 'active'" xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink" fill="none" version="1.1" width="16" height="16"
                        viewBox="0 0 16 16">
                        <g style="mix-blend-mode:passthrough">
                            <g style="mix-blend-mode:passthrough">
                                <rect x="0" y="0" width="16" height="16" rx="8" fill="#3370FF" fill-opacity="1" />
                                <rect x="0.5" y="0.5" width="15" height="15" rx="7.5" fill-opacity="0"
                                    stroke-opacity="1" stroke="#3370FF" fill="none" stroke-width="1" />
                            </g>
                            <g style="mix-blend-mode:passthrough">
                                <path
                                    d="M8.10872,4.72135C7.34701,4.72135,6.72852,4.97201,6.26953,5.48958C5.817057,5.97135,5.589193,6.61263,5.579427,7.41016L6.66992,7.41016C6.68945,6.83073,6.8099,6.40104,7.04753,6.10156C7.27865,5.79232,7.61719,5.64258,8.069659999999999,5.64258C8.48958,5.64258,8.81836,5.74023,9.03971,5.94206C9.25781,6.14062,9.368490000000001,6.4401,9.368490000000001,6.8405C9.368490000000001,7.26042,9.19922,7.65104,8.88021,8.01237C8.67839,8.2207,8.33984,8.49089,7.85807,8.83268C7.04753,9.39258,6.50716,9.82227,6.25,10.1315C5.768229,10.6719,5.537109,11.2904,5.537109,12L10.45898,12L10.45898,11.043L6.88802,11.043C7.0377600000000005,10.6523,7.5,10.1999,8.268229999999999,9.69206C8.97786,9.21029,9.4694,8.83268,9.74935,8.53971C10.20833,8.0319,10.44922,7.47201,10.44922,6.85026C10.44922,6.22201,10.22786,5.71094,9.79818,5.31055C9.35872,4.91016,8.78906,4.72135,8.10872,4.72135Z"
                                    fill="#FFFFFF" fill-opacity="1" />
                            </g>
                        </g>
                    </svg>
                    <!-- 已完成状态图标 -->
                    <svg v-if="item.status === 'complted'" xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink" fill="none" version="1.1" width="16" height="16"
                        viewBox="0 0 16 16">
                        <g style="mix-blend-mode:passthrough">
                            <g style="mix-blend-mode:passthrough">
                                <rect x="0" y="0" width="16" height="16" rx="8" fill="#FFFFFF" fill-opacity="1" />
                                <rect x="0.5" y="0.5" width="15" height="15" rx="7.5" fill-opacity="0"
                                    stroke-opacity="1" stroke="#3370FF" fill="none" stroke-width="1" />
                            </g>
                            <g style="mix-blend-mode:passthrough">
                                <path
                                    d="M7.81771,4.86133C7.580080000000001,5.12175,7.27734,5.34961,6.91927,5.57096C6.55794,5.76953,6.229167,5.90951,5.906901,5.99089L5.906901,7.07161C6.56771,6.87956,7.11784,6.58008,7.54753,6.16992L7.54753,12L8.638020000000001,12L8.638020000000001,4.86133L7.81771,4.86133Z"
                                    fill="#3370FF" fill-opacity="1" />
                            </g>
                        </g>
                    </svg>
                </template>
            </el-step>
        </el-steps>
        <el-steps :active="activeIndex">
            <el-step :ref="item.title" v-for="item, i in nodes" :key="i" :title="item.title" :description="item.desc">
                <template #icon>
                    <!-- 待办节点图标 -->
                    <svg v-if="item.status === 'unactive'" xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink" fill="none" version="1.1" width="8" height="8"
                        viewBox="0 0 8 8">
                        <g style="mix-blend-mode:passthrough">
                            <ellipse cx="4" cy="4" rx="4" ry="4" fill="#E0E0E0" fill-opacity="1" />
                        </g>
                    </svg>
                    <!-- 当前步骤图标 -->
                    <svg v-if="item.status === 'active'" xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink" fill="none" version="1.1" width="12" height="12"
                        viewBox="0 0 12 12">
                        <g style="mix-blend-mode:passthrough">
                            <ellipse cx="6" cy="6" rx="6" ry="6" fill="#3370FF" fill-opacity="1" />
                        </g>
                    </svg>
                    <!-- 已完成状态图标 -->
                    <svg v-if="item.status === 'complted'" xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink" fill="none" version="1.1" width="8" height="8"
                        viewBox="0 0 8 8">
                        <g style="mix-blend-mode:passthrough">
                            <ellipse cx="4" cy="4" rx="4" ry="4" fill="#3370FF" fill-opacity="1" />
                        </g>
                    </svg>
                </template>
            </el-step>
        </el-steps>
    </div>
</template>

<script>
export default {
    name: 'ProgressEdit',
    mounted() {
        this.$nextTick(() => {
            const x = this.$refs['生产'][0].$el.offsetLeft;

            const h = this.$refs.order.$el.offsetHeight;

            this.orderStyle = {
                position: 'absolute',
                top: -h + 50 + 'px',
                left: x - 6 + 'px'
            }

        })
    },
    data() {
        return {
            orderStyle: {},
            orders: [
                {
                    title: '生产订单：SC20221019001',
                    status: 'complted',
                },
                {
                    title: '生产订单：SC20221019001',
                    status: 'active',
                },
                {
                    title: '生产订单：SC20221019001',
                    status: 'unactive',
                },
                {
                    title: '生产订单：SC20221019001',
                    status: 'unactive',
                }
            ],
            nodes: [
                {
                    title: '订单创建',
                    desc: '订单创建',
                    status: 'complted',
                },
                {
                    title: '提交',
                    desc: '业务员提交',
                    status: 'complted',
                },
                {
                    title: '审核',
                    desc: '领导审核',
                    status: 'complted',
                },
                {
                    title: '生产',
                    desc: '订单生产',
                    status: 'active',
                },
                {
                    title: '入库',
                    desc: '订单入库',
                    status: 'unactive',
                },
                {
                    title: '出货',
                    desc: '订单出货',
                    status: 'unactive',
                },
                {
                    title: '出货单',
                    desc: '生成出货单',
                    status: 'unactive',
                },
                {
                    title: '收款',
                    desc: '订单收款',
                    status: 'unactive',
                },
            ]
        }
    },
    computed: {
        activeIndex() {
            // 从1开始
            return this.nodes.findIndex(n => n.status === 'active') + 1;
        },
        orderActive() {
            return this.orders.findIndex(n => n.status === 'active') + 1;
        }
    }
}
</script>

<style lang="scss">
.progress-edit {
    margin: 227px 112px 85px;
    position: relative;

    .order-progress {

        .el-step.is-vertical .el-step__main {
            position: relative;
            top: -7px;
        }

    }

    .el-step__icon {
        position: absolute;
        top: -5px;
    }

    .el-step.is-vertical {
        .el-step__icon {
            position: absolute;
            top: 0;
            left: 4px;
        }

        .el-step__title.is-finish,
        .el-step__description.is-finish {
            font-family: 'PingFangSC-Medium';
            color: #333333;
            font-size: 14px;
            font-weight: normal;
        }

        .el-step__title.is-process,
        .el-step__description.is-process {
            font-family: 'PingFangSC-Medium';
            color: #999999;
            font-size: 14px;
            font-weight: normal;
        }

        .el-step__title.is-wait,
        .el-step__description.is-wait {
            font-family: 'PingFangSC-Medium';
            color: #999999;
            font-size: 14px;
            font-weight: normal;
        }
    }

    .el-step__icon {
        width: auto;
        height: 12px;
    }

    .el-step__line {
        background: #E0E0E0;
    }

    .el-step__head.is-finish {
        border-color: #3370FF;
        color: #333333;
    }



    .el-step__title.is-finish {
        font-family: 'PingFangSC-Regular';
        color: #333333;
        font-size: 12px;
        font-weight: normal;
    }

    .el-step__description.is-finish {
        font-family: PingFangSC-Regular;
        font-size: 12px;
        font-weight: normal;
        /* 中性色/G8-次要文本 */
        color: #666666;
    }

    .el-step__title.is-process,
    .el-step__description.is-process {
        font-family: 'PingFangSC-Regular';
        color: #999999;
        font-size: 12px;
        font-weight: normal;
    }

    .el-step__title.is-wait,
    .el-step__description.is-wait {
        font-family: 'PingFangSC-Regular';
        color: #999999;
        font-size: 12px;
        font-weight: normal;
    }

    .el-step.is-horizontal .el-step__line {
        top: 0;
        height: 1px;
    }

    .el-step__icon {
        border: none;
        background: none;
    }
}
</style>