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
```

## 属性说明

#### DropdownItem Props

| 参数         | 说明                    | 类型    | 默认值             |
| ------------ | ----------------------- | ------- | ------------------ |
| value      | 当前选中项对应的 value，可以通过v-model双向绑定 | number \| String \| Object | -               |
| list         | 选项数组      | Option[] | []                 |
| title | 自定义标题 | String  | $uni-color-primary |

#### Option 数据结构

| 键名  | 说明   | 类型                         |
| ---- | ----- | --------------------------- |
| text  | 文字   | string                    |
| value | 标识符 | string \| number \| Object |

## 方法说明
通过 ref 获取DropdownItem组件调用方法

| 方法称名  | 说明   |
| ---- | ----- |
| choose  | 输出value  |
| closePopup | 关闭下拉框 |

## 实现方案
#### 实现下拉框下拉效果
**1.设置下拉框样式**

```css
/* 收起样式 */
transform: translateY(-100%);
transition: all .3s;
/* 展开样式 */
transform: translateY(0);
transition: all .3s;
```
**2.设置下拉框展开收起的位置**

```javascript
this.getElementData('.dropdown-item__selected', (data)=>{
	this.contentTop = data[0].bottom
})
```
该操作是为了设置下拉框的位置，使下拉框展开收起的动画效果在超出下拉框位置的时候就会隐藏。从而实现真正的从菜单按钮底部开始展开，收起结束的动画效果。

#### 实现多个下拉框情况下只允许一个展开
该操作的实现主要是通过`emit`和`on`的通信来实现的。步骤如下：
1.点击DropdownItem组件，在展开下拉框之前，通过`this.$parent.$emit('close')`触发DropdownMenu组件的close事件；
2.DropdownMenu组件通过`this.$on('close', this.closeDropdown)`监听自定义事件；
3.DropdownMenu组件接收通知后，遍历DropdownItem组件，执行DropdownItem组件的`close()`方法

```javascript
closeDropdown() {
	this.$children.forEach(item =>{
	item.close();
	})
}
```

#### 实现支持自定义下拉内容

可通过DropdownItem组件的slot插槽来实现自定义下拉框内容。通过ref定位DropdownItem组件，调用`choose`方法（输出value）和`closePopup`方法（收起下拉框）实现自定义下拉内容。