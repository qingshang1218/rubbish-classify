<template>
	<view :animation="animationData">
		
		<view class="ceshi">
			<text style="font-size: 25px;">测试</text>
		</view>
		<view class="fengeline"></view>
		<!-- 序号 -->
		<view class="xuhao">
			<view class="current">
				{{index}}
			</view>
			
			<view class="gray">
				／10
			</view>
		</view>
		
		<!--  题目-->
		<view class="">
			<view class="question_title">
				<view class="question_item_1">
					<text id="keyword">{{rubbish}}</text>
				</view>
			
			<!--  选项-->
				
					<view class="main-classify"  @click="switchTabToClassify(0) " >
						<text class="xuanxiang-text">干垃圾</text>
					</view>
					<view class="main-classify" @click="switchTabToClassify(1)" >
						<text class="xuanxiang-text">湿垃圾</text>
					</view>
				
					<view class="main-classify" @click="switchTabToClassify(3)">
						<text class="xuanxiang-text">有害垃圾</text>
					</view>
					<view class="main-classify" @click="switchTabToClassify(2)">
						<text class="xuanxiang-text">可回收物</text>
						
					</view>

			</view>
		</view>
	</view>
</template>

<script>
	
	let questions = [
		// 0 干
		// 1 湿垃圾
		// 2 有害垃圾
		// 3 可回收
		{
			id:1,
			name:'干电池',
			type:0,
			classify:'干垃圾',
		},
		{
			id:2,
			name:'西瓜',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:3,
			name:'塑料瓶',
			type:2,
			classify:'可回收物',
		},
		{
			id:4,
			name:'有汞电池',
			type:3,
			classify:'有害垃圾',
		},
		{
			id:5,
			name:'烟头',
			type:0,
			classify:'干垃圾',
		},
		{
			id:6,
			name:'尿不湿',
			type:0,
			classify:'干垃圾',
		},
		{
			id:7,
			name:'废纸',
			type:0,
			classify:'干垃圾',
		},
		{
			id:8,
			name:'一次性杯子',
			type:0,
			classify:'干垃圾',
		},
		
		{
			id:9,
			name:'面包',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:10,
			name:'过期食品',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:11,
			name:'瓜皮果壳',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:12,
			name:'花卉植物',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:13,
			name:'烟头',
			type:0,
			classify:'干垃圾',
		},
		{
			id:14,
			name:'动物内脏',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:15,
			name:'白菜',
			type:1,
			classify:'湿垃圾',
		},
		{
			id:16,
			name:'鸡蛋',
			type:1,
			classify:'湿垃圾',
		},
		];
	export default {
		data() {
			return {
				index:1,
				scores:0,
				rubbish:'干电池',
				animationData:{},
				problems:[],
            }
		},
		onLoad() {
			//随机生成一道题
		
			
			
		},
		onShow() {
			this.index = 1;
			getApp().globalData.totalScores = 0;
			//利用Set不能重复往里放
			let  set = new Set();
			while(set.size<10){
				set.add(Math.round(Math.random()*questions.length));
			}
			console.log(set.size);
			for(let i of set){
				this.problems.push(questions[i]);
			}
			
			console.log(this.problems);
			this.rubbish = this.problems[this.index-1].name;
			
		},
		methods: {
			donghua() {
				var animation = uni.createAnimation({
					duration: 5000,
					timingFunction: 'ease',
				})
				this.animation = animation
				animation.opacity(0).translate(-300).step()
				this.animationData = animation.export()
				
				setTimeout(function() {
					animation.translate(10).opacity(0.5).step({
						duration: 0
					})
					this.animationData = animation.export()
				}.bind(this), 400);
				setTimeout(function() {
					animation.translate(0).opacity(1).step({
						duration: 800
					})
					this.animationData = animation.export()
					
				}.bind(this), 500);
				
			},
			
			switchTabToClassify(answer){
				// this.activeColor= '#FF9224'
				// this.color='#999999'
			
				if(this.index>=10){
					//统计
					getApp().globalData.totalScores = this.scores;
					let score = "恭喜您" + ",答对了"+this.scores+"道题，加油，再接再厉"
					uni.showModal({
						title:"恭喜您",
						content:score,
						confirmText:'确定',
						showCancel:false,
						success:function(res){
							uni.switchTab({
								url:'../index/index',
							})
						}
					})
					return;
				}
				
				let that = this;
				let yourAnswer = '';
				
				if(this.index-1==9){
					console.log(rubbish);
				}
				
				let rightAnswer = this.rubbish + "属于"+ this.problems[this.index-1].classify;
				if(answer===this.problems[this.index-1].type){
					console.log("下一道题");
					//进行下一道题
					this.scores++;
					this.donghua();
				}else{
					if(answer===0){
						yourAnswer = '干垃圾'
					}else if(answer===1){
						yourAnswer = '湿垃圾'
					}else if(answer===2){
						yourAnswer = '有害垃圾'
					}else if(answer===3){
						yourAnswer = '可回收物'
					}
					
					uni.showModal({
						title:"您选错啦",
						content:rightAnswer,
						confirmText:'知道了',
						showCancel:false,
						//进行下一道题
						success:function(res){
							that.donghua();
						}
					})
				}
				this.index++;
				console.log(this.index)
				this.rubbish = this.problems[this.index-1].name;
			}
		}
	}
</script>

<style>
	.xuanxiang-text{
		
		color: #000000;
	}
	.fengeline{
		width: 100%;
		height: 5upx;
		background-color: #C5C5C5;
	}
	.ceshi{
		text-align: center;
		margin-top: 100upx;
		/* text-align: justify; */
		width: 100%;
		height: 50px;
	}
	.current{
		color: #007AFF;
	}
	
	.gray{
		color: #555555;
	}
	#keyword{
		font-size: 50upx;
		color: #000000;
	}
	
	.question_title{
		display: flex;
		flex-direction: column;
		width: 90%;
		height: 700upx;
		color: #2C405A;
		font-size: 32upx;
		margin-bottom: 15upx;
		margin: 0upx auto;
		margin-top: 100upx;
	}
	
	.question_item_1{
		display: flex;
		justify-content: center;
		align-items: center;
		margin-bottom: 10upx;
	}
	
	.question_item_2{
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 24upx;
		color: #F0AD4E;
		margin-top: 5upx;
	}
	
	.main-classify{
		width: 70%;
		height: 90upx;
		display: flex;
		justify-content: center;
		direction: center;
		border: 1px solid #999999;
		text-align: center;
		line-height: 90upx;
		margin: 30upx auto;
		border-radius: 40upx;
	}
		
	.xuhao{
		width: 90%;
		display: flex;
		margin: 0upx auto;
		/* justify-content: center; */
		margin-top: 10upx;
	}
</style>
