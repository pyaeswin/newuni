<template>
	<view class="wanlshop-container">
		<view
			v-if="common.modulesData.homeModules.page"
			class="wanlshop-container__head"
			:style="{
				height: headHeight + 'px',
				color: common.modulesData.homeModules.page ? common.modulesData.homeModules.page.style.navigationBarTextStyle == '#000' ? '#000' : '#333333' : ''
			}"
		>
			<view :style="{ height: headHeight + 'px', paddingTop: headTop + 'px' }" >
				<view class="toolbar flex padding-lr-bj align-center">
					<scroll-view
						class="scroll"
						scroll-x
						scroll-with-animation
						:scroll-left="scrollLeft"
					>
						<view class="scroll__item" v-for="(item, index) in common.modulesData.categoryModules" :key="item.id" :class="{ action: currentItemId === 'cid' + item.id }" @tap="handleSelect('cid' + item.id, index)" >
							{{ item.name }}
						</view>
					</scroll-view>
					<!-- <view class="category flex align-center" @tap="handleModal">
						<text v-if="isModal" class="wlIcon-fanhui3"></text>
						<text v-else class="wlIcon-fanhui4"></text>
					</view> -->
				</view>
			</view>
		</view>
		<!-- 主体 -->
		<swiper
			class="wanlshop-container__main"
			:current-item-id="currentItemId"
			:style="{
				height: windowHeight + 'px'
			}"
			@change="changeCurrent"
			@animationfinish="animationFinish"
		>
			<!-- 分类 -->
			<swiper-item
				v-for="(item, index) in common.modulesData.categoryModules"
				:key="item.id"
				:item-id="'cid' + item.id"
			>
				<wanl-shop-category
					:cid="item.id"
					:headHeight="headHeight"
					:windowHeight="windowHeight"
					:currentItemId="currentItemId"
					:homeModules="common.modulesData.homeModules"
					:childlist="item.childlist"
				/>
				<wanl-shop-list
					:cid="item.id"
					:currentItemId="currentItemId"
				/>
			</swiper-item>
		</swiper>
		<!-- 弹窗 -->
		<view class="WANL-MODAL" @touchmove.stop.prevent="moveHandle">
			<!-- 顶部 -->
			<view class="cu-modal top-modal" :class="{ show: isModal }" @tap="handleModal">
				<view
					class="cu-dialog padding-lr-bj padding-bottom-bj"
					:style="{ paddingTop: headHeight + 12 + 'px' }"
					@tap.stop=""
				>
					<view class="category text-min">
						<view
							class="item round bg-gray"
							v-for="(item, index) in common.modulesData.categoryModules"
							:key="item.id"
							:class="{ action: currentItemId === 'cid' + item.id }"
							@tap="handleSelect('cid' + item.id, index)"
						>
							{{ item.name }}
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
import { mapState, mapActions } from 'vuex';
import wanlShopList from '@/components/wanl-shop/list';

export default {
	components: {
		wanlShopList
	},
	computed: {
		...mapState(['common', 'user', 'update'])
	},
	data() {
		return {
			headHeight: 44,
			windowHeight: 0,
			headTop: 0,
			currentItemId: 'cid1',
			currentData: {},
			scrollLeft: 0,
			isModal: false,
		};
	},
	onLoad() {
		//设置tabbar栏语言
		this.langType();
	},
	onShow() {
		// #ifdef APP-PLUS
		plus.navigator.setFullscreen(false);
		// #endif
		// 计算页面尺寸
		let sys = this.$wanlshop.wanlsys();
		this.headTop = sys.top;
		// this.headHeight = sys.height + uni.upx2px(60);
		this.headHeight = 44;
		this.windowHeight = sys.windowHeight;
	},
	methods: {
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
		// 选择Tag
		handleSelect(id, index) {
			this.currentItemId = id;
			this.scrollLeft = (index - 1) * 50;
		},
		// 弹出层
		handleModal() {
			this.isModal = !this.isModal;
		},
		// 动画
		animationFinish(e) {
			//#ifdef APP-PLUS
			this.changeCurrent(e);
			//#endif
		},
		// 滚动的tag
		changeCurrent(e) {
			this.currentItemId = e.detail.currentItemId;
			this.scrollLeft = (e.detail.current - 1) * 50;
		},
		// 搜索
		handleSearch(text) {
			this.$wanlshop.to(`/pages/page/search?type=goods&keywords=${text}`, 'fade-in', 100);
		},
		//禁止父元素滑动 1.0.3升级
		moveHandle() {}
	}
};
</script>

<style lang="scss">
	.wanlshop-container__head {
		background-color: #fff;
	}
.lang_list{
	background-color: #fff;flex: 1 1 0%; width: 130px; position: absolute; right: 0px;z-index: 99;box-shadow: 0 0 5px 2px #ccc;border-radius: 7px;
}
.lang_item{
	margin: 15px 10px;
}
.lang_item_view{
	display: flex;flex-direction: row;align-items: center;
}
.lang_item_img{
	width: 34px; height: 20px;will-change: transform;
}
.lang_item_text{
	color: rgb(96, 98, 102); font-size: 15px; margin: 0px 0px 0px 14px;
}
.wanlshop-container {
	&__head {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 999;
		background-size: 100% auto;
		background-repeat: no-repeat;
		.navigater {
			height: 86rpx;
			padding-left: 25rpx;
			padding-right: 25rpx;
			/* #ifdef MP */
			padding-right: 220rpx;
			/* #endif */
			.search {
				flex: 1;
				background-color: #fff;
				height: 66rpx;
				border: 2rpx solid #fff;
				.icon {
					margin: 0 20rpx;
				}
				swiper {
					height: 100%;
					width: 100%;
					margin-right: 10rpx;
					swiper-item {
						display: flex;
						align-items: center;
					}
				}
			}
		}
		.toolbar {
			.scroll {
				flex: 1;
				white-space: nowrap;
				overflow: hidden;
				width: 100%;
				&__item {
					position: relative;
					z-index: 2;
					font-size: 28rpx;
					display: inline-flex;
					height: 58rpx;
					align-items: center;
					margin-right: 40rpx;
					&.action {
						position: relative;
						font-weight: bold;
						font-size: 30rpx;
						&::after {
							content: ' ';
							position: absolute;
							bottom: 0;
							left: 50%;
							-webkit-transform: translateX(-50%);
							transform: translateX(-50%);
							height: 6rpx;
							width: 40rpx;
							border-radius: 100px;
							background-color: #3c9cff;
						}
					}
				}
			}
			.category {
				box-shadow: #eee -16rpx 0 16rpx -16rpx;
				height: 58rpx;
				font-size: 28rpx;
				padding-left: 25rpx;
			}
		}
	}
	&__main {
		position: relative;
		z-index: 99;
	}
	.WANL-MODAL {
		.cu-modal {
			&.top-modal {
				background: rgba(0, 0, 0, 0.6);
				text-align: inherit;
				.cu-dialog {
					background: #fff;
					border-radius: 0 0 18rpx 18rpx;
					.category {
						display: grid;
						grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
						grid-auto-flow: row dense;
						grid-gap: 16rpx;
						.item {
							display: flex;
							align-items: center;
							justify-content: center;
							padding: 12rpx 0;
							border: 2rpx solid transparent;
							&.action {
								background-color: transparent;
								border-color: #f40;
								color: #f40;
								font-weight: bold;
							}
						}
					}
				}
			}
		}
	}
}
</style>