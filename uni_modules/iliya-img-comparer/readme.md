# iliya-img-comparer图片比较器

已测试h5和微信小程序，理论支持全平台

亮点：
使用js计算而不是resize属性，定制化程度更高<br>
组件挂在后可播放指示线动画，提示用户可以拖拽比较图片<br>
左右下角可配置文字，且指引线距离文字过近时文字会变透明

## 使用示例

```
<template>
	<view class="content">
		<iliya-img-comparer left-img="https://env-00jxhol06ilx.normal.cloudstatic.cn/comparer/left-img.webp"
			right-img="https://env-00jxhol06ilx.normal.cloudstatic.cn/comparer/right-img.webp" right-text="RTX ON">
			<template #left-text>
				<view style="color: red;">
					RTX OFF
				</view>
			</template>
		</iliya-img-comparer>
	</view>
</template>

<script>
	export default {
		data() {
			return {}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
</style>
```

## Props


| 参数名         | 类型            | 说明                   | 默认值 | 可选值                        |
| -------------- | --------------- | ---------------------- | ------ | ----------------------------- |
| leftImg        | string          | 左图                   | /      | 图片地址                      |
| rightImg       | string          | 右图                   | /      | 图片地址                      |
| width          | string          | 组件宽度               | 730rpx |                               |
| height         | string          | 组件高度               | 400rpx |                               |
| leftText       | string/slot     | 左侧文本               |        | 使用插槽left-text或传递props  |
| rightText      | string/slot     | 右侧文本               |        | 使用插槽right-text或传递props |
| leftColor      | string          | 左侧文本颜色           | #fff   |                               |
| rightColor     | string          | 右侧文本颜色           | #fff   |                               |
| animeStartLeft | string / number | 指引线动画初次移动距离 | 80%    |                               |
| animeEndLeft   | string / number | 指引线动画结束移动距离 | 40%    |                               |
