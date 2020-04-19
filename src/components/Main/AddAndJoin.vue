<template>
    <div style="height: 30px;width: 200px;float: bottom;position: absolute;top:660px;margin-left: 20px;">
        <span style="color: #ffffff;float: right;font-size: 25px">
            <el-tooltip class="item" effect="dark" content="创建组群" placement="bottom-start">
                <i class="el-icon-circle-plus-outline" @click="addGroup"></i>
            </el-tooltip>
        </span>
        <span style="color: #ffffff;float: right;margin-right:20px;font-size: 25px">
             <el-tooltip class="item" effect="dark" content="加入组群" placement="bottom-start">
                <i class="el-icon-position" @click="joinGroup"></i>
            </el-tooltip>
        </span>

        <el-dialog title="创建群组" :visible.sync="dialogFormVisible" width="300px">
            <el-form :model="form">
                <el-form-item label="群组名称:">
                    <el-input v-model="form.name" autocomplete="off"></el-input>
                </el-form-item>
            </el-form>
            <el-form :model="form">
                <el-form-item label="群组ID:">
                    <el-input v-model="form.id" autocomplete="off"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="createGroup">确 定</el-button>
            </div>
        </el-dialog>

    </div>
</template>

<script>
    import Vue from 'vue'
    // eslint-disable-next-line no-unused-vars
    import TIM from 'tim-js-sdk';
    import {EventBus} from "../../tools/EventBus";
    export default {
        name: "AddAndJion",
        data(){
            return{
                dialogFormVisible:false,
                form:{name:'',
                id:''}
            }
        },
        methods:{
            addGroup:function () {
                this.dialogFormVisible=true;
            },
            joinGroup:function () {
                var that=this;
                this.$prompt('请组群ID', '加入组群', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                }).then(({ value }) => {
                    let promise =  Vue.prototype.tim.joinGroup({ groupID: value, type: TIM.TYPES.GRP_PUBLIC });
                    promise.then(function(imResponse) {
                        switch (imResponse.data.status) {
                            case TIM.TYPES.JOIN_STATUS_WAIT_APPROVAL: break; // 等待管理员同意
                            case TIM.TYPES.JOIN_STATUS_SUCCESS: // 加群成功
                                console.log('加群成功！');
                                console.log(imResponse.data.group); // 加入的群组资料
                                that.$message({
                                    type: 'success',
                                    message: '成功加入组群 ' + value
                                });
                                EventBus.$emit('joinGroup');
                                break;
                            default: break;
                        }
                    }).catch(function(imError){
                        that.$message.error({
                            message: '加群失败 ' + imError
                        });
                        console.warn('joinGroup error:', imError); // 申请加群失败的相关信息
                    });
                }).catch(() => {
                    this.$message({
                        type: 'info',
                        message: '取消输入'
                    });
                });
            },
            createGroup:function () {
                var that=this;
                let promise = Vue.prototype.tim.createGroup({
                    type: TIM.TYPES.GRP_PUBLIC,
                    joinOption:TIM.TYPES.JOIN_OPTIONS_FREE_ACCESS,
                    groupID:that.form.id,
                    name: that.form.name,
                });
                promise.then(function(imResponse) { // 创建成功
                    that.$message({
                        type: 'success',
                        message: '创建成功！'
                    });
                    console.log(imResponse.data.group); // 创建的群的资料
                    var msg={
                        id:'GROUP'+that.form.id,
                        name:that.form.name
                    }
                    EventBus.$emit('createGroup',msg);
                    that.dialogFormVisible=false;
                }).catch(function(imError) {
                    that.$message.error({
                        message: '创建失败 ' + imError
                    });
                    console.warn('createGroup error:', imError); // 创建群组失败的相关信息
                });
            }
        }
    }
</script>

<style scoped>

</style>
