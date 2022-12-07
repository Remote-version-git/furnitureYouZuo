
<template>
    <div class="furniture-step">
        <div class="furniture-rect">
            <h3 class="title" v-if="typeof title === 'string' && title.length > 0">{{ title }}</h3>
            <slot v-else name="title"></slot>
            <slot name="desc"></slot>
        </div>
        <div v-if="!hiddenLine" class="line">
            <svg class="arrow" t="1670425717467" viewBox="0 0 1024 1024" version="1.1"
                xmlns="http://www.w3.org/2000/svg" p-id="2674" width="12" height="12">
                <path d="M65.582671 288.791335l446.417329 446.41733 446.417329-446.41733z" p-id="2675"></path>
            </svg>
        </div>
        <div class="cross-line" v-if="childData">
            <div class="horizontal-line">
                <div class="vertical-top-line">
                    <div class="horizontal-line">
                        <svg t="1670427842011" class="right-arrow" viewBox="0 0 1024 1024" version="1.1"
                            xmlns="http://www.w3.org/2000/svg" p-id="3587" width="12" height="12">
                            <path d="M288.791335 65.582671l446.41733 446.417329-446.41733 446.417329z" p-id="3588">
                            </path>
                        </svg>
                        <div class="child-rect">
                            <div class="furniture-rect horizontal">
                                <h3 class="title">生产订单</h3>
                                <div class="desc">
                                    <p>审核时间（最新单）：</p>
                                    <p>负责人：小王王、小丽丽、精一西庆-李开良</p>
                                </div>
                                <div class="horizontal-line">
                                    <svg t="1670427842011" class="right-arrow" viewBox="0 0 1024 1024" version="1.1"
                                        xmlns="http://www.w3.org/2000/svg" p-id="3587" width="12" height="12">
                                        <path d="M288.791335 65.582671l446.41733 446.417329-446.41733 446.417329z"
                                            p-id="3588">
                                        </path>
                                    </svg>
                                </div>
                            </div>
                            <div class="furniture-rect horizontal">
                                <h3 class="title">生产入库</h3>
                                <div class="desc">
                                    <p>审核时间（最新单）：</p>
                                    <p>负责人：小王王、小丽丽</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="vertical-bottom-line">
                    <div class="horizontal-line">
                        <svg t="1670427842011" class="right-arrow" viewBox="0 0 1024 1024" version="1.1"
                            xmlns="http://www.w3.org/2000/svg" p-id="3587" width="12" height="12">
                            <path d="M288.791335 65.582671l446.41733 446.417329-446.41733 446.417329z" p-id="3588">
                            </path>
                        </svg>
                        <div class="child-rect">
                            <div class="furniture-rect horizontal">
                                <h3 class="title">采购订单</h3>
                                <div class="desc">
                                    <p>审核时间（最新单）：</p>
                                    <p>负责人：小王王、小丽丽</p>
                                </div>
                                <div class="horizontal-line">
                                    <svg t="1670427842011" class="right-arrow" viewBox="0 0 1024 1024" version="1.1"
                                        xmlns="http://www.w3.org/2000/svg" p-id="3587" width="12" height="12">
                                        <path d="M288.791335 65.582671l446.41733 446.417329-446.41733 446.417329z"
                                            p-id="3588">
                                        </path>
                                    </svg>
                                </div>
                            </div>
                            <div class="furniture-rect horizontal">
                                <h3 class="title">采购入库</h3>
                                <div class="desc">
                                    <p>审核时间（最新单）：</p>
                                    <p>负责人：小王王、小丽丽</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'FurnitureStep',
    props: {
        title: {
            type: String,
            default: ''
        },
        hiddenLine: {
            type: Boolean,
            default: false
        },
        childData: {
            type: Object,
            required: false
            // default() {
            //     return {
            //         products: [],
            //         purchases: []
            //     }
            // }
        }
    }
}
</script>

<style lang="scss" scoped>
.furniture-step {
    position: relative;
    width: 160px;
    font-size: 12px;

    .furniture-rect {
        padding: 10px 6px;
        color: #ffffff;
        background: #83a59c;
        border: 1px solid #829691;
        border-radius: 10px;

        .title {
            font-size: 16px;
            text-align: center;
        }

        &.horizontal {
            width: 120px;
            padding: 5px 3px;

            .title {
                font-size: 14px;
            }
        }

    }




    .line {
        position: relative;
        left: 50%;
        width: 1px;
        min-height: 20px;
        margin-bottom: 6px;
        background: #686868;


        .arrow {
            position: absolute;
            left: -6px;
            bottom: -9px;
        }
    }

    .cross-line {
        position: absolute;
        top: 50%;
        right: -30px;
    }

    .horizontal-line {
        position: relative;
        width: 30px;
        height: 1px;
        background: #686868;

        .right-arrow {
            position: absolute;
            top: -6px;
            right: -9px;
        }
    }

    .vertical-top-line {
        position: absolute;
        top: -70px;
        right: 0;
        width: 1px;
        height: 70px;
        background: #686868;
    }

    .vertical-bottom-line {
        position: absolute;
        bottom: -70px;
        right: 0;
        width: 1px;
        height: 70px;
        background: #686868;

        .horizontal-line {
            position: relative;
            top: 100%;
            width: 30px;
            height: 1px;
            background: #686868;

            .right-arrow {
                position: absolute;
                top: -6px;
                right: -9px;
            }
        }

    }

    .child-rect {
        position: absolute;
        left: 130%;
        top: -43px;
        display: flex;

        .horizontal-line {
            position: absolute;
            right: -30px;
            top: 50%;
        }

        .furniture-rect {
            position: relative;
            margin-right: 35px;
        }
    }
}
</style>