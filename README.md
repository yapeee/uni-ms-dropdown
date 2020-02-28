## 简介

下拉选择框，通过v-model绑定下拉数组选中值。支持自定义下拉内容，输出指定内容。

## demo

源码：https://github.com/yapeee/uni-ms-dropdown

## 基础用法

```vue
<template>
	<view>
		<ms-dropdown-menu>
			<ms-dropdown-item v-model="value1" :list="list"></ms-dropdown-item>
			<!-- <ms-dropdown-item v-model="value2" :list="list"></ms-dropdown-item> -->
			<ms-dropdown-item v-model="value2" :list="list">
				<view slot="title">
					<view class="dropdown-item-title">
						<view class="title">自定义title</view>
						<view class="btn">打开</view>
					</view>
				</view>
			</ms-dropdown-item>
			<ms-dropdown-item v-model="value3" :hasSlot="true" title="自定义下拉框内容" ref="dropdownItem">
				<view class="dropdown-item-content">
					<view>=====此为测试内容=====</view>
					<view class="btn" @click="choose">输出：test</view>
					<view class="btn" @click="close">关闭</view>
				</view>
			</ms-dropdown-item>
		</ms-dropdown-menu>
		<view>输出：{{value1}}</view>
		<view>输出：{{value2}}</view>
		<view>输出：{{value3}}</view>
	</view>
</template>

<script>
	import msDropdownMenu from '@/components/ms-dropdown/dropdown-menu.vue'
	import msDropdownItem from '@/components/ms-dropdown/dropdown-item.vue'
	export default {
		components: {
			msDropdownMenu,
			msDropdownItem
		},
		props: {
		},
		data() {
			return {
				list: [
					{
						text: 'item1',
						value: 0
					},
					{
						text: 'item2',
						value: 1
					},
					{
						text: 'item3',
						value: 2
					}
				],
				value1: 0,
				value2: 1,
				value3: 'init'
			}
		},
		watch: {
		},
		mounted() {
		},
		methods: {
			choose() {
				let obj = {
					value: 'test'
				}
				this.$refs.dropdownItem.choose(obj)
			},
			close() {
				this.$refs.dropdownItem.closePopup()
			}
		}
	}
</script>
```

## 属性说明

#### DropdownItem Props

| 参数         | 说明                    | 类型    | 默认值             |
| ------------ | ----------------------- | ------- | ------------------ |
| value      | 当前选中项对应的 value，可以通过v-model双向绑定 | number \| String \| Object | -               |
| list         | 选项数组      | Option[] | []                 |
| title | 自定义标题字符串 | String  | $uni-color-primary |

#### Option 数据结构

| 键名  | 说明   | 类型                         |
| ---- | ----- | --------------------------- |
| text  | 文字   | string                    |
| value | 标识符 | string \| number \| Object |

#### DropdownItem Slots

| 名称  | 说明   |
| ---- | ----- |
| default  | 菜单内容   |
| title | 自定义标题 |

## 方法说明
通过 ref 获取DropdownItem组件调用方法

| 方法称名  | 说明   |
| ---- | ----- |
| choose  | 输出value  |
| closePopup | 关闭下拉框 |