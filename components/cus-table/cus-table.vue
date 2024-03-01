<template>
    <view class="table-wrap" :style="{'--cell-height': cellHeight}">
        <scroll-view class="table-con-wrap" scroll-x="true">
            <view class="fixed-wrap left-fixed-wrap">
                <uni-table>
                    <uni-tr>
                        <block v-for="field in fields.filter(col => col.fixed == 'left')" v-if="!field.hidden">
                            <cus-th align="center" :field="field"></cus-th>
                        </block>
                    </uni-tr>
                    <uni-tr class="tran03" v-for="(item,index) in data" :key="index">
                        <block v-for="field in leftFixedFields" v-if="!field.hidden">
                            <cus-td :field="field" :item="item" :index="index" @callback="pressBtn">
                                <slot :name="field.field" :item="item" :index="index"></slot>
                            </cus-td>
                        </block>
                    </uni-tr>
                </uni-table>
            </view>
            <uni-table emptyText="无数据" :style="{minWidth: width}">
                <uni-tr>
                    <block v-for="field in fields.filter(col => !col.fixed)" v-if="!field.hidden">
                        <cus-th align="center" :field="field"></cus-th>
                    </block>
                </uni-tr>
                <uni-tr class="tran03" v-for="(item,index) in data" :key="index">
                    <block v-for="field in normalFields" v-if="!field.hidden">
                        <cus-td :field="field" :item="item" :index="index" @callback="pressBtn">
                            <slot :name="field.field" :item="item" :index="index"></slot>
                        </cus-td>
                    </block>
                </uni-tr>
            </uni-table>
            <view class="fixed-wrap right-fixed-wrap">
                <uni-table>
                    <uni-tr>
                        <block v-for="field in rightFixedFields" v-if="!field.hidden">
                            <cus-th align="center" :field="field"></cus-th>
                        </block>
                    </uni-tr>
                    <uni-tr class="tran03" v-for="(item,index) in data" :key="index">
                        <block v-for="field in fields.filter(col => col.fixed == 'right')" v-if="!field.hidden">
                            <cus-td :field="field" :item="item" :index="index" @callback="pressBtn">
                                <slot :name="field.field" :item="item" :index="index"></slot>
                            </cus-td>
                        </block>
                    </uni-tr>
                </uni-table>
            </view>
        </scroll-view>
    </view>
</template>
<script>
export default {
    name: "cus-table",
    props: {
        cellHeight: {
            type: [Number, String],
            default: ''
        },
        fields: {
            type: Array,
            default: []
        },
        data: {
            type: Array,
            default: []
        }
    },
    computed: {
        normalFields() {
            return this.fields.filter(col => !col.fixed)
        },
        leftFixedFields() {
            return this.fields.filter(col => col.fixed == 'left')
        },
        rightFixedFields() {
            return this.fields.filter(col => col.fixed == 'right')
        }
    },
    data() {
        return {
            width: '100%',
            default_width: 88,
        }
    },
    created() {
        let width = 0
        this.fields.forEach((item) => {
            width += item.width || this.default_width
        })
        this.width = width + 'px'
        // console.log('this.dataSources---->', this.dataSources);
    },
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
.table-wrap {
    border: solid #dfdfdf;
    box-sizing: border-box;
    border-width: 1upx 0 0 0;

    .fixed-wrap {
        z-index: 10;
        position: sticky;
        background: #fff;
        box-shadow: rgba(99, 99, 99, 0.5) 0 0 10upx 0;

        &.left-fixed-wrap {
            left: -2upx;
        }

        &.right-fixed-wrap {
            right: -2upx;
        }
    }

    ::v-deep .uni-scroll-view-content {
        display: flex;
    }

    ::v-deep .uni-table {
        min-width: 100%;

        .uni-table-tr {
            &:last-of-type {
                .uni-table-td {
                    border-bottom-style: solid;
                }
            }

            &:hover {
                .text-line {
                    display: block;
                }
            }


            .uni-table-td {

                .td-cell {
                    height: var(--cell-height);
                }

            }

        }

    }

}
</style>