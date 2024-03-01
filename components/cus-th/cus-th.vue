<template>
    <uni-th align="center" :width="field.width || width">
        <view class="th-cell flex ccc">{{field.name}}</view>
    </uni-th>
</template>
<script>
export default {
    name: "cus-th",
    props: {
        field: {
            type: Object,
            default: {}
        },
        width: {
            type: [String, Number],
            default: 90,
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
.uni-table-th {
    color: #fff;
    font-size: 24upx;
    padding: 10px 6px;
    box-sizing: border-box;
    background: $uni-primary;
    border-right: 1upx solid #dfdfdf;
    border-bottom: 1upx solid #dfdfdf;

    &:first-child {
        border-left: 1upx solid #dfdfdf;
    }
}
</style>