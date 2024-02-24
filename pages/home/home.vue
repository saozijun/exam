<template>
	<view class="box">
		<view class="content">
			<view class="seach" @click="seach('影视')">搜索</view>
			<view class="content_title">影视</view>
			<view class="content_item">
				<view @click="toexam2('单选')">
					<text>单选题</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view>
				<view @click="toexam2('多选')">
					<text>多选题</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view>
				<view @click="toexam2('判断')">
					<text>判断题</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view>
<!-- 				<view @click="toexam2('填空')">
					<text>填空题</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view>
				<view @click="toexam2('简答')">
					<text>简答题</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view>
				<view @click="toexam2('名词解释')">
					<text>名词解释</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view> -->
			</view>
		</view>
		<view class="content">
			<view class="seach" @click="seach('英语')">搜索</view>
			<view class="content_title">英语</view>
			<view class="content_item">
				<view @click="toexam('词汇结构')">
					<text>词汇结构</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view>
				<view @click="toexam('听力')">
					<text>听力</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view>
				<view @click="toexam('阅读理解')">
					<text>阅读理解</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view>
			</view>
			<view class="btn animated jello" @click="toexam('模拟考试')">模拟考试（英语）</view>
		</view>

		<view class="content">
			<view class="gg">已挂科</view>
			<view class="content_title">数学</view>
			<view class="content_item">
				<view @click="toexam3('判断')">
					<text>判断题</text>
					<img class="img" src="/static/rigth.png" alt="">
				</view>
			</view>
		</view>
	<!-- 	<button class="btn_contact" style="padding: 2px;border:none; font-size: 32rpx;" type="default"
			open-type="contact">
			<img class="img" src="/static/kf.png" alt="">
		</button> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
			};
		},
		methods: {
			toexam(text) {
				uni.navigateTo({
					url: '/pages/index/elg?text=' + text
				});
			},
			toexam2(text) {
				uni.navigateTo({
					url: '/pages/index/index?type=影视&text=' + text
				});
			},
			toexam3(text) {
				uni.navigateTo({
					url: '/pages/index/mathematics?type=数学&text=' + text
				});
			},
			seach(text){
				uni.navigateTo({
					url: '/pages/seach/seach?text=' + text
				});
			}
		}
	}
</script>

<style lang="less">
	.seach{
		width: 120rpx;
		height: 120rpx;
		line-height: 120rpx;
		border-radius: 100rpx;
		background-color: #f8f8f8;
		position: absolute;
		box-shadow: 1px 4px 7px 0px #107d0947;
		transform: translate(-50%,-50%);
		bottom: 180rpx;
		right: 20rpx;
		color: #666;
		opacity: 0.6;
		text-align: center;
	}
	.btn {
		width: 80%;
		margin: 40rpx auto;
		text-align: center;
		background-color: #759960d4;
		border-radius: 74px;
		color: #fff;
		padding: 30rpx 0;
		margin-bottom: 100rpx;
	}

	.btn_contact {
		display: block;
		position: fixed;
		transform: translate(-50%,-50%);
		bottom: 200rpx;
		background-color: #c7c7c74a;
		color: #fff;
		right: 20rpx;
		border-radius: 50%;
		width: 120rpx;
		height: 120rpx;
		border: none;
		box-shadow: 1px 4px 7px 0px #107d0947;
		opacity: 0.6;
		&::after{
			border: none;
		}
		.img{
			position: absolute;
			width: 100% ;
			height: 100%;
			top: 0;
			left: 0;
		}
	}

	.box {
		background-color: #f4f4f4;
		border: 2rpx solid #f4f4f4;
		width: 100%;
		height: 100vh;
		display: flex;
		overflow-y: scroll;
		scroll-snap-type: x mandatory;
		flex-wrap: wrap;
		color: #0f571f;
		flex-direction: column;
		&::-webkit-scrollbar{
			width: 0;
		}
		.ma{
			margin-top: 130rpx;
		}
		.content{
			width: 100%;
			height: 100vh;
			padding: 10rpx;
			scroll-snap-align: start;
			scroll-snap-stop: always;
			position: relative;
			.gg{
				position: absolute;
				width: 300rpx;
				height: 300rpx;
				border: 3px solid;
				transform: translate(-50%,-50%) rotate(-30deg);
				left: 50%;
				top: 50%;
				color: #ff000091;
				border-radius: 50%;
				font-size: 68rpx;
				font-family: "";
				text-align: center;
				line-height: 300rpx;
				opacity: 0;
				font-weight: bold;
				animation: gg 2s ease-in-out forwards;
				&::after{
					content: "";
					position: absolute;
					width: 320rpx;
					height: 320rpx;
					border: 3px solid;
					transform: translate(-50%,-50%);
					left: 50%;
					top: 50%;
					border-radius: 50%;
					opacity: 0.6;
				}
				@keyframes gg {
					100%{
						opacity: 0.2;
					}
				}
			}
			.shou{
				color: #079d298c;
				font-size: 100rpx;
			}
		}
	}

	.content {
		width: 96%;
		margin: 0 auto;
		margin-top:40rpx;
		box-sizing: border-box;
		background-color: #fff;
		border-top-left-radius: 24rpx;
		border-top-right-radius: 24rpx;
		    // box-shadow: 0px -4px 11px 0px #38ad2214;
		position: relative;
		.btn{
			position: absolute;
			transform: translate(-50%,-50%);
			bottom: 20%;
			left: 50%;
		}
		.content_title {
			font-size: 36rpx;
			padding: 40rpx;
			border-bottom: 2rpx solid #f4f4f4;
			position: relative;
			font-weight: bold;
			z-index: 111;
			&::after{
				content: "";
				width: 40rpx;
				height: 40rpx;
				background-image: linear-gradient(to right, #107d0947, #ffffff00);
				border-radius: 50%;
				position: absolute;
				top: 46rpx;
				left: 26rpx;
				z-index: -1;
			}
		}

		.content_item {
			.img {
				width: 40rpx;
				height: 40rpx;
			}

			>view {
				padding: 40rpx;
				display: flex;
				align-items: center;
				justify-content: space-between;
				border-bottom: 2rpx solid #f4f4f4;

				&:last-child {
					border: none;
				}
			}
		}
	}
</style>