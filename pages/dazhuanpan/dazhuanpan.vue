<template>
	
	<view style="flex-flow: column nowrap;width: 100%; position: relative;">
 
		<image class="" src="../../static/choujiang/game_bg_01_02.jpg" style="width:750upx;   height: 250upx; ">
			   <view class="icontext" style="position: absolute; right: 30upx; top: 50upx; font-size:44upx ; color: #800080;" @click="gotolist"> &#xe745; </view>
		</image>
		<image class="" src="../../static/choujiang/game_bg_02.jpg" style="width:750upx;   height: 250upx; "></image>
		<image class="" src="../../static/choujiang/game_bg_03.jpg" style="width:750upx;   height: 400upx; "></image>
		<image class="" src="../../static/choujiang/game_bg_04.jpg" style="width:750upx;   height: 300upx; "></image>
		<image class="" src="../../static/choujiang/game_bg_05.jpg" style="width:750upx;   height: 750upx; "></image>



		<image :animation="animationData" class="" src="../../static/choujiang/game-wheel_31.png" style=" width: 700upx;height: 700upx; z-index: 10;position: absolute;top:600upx;left: 25upx;"></image>
		<image class="" @click="rotateAndScale1" src="../../static/choujiang/game-arrow.png" style=" width: 160upx;height: 200upx; z-index: 20;position:  absolute;top:820upx;left: 295upx;"></image>


		<view class="" style="position: absolute;top: 1350upx;left: 50upx; flex-flow: column nowrap;">


			<text style="font-size: 50upx;">活动规则：</text>
			<text style="font-size: 38upx;">1，特等奖一名</text>
			<text style="font-size: 38upx;">2，一等奖两名</text>
			<text style="font-size: 38upx;">3，二等奖三名</text>
			<text style="font-size: 38upx;">4，三等奖四名</text>

			<text style="font-size: 30upx;color: black;padding-top: 20upx;width: 680upx;padding-top: 100upx;">ps:本次活动负责人，中国平安客服经理张田雨，工号代码1025061724，最终解释权归负责人所有。</text>

		</view>
		<view class="" style="position: absolute;top: 1300upx;left: 460upx; flex-flow: column nowrap;padding: 0;">
			<image @click="goLtodenglu" class="" src="../../static/choujiang/timg.png" style="width:200upx ;  height: 200upx; z-index: 40;"></image>
			<text style="padding-left: 40upx; font-size: 33upx; ">我的奖品</text>
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
				show_win_flag:false,
			}
		},

		onLoad(option) {

			this.realname = option.realname;
			this.phonenum = option.phonenum;
			this.zhongjiang_flag = option.zhongjiang_flag;
			
         	console.log(option.realname,option.phonenum,option.zhongjiang_flag)
			this.upload_oauthData()

		},
		methods: {

			rotateAndScale1: function() {
				// 				uni.showToast({
				// 					title: ":"+this.zhongjiang_flag
				// 				})
       const that=this;        
               					
				if (that.enable_zhuan) {
					that.enable_zhuan = false;
        
					var rotate_angel = 45
					if (that.zhongjiang_flag == 5) {
						rotate_angel = (Math.floor(Math.random() * 4) + 1) * 90 + 45
					} else if (that.zhongjiang_flag == 0) {
						rotate_angel = 0
					} else if (that.zhongjiang_flag == 1) {
						rotate_angel = 270
					} else if (that.zhongjiang_flag == 2) {
						rotate_angel = 180
					} else if (that.zhongjiang_flag == 3) {
						rotate_angel = 90
					} else {
						rotate_angel = 45
					}
					rotate_angel+=1800+720;
					console.log(that.zhongjiang_flag + "-----" + rotate_angel + "-----" + that.rand_num)
// 
					var animation = uni.createAnimation({
						duration: 6,
						timingFunction: 'ease',
					})
					// 旋转同时放大
					animation.rotate(0).step()
					that.animationData = animation.export()
					setTimeout(function() {
						var animation = uni.createAnimation({
							duration: 6000,
							timingFunction: 'ease',
						})

						animation.rotate(rotate_angel).step()
						that.animationData = animation.export()
					}.bind(that), 10)
					
					setTimeout(function() {
						if (that.zhongjiang_flag ==5 ) {
							that.enable_zhuan = true;
							that.upload_oauthData();
				    }
				     else {
							 that.show_win_flag=true;	that.enable_zhuan = false;
				      	uni.showToast({
				      		title: "您已经中奖"
				      	})
				    }
				
				}, 6500)
						
				}
				
				
				if ((that.zhongjiang_flag == 5 || that.zhongjiang_flag == null) && that.enable_zhuan==false) {
           uni.showToast({
						 	icon: "none",	
						title: "等待中",
						duration:7000,
					})
				} 
				if (that.zhongjiang_flag <5 && this.show_win_flag==true) {
					uni.showToast({
						title: "您已经中奖"
					})
				}
				if (that.zhongjiang_flag <5 ) {
						that.enable_zhuan = false;
				}
				



			},

			upload_oauthData() {
				uni.request({
					url: 'https://kaikaiomg.applinzi.com/uni_app_upload.php',
					method: "POST",
					data: {
						'form': 'oauthData_dazhuanpan',
						'nickName': this.nickName,
						'openId': this.openId,
						'avatarUrl': this.avatarUrl,
						'realname': this.realname,
						'phonenum': this.phonenum,
						'zhongjiang_flag': this.zhongjiang_flag,
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: (res) => {

						if (res.data != 404) {
							this.zhongjiang_flag = res.data % 10;
							this.rand_num = res.data / 10;


						}

					},
					fail: () => {
						uni.showModal({
							content: "请求失败，请重试!",
							showCancel: false
						})
					}
				})
			},
			goLtodenglu() {

				uni.navigateTo({
					url: "../dazhuanpan_denglu/dazhuanpan_denglu?phonenum="+this.phonenum+"&realname="+this.realname+"&zhongjiang_flag="+this.zhongjiang_flag 
				})

			},
			gotolist() {

				uni.reLaunch({
			
					url: "../dazhuanpan_list/dazhuanpan_list"
				})

			},





		}
	}
</script>

<style>
	
@font-face {
	font-family: texticons;
	font-weight: normal;
	font-style: normal;
	src: url('https://kaikaiomg.applinzi.com/icon/iconfont.ttf') format('truetype');
}


.icontext {
	font-family: texticons;
}

</style>
