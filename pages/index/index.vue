<template>
	<view>
		<view id="head">
			<view class="head-lift">
				<image src="../../static/zhinan/weizhi.png" mode="" style="width: 35upx; height: 45upx;"></image>
				<text>郑州</text>
			</view>
			
			<view class="head-right" @click="search()">
				<image src="../../static/zhinan/serch.png" style="width: 35upx; height: 35upx;"></image>
			</view>
			
		</view>
		
		<!-- <swiper :indicator-dots="false" :autoplay="true" :interval="3000" :duration="1000">
			<swiper-item v-for="(item,index) in swipers" :key="index" class="swiper-flex">
				<view class="swiper-item">
					<image class="swiper-img" :src="item" mode=""></image>
				</view>
			</swiper-item> -->
		<!-- </swiper> -->
		<view>
			<image src="../../static/zhinan/banner.png"></image>
		</view>
		
		<view class="fengeline"></view>
		<!-- 搜索框 -->
		<view>
			<!-- 提示框-->
			<view class="tip-text">
				<view class="sousuo-img-view" @click="startRecord()">
					<image src="../../static/zhinan/yuyinshibie.png" class="sousuo-img"></image>
					<view >
						<text style="color:#FF9224 ;">语音识别</text>
					</view>
					
				</view>
				<view class="sousuo-img-view">
					<image src="../../static/zhinan/shaomiaoshibie.png"  class="sousuo-img"></image>
					<view>
						<text  style="color:#FF9224 ;">扫描识别</text>
					</view>
				</view>
				<view class="sousuo-img-view"  @click="imgShiBie()">
					<image src="../../static/zhinan/tupianshibie.png"   class="sousuo-img"></image>
					<view>
						<text  style="color:#FF9224 ;">图片识别</text>
					</view>
				</view>
			</view>
			
			
			<view class="fengeline"></view>
			<!-- 搜索框框-->
			<form @submit="formSubmit" @reset="formReset">
				
			</form>
		</view>
		<!-- 垃圾分类小常识-->
		<view class="xiaochangshi-view">
			<!-- <view class="simepleTips">
				{{tip}}
			</view> -->
			<view style="margin-top: 20upx;">
				<text style="color: black;">垃圾分类小常识</text>
			</view>
			
			<image src="../../static/zhinan/lunbotu.png" style="margin-top: 20upx; width: 100%;"></image>
		</view>
		
		
		<view class="fengeline"></view>
		
		<view class="xiaochangshi-view" style="margin-top: 20upx;"><text>垃圾分类</text></view>
		
		<view style="width: 90%; height: 5upx; background-color: #C8C7CC; margin: 0px auto; margin-top: 20upx; margin-bottom: 20upx;"></view>
		<!-- 大分类 -->
		<view>
			<view class="main-classify">
				<view class="view-img" @click="switchTabToClassify(0)">
					<image src="../../static/zhinan/ganlaji.png" mode="" class="classify-img"></image>
					<view class="text-view"><text style="font-size: 20px; color: #D200D2;">干垃圾</text></view>
					
				</view>
				<view class="view-img" @click="switchTabToClassify(1)">
					<image src="../../static/zhinan/shilaji.png" mode="" class="classify-img"></image>
					<view class="text-view"><text style="font-size: 20px; color: #007AFF;">湿垃圾</text></view>
				</view>
			</view>
			
			<view class="main-classify">
				<view class="view-img" @click="switchTabToClassify(3)">
					<image src="../../static/zhinan/kehuishouwu.png" mode="" class="classify-img"></image>
					<view class="text-view"><text style="font-size: 20px; color: #02F78E;">可回收物</text></view>
				</view>
				<view class="view-img" @click="switchTabToClassify(2)">
					<image src="../../static/zhinan/youhailaji.png" mode="" class="classify-img"></image>
					<view class="text-view"><text style="font-size: 20px; color: #FF9224;">有害垃圾</text></view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	//录音管理器
	const recorderManager = uni.getRecorderManager();
	const innerAudioContext = uni.createInnerAudioContext();
	
	innerAudioContext.autoplay = true;
	export default {
		data() {
			return {
				swipers:[
					'../../static/swipers/lunbo1.jpg',
					'../../static/swipers/lunbo2.jpg',
					'../../static/swipers/lunbo3.jpg'
				],
				keyword:'干电池',
				tip:'',
				simpleTips:[
					"让垃圾找到自己的归属",
					"请给垃圾找个合适的家",
					"希望有一天，垃圾桶也会下岗",
					"垃圾分类放，环境有保障",
					"垃圾要分类，资源要利用",
					"今天分一分，明天美十分",
					"积极参与垃圾分类，共创优美社区环境",
					"分类收集人人有责，男女老幼齐来参与",
					"提高社区的品味，从垃圾分类开始",
					"垃圾分类，举手之劳",
					"配合垃圾分类，争做文明市民",
					"垃圾分类益处多，环境保护靠你我",
					"要是垃圾变为宝，分类回收不可少",
					"请给垃圾找个合适的家",
					"垃圾儿女要分家，安居乐业靠大家",
					"垃圾分类人人做，做好分类为人人",
					"让垃圾找到自己的归属",
					"垃圾要分类，生活变美好",
					"你我虽渺小，但意义重大",
					"让垃圾找到自己的归属",
					"请给垃圾找个合适的家",
				],
			}
		},
		//生命周期
		onShow() {
			//随机给tip一个值，生成一个随机下标
			//[0,1)小数，0～19
			let index = Math.round(Math.random() * this.simpleTips.length);
			this.tip = this.simpleTips[index];
			
		},
		onLoad() {
			let self = this;
			recorderManager.onStop(function (res) {
				console.log('recorder stop' + JSON.stringify(res));
				let src = res.tempFilePath;
				//拿到数据之后开始发送http请求获取垃圾类型
				wx.uploadFile({
				  url: 'https://api.tianapi.com/txapi/voicelajifenlei/index', //语音垃圾分类接口
				  method:'POST',
				  header:{
				  	'content-type':'multipart/form-data'
				  },
				  filePath: src,
				  name: 'say',
				  formData: {
				    key: '6b399673a906328e466ee64b5fc900f8',
				    format:'pcm'
				  },
				  success (res){
				    console.log(res.data)
				    wx.showModal({
				      title: '提示',
				      content: res.data,
				      showCancel: false
				    })
				  },
				  fail:function(res){
					  console.log(res.data)
					  wx.showModal({
					    title: '提示',
					    content: res.data,
					    showCancel: false
					  })
				  }
				})
			});
		},
		methods: {
		
			//图像识别方法
			imgShiBie(){
				//1)获取图片--->a:打开本地图片，b:拍照
				//2)base64编码
				//3)发送请求进行识别
				wx.chooseImage({
					count:1,                      //默认9张
					sourceType:['album','camera'],//图片的来源，一个是相册，一个是照相机
					sizeType:['compressed'],      //压缩
					success:function(result){
						wx.getFileSystemManager().readFile({
							filePath:result.tempFilePaths[0],
							encoding:'base64',
							//读取成功
							success:res=>{
								//console.log("base64"+res.data);
								//拿到编码之后开始发送图像识别接口
								wx.request({
									url:'https://api.tianapi.com'+'/txapi/imglajifenlei/index',
									method:'POST',
									header:{
										'content-type':'application/x-www-form-urlencoded'
									},
									data:{
										key:'6b399673a906328e466ee64b5fc900f8',
										img:'data:image/png;base64,'+res.data,
										mode:1
									},
									success:function(response){
										console.log(response);
										getApp().globalData.rubbishList = [];
										
										for (let rubbish of response.data.newslist) {
											let obj = {};
											//src
											if(rubbish.lajitype==1){
												obj.src = '../../static/icos/ico-4.jpg';
											}else if(rubbish.lajitype==3){
												obj.src = '../../static/icos/ico-1.jpg';
											}else if(rubbish.lajitype==2){
												obj.src = '../../static/icos/ico-2.jpg';
											}else if(rubbish.lajitype==0){
												obj.src = '../../static/icos/ico-3.jpg';
											}else if(rubbish.lajitype==4){
												obj.src = '../../static/icos/share.png';
											}
											//title
											obj.title = rubbish.lajitip;
											obj.showText = true;
											obj.text = rubbish.name;
											getApp().globalData.rubbishList.push(obj);
										}
										uni.navigateTo({
											url:'../detail/index'
										})
										
									}
								});
							}
						});
					}
				});
			},
			// uni.getLocation({
			//     type: 'wgs84',
			//     success: function (res) {
			//         console.log('当前位置的经度：' + res.longitude);
			//         console.log('当前位置的纬度：' + res.latitude);
			//     }
			// }),
			searchKeyword(){
				//发送异步请求，到我们的天行接口，查询当前垃圾垃圾类型
				this.request({
					//url:'http://api.tianapi.com/txapi/lajifenlei/index?key=6b399673a906328e466ee64b5fc900f8&word='+this.keyword
					url:'http://api.tianapi.com/txapi/lajifenlei/index',
					data:{
						key:'6b399673a906328e466ee64b5fc900f8',
						word:this.keyword
					}
				}).
				then(response=>{
					
					console.log(response);
					getApp().globalData.rubbishList = [];
					for (let rubbish of response.newslist) {
						let obj = {};
						//src
						if(rubbish.type==1){
							obj.src = '../../static/icos/ico-4.jpg';
						}else if(rubbish.type==3){
							obj.src = '../../static/icos/ico-1.jpg';
						}else if(rubbish.type==2){
							obj.src = '../../static/icos/ico-2.jpg';
						}else if(rubbish.type==0){
							obj.src = '../../static/icos/ico-3.jpg';
						}
						//title
						obj.title = rubbish.explain;
						obj.showText = true;
						obj.text = rubbish.name;
						getApp().globalData.rubbishList.push(obj);
						
						//跳转到详细页面
						uni.navigateTo({
							url:'../detail/index'
						})
					}
				}
				
				
				)
			},
			
			switchTabToClassify(current){
				getApp().globalData.current = current;
				
				uni.switchTab({
				    url: '/pages/type/index'
				});
			},
			search(){
				uni.navigateTo({
				    url: '/pages/search/search'
				});
			},
			//语音部分方法开始
			 startRecord() {
				console.log('开始录音');
	
				recorderManager.start(
					{
					  duration: 20000,
					  numberOfChannels:2,
					  sampleRate:16000,
					  format: 'PCM'
					}
				);
			},
			endRecord() {
				console.log('录音结束');
				recorderManager.stop();
			},
			//语音部分方法结束
		}
	}
</script>

<style>
	.xiaochangshi-view{
		width: 90%;
		display: flex;
		flex-direction: column;
		margin: 0upx auto;
	}
	.fengeline{
		height: 10upx;
		width: 100%;
		margin-top: 50upx;
		background-color: #F1F1F1;
	}
	.sousuo-img-view{
		display: flex;
		height: 140upx;
		flex-direction: column;
	}
	.sousuo-img{
		height: 100upx;
		width: 80upx;
		margin-left: 10upx;
		margin-bottom: 20upx;
	}
	.head-lift{
		display: flex;
		width: 110upx;
		justify-content: space-between;
	}
	.head-right{
		margin-top: 5px;
	}
	.text-view{
		margin-top: 0px;
		margin-left: 30px;
	}
	.view-img{
		width: 350upx;
		height: 350upx;
		text-align: center;
	}
	#head{
		width: 90%;
		height: 45upx;
		display: flex;
		margin: 0px auto;
		justify-content: space-between;
	}
	.main-classify{
		display: flex;
		justify-content: center;
		direction: center;
	}
	
	.classify-img{
		margin-bottom: 0px;
		width: 250upx;
		height: 210upx;
		padding: 20upx;
	}
	.simepleTips{
		display: flex;
		justify-content: center;
		font-size: 36upx;
		color: #4CD964;
	}
	
	
	.swiper-img{
		width: 750upx;
		height: 300upx;
	}
	
	.swiper-flex{
		display: flex;
		justify-content: center;
	}
	
	.tip-text{
		display: flex;
		justify-content: space-around;
		text-align: center;
		margin-top: 50rpx;
		font-size: 24rpx;
		color: #2C405A;
	}
	.input-view{
		display: flex;
		direction: row;
		justify-content: center;
		align-items: center;
		border: 1rpx solid #72c69c;
		border-radius: 50upx;
		box-shadow: 10upx 10upx 20upx #c5c5c5;
		margin: 10upx 20upx 20upx 20upx;
	}
	
	.search-img{
		width: 80upx;
		height: 80upx;
	}
	
	.input-view-item{
		height: 90upx;
		align-items: center;
		padding: 10upx;
	}
	
	.search-text{
		height: 80upx;
		width: 320upx;
		text-align: center;
	}
	
	.font-search{
		height: 80upx;
		padding: 10upx;
		color: #72C69C;
		font-size: 35rpx;
		font-weight: bold;
		margin-top: 30upx;
	}
</style>
