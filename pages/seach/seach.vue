<template>
	<view>
		<input focus="true" v-model="ipt" class="ipt" type="text" :placeholder="text+'题搜索'" @input="search()">
		<view class="content">
			<view v-for="item in resut" class="item">
				<view class="title">{{item.result==""?"填空題":"题目"}}：{{item.title}}</view>
				<view style="color: #307f00;" v-if="item.type">
					<view v-for="(item2,index) in item.result.split('')">
						{{index+1}}:{{item.option[opArr[item2]]}}
					</view>
				</view>
				<view style="color: #307f00;" v-else-if="item.result!=''">答案：{{item.result}}</view>
			</view>
		</view>
		<view style="height: 100rpx;"></view>
	</view>
</template>

<script>
	import English from '../json/english2.json'
	import movies from '../json/movies2.json'
	export default {
		data() {
			return {
				text: "",
				ipt: "",
				list: [],
				resut: [],
				opArr: {
					A: 0,
					B: 1,
					C: 2,
					D: 3,
					E: 4,
					F: 5,
				}
			};
		},
		onLoad(options) {
			if (options.text) {
				this.text = options.text
				if (this.text == "影视") {
					this.list = movies['判断'].concat(movies['单选'], movies['名词解释'], movies['填空'], movies['多选'])
				} else if (this.text == "英语") {
					this.list = English['听力'].concat(English['词汇结构'], English['阅读理解'])
				}
			}
		},
		methods: {
			search() {
				this.resut = []
				if (this.ipt == "") return
				this.list.forEach(item => {
					if (item.title.indexOf(this.ipt) != -1) {
						item.result = item.result == '1' ? "正确" : (item.result == '0' ? '错误' : item.result)
						this.resut.push(item)
					}
				})
				console.log(this.resut);
			}
		},
		directives: {
			Focus: {
				update: function(el, {
					value
				}) {
					if (value) {
						// if(true)就聚焦
						el.focus();
					}
				},
			},
		},
	}
</script>

<style lang="less">
	.ipt {
		width: 90%;
		height: 90rpx;
		margin: 40rpx auto;
		background-color: #f7f7f7;
		box-sizing: border-box;
		border-radius: 50rpx;
		padding: 0 40rpx;
		color: #666;
		font-size: 36rpx;
	}

	.content {
		padding: 0 40rpx;
		margin-bottom: 50rpx;

		.item {
			margin-bottom: 20rpx;
			padding-bottom: 20rpx;
			border-bottom: 2rpx solid #f4f4f4;

			&:last-child {
				border: none;
			}

			.title {
				font-weight: bold;
				font-size: 26rpx;
				color: #5b5b5b;
				margin-bottom: 10rpx;
			}

		}
	}
</style>