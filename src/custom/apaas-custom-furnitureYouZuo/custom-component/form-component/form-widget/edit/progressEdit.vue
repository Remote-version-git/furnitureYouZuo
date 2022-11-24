<template>
    <div>
        <el-steps :active="3" finish-status="success">
            <el-step title="创建" description="订单创建"></el-step>
            <el-step title="提交" description="业务员提交"></el-step>
            <el-step title="审核" description="领导审核"></el-step>
            <el-step title="生产" description="订单生产"></el-step>
            <el-step title="入库" description="订单入库"></el-step>
            <el-step title="出货" description="订单出货"></el-step>
            <el-step title="出货单" description="生成出货单"></el-step>
            <el-step title="收款" description="订单收款"></el-step>
        </el-steps>
    </div>
</template>

<script>
export default {
    name: 'ProgressEdit'
}
</script>

<style>

</style>