<template>
    <div>
        <el-form ref="form" :model="item" label-width="0px">
            <slot name="form"></slot>
            <el-form-item>
                <el-button-group>
                <el-button type="primary" @click="save">保存</el-button>
                <el-button @click="fill">填充</el-button>
                </el-button-group>
            </el-form-item>          
        </el-form>
    </div>   
</template>
<script>
export default {
    props:{
        form:{
            type:Object,
        },
    },
    data(){
        return {
            item:{}
        }
    },
    methods:{
        save(){
            this.$emit('save')
        },
        fill(){
            this.$emit('fill')
        },

    }
}
</script>
<style>

</style>


