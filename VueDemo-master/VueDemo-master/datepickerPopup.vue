<template>
    <dl class="clue-dl" style="padding:0 1.6rem;
">
        <dt class="clue-dt">
            <button :disabled="disabled" class="autopopup-button" @click="openPicker">{{activeText}}</button>
            <mt-date-picker ref="picker" type="date" v-model="dateValue" year-format="{value} 年" month-format="{value} 月" date-format="{value} 日" class="monthDate" @confirm="affirm" @cancel="cancelDate" :closeOnClickModal="false" :endDate="endDate" :startDate="startDate">
            </mt-date-picker>
        </dt>
        <span>—</span>
        <dd class="clue-dd">
            <button class="autopopup-button" @click="openPicker">{{activeText2}}</button>
            <mt-date-picker ref="picker" type="date" v-model="dateValue2" year-format="{value} 年" month-format="{value} 月" date-format="{value} 日" class="monthDate" @confirm="affirm" @cancel="cancelDate" :closeOnClickModal="false" :endDate="endDate" :startDate="startDate">
            </mt-date-picker>
        </dd>
    </dl>
</template>
<script>
    import {
        Popup,
        DatetimePicker
    } from 'mint-ui';
    export default {
        name: "datepickerpopup",
        props: {
            endDate:{
                type:Date,
                defaultData:new Date()
            },
            startDate:{
                type:Date,
                defaultData:new Date()
            },
            ifRequire: {
                type: Boolean,
                default: true
            },
            value: {
                //默认值
                type: String,
                default: ''
            },
            disabled: {
                //是否禁用
                type: Boolean,
                default: false
            },
            confirm: {
                //输入按键回调
                type: Function
            }
        },
        data() {
            return {
                show: false, //显示是否
                activeText: this.value ? this.value : '请选择',
                activeText2: this.value ? this.value : '请选择',
                dateValue: new Date(),
                dateValue2: null
            };
        },
        methods: {
            openPicker() {
                // console.log(new Date(this.value);
                this.dateValue = this.value ? new Date(new Date(this.value.slice(0, 4) + '/' + this.value.slice(5,7) + '/' + this.value.slice(8)).getTime()) : ''
                this.$refs.picker.open();
            },
            openPicker2(){
                this.$refs.picker.open();
            },
            cancel() {
                this.show = false;
            },
            affirm(value) {
                this.activeText = this.formatTimeToStr(new Date(value), 'yyyy/MM/dd')
                this.confirm(value);
            },
            // affirm(value) {
            //     this.activeText2 = this.formatTimeToStr(new Date(value), 'yyyy/MM/dd')
            //     this.confirm(value);
            //     this.getData();
            // },
            cancelDate() {},
            getData(){
                console.log('dodo')
            }
        },
        components: {
            mtPopup: Popup,
            mtDatePicker: DatetimePicker
        },
    };
</script>
<style scoped lang="less">
    @color :#509ff7;
    @font: 14px;
    .clue-section .clue-dl {
        display: flex;
        display: -webkit-box;
        display: -webkit-flex;
        align-items: center;
        justify-content: space-between;
        overflow: hidden;
        padding: .2rem;
        height: .99rem;
        border-bottom: 1px solid #ddd;
        border-left: 1px solid #fff;
        border-top: 1px solid #fff;
        border-right: 1px solid #fff;
        color: #000;
        background: #fff;
        box-sizing: border-box;
    }
    .clue-section .clue-dt {
        min-width: 25%;
        text-align: left;
    }
    .autopopup {
        display: inline-block;
        vertical-align: middle;
    }
    .iconfontArrow {
        display: inline-block;
        width: 1rem;
    }
    .autopopup-button {
        background-color: #f3f3f3;
        height: .4rem;
        vertical-align: middle;
        text-align: center;
        line-height: .4rem;
        width: 1.9rem;
    }
    .selected {
        color: #000;
    }
    .autopopup-top {
        display: flex;
        height: .89rem;
        line-height: .89rem;
        margin: 0 auto;
        justify-content: space-between;
        padding: 0 .4rem;
        color: @color;
        font-size: 16px;
        border-bottom: 1px solid #eeeeee;
    }
    .autopopup-list {
        width: 7.5rem;
    }
    .overlayer {
        position: fixed;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        z-index: 10;
    }
</style>
