<template>
	<view class="skudetail">
		<view class="title">
			<text>{{group.name}}</text>
		</view>
		<view class="items">
			<view class="item" v-for="item in group.spec_items" :key="item.id">
				<view class="itemd" :class="[{'primary':selectedSpecs.findIndex((i) => i.id === item.id) > -1},
				{'disadleS':unSelectSpecs.includes(item.id)}]"
					@click="clickSpec(item)">
					{{item.name}}
				</view>

			</view>
		</view>
	</view>
</template>
<script>
	export default {
		props: {
			group: {
				type: Object,
				default: () => {}
			},
			selectedSpecs: {
				type: Array,
				default: () => []
			},
			unSelectSpecs:{
				type: Array,
				default: () => []
			}
		},
		data() {
			return {}
		},
		onLoad() {},
		methods: {
			clickSpec(item) {
				if(this.unSelectSpecs.includes(item.id)) return
				this.$emit('change', item, this.group)
			}
		}
	}
</script>
<style lang="scss" scoped>
	.skudetail {
		.title {
			margin-top: 15rpx;
			margin-bottom: 20rpx;
		}

		.items {
			display: flex;
			flex-wrap: wrap;

			.itemd {
				display: inline-block;
				margin-right: 15rpx;
				margin-bottom: 20rpx;
				padding: 10rpx 20rpx;
				min-width: 220rpx;
				line-height: 35rpx;
				font-size: 26rpx;
				color: #2D3341;
				text-align: center;
				background: #F4F5F7;
				border: 2rpx solid #F4F5F7;
				border-radius: 32rpx;
			}
		}

		.primary {
			background: linear-gradient(150deg, #FF6B59 0%, #FF2525 100%) !important;
			color: #ffffff !important;
		}
		.disadleS{
			background-color:#808080 !important;
			color: #ffffff !important;
		}
	}
</style>
