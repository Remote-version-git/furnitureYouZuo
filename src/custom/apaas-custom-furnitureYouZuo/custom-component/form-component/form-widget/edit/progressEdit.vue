<template>
    <div class="progress-edit">
        <el-steps :active="activeIndex">
            <el-step v-for="item, i in nodes" :key="i" :title="item.title" :description="item.desc">
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
    data() {
        return {
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
        }
    }
}
</script>

<style lang="scss">
.progress-edit {
    margin: 227px 112px 85px;

    .el-step__icon {
        position: absolute;
        top: -5px;
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

    .el-step__title.is-finish,
    .el-step__description.is-finish {
        font-family: 'PingFangSC-Regular';
        color: #333333;
        font-size: 12px;
        font-weight: normal;
    }

    .el-step__title.is-process,
    .el-step__description.is-process {
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