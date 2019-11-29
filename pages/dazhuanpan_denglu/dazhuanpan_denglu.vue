<template>
	<view style="flex-flow: column nowrap;width: 750upx; position: relative;">

		<image class="" src="../../static/choujiang/tianshu5.jpg" style="width:750upx;   height: 400upx; "></image>
		<image class="" src="../../static/choujiang/tianshu6.jpg" style="width:750upx;   height: 650upx; "></image>
		<image class="" src="../../static/choujiang/tianshu7.jpg" style="width:750upx;   height: 550upx; "></image>

		<view class="" style="position: absolute;top: 450upx; left: 75upx; border-radius: 22upx;background-color: #71C671;width: 600upx;height: 490upx; border: #000000 solid 1upx; flex-flow: column;">
			<!-- 	<image class="logo-img" src="avatarUrl "></image> -->
			<view class="" style="flex-flow:column nowrap; padding: 10upx;">
				<text class="uer-name" style="font-size: 60upx;color: black; font-family: KaiTi;padding-bottom:10upx ;">尊敬的:
					{{realname}}</text>
				<view class="" style="flex-flow:row wrap;">
					<text class="uer-name" style="font-size: 34upx;color: black;width: 430upx;height: 50upx;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;恭喜您获得本次活动的 </text>
					<text class="uer-name" style="font-size: 43upx;color: red; width: 130upx;height: 50upx; font-weight: bold; ">{{liping}} </text>
					<text class="uer-name" style="font-size: 34upx; color: black;"> 请联系工作人员领取.感谢您参与本次活动。</text>
				</view>


			</view>
			
			<image class="" style="width: 200upx;height: 200upx; padding-left: 200upx;"  src="../../static/choujiang/erweima.jpg" ></image>
			
		</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				providerList: [],
				hasProvider: false,
				openId: null,
				nickName: '',
				avatarUrl: null,
				phonenum: null,
				realname: null,
				zhongjiang_flag: 5,
				animationData: {},
				rand_num: 0,
				enable_zhuan: true,
				liping: "参与奖"
			}
		},

		onLoad(option) {

			this.realname = option.realname;
			this.phonenum = option.phonenum;
			this.zhongjiang_flag = option.zhongjiang_flag;
			console.log(option.realname, option.phonenum, option.zhongjiang_flag)
			if (this.zhongjiang_flag == 0) {
				this.liping = "特等奖"
			} else if (this.zhongjiang_flag == 1) {
				this.liping = "一等奖"
			} else if (this.zhongjiang_flag == 2) {
				this.liping = "二等奖"
			} else if (this.zhongjiang_flag == 3) {
				this.liping = "三等奖"
			} else {
				this.liping = "参与奖"
			}
		},
		methods: {



			goLtodenglu() {

				uni.navigateTo({
					url: "../dazhuanpan_denglu/dazhuanpan_denglu?phonenum=" + this.phonenum + "&realname=" + this.realname +
						"&zhongjiang_flag=" + this.zhongjiang_flag
				})

			},
			goLtodazhuanpan() {

				uni.reLaunch()({
					url: "../dazhuanpan/dazhuanpan"
				})

			},





		}
	}
</script>

<style>
	.logo {
		width: 750px;
		height: 240px;
		padding: 20px;
		box-sizing: border-box;
		background-color: #FF80AB;
		flex-direction: row;
		align-items: center;
	}

	.logo-hover {
		opacity: 0.8;
	}
</style>
