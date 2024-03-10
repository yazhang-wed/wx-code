<template>
	<view class="model">
		<view class="left-model">
			<view :class="['nav-item',{ 'active': isActive === index }]" v-for="(item,index) in navList" :key="index">
				<view class="b"></view>
				<view class="b"></view>
				<view class="nav-text" @click="checked(index)">
					{{item.title}}
				</view>
			</view>
		</view>
		<view class="right-model">
			<view class="imgage-model" v-for="(item,index) in childList" :key="index" @click="checkedImage(item.url)">
				<view class="title-code">{{item.title}}</view>
				<image class="imgage-code" mode="aspectFit" :src="item.url"></image>
			</view>
		</view>
		<view class="hidden-model" v-show="showView">
			<view class="delete-button" @click="checkedHidden">
				X
			</view>
			<view class="imgage-div">
				<image class="imgage" mode="aspectFit" :src="selecedUrl" show-menu-by-longpress="true">
				</image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isActive: 0,
				navList: [{
						title: '广东',
						child: [{
							url: '/static/logo.png',
							title: '广东测试1'
						}, {
							url: '/static/logo.png',
							title: '广东测试2'
						}, {
							url: '/static/logo.png',
							title: '广东测试3'
						}, {
							url: '/static/logo.png',
							title: '广东测试4'
						}, {
							url: '/static/logo.png',
							title: '广东测试5'
						}, {
							url: '/static/logo.png',
							title: '广东测试6'
						}]
					},
					{
						title: "湖南",
						child: [{
							url: '/static/logo.png',
							title: '湖南测试1'
						}, {
							url: '/static/logo.png',
							title: '湖南测试2'
						}, {
							url: '/static/logo.png',
							title: '湖南测试3'
						}, {
							url: '/static/logo.png',
							title: '湖南测试4'
						}, {
							url: '/static/logo.png',
							title: '湖南测试5'
						}, {
							url: '/static/logo.png',
							title: '湖南测试6'
						}]
					},
					{
						title: "福建"
					}
				],
				selecedUrl: null,
				showView: false
			}
		},
		computed: {
			childList: function() {
				return this.navList[this.isActive].child || []
			},
		},
		methods: {
			checked(index) {
				this.isActive = index
			},
			// 点击图片显示全屏
			checkedImage(url) {
				this.showView = true
				this.selecedUrl = url
			},
			// 关闭全屏显示
			checkedHidden() {
				this.showView = false
			}
		}
	}
</script>

<style lang="scss" scoped>
	.model {
		width: 100%;
		height: 100vh;
		display: flex;

		// $back-col: rgb(73, 57, 113);
		// $back-col: #7dbfff;
		// $back-col: #33835F;
		// $back-col: #394264;
		$back-col: #1f253d; 
		$right-back-col: #f6f7fb;

		.left-model {
			width: 70px;
			height: 100%;
			overflow-y: auto;
			overflow-x: hidden;
			background-color: $back-col;
			padding-top: 15px;

			.nav-item {
				position: relative;

				.nav-text {
					position: relative;
					display: flex;
					flex-direction: row;
					align-items: center;
					justify-content: center;
					color: #fff;
					font-size: 1rem;
					padding: 15px 0;
					margin-left: 10px;
					border-top-left-radius: 20px;
					border-bottom-left-radius: 20px;
				}

				.b:nth-child(1) {
					position: absolute;
					top: -15px;
					height: 15px;
					width: 100%;
					background: #fff;
					display: none;

					&::before {
						content: "";
						position: absolute;
						top: 0;
						left: 0;
						width: 100%;
						height: 100%;
						border-bottom-right-radius: 20px;
						background: $back-col;
					}
				}

				.b:nth-child(2) {
					position: absolute;
					bottom: -15px;
					height: 15px;
					width: 100%;
					background: #fff;
					display: none;

					&::before {
						content: "";
						position: absolute;
						top: 0;
						left: 0;
						width: 100%;
						height: 100%;
						border-top-right-radius: 20px;
						background: $back-col;
					}
				}
			}

			.active {
				.nav-text {
					text-decoration: none;
					color: #000;
					background: rgb(254, 254, 254);
				}

				.b:nth-child(1),
				.b:nth-child(2) {
					display: block;
				}
			}
		}

		.right-model {
			width: calc(100% - 70px);
			height: 100%;
			overflow-y: auto;
			background-color: $right-back-col;

			.imgage-model {
				width: 100%;
				display: flex;
				flex-direction: column;
				align-items: center;
				padding: 15px 0;
				margin-bottom: 20px;
				background-color: #fff;

				&:last-child {
					margin-bottom: 0;
				}

				.imgage-code {
					object-fit: contain;
					width: 100%;
					box-sizing: border-box;
					padding: 0 20px;
					height: 140px;
				}

				.title-code {
					height: 36px;
					line-height: 36px;
					width: 100%;
					text-align: center;
					font-size: 18px;
					color: #333;
				}
			}
		}

		.hidden-model {
			position: absolute;
			left: 0;
			top: 0;
			width: 100%;
			height: 100%;
			background-color: rgba(2, 2, 2, 0.5);

			.delete-button {
				position: absolute;
				right: 0;
				top: 0;
				width: 20px;
				height: 20px;
				border-radius: 50%;
				background-color: $back-col;
				display: flex;
				justify-content: center;
				align-items: center;
				font-size: 15px;
				color: #fff;

				&:hover {
					opacity: 1;
				}
			}

			.imgage-div {
				background-color: #fff;
				height: 60%;
				width: 100%;
				position: absolute;
				top: 0;
				bottom: 0;
				left: 0;
				right: 0;
				margin: auto;
				padding: 20px;
				box-sizing: border-box;

				.imgage {
					width: 100%;
					height: 100%;
				}
			}
		}
	}
</style>