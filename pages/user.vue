<template>
	<!-- <view v-if="user.isLogin"> -->
	<view>
		<!-- #ifdef MP -->
		<view class="cu-custom text-white" :style="{color: common.appStyle.user_font_color == 'light'?'#ffffff':'#222222'}">
			<view class="cu-bar fixed" :style="{ height: $wanlshop.wanlsys().height + 'px', paddingTop: $wanlshop.wanlsys().top + 'px' }">
				<view class="action mp text-bold">
					<text class="wlIcon-shezhi" style="margin-right: 0.6em;" @tap="$wanlshop.auth('/pages/user/setting/setting')"></text>
					<text class="wlIcon-shiyongbangzhu1" @tap="help"></text>
				</view>
				<!-- 背景 -->
				<view class="bar-bg" v-if="headerOpacity > 0" :style="{ 
					height: $wanlshop.wanlsys().height + 'px', 
					opacity: headerOpacity,
					backgroundColor: common.appStyle.user_nav_color?common.appStyle.user_nav_color:'#f7f7f7',
					backgroundImage: 'url(' + $wanlshop.oss(common.appStyle.user_nav_image, 414, 0, 1, 'transparent', 'png') + ')'
				}"></view>
			</view>
		</view>
		<!-- #endif -->
		<!-- #ifndef MP -->
		<view class="cu-custom" :style="{color: common.appStyle.user_font_color == 'light'?'#ffffff':'#222222'}">
			<view class="cu-bar fixed" :style="{ height: $wanlshop.wanlsys().height + 'px', paddingTop: $wanlshop.wanlsys().top + 'px' }">
				<view class="text-lg" @tap="$wanlshop.auth('/pages/user/setting/user')">
					<view v-if="headerOpacity == 1">
						<view class="cu-avatar round margin-right-xs" :style="{ backgroundImage: 'url(' + $wanlshop.oss(user.avatar, 35, 35, 2, 'avatar') + ')' }"></view>
						<text v-if="user.isLogin">{{ user.nickname }}</text>
						<text v-else>{{$t('user.login')}} / {{$t('user.registered')}}</text>
					</view>
				</view>
				<!-- 背景 -->
				<view class="bar-bg" v-if="headerOpacity > 0" :style="{ 
					height: $wanlshop.wanlsys().height + 'px', 
					opacity: headerOpacity,
					backgroundColor: common.appStyle.user_nav_color?common.appStyle.user_nav_color:'#f7f7f7',
					backgroundImage: 'url(' + $wanlshop.oss(common.appStyle.user_nav_image, 0, 50, 1, 'transparent', 'png') + ')',
					color: common.appStyle.user_font_color == 'light'?'#ffffff':'#222222'
				}"></view>
				<view class="action">
					<block>
						<text class="wlIcon-erweima" @tap="$wanlshop.auth('/pages/user/qrcode/qrcode')"></text>
						<text class="margin-right text-sm" @tap="$wanlshop.auth('/pages/user/qrcode/qrcode')">{{$t('user.member_code')}}</text>
					</block>
					<text class="wlIcon-shezhi" @tap="$wanlshop.auth('/pages/user/setting/setting')"></text>
					<text class="wlIcon-xiaoxizhongxin" @tap="$wanlshop.to('/pages/notice/notice')"></text>
					<view class="cu-tag badge" v-if="statistics.notice.notice +statistics.notice.order +statistics.notice.chat > 0">{{ statistics.notice.notice +statistics.notice.order +statistics.notice.chat }}</view>
				</view>
			</view>
		</view>
		<!-- #endif -->
		<view class="wanl-user" :style="{ 
			backgroundColor: common.appStyle.user_bg_color?common.appStyle.user_bg_color:'#f7f7f7',
			backgroundImage: 'url(' + $wanlshop.oss(common.appStyle.user_bg_image, 414, 0, 1, 'transparent', 'png') + ')',
			color: common.appStyle.user_font_color == 'light'?'#ffffff':'#222222'}">
		<view style="padding-left: 14px; padding-right: 14px;">
			<view class="user" :style="{ paddingTop: $wanlshop.wanlsys().height + 'px' }">
				<view class="avatar margin-right-bj" @tap="portrai"><image class="round" :src="$wanlshop.oss(user.avatar, 62, 62, 2, 'avatar')" mode="widthFix"></image></view>
				<view class="content" v-if="user.isLogin">
					<view class="text-xxl" @tap="$wanlshop.auth('/pages/user/setting/user')">{{ user.nickname }}</view>
					<view class="text-sm" @tap="$wanlshop.auth('/pages/user/signin/signin')">
						<view class="cu-tag sm radius bg-orange">
							Lv {{ user.level }}
						</view>
						<view class="cu-tag sm radius bg-orange">
							{{$t('user.user_points')}} {{ user.score }}
						</view>
					</view>
				</view>
				<view class="content" @tap="$wanlshop.auth('/pages/user')" v-else>
					<view class="text-xxl">{{$t('user.login')}} / {{$t('user.registered')}}</view>
					<!-- <view class="cu-tag bg-orange sm radius">Hi</view> -->
					<view class="cu-tag wanl-bg-pink sm radius">Hi，{{$t('user.welcome_to_login')}}</view>
				</view>
			</view>
			<view class="operate">
				<view class="text-sm" @tap="$wanlshop.auth('/pages/user/collect')">
					<text class="text-bold">{{ statistics.dynamic.collection }}</text>
					{{$t('user.collection')}}
				</view>
				<view class="text-sm" @tap="$wanlshop.auth('/pages/user/coupon/mycard')">
					<text class="text-bold">{{ statistics.dynamic.concern }}</text>
					{{$t('user.my_card')}}
				</view>
				<view class="text-sm" @tap="$wanlshop.auth('/pages/user/follow')">
					<text class="text-bold">{{ statistics.dynamic.concern }}</text>
					{{$t('user.focus_store')}}
				</view>
				<view class="text-sm" @tap="$wanlshop.auth('/pages/user/footprint')">
					<text class="text-bold">{{ statistics.dynamic.footprint }}</text>
					{{$t('user.footprint')}}
				</view>
				<!-- <view class="text-sm" @tap="$wanlshop.auth('/pages/user/order/order')">
					<text class="text-bold">{{ $wanlshop.toFormat(statistics.order.whole, 'hundred') }}</text>
					{{$t('user.all_order')}}
				</view> -->
				
			</view>
		</view>
			<!-- <view class="activity padding-bj">
				<view class="bg-white radius grid text-center col-2 padding-lr-bj padding-tb-sm">
					<view class="solid-right flex justify-between" @tap="$wanlshop.auth('/pages/user/coupon/list')">
						<view class="content">
							<view class="wanl-black text-sm text-bold6">{{$t('user.card_voucher')}}</view>
							<view class="text-min text-orange">
								{{$t('user.get_coupon')}}
								<text class="wlIcon-fanhui2 margin-left-xs"></text>
							</view>
						</view>
						<view class="cu-avatar" :style="{ backgroundImage: 'url(' + $wanlshop.appstc('/user/icon_card_bag.png') + ')' }"></view>
					</view>
					<view class="flex justify-between" @tap="$wanlshop.auth('/pages/user/signin/log')">
						<view class="content margin-left-bj">
							<view class="wanl-black text-sm text-bold6">{{$t('user.my_points')}}</view>
							<view class="text-min text-orange">
								{{$t('user.points_log')}}
								<text class="wlIcon-fanhui2 margin-left-xs"></text>
							</view>
						</view>
						<view class="cu-avatar" :style="{ backgroundImage: 'url(' + $wanlshop.appstc('/user/icon_super_vip.png') + ')' }"></view>
					</view>
				</view>
			</view> -->
			<image  src="../static/images/arc.png" style="     position: relative;
			width: 100%;
			height: 19px;">
		</view>
		
	
			
		</image>
		<view class="wanl-user-order padding-sm margin-bj">
			<view class="uni-list">
				<view class="uni-list-item">
					<view class="uni-list-item__container uni-list-item--first">
						<view class="uni-list-item__content">
							<text class="uni-list-item__content-title">{{$t('user.my_order')}}</text>
						</view>
						<view class="uni-list-item__extra-text" @tap="$wanlshop.auth('/pages/user/order/order?state=0')">
							<span>{{$t('user.all_order')}} </span>
						</view>
						<text class="wlIcon-fanhui2" style="color: rgb(187, 187, 187);font-size: 15px;"></text>
					</view>
				</view>	
			</view>
			<view class="project text-sm wanl-gray-dark">
				<view @tap="$wanlshop.auth('/pages/user/order/order?state=1')">
					<text class="wlIcon-hongbao wanl-red"></text>
					{{$t('user.pending_payment')}}
					<view class="cu-tag badge bg-red" v-if="statistics.order.pay > 0">{{ $wanlshop.toFormat(statistics.order.pay, 'hundred') }}</view>
				</view>
				<view @tap="$wanlshop.auth('/pages/user/order/order?state=2')">
					<text class="wlIcon-31daifahuo wanl-red"></text>
					{{$t('user.to_be_shipped')}}
					<view class="cu-tag badge bg-red" v-if="statistics.order.delive > 0">{{ $wanlshop.toFormat(statistics.order.delive, 'hundred') }}</view>
				</view>
				<view @tap="$wanlshop.auth('/pages/user/order/order?state=3')">
					<text class="wlIcon-dianpu wanl-red"></text>
					{{$t('user.pending_receipt')}}
					<view class="cu-tag badge bg-red" v-if="statistics.order.receiving > 0">{{ $wanlshop.toFormat(statistics.order.receiving, 'hundred') }}</view>
				</view>
				<view @tap="$wanlshop.auth('/pages/user/order/order?state=4')">
					<text class="wlIcon-leimu wanl-red"></text>
					{{$t('user.to_be_commented')}}
					<view class="cu-tag badge bg-red" v-if="statistics.order.evaluate > 0">{{ $wanlshop.toFormat(statistics.order.evaluate, 'hundred') }}</view>
				</view>
				<view @tap="$wanlshop.auth('/pages/user/refund/lists')">
					<text class="wlIcon-tuikuan wanl-red"></text>
					{{$t('user.return')}}
					<view class="cu-tag badge bg-red" v-if="statistics.order.customer > 0">{{ $wanlshop.toFormat(statistics.order.customer, 'hundred') }}</view>
				</view>
			</view>
			<!-- <view class="logistics margin-top-bj padding-sm" v-if="statistics.logistics.length > 0">
				<swiper vertical autoplay circular disable-touch interval="4000" class="swiper">
					<swiper-item @tap="$wanlshop.auth('/pages/notice/logistics/details')">
						<view class="title">
							<view class="text-sm">最新物流</view>
							<view class="text-sm">18:00</view>
						</view>
						<view class="flex align-center">
							<view class="cu-avatar" :style="{ backgroundImage: 'url(' + $wanlshop.oss(user.avatar, 40, 40) + ')' }"></view>
							<view class="content">
								<view class="text-df">
									<text class="wlIcon-paisongtixing"></text>
									派送中
								</view>
								<view class="text-sm">【自提柜】已签收，签收人凭取件码 已取件。</view>
							</view>
						</view>
					</swiper-item>
				</swiper>
			</view> -->
		</view>
		

		
		<view class="wanl-user-order padding-sm margin-bj" style="margin-top: 25rpx;">
			<view class="uni-list">
				<view class="uni-list-item">
					<view class="uni-list-item__container uni-list-item--first">
						<view class="uni-list-item__content">
							<text class="uni-list-item__content-title">{{$t('user.wallet')}}</text>
						</view>
						<view class="uni-list-item__extra-text" @tap="$wanlshop.auth('/pages/user/money/money')">
							<span>{{$t('user.enter_wallet')}} </span>
						</view>
						<text class="wlIcon-fanhui2" style="color: rgb(187, 187, 187);font-size: 15px;"></text>
					</view>
				</view>	
			</view>
			<view class="project text-sm wanl-gray-dark">
				<view style="line-height: 1.8;" @tap="$wanlshop.auth('/pages/user/money/money')">
					<view class="wanl-pip text-lg text-bold6">{{ user.money?user.money:'0.00' }}</view>
					{{$t('user.balance')}}
				</view>
				<view style="line-height: 1.8;" @tap="$wanlshop.auth('/pages/user/coupon/mycard')">
					<view class="wanl-pip text-lg text-bold6">{{ statistics.dynamic.coupon }}</view>
					{{$t('user.my_card')}}
				</view>
				<!-- <view style="line-height: 1.8;" @tap="$wanlshop.auth('/pages/user/bank/bank')">
					<view class="wanl-pip text-lg text-bold6">{{ statistics.dynamic.accountbank }}</view>
					{{$t('user.bank_card')}}
				</view>
				<view style="line-height: 1.8;" @tap="$wanlshop.auth('/pages/user/signin/log')">
					<view class="wanl-pip text-lg text-bold6">{{ user.score?user.score:0 }}</view>
					{{$t('user.points')}}
				</view>
				<view @tap="$wanlshop.auth('/pages/user/money/list')">
					<text class="wlIcon-hongbao wanl-orange"></text>
					{{$t('user.billing_details')}}
				</view> -->
			</view>
			
		</view>
		
		<view class=" padding-sm margin-bj" style="position: relative;">
			<image src="../static/images/mypage.jpg" style="width: 382px; height: 110px;">
				
			</image>
		</view>
		
		
		<view class="wanl-user-tool padding-top-bj margin-lr-bj">
			<view class="list text-sm grid col-5 wanl-gray-dark">
				<!-- <view @tap="$wanlshop.auth('/pages/user/money/money')">
					<text class="wlIcon-youhuiquantuangou wanl-orange"></text>
					{{$t('user.wallet')}}
				</view> -->
				<!-- <view @tap="$wanlshop.auth('/pages/apps/groups/order/order')">
					<text class="wlIcon-pintuan2 wanl-text-yellow"></text>
					{{$t('user.group_order')}}
					<view class="cu-tag badge bg-orange" v-if="statistics.order.groups > 0">{{ $wanlshop.toFormat(statistics.order.groups, 'hundred') }}</view>
				</view> -->
			<!-- 	<view @tap="$wanlshop.auth('/pages/user/comment/comment')">
					<text class="wlIcon-icon_pinglun wanl-text-red"></text>
					{{$t('user.comments')}}
				</view> -->
				<!-- <view @tap="$wanlshop.auth('/pages/user/distribution/distribution')">
					<text class="wlIcon-fenxiao wanl-text-yellow"></text>
					分销
				</view>
				<view @tap="$wanlshop.auth('/pages/user/order/bargain')">
					<text class="wlIcon-jiage wanl-text-light-blue"></text>
					我的砍价
				</view> -->
				<view @tap="$wanlshop.auth('/pages/user/address/address')">
					<text class="wlIcon-dizhi wanl-text-yellow"></text>
					{{$t('user.address')}}
				</view>
		<!-- 		<view @tap="$wanlshop.auth('/pages/user/signin/signin')">
					<text class="wlIcon-mianxing-rili wanl-orange"></text>
					{{$t('user.sign_in')}}
				</view>
				<view @tap="$wanlshop.auth('/pages/user/complaint/lists')">
					<text class="wlIcon-31guanzhuxuanzhong wanl-text-light-blue"></text>
					{{$t('user.my_report')}}
				</view>
				<view @tap="$wanlshop.auth('/pages/user/feedback/lists')">
					<text class="wlIcon-pingjiazongjie wanl-text-blue"></text>
					{{$t('user.feedback')}}
				</view> -->
				<!-- <view @tap="help">
					<text class="wlIcon-bangzhu3 wanl-text-green"></text>
					{{$t('user.help_center')}}
				</view> -->
				<view @tap="$wanlshop.auth('/pages/user/service')">
					<text class="wlIcon-icon-service wanl-text-purple"></text>
				<!-- 	{{$t('user.customer_service_xiaomi')}} -->
				Customer Service
				</view>
				<!-- <view @tap="$wanlshop.auth('/pages/apps/find/user')">
					<text class="wlIcon-pengyouquan wanl-text-red"></text>
					{{$t('user.writing_center')}}
				</view> -->
				<view v-if="!shopdata.id" @tap="$wanlshop.auth('/pages/shop/apply/details')">
					<text class="wlIcon-pengyouquan wanl-text-red"></text>
					{{$t('user.mch_apply')}}
				</view>
				<view v-if="shopdata.id" @tap="$wanlshop.auth('/pages/shop/details')">
					<text class="wlIcon-dianpu wanl-text-yellow"></text>
					{{$t('user.mch_info')}}
				</view>
				<view v-if="shopdata.id && shopdata.mch_identity == 1" @tap="ghsApply()">
					<text class="wlIcon-guanfang wanl-text-red"></text>
					{{$t('user.mch_ghs')}}
				</view>
				<view v-if="shopdata.id && shopdata.mch_identity == 2" @tap="$wanlshop.auth('/pages/shop/addGoods')">
					<text class="wlIcon-goods wanl-text-red"></text>
					{{$t('user.add_good')}}
				</view>
				<view v-if="shopdata.id" @tap="$wanlshop.auth('/pages/shop/wholesale')">
					<text class="wlIcon-shangpin wanl-text-violet"></text>
					{{$t('user.mch_pifa')}}
				</view>
				<view v-if="shopdata.id" @tap="$wanlshop.auth('/pages/shop/upgrade')">
					<text class="wlIcon-31huiyuanqia wanl-text-light-blue"></text>
					{{$t('user.mch_level')}}
				</view>
				<view v-if="shopdata.id" @tap="$wanlshop.auth('/pages/shop/goodsList')">
					<text class="wlIcon-shangpin1 wanl-text-green"></text>
					{{$t('user.mch_good')}}
				</view>
				<view v-if="shopdata.id" @tap="$wanlshop.auth('/pages/shop/order')">
					<text class="wlIcon-dingdan1 wanl-text-purple"></text>
					{{$t('user.mch_order')}}
					<view class="cu-tag badge bg-orange">{{ $wanlshop.toFormat(shopdata.mch_order, 'hundred') }}</view>
				</view>
				
				<view @tap="setting">
					<text class="wlIcon-shezhi1 wanl-text-green"></text>
					{{$t('user.setting')}}
				</view>
			</view>
		</view>
		<!-- <view class="wanl-you-like" :style="{ backgroundImage: 'url(' + $wanlshop.appstc('/common/guess_you_like_it.png') + ')' }"></view>
		<wanl-product :dataList="likeData"/>
		<uni-load-more :status="status" :content-text="contentText" />
		<view class="edgeInsetBottom"></view> -->
	</view>
	<!-- <view v-else>
		{{$wanlshop.auth('/pages/user')}}
	</view> -->
</template>

<script>
import { mapState } from 'vuex';
export default {
	data() {
		return {
			headerOpacity: 0,
			// 上拉刷新
			reload: true,
			likeData: [],
			current_page: 1, //当前页码
			last_page: 1, //总页码
			status: 'loading',
			contentText: {
				contentdown: ' ',
				contentrefresh: '正在加载...',
				contentnomore: '没有更多数据了'
			},
			shopdata: {}
		};
	},
	computed: {
		...mapState(['user', 'statistics','common'])
	},
	onPullDownRefresh() {
		this.loadData();
	},
	onShow() {
		setTimeout(()=> {
			uni.setNavigationBarColor({  
				frontColor: this.$store.state.common.appStyle.user_font_color == 'light'?'#ffffff':'#000000',  
				backgroundColor: this.$store.state.common.appStyle.user_nav_color
		    })  
		}, 200);
	},
	onLoad() {
		//设置tabbar栏语言
		this.langType();
		if (this.$store.state.user.isLogin) {
			this.loadData();
			this.getShopInfo();
		}
		this.loadlikeData();
	},
	onPageScroll(e) {
		let tmpY = 50;
		e.scrollTop = e.scrollTop > tmpY ? 50 : e.scrollTop; //如果当前高度大于250则250否则当前高度
		this.headerOpacity = e.scrollTop * (1 / tmpY); //$headerOpacity 赋值当前高度x（1÷250）
	},
	onReachBottom() {
		//判断是否最后一页
		if (this.current_page >= this.last_page) {
			this.status = 'noMore';
		} else {
			this.reload = false;
			this.current_page = this.current_page + 1; //页码+1
			this.status = 'loading';
			this.loadlikeData();
		}
	},
	methods: {
		//获取商家信息
		async getShopInfo() {
			this.$api.get({
				url: '/wanlshop/shop/getUserShopInfo',
				success: res => {
					if (res) {
						this.shopdata = res;
					}else{
						this.shopdata = {};
					}
				}
			});
		},
		async ghsApply(){
			this.$api.get({
				url: '/wanlshop/shop/ghsApply',
				success: res => {
					this.$wanlshop.msg($t('user.mch_reply'));
					// apply success! please wait for review
				}
			});
		},
		langType(){
			uni.setTabBarItem({
			    index: 0,
			    text: this.$t('index.home')
			})
			uni.setTabBarItem({
			    index: 1,
			    text: this.$t('index.classification')
			})
			uni.setTabBarItem({
			    index: 2,
			    text: this.$t('index.cart')
			})
			uni.setTabBarItem({
			    index: 3,
			    text: this.$t('index.shop')
			})
			uni.setTabBarItem({
			    index: 4,
			    text: this.$t('index.my')
			})
		},
		async loadData() {
			this.$api.post({
				url: '/wanlshop/user/refresh',
				success: res => {
					this.$store.commit('statistics/edit', res.statistics);
					this.$store.commit('user/setUserInfo', res.userinfo);
				}
			});
			uni.stopPullDownRefresh();
		},
		// 滚动底部加载猜你喜欢
		async loadlikeData() {
			this.$api.get({
				url: '/wanlshop/product/likes?pages=user',
				data: {
					page: this.current_page
				},
				success: res => {
					this.likeData = this.reload ? res.data : this.likeData.concat(res.data); //评论数据 追加
					this.current_page = res.current_page; //当前页码
					this.last_page = res.last_page; //总页码
					this.status = res.total == 0 ? 'noMore' : 'more';
				}
			});
		},
		// 帮助
		help() {
			this.$wanlshop.to('/pages/user/help');
		},
		// 设置
		setting() {
			this.$wanlshop.to('/pages/user/setting/setting');
		},
		portrai() {
			this.$wanlshop.to('/pages/user/portrait/portrait');
		}
	}
};
</script>

<style>
.cu-bar .action.mp:first-child > text[class*='wlIcon-'] {
	margin-left: 0;
}

.wanl-user {
	background-repeat: no-repeat;
	background-size: 100%;
	position: relative;
	/* padding: 0px 14px 0; */
	position: relative;
}

.wanl-user .user {
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 0 30rpx;
	padding-bottom: 30rpx;
}

.wanl-user .user .avatar {
	position: relative;
	height: 123rpx;
	width: 123rpx;
	border-radius: 5000rpx;
	overflow: hidden;
	border: 3px solid rgba(255, 255, 255, .25);
}

.wanl-user .user .avatar image {
	height: 120rpx;
}

.wanl-user .user .avatar .tag {
	position: absolute;
	bottom: 0;
	right: 0;
}

.wanl-user .user .content {
	flex: 1;
}

/* 操作 */
.wanl-user .operate {
	display: flex;
	justify-content: space-around;
	text-align: center;
	padding-bottom: 70rpx;
	line-height: 1.3;
}

.wanl-user .operate .text-sm {
	width: 25%;
}

.wanl-user .operate text {
	display: block;
	font-size: 32rpx;
}

/* 活动 */
.wanl-user .activity {
	position: absolute;
	width: 100%;
	bottom: -80rpx;
}

.wanl-user .activity .radius {
	border-radius: 24rpx;
}

.wanl-user .activity .cu-avatar {
	width: 69rpx;
	height: 69rpx;
	margin-right: 30rpx;
	background-color: transparent;
	/* border: 1px solid rgba(255,255,255,.6); */
}

.wanl-user .activity .content {
	text-align: left;
	height: 68rpx;
}

/* 订单 */
.wanl-user-order {
/* 	margin-top: 80rpx;
 */	border-radius: 24rpx;
	background-color: white;
}

.wanl-user-order .uni-list {
	display: flex;
	background-color: #fff;
	position: relative;
	flex-direction: column;
}

.wanl-user-order .uni-list-item {
	font-size: 15px;
	position: relative;
	flex-direction: column;
	justify-content: space-between;
	padding-left: 14px;
}

.uni-list-item__container {
    position: relative;
    display: flex;
    flex-direction: row;
    padding: 11px 14px;
    padding-left: 0;
    flex: 1;
    position: relative;
    justify-content: space-between;
    align-items: center;
}

.uni-list-item__content {
    display: flex;
    flex: 1;
    overflow: hidden;
    flex-direction: column;
    color: #3b4144;
}

.uni-list-item__content-title {
    font-size: 13px;
    color: #3b4144;
    overflow: hidden;
}

.uni-list-item__extra-text {
    color: #999;
    font-size: 11px;
}

.wanl-user-order .title {
	display: flex;
	justify-content: space-between;
	align-items: flex-end;
}

.wanl-user-order .title text {}



/* 状态 */
.wanl-user-order .project {
	display: flex;
	justify-content: space-around;
	text-align: center;
}

.wanl-user-order .project>view {
	position: relative;
	flex: 1;
}

.wanl-user-order .project>view .cu-tag {
	top: -4rpx;
	right: 26rpx;
}

.wanl-user-order .project text {
	display: block;
	font-size: 50rpx;
	margin-bottom: 6rpx;
}

/* 物流 */
.wanl-user-order .logistics {
	background-color: #f9f9f9;
	border-radius: 24rpx;
}

.wanl-user-order .logistics .swiper {
	height: 120rpx;
}

.wanl-user-order .logistics .swiper .title {
	display: flex;
	justify-content: space-between;
	margin-bottom: 10rpx;
	color: #999999;
}

.wanl-user-order .logistics .swiper .cu-avatar {
	margin-right: 10rpx;
	height: 66rpx;
	width: 66rpx;
	border-radius: 12rpx;
	background-color: #ffffff;
}

.wanl-user-order .logistics .swiper .content .text-df {
	color: #3797e0;
	font-size: 27rpx;
	margin-bottom: 2rpx;
}

.wanl-user-order .logistics .swiper .content .text-sm {
	color: #999999;
}

.wanl-user-order .logistics .swiper .content text {
	font-size: 32rpx;
	margin-left: 15rpx;
	margin-right: 10rpx;
}

/* 工具箱 */
.wanl-user-tool {
	background-color: #ffffff;
	border-radius: 24rpx;
}

.wanl-user-tool .title {
	display: flex;
	justify-content: space-between;
	align-items: flex-end;
}

.wanl-user-tool .title text {
	margin-left: 2rpx;
}

/* 状态 */
.wanl-user-tool .list {
	text-align: center;
}

.wanl-user-tool .list>view {
	margin-bottom: 28rpx;
	position: relative;
}
.wanl-user-tool .list>view .cu-tag {
	top: -8rpx;
	left: 80rpx;
	right: unset;
}

.wanl-user-tool .list text {
	display: block;
	font-size: 54rpx;
	margin-bottom: 8rpx;
}
</style>
