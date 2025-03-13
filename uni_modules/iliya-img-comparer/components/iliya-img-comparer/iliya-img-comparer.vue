<template>
	<view class="iliya-img-comparer" :style="{
			width,
			height
		}">
		<view class="comparer-left" :animation="imgAnimation"
			:style="{width:currentLeft,transition:!imgAnimation ? 'none  !important' : ''}">
			<image class="comparer-left_img" :style="{width}" :src="leftImg"></image>
			<view class="comparer-left_text" :class="{'comparer-left_text_fade':fadeLeftText}" :style="{color:leftColor}">
				<slot name="left-text"></slot>
				{{leftText}}
			</view>
		</view>
		<view class="comparer-right">
			<image class="comparer-right_img" :style="{width}" :src="rightImg"></image>
			<view class="comparer-right_text" :class="{'comparer-left_text_fade':fadeRightText}" :style="{color:rightColor}">
				<slot name="right-text"></slot>
				{{rightText}}
			</view>
		</view>
		<view class="comparer-slider" :animation="lineAnimation" :class="{'comparer-slider_acitve':active}"
			:style="{left:currentLeft,transition:!lineAnimation ? 'none !important' : ''}" @touchmove="touchmoveSlider"
			@touchstart="touchstartSlider" @touchend="touchendSlider">
			<view class="comparer-slider_line"></view>
			<view class="comparer-slider_btn">
				<view class="left-arrow"></view>
				<view class="right-arrow"></view>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				containerWidth: 0,
				currentLeft: '0px',
				startPageX: 0,
				active: false,
				lineAnimation: null,
				imgAnimation: null,
				fadeLeftText: false,
				fadeRightText: false
			}
		},
		props: {
			// 左图地址
			leftImg: {
				type: String,
				default: ''
			},
			// 右图地址
			rightImg: {
				type: String,
				default: ''
			},
			// 组件宽度
			width: {
				type: [String, Number],
				default: '730rpx'
			},
			// 组件高度
			height: {
				type: [String, Number],
				default: '400rpx'
			},
			// 左侧文本
			leftText: {
				type: String,
				default: ''
			},
			// 右侧文本
			rightText: {
				type: String,
				default: ''
			},
			// 左侧文本颜色
			leftColor: {
				type: String,
				default: '#fff'
			},
			// 右侧文本颜色
			rightColor: {
				type: String,
				default: '#fff'
			},
			// 指引线动画初次移动距离
			animeStartLeft: {
				type: [String, Number],
				default: '80%'
			},
			// 指引线动画结束移动距离
			animeEndLeft: {
				type: [String, Number],
				default: '40%'
			},
		},
		mounted() {
			const query = uni.createSelectorQuery().in(this);
			query
				.select('.iliya-img-comparer')
				.boundingClientRect((data) => {
					this.containerWidth = data.width
				})
				.exec();


			setTimeout(() => {
				var lineAnimation = uni.createAnimation({
					duration: 1000,
					timingFunction: 'ease',
				})
				lineAnimation.left(this.animeStartLeft).step().left(this.animeEndLeft).step()
				this.lineAnimation = lineAnimation.export()

				var imgAnimation = uni.createAnimation({
					duration: 1000,
					timingFunction: 'ease',
				})
				imgAnimation.width(this.animeStartLeft).step().width(this.animeEndLeft).step()
				this.imgAnimation = imgAnimation.export()

				setTimeout(() => {
					this.lineAnimation = null
					this.imgAnimation = null
				}, 2000)
			}, 500)
		},
		methods: {
			touchmoveSlider(e) {
				console.log('touchmoveSlider：', e);
				this.currentLeft = Math.floor(e.changedTouches[0].clientX - this.startPageX) + 'px !important'
				if (e.changedTouches[0].clientX < 0) {
					this.currentLeft = '0px'
				} else if (e.changedTouches[0].clientX > this.containerWidth) {
					this.currentLeft = this.containerWidth + 'px'
				}
				if (e.changedTouches[0].clientX < 100) {
					this.fadeLeftText = true
					this.fadeRightText = false
				} else {
					this.fadeLeftText = false
				}
				if (e.changedTouches[0].clientX > this.containerWidth * 0.8) {
					this.fadeRightText = true
					this.fadeLeftText = false
				} else {
					this.fadeRightText = false
				}
				this.active = true
			},
			touchstartSlider(e) {
				// console.log('touchstartSlider：', e);
				this.startPageX = e.changedTouches[0].clientX - e.currentTarget.offsetLeft
			},
			touchendSlider() {
				this.active = false
			}
		}
	}
</script>
<style scoped lang="scss">
	.iliya-img-comparer {
		position: relative;
		box-sizing: border-box;
		overflow: hidden;

		.comparer-left,
		.comparer-right {
			position: absolute;
			top: 0;
			width: 100%;
			height: 100%;
			overflow: hidden;

			&_img {
				height: 100%;
			}

			&_text {
				font-size: 24rpx;
				position: absolute;
				bottom: 10px;
				white-space: nowrap;
				z-index: 10;
				transition: opacity 0.2s;

				&_fade {
					opacity: 0.6;
				}
			}
		}

		.comparer-left {
			left: 0;
			z-index: 2;

			&_text {
				left: 10px;
			}
		}

		.comparer-right {
			width: 100%;
			left: 0;
			z-index: 1;

			&_text {
				right: 10px;
			}
		}

		.comparer-slider {
			position: absolute;
			top: 0;
			height: 100%;
			z-index: 9;
			touch-action: none;

			&_line {
				height: 100%;
				width: 2px;
				background-color: greenyellow;
				transition: background-color 0.2s ease;
			}

			&_btn {
				position: absolute;
				width: 25px;
				height: 25px;
				background-color: greenyellow;
				border-radius: 25px;
				top: 50%;
				left: 50%;
				margin-left: -12px;
				margin-top: -12px;
				transition: background-color 0.2s ease;

				.left-arrow,
				.right-arrow {
					position: absolute;
					top: 50%;
					margin-top: -6px;
				}

				.left-arrow {
					left: -2px;
					border-top: 6px solid transparent;
					border-left: 6px solid transparent;
					border-right: 6px solid rgba(0, 0, 0, 0.6);
					border-bottom: 6px solid transparent;
				}

				.right-arrow {
					right: -2px;
					border-top: 6px solid transparent;
					border-left: 6px solid rgba(0, 0, 0, 0.6);
					border-right: 6px solid transparent;
					border-bottom: 6px solid transparent;
				}
			}

			&.comparer-slider_acitve {

				.comparer-slider_line,
				.comparer-slider_btn {
					background-color: rgba(255, 255, 255, 0.6);
				}
			}
		}
	}
</style>