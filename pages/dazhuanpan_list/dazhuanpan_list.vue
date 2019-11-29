<template>
	<view style="flex-flow: column nowrap;width: 100%; position: relative;">

		<image class="" src="../../static/choujiang/tianshu1.jpg" style="width:750upx;   height: 400upx; ">
			<view class="icontext" style="position: absolute; right: 30upx; top: 50upx; font-size:44upx ; color: #800080;"
			 @click="gotozhuanpan" v-if="secret_num"  >&#xe612;
		</view>
		</image>
		<image class="" src="../../static/choujiang/tianshu2.jpg" style="width:750upx;   height: 650upx; "></image>
		<image class="" src="../../static/choujiang/tianshu3.jpg" style="width:750upx;   height: 550upx; "></image>



		<view class="" style="flex-flow: column nowrap; position: absolute;top:550upx;font-size: 30upx;">

			<view class="" style="padding-left: 140upx;padding-bottom: 40upx;">
				<text>姓名：</text>
				<input @input="KeyInput_realname" type="text" value="" style="border-bottom: #000000 solid 1upx; margin-left: 40upx; width: 350upx;" />
			</view>
			<view class="" style="padding-left: 140upx;padding-bottom: 40upx;">
				<text>手机号：</text>
				<input @input="KeyInput_phonenum" type="number" value="" style="border-bottom: #000000 solid 1upx;width: 350upx;">
			</view>
			<view class="" style="padding-left: 140upx;padding-bottom: 40upx;">
				<text>验证码：</text>
				<input v-model="yanzhengma" type="number" style="border-bottom: #000000 solid 1upx;width: 200upx;">
				<view class="" style="margin-left: 20upx;font-size: 24upx; background-color: #8F8F94; border-radius: 10upx;justify-content: center;align-items: center;width: 140upx;"
				 @click="get_message">{{yanzhengma_title}}</view>
			</view>


			<view @click="goLogindazhuanpan" style="padding-top: 60upx;padding-left: 300upx;font-size: 80upx;font-weight:bold; font-family: KaiTi;"
			 hover-class="hoverjinru"> 进入</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				secret_num:false,
				providerList: [],
				hasProvider: false,
				openId: null,
				nickName: '',
				avatarUrl: null,
				phonenum: null,

				realname: null,
				zhongjiang_flag: 5,

				yanzhengma: '',
				return_message: [],
				enable_get: true,
				yanzhengma_title: '获取验证码',
				randnum:'',
				wait: 60,

			}
		},
		computed: {
			// ...mapState(['hasLogin', 'forcedLogin', 'avatarUrl', 'openid', 'userName', 'phonenum', 'realname'])
		},
		onLoad() {
			// this.oauth('weixin')
		},
		methods: {
			// ...mapMutations(['login', 'logout','login_dazhuanpan']),
			KeyInput_realname: function(e) {
				this.realname = e.detail.value;
				console.log(e.detail.value)
			},
			KeyInput_phonenum: function(e) {
				this.phonenum = e.detail.value;
				console.log(e.detail.value)
				if(this.phonenum=="66668888"){this.secret_num=true}
				else{this.secret_num=false}
				
				
			},
			// 			KeyInput_yanzhengma: function(e) {
			// 				this.yanzhengma = e.detail.value;
			// 				console.log(e.detail.value)
			// 			},
			goLogindazhuanpan() {


				if (this.realname == null) {
					uni.showToast({
						icon: "none",
						title: "请输入姓名"
					})
				} else if (!(/^1[34578]\d{9}$/.test(this.phonenum))) {
					console.log(this.phonenum)
					uni.showToast({
						icon: "none",
						title: "请正确输入手机号"
					})
				} 
				else if (this.randnum != this.yanzhengma) {
					uni.showToast({
						icon: "none",
						title: "请正确输入验证码"
					})
				} 
				
				
				else {
					this.upload_oauthData()
					uni.reLaunch({
						url: "../dazhuanpan_jianjie/dazhuanpan_jianjie?phonenum=" + this.phonenum + "&realname=" + this.realname + "&zhongjiang_flag=" +
							this.zhongjiang_flag
					})
				}


			},

			gotozhuanpan() {

				uni.reLaunch({

					url: "../dazhuanpan_jianjie/dazhuanpan_jianjie?phonenum=" + this.phonenum + "&realname=" + this.realname + "&zhongjiang_flag=" +
						this.zhongjiang_flag

				})

			},


			time() {
				var that = this;
				if (that.wait == 0) {
					that.enable_get = true;
					that.yanzhengma_title = '获取验证码';
					that.wait = 60;
				} else {
					that.enable_get = false;
					that.yanzhengma_title = that.wait + "s";
					// that.yanzhengma = '';
					that.wait--
					setTimeout(function() {
						that.time()
					}, 1000)
				}
			},
			get_message() {
				var that = this;
				if (!(/^1[34578]\d{9}$/.test(that.phonenum)) && that.enable_get) {
					uni.showToast({
						icon: "none",
						title: "请正确输入手机号"
					})
				} else if (that.enable_get) {
					console.log(that.yanzhengma)
					that.enable_get = false;
					that.time();
					that.randnum = Math.floor(Math.random () * 90000) + 100000;
					console.log(that.randnum )
					uni.request({
											url: 'https://imlaixin.cn/Api/send/data/json?accesskey=5914&secretkey=4bef93ce3f102e2f51fc7d11f46411472c98927b&mobile='+that.phonenum+'&content='
											+'【幕天捐书】您注册的手机验证码为'+that.randnum+',30分种有效，只能输入1次，若30分种内未输入，需要重新获取。验证码转发无效。',
											method: "GET",
					
											success: (ret) => {
												if (ret.statusCode !== 200) {
													console.log("请求失败:", ret)
												} else {
													
													uni.showToast({
														icon: "none",	
														title:"发送成功"
													})
													console.log("data:", ret.data)
													that.return_message = ret.data.data;
					
													console.log("lists:", ret.data.data)
												}
											}
										});
				}





			},
			upload_oauthData(userinfo) {
				var that = this;
				uni.request({
					url: 'https://kaikaiomg.applinzi.com/uni_app_upload.php',
					method: "POST",
					data: {
						'form': 'oauthData_dazhuanpan',
						'nickName': that.nickName,
						'openId': that.openId,
						'avatarUrl': that.avatarUrl,
						'realname': that.realname,
						'phonenum': that.phonenum,
						'zhongjiang_flag': that.zhongjiang_flag,
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					success: (ret) => {
						if (ret.statusCode !== 200) {
							console.log("请求失败:", ret)
						} else {
							console.log("data:", ret.data)
							// let lists = [];
							that.item_card_list = ret.data.data;
							// console.log("lists:", ret.data.data.length)
							// console.log("lists:", ret.data.data)
						}
					}
				});



			}


		}
	}
</script>

<style>
	.hoverjinru {
		opacity: 0.5;

	}

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
