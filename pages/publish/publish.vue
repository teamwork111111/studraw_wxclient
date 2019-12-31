<template>
	<view class="page" @touchstart="touchStart" @touchend="touchEnd">
		<form>
			<view class="uni-textarea">
				<textarea placeholder="这一刻的想法..." v-model="input_content" />
			</view>
			<view class="footer">
				<button type="default" class="feedback-submit" @click="publish">提交</button>
			</view>
		</form>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				input_content:'',
				startX: 0, //点击屏幕起始位置
				movedX: 0, //横向移动的距离
				endX: 0, //接触屏幕后移开时的位置
			}
		},
		methods: {
			async publish(){
				if (!this.input_content) {
					uni.showModal({ content: '内容不能为空', showCancel: false, });
					return;
				}
				
				uni.showLoading({title:'发布中'});
				
				uni.request({
								url: 'http://47.95.4.199:8081/api/summary/addSummary',
								method:'POST',
								header : {'content-type':'application/x-www-form-urlencoded'},
								data: {
								       content:this.input_content
								 },
								 success: (res) => {
								    console.log("publicsh页面："+res.data);
									uni.hideLoading();
									uni.navigateTo({
										url: './summary/summary',
										success: res => {},
										fail: () => {},
										complete: () => {}
									});
								 }
							});
			},
				touchStart: function(e) {
							this.startX = e.mp.changedTouches[0].pageX;
						},
						
				touchEnd: function(e) {
							this.endX = e.mp.changedTouches[0].pageX;
							if (this.endX - this.startX > 200) {
								uni.navigateBack();
							}
						}
		}
	}
</script>

<style scoped>
	@import '../../common/uni.css';
	.footer {
		margin-top: 80upx;
	}
	
	.cell-pd {
		padding: 20upx 30upx;
	}

	.uni-textarea {
		width: auto;
		padding: 20upx 25upx;
		line-height: 1.6;
		height: 150upx;
	}
	.uni-list::before {
		height: 0;
	}
	.uni-list:after {
		height: 0;
	}
	.list-pd {
		margin-top: 0;
	}
	.close-view{
	    text-align: center;
		line-height:30upx;
		height: 35upx;
		width: 35upx;
		background: #ef5350;
		color: #FFFFFF;
		position: absolute;
		top: 1upx;
		right: 1upx;
		font-size: 35upx;
		border-radius: 8upx;
	}
	.page {
		width: 750upx;
		height: 100%;
	}
</style>
