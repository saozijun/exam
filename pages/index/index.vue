<template>
	<view class="content">
		<view class="box" v-if="isshow">
			<view v-if="text!='简答' && text!='名词解释' && text!='填空'">
				<view style="padding: 10px 0;">{{index}}{{'/'}}{{list.length}}</view>
				<view class="title">
					{{index}}.{{list[index-1].title}}{{list[index-1].title==""?'听力题':""}}
				</view>
			</view>
			<view class="sy_box" v-if="text=='简答' || text=='名词解释' || text=='填空'">
				<view class="ty_content"  v-for="(item,i) in list">
					<view style="padding: 10px 0;">{{i+1}}{{'/'}}{{list.length}}</view>
					<view class="title">
						{{i+1}}.{{list[i].title}}{{list[i].title==""?'听力题':""}}
					</view>
					<view style="padding:20px;" v-if="text!='填空'">
						答案：<span style="line-height: 28px;color: #353535; font-weight: bold;">{{list[i].result}}</span>
					</view>
				</view>
			</view>
			<view class="option_box" v-if="text=='多选'" :class="{active:str.indexOf(option[optionIndex])!=-1}" v-for="(item,optionIndex) in list[index-1].option"
				@click="selectOn(optionIndex,$event)">
				<view class="a">{{option[optionIndex]}}</view>
				<view class="option_item">
					{{item}}
				</view>
			</view>
			<view class="option_box" v-if="text=='单选'" v-for="(item,optionIndex) in list[index-1].option"
				@click="selectOn(optionIndex)" :class="{active:optionIndex==active}">
				<view class="a">{{option[optionIndex]}}</view>
				<view class="option_item">
					{{item}}
				</view>
			</view>
			<view class="option_box" v-if="text=='判断'" v-for="(item,optionIndex) in 2"
				@click="selectOn(optionIndex)" :class="{active:optionIndex==active}">
				<view class="a">{{option[optionIndex]}}</view>
				<view class="option_item">
					{{item==1?'正确':'错误'}}
				</view>
			</view>
			<template v-if="active!=-1">
				<view class="footer" v-if="text=='多选'"
					:style="{color:(this.active == list[index-1].result?'#5cd617':'red')}">
					<text>{{this.active == list[index-1].result?'回答正确':'回答错误' }}</text>
				</view>
				<view class="footer" v-if="text == '判断'"
					:style="{color:(this.active != list[index-1].result?'#5cd617':'red')}">
					<text>{{this.active != list[index-1].result?'回答正确':'回答错误' }}</text>
				</view>
				<view class="footer" v-if="text=='单选'"
					:style="{color:(list[index-1].option[active] == list[index-1].result?'#5cd617':'red')}">
					<text>{{list[index-1].option[active] == list[index-1].result?'回答正确':'回答错误' }}</text>
				</view>
				<view class="footer" style="color: #96764f;">
					<view>
						<view v-if="text =='单选'">正确答案是 : {{getsele(list[index-1])}}</view>
						<view v-if="text =='单选'">{{list[index-1].result}}</view>
						<view v-if="text =='多选'">正确答案是 : {{list[index-1].result}} </view>
					</view>
				</view>
			</template>
			<view class="sum" v-if="text!='填空' && text!='简答' && text!='名词解释'">
				<view>{{bingo}}</view>
				<view>{{wrongQuestion}}</view>
			</view>
			<view class="btn footer_btn2 footer_btn3" v-if="text=='多选' && active == -1" @click="submit()">提交</view>
			<view class="btn footer_btn2" v-if="(text=='填空' || text=='简答' || text=='名词解释') && this.index>1" @click="back">上一题</view>
			<view class="btn footer_btn" v-if="active!=-1 && show " @click="next">下一题</view>
		</view>
		<view style="text-align: center;margin-top: 50px;" v-else>暂未录入</view>
	</view>
</template>

<script>
	import English from '../json/english.json'
	import movies from '../json/movies2.json'
	export default {
		data() {
			return {
				list: [],
				index: 1,
				seleindex: -1,
				active: -1,
				wrongQuestion: 0,
				bingo: 0,
				isshow: true,
				text: "",
				str:[],
				show: true,
				option: ['A', 'B', 'C', 'D', 'E', 'F'],
				tmepArr:[],
				tempIndex:0
			}
		},
		onLoad(options) {
			if (options.text) {
				this.text = options.text
				if (options.text == "模拟考试") {
					this.getexam()
				} else {
					uni.setNavigationBarTitle({
						title: options.text + '题'
					})
					// console.log(options.type);
					if (options.type == "影视") {
						if (movies[options.text] == undefined) {
							this.isshow = false
							return
						}
						//this.getnewarr(movies[options.text])
						this.list = this.getnewarr(movies[options.text])
					}

				}
			}
		},
		methods: {
			submit() {
				this.active = this.str.sort().join("")
				if (this.list[this.index - 1].result == this.active) {
					this.bingo++
					this.show = false
					setTimeout(() => {
						this.next()
					}, 500)
				} else {
					this.wrongQuestion++
				}
			},
			selectOn(optionIndex,e) {
				if (this.active != -1) return
				if (this.text == "多选") {
					if(this.str.indexOf(this.option[optionIndex])==-1){
						this.str.push(this.option[optionIndex])
						
					}else{
						this.str.splice(this.str.indexOf(this.option[optionIndex]),1)
					}
					console.log(this.str);
				} else if(this.text == "单选") {
					if (this.list[this.index - 1].option[optionIndex] == this.list[this.index - 1].result) {
						this.bingo++
						// if(this.tempArr.length>this.tempIndex+2)this.tempIndex++
						this.show = false
						setTimeout(() => {
							this.next()
						}, 500)
					} else {
						this.wrongQuestion++
					}
					this.active = optionIndex
				} else if(this.text == "判断") {
					if (this.list[this.index - 1].result != optionIndex) {
						this.bingo++
						this.show = false
						setTimeout(() => {
							this.next()
						}, 500)
					} else {
						this.wrongQuestion++
					}
					this.active = optionIndex
				}
			},
			getsele(e) {
				let tempIndex = -1
				e.option.map((item, index) => {
					if (item == e.result) tempIndex = index
				})
				return tempIndex == 0 ? 'A' : (tempIndex == 1 ? 'B' : (tempIndex == 2 ? 'C' : 'D'))
			},
			getexam() {
				let tempList = new Array
				let tl = this.getnewarr(English['听力'])
				let yd = this.getnewarr(English['阅读理解'])
				let ch = this.getnewarr(English['词汇结构'])
				for (let i = 0; i < 15; i++) {
					tempList.push(tl[i])
				}
				for (let i = 0; i < 20; i++) {
					tempList.push(yd[i])
				}
				for (let i = 0; i < 15; i++) {
					tempList.push(ch[i])
				}
				this.list = tempList
			},
			getnewarr(arr) {
				const shuffle = (arr) => {
					for (let i = 0; i < arr.length; i++) {
						const randomIndex = Math.round(Math.random() * (arr.length - 1 - i)) + i;
						[arr[i], arr[randomIndex]] = [arr[randomIndex], arr[i]]
					}
					return arr
				};

				// console.log(shuffle(arr))
				return shuffle(arr)
			},
			back(){
				this.index--
				this.str = ''
			},
			next() {
				if (this.index == this.list.length) {
					let content = `总题目${this.list.length}题,你答对了 ${this.bingo} 题,答错了 ${this.wrongQuestion} 题`
					uni.showModal({
						title: '答题完成',
						content: content,
						confirmText: '重新答题',
						cancelText: '返回',
						success: (res) => {
							if (res.confirm) {
								this.active = -1
								this.index = 1
								this.wrongQuestion = 0
								this.bingo = 0
								this.str = []
							} else if (res.cancel) {
								uni.reLaunch({
									url: '/pages/home/home'
								});
							}
						}
					});
					return
				}
				this.show = true
				this.active = -1
				this.index++
				this.str = []
			}
		}
	}
</script>

<style lang="less">
	.box {
		padding: 20rpx;
		box-sizing: border-box;
		height: 100vh;
		.sy_box{
			width: 100%;
			height: 100vh;
			display: flex;
			overflow-y: scroll;
			scroll-snap-type: x mandatory;
			flex-wrap: wrap;
			flex-direction: column;
			&::-webkit-scrollbar{
				width: 0;
			}
			.ty_content{
				width: 100%;
				height: 100vh;
				padding: 10rpx;
				padding: 10rpx;
				scroll-snap-align: start;
				scroll-snap-stop: always;
			}
		}
		
		.option_box {
			display: flex;
			align-items: center;
			margin-top: 20rpx;
			width: 100%;
			padding: 10px;
			margin: 40rpx 0;
			box-sizing: border-box;
			border-radius: 4rpx;

			.option_item {
				width: 80%;
			}

			.a {
				width: 60rpx;
				height: 60rpx;
				border-radius: 50%;
				border: 1px solid #999999;
				text-align: center;
				line-height: 60rpx;
				margin-right: 20rpx;
			}
		}
	}

	.sum {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: center;
		position: fixed;
		bottom: 40rpx;
		left: 24rpx;

		view {
			width: 100rpx;
			position: relative;

			&:before {
				content: "";
				width: 20rpx;
				height: 20rpx;
				background-color: #6ede2f;
				border-radius: 50%;
				position: absolute;
				left: 8rpx;
				top: 12rpx;
			}

			&:nth-child(2) {
				&:before {
					background-color: red;
				}
			}
		}
	}

	.active {
		background-color: #4e703b;
		color: #fff;
		transition: 0.4s all;

		.a {
			border: 2rpx solid #fff !important;
		}
	}

	.footer {
		text-align: center;
		padding: 20rpx;
	}

	.btn {
		width: 80%;
		margin: 40rpx auto;
		text-align: center;
		background-color: #4e703b;
		color: #fff;
		padding: 30rpx 0;
	}
	.footer_btn{
		position: absolute;
		transform: translate(-50%,-50%);
		bottom: 20px;
		left: 50%;
		background-color: #4e703bcc;
	}
	.footer_btn2{
		position: absolute;
		transform: translate(-50%,-50%);
		bottom: 80px;
		left: 50%;
		background-color: #4e703bcc;
	}
	.footer_btn3{
		bottom: 30rpx;
	}
</style>