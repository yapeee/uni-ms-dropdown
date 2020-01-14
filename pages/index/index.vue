<template>
	<view>
		<ms-dropdown-menu>
			<ms-dropdown-item v-model="value1" :list="list"></ms-dropdown-item>
			<ms-dropdown-item v-model="value2" :list="list"></ms-dropdown-item>
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
	import msDropdownMenu from '@/components/dropdown-select/dropdown-menu.vue'
	import msDropdownItem from '@/components/dropdown-select/dropdown-item.vue'
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
				value3: {
					name: 'init'
				}
			}
		},
		watch: {
		},
		mounted() {
		},
		methods: {
			choose() {
				let obj = {
					value: {
						test: 1,
						name: 'test'
					}
				}
				this.$refs.dropdownItem.choose(obj)
			},
			close() {
				this.$refs.dropdownItem.closePopup()
			}
		}
	}
</script>

<style lang="scss">
	.dropdown-item-content {
		padding: 20rpx;
		.btn {
			background: $uni-color-primary;
			background: #007aff;
			color: #fff;
			padding: 20rpx;
			display: inline-block;
			border-radius: 10rpx;
		}
	}
</style>
