<template>
	<view>
		<view class="tab">
			<!-- 分段器 -->
			<uni-segmented-control :current="current" :values="items.map(v=>v.title)" 
			@clickItem="onClickItem" 
			style-type="text" 
			active-color="#4cd964">
			</uni-segmented-control>
		</view>
		
		<!-- 显示不同的组件 -->
		<view >
			<view v-if="current==0">
				<GanRubbish></GanRubbish>
			</view>
			
			<view v-if="current==1">
				<ShiRubbish></ShiRubbish>
			</view>
			
			<view v-if="current==2">
				<YouHaiRubbish></YouHaiRubbish>
			</view>
			
			<view v-if="current==3">
				<KeHuiShou></KeHuiShou>
			</view>
		</view>
	</view>
</template>

<script>
	//导入子组件
	import GanRubbish from './rubbish_gan/index.vue'
	import ShiRubbish from './rubbish_shi/index.vue'
	import YouHaiRubbish from './rubbish_yhlj/index.vue'
	import KeHuiShou from './rubbish_khs/index.vue'
	//分段器
	import {uniSegmentedControl} from '@dcloudio/uni-ui'
	export default {
		data() {
			return {
				items:[
					{title:'干垃圾'},
					{title:'湿垃圾'},
					{title:'有害垃圾'},
					{title:'可回收物'},
				],
				current: 0
			}
		},
		onShow() {
			this.current = getApp().globalData.current;
		},
		methods: {
			 onClickItem(e) {
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex;
				}
			}
		},
		components:{
			GanRubbish,
			ShiRubbish,
			YouHaiRubbish,
			KeHuiShou,
			uniSegmentedControl
		}
	}
</script>

<style>
	
</style>
