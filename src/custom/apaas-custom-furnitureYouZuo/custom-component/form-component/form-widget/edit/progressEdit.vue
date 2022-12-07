<template>
    <div class="progress-edit">
        <!-- 生产订单步骤条  -->
        <div class="time_line_box">
            <div class="time_line" style="100%;">
                <ol>
                    <li v-for="(item,index) in nodes">
                        <div class="order_item" :style="getLeft(index)" :class="{'finished':item.status=='completed','selected':item.status=='active'}">
                            <p @click="showDetail(item)" class="title" :class="{'len':item.length}" v-if="item.title">{{item.title}} <span class="tip" v-if="item.length">{{item.length}}</span></p>
                            <p>{{item.statusName}}</p>
                            <p>{{item.desc}}</p>
                            
                        </div>
                    </li>
                </ol>
                <span class="filling_line" style="transform: scaleX(0.28);"></span>
            </div>
        </div>
        <el-dialog  :visible="open" @close="open = false">
            <div style="max-height:60vh;overflow:auto">
                <el-steps :active="currentList.length" direction="vertical" >
                    <el-step active v-for="(it,idx) in currentList" :title="it.nodeName+'('+it.lineCode+')'" :key="idx" >
                            <template slot="description">
                                <p>{{it.nodeDate}}</p>
                                <p>{{it.status}}</p>
                            </template>
                    </el-step>
                </el-steps>
            </div>
            
        </el-dialog>
    </div>
</template>

<script>
export default {
    name: 'ProgressEdit',
    props:['orderCode','url',],
    mounted() {
    },
    created(){
        // this.url = 'http://localhost:9091';
        // this.orderCode = '147';
        this.$request({
            method: 'get',
            url: this.url+'/custom/crm/getOrderCurrentProgress',
            params: {
                orderCode: this.orderCode
            }
        })
        .asyncThen(res => {
                if (res.data) {
                    var list = [];
                    res.data.forEach((item,index)=>{
                        if(this.finishStatus.indexOf(item.status)>-1){
                            item.checkStatus = 1;
                        }
                        else{
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
                    res.data.forEach((item,index)=>{
                        if(this.finishStatus.indexOf(item.status)>-1){
                            item.checkStatus = 'completed';
                        }
                        else if(this.waitStatus.indexOf(item.status)>-1){
                            item.checkStatus = 'active';
                        }
                        else{
                            item.checkStatus = 'unactive';
                        }
                        let len = 0;
                        if(item.list){
                            len = item.list.length;
                        }
                        var node = {
                            title: item.nodeName,
                            desc: item.nodeDate,
                            status: item.checkStatus,
                            statusName:item.status,
                            children:item.list,
                            length:len
                        }
                        this.nodes.push(node);
                    });
                    

                    
                }
            })
    },
    data() {
        return {
            finishStatus:["审核","自然关闭","短缺关闭","已完成","已核销","月结客户","完工","已核准","已认款"],
            waitStatus:['审核中','进行中','部分认款','部分核销'],
            orderStyle: {},
            orders: [

            ],
            currentList:[],
            nodes: [],
            open:false
        }
    },
    methods:{
        showDetail:function(item){           
            if(item.children && item.children.length>0){
                this.open = true;
                this.currentList = item.children;
            }
         },
         handleClose(){
            this.open = false;
            this.currentList = [];
         }
    },
    
    computed: {
        getLeft(){
            return function(index){
                let it = 100/this.nodes.length;
                return 'left:'+(it*index+5)+'%';
            }
        }
    }
}
</script>

<style lang="scss">
.progress-edit {
    position: relative;
    margin: 5%;
    li{
        list-style:none;
    }
    .time_line_box{
        position: relative;
        height: 60px;
        // overflow: hidden;
    }
    .time_line{
        width:100%;
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
    .order_item{
        position: absolute;
        top: 0;
        width:10%;
        z-index: 2;
        text-align: center;
        font-size: 13px;
        padding-top: 15px;
        color: #027AFF;
        .len{
            cursor:pointer;
        }
        .title{
            font-weight:bolder;
            position:relative;
            .tip{
                position:absolute;
                color:white;
                font-size:6px;
                font-weight:bolder;
                border:1rpx solid #999999;
                background:#027AFF;
                width:10px;
                height:10px;
                line-height:10px;
                border-radius:50%;
            }
        }
    }
    .order_item:after{
        content: '';
        position: absolute;
        left: 50%;
        right: auto;
        transform: translateX(-50%);
        top: -10px;
        height: 20px;
        width:20px;
        border-radius: 50%;
        border: 2px solid #dfdfdf;
        background-color: #f8f8f8;
        align:center;
        line-height:20px;
        font-weight:bolder;
    }
    .finished:after{
        content: '✓';
        border-color: #027AFF;

    }
    .selected:after{
        background-color: #027AFF;
        border-color: #027AFF;
    }
    .filling_line{
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
    .time_tip{
        height: 100px;
        line-height: 70px;
        text-align: center;
        color: #151BFD;
        border-bottom: 1px solid #ddd;
    }
}
</style>