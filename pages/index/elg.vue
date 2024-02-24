<template>
	<view class="content">
		<view class="box" v-if="isshow">
			<view style="padding: 10px 0;">{{index}}{{'/'}}{{list.length}}</view>
			<view class="title">
				{{index}}.{{list[index-1].title}}{{list[index-1].title==""?'听力题':""}}
			</view>
			<view class="option_box" v-for="(item,optionIndex) in list[index-1].option" @click="selectOn(optionIndex)" :class="{active:optionIndex==active}">
				<view class="a">{{optionIndex==0?'A':(optionIndex==1?'B':(optionIndex==2?'C':'D'))}}</view>
				<view class="option_item" v-if="item.indexOf('.png')==-1">
					{{item}}
				</view>
				<view class="option_item" v-else>
					<img style="width: 300rpx;height: 120rpx;" :src="item" alt="">
				</view>
			</view>
			<template v-if="active!=-1">
				<view class="footer" :style="{color:(list[index-1].option[active] == list[index-1].result?'#5cd617':'red')}">
					<text>{{list[index-1].option[active] == list[index-1].result?'回答正确':'回答错误' }}</text>
				</view>
				<view class="footer" style="color: #96764f;" v-if="list[index-1].option[active] != list[index-1].result">
					<view>
						<view>正确答案是 : {{getsele(list[index-1])}}</view>
						<view v-if="list[index-1].result.indexOf('.png')==-1">{{list[index-1].result}}</view>
					</view>
				</view>
			</template>
			<view class="sum">
				<view>{{bingo}}</view>
				<view>{{wrongQuestion}}</view>
			</view>
			<view class="btn" v-if="active!=-1 && show" @click="next">下一题</view>
		</view>
		<view style="text-align: center;margin-top: 50px;" v-else>暂未录入</view>
	</view>
</template>

<script>
	import English from '../json/english2.json'
	export default {
		data() {
			return {
				list:[],
				index:1,
				seleindex:-1,
				active:-1,
				wrongQuestion:0,
				bingo:0,
				isshow:true,
				text:"",
				show:true
			}
		},
		onLoad(options) {
			if(options.text){
				this.text = options.text
				if(options.text == "模拟考试"){
					this.getexam()
				}else{
					uni.setNavigationBarTitle({
						title:options.text+'题'
					})
					if(English[options.text]==undefined){
						this.isshow = false
						return
					}
					this.list = this.getnewarr(English[options.text])
				}
			}
		},
		methods: {
			selectOn(optionIndex){
				if(this.active!=-1) return
				if(this.list[this.index-1].option[optionIndex] == this.list[this.index-1].result){
					this.bingo++
					this.show = false
					setTimeout(()=>{
						this.next()
					},500)
				}else{
					this.wrongQuestion++
				}
				this.active = optionIndex
			},
			getsele(e){
				let tempIndex = -1
				e.option.map((item,index)=>{
					if(item == e.result)tempIndex = index
				})
				return tempIndex==0?'A':(tempIndex==1?'B':(tempIndex==2?'C':'D'))
			},
			getexam(){
				let tempList = new Array
				let tl = this.getnewarr(English['听力'])
				let yd = this.getnewarr(English['阅读理解'])
				let ch = this.getnewarr(English['词汇结构'])
				for(let i = 0;i<15;i++){
					tempList.push(tl[i])
				}
				for(let i = 0;i<20;i++){
					tempList.push(yd[i])
				}
				for(let i = 0;i<15;i++){
					tempList.push(ch[i])
				}
				this.list = tempList
			},
			getnewarr(arr){
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
			next(){
				if(this.index==this.list.length){
					let content = `总题目${this.list.length}题,你答对了 ${this.bingo} 题,答错了 ${this.wrongQuestion} 题`
					uni.showModal({
						title: '答题完成',
						content: content,
						confirmText:'重新答题',
						cancelText:'返回',
						success: (res)=> {
							if (res.confirm) {
								this.active = -1
								this.index = 1
								this.wrongQuestion = 0
								this.bingo = 0
							} else if (res.cancel) {
								uni.redirectTo({
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
			}
		}
	}
</script>

<style lang="less">
.box{
	padding: 20rpx;
	box-sizing: border-box;
	.option_box{
		display: flex;
		align-items: center;
		margin-top: 20rpx;
		width: 100%;
		padding: 10px;
		margin: 40rpx 0;
		box-sizing: border-box;
		border-radius: 4rpx;
		.option_item{
			width: 80%;
		}
		.a{
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
.sum{
	width: 100%;
	display: flex;
	align-items: center;
	justify-content: center;
	text-align: center;
	position: fixed;
	bottom: 40rpx;
	left: 24rpx;
	view{
		width: 100rpx;
		position: relative;
		&:before{
			content: "";
			width: 20rpx;
			height: 20rpx;
			background-color: #6ede2f;
			border-radius: 50%;
			position: absolute;
			left: 8rpx;
			top: 12rpx;
		}
		&:nth-child(2){
			&:before{
				background-color: red;
			}
		}
	}
}
.active{
	background-color: #4e703b;
	color: #fff;
	transition: 0.4s all;
	.a{
		border: 2rpx solid #fff !important;
	}
}
.footer{
	text-align: center;
	padding: 20rpx;
}
.btn{
	width: 80%;
	margin: 40rpx auto;
	text-align: center;
	background-color: #4e703b;
	color: #fff;
	padding: 30rpx 0;
}
</style>
