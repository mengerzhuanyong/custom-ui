<template>
    <uni-td v-if="!field.hidden" class="cus-td" :align="field.align || 'center'">
        <view class="td-cell flex ccc">
            <template v-if="field.showSlot">
                <slot :item="item" :index="index"></slot>
            </template>
            <template v-else-if="field.format">
                <view :class="{small: field.size === 'small', hide: field.format.hide_value && field.format.hide_value.indexOf(item[field.format.hide_field]) > -1}">
                    <template v-if="field.format.type == 'btn'">
                        <text class="btn" v-if="item[field.format.field] == field.format.value" @click.stop="pressBtn" :data-item="field.format" :data-index="index" :class="{red: field.format.less_red && item[field.field] * 1 < 0}">{{field.format.text}}</text>
                        <text v-else :class="{red: field.format.less_red && item[field.field] * 1 < 0}">{{field.format.default|| item[field.field]}}</text>
                    </template>
                    <text class="btn-text" v-else-if="field.format.type == 'btn-text'" @click.stop="pressBtn" :data-item="field.format" :data-index="index" :class="{red: field.format.less_red && item[field.field] * 1 < 0}">{{item[field.field] || field.default}}</text>
                    <text v-else :class="{red: field.format.less_red && item[field.field] * 1 < 0}">{{item[field.field] || field.default}}</text>
                </view>
            </template>
            <template v-else>
                <text class="tran03 text-wrap" :class="[{small: field.size === 'small', 'text-line': field.maxline}, `line-${field.maxline}`]">{{item[field.field] || field.default}}</text>
            </template>
        </view>
    </uni-td>
</template>
<script>
export default {
    name: "cus-td",
    props: {
        item: {
            type: Object,
            default: {}
        },
        index: {
            type: [Number, String],
            default: 0
        },
        field: {
            type: Object,
            default: {}
        },
    },
    data() {
        return {}
    },
    created() {},
    methods: {
        pressBtn(e) {
            let { item, index } = e.currentTarget.dataset
            this.$emit('callback', {
                index: index,
                data: item
            })
        }
    },
    options: {
        // 解除小程序中样式隔离
        // 微信中的组件存在组件样式隔离，即自定义组件的样式只受到自定义组件 wxss 的影响
        styleIsolation: 'shared'
    },
}
</script>
<style lang="scss" scoped>
.cus-td {
    font-size: 24upx;
    line-height: 1.5;
    padding: 10px 6px;
    background: #fffefd;
    // white-space: wrap;
    word-break: break-all;
    box-sizing: border-box;
    border-right: 1upx solid #dfdfdf;
    border-bottom: 1upx dashed #dfdfdf;

    &:first-child {
        border-left: 1upx solid #dfdfdf;
    }

    .text-line {}

    .small {
        font-size: 22upx;
    }

    .btn {
        color: #b9761d;
        line-height: 1;
        padding: 8upx 30upx;
        border-radius: 6upx;
        background: linear-gradient(135deg, #fcf1e3 4%, #f9ddbb 100%);
    }

    .info-btn-item {
        margin: 0 10upx;
    }

    .btn-text {
        color: #007AFF;
        line-height: 1;
        border-radius: 6upx;
    }

    .text-red {
        color: #e54d42;
    }

    .red {
        color: #E43D33 !important;
    }

    .hide {
        display: none;
    }
}
</style>