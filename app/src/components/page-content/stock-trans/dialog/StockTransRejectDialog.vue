<template>
    <div class="StockTransRejectDialog">
        <el-dialog
                :visible="visible"
                @update:visible="showDialog"
                width="300px"
                title="请填写驳回理由"
        >
            <el-form
                    label-position="top"
            >
                <el-form-item label="单号">
                    <el-input :value="form.number" :disabled="true"></el-input>
                </el-form-item>
                <el-form-item label="驳回理由" :required="true">
                    <el-input type="textarea" v-model="form.rejectRemark" :autosize="{ minRows: 4, maxRows: 10}"></el-input>
                </el-form-item>
            </el-form>
            <div class="">
                <el-button-submit
                        @click.native="submit"
                        :loading="loading"
                >提交驳回
                </el-button-submit>
            </div>
        </el-dialog>
    </div>
</template>

<script>
    import StockEntryModel from "@/project/model/StockEntryModel";
    import ElButtonSubmit from "@/components/common/button/ElButtonSubmit";
    import Api from "@/assets/api/Api";
    import DialogUtil from "@/util/DialogUtil";
    import StockTransModel from "@/project/model/StockTransModel";

    export default {
        name: "StockTransRejectDialog",
        components: {ElButtonSubmit},
        props: {
            visible: {
                default: false
            },
            stockTrans: {
                default() {
                    return new StockTransModel()
                }
            }
        },
        data() {
            return {
                form: Object.assign({}, this.stockTrans),
                loading: false,
            }
        },
        mounted() {
            this.reload();
        },
        methods: {
            reload() {
            },
            showDialog(v) {
                this.$emit('update:visible', v);
            },
            submit() {
                this.loading = true;
                this.form.status = 4;
                this.$ajax.request(Api.stockTrans.approve, this.form).then(resp => {
                    DialogUtil.toastSuccess(resp);
                    this.showDialog(false);
                    this.$emit('finish');
                }).finally(() => this.loading = false)
            }
        },
        watch: {
            visible(v) {
                if (v) {
                    this.form = Object.assign({}, this.stockTrans);
                }
            }
        },
        computed: {},

    }
</script>

<style lang="less" scoped>
    @import (reference) "~style/all.less";

    .StockTransRejectDialog {
    }
</style>