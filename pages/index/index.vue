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