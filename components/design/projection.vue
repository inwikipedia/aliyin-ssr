<template>
	<div class="color-box">
		<!-- 我的专属颜色 -->
		<div class="my-color">
			<div>
				<span>我的专属颜色 : </span>
				<span class="color-icon"></span>
			</div>
			<div class="color-list">
				<template v-if="myColor">
					<div class="color-list-item" :style="{background: item.ColorCode}" v-for="(item,index) in myColor" :key="index"  @click="selectBg(item.ColorCode)"></div>
				</template>
				<template v-else>
					<div>
						<span>还没有专属颜色</span>
						<span>去设置</span>
					</div>
				</template>
			</div>
		</div>
		<!-- 当前模板颜色 -->
		<div class="current-tempcolor">
			<div>当前模板颜色 : </div>
			<div>
				<el-color-picker v-model="color5" @active-change="handleChange"></el-color-picker>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	// props: ['color5'],
	data () {
		return {
			color5: '#00c1de',
			// 团队专属颜色
			myColor: null,
		}
	},
	methods: {
		// 1. 获取团队专属颜色
		getTeamColors () {
			let TeamNum = localStorage['teamNum']
			this.$axios.get(this.$store.state.port.TeamColors + `?TeamNum=${TeamNum}`)
				.then(res => {
					if(!res.data) return
					this.myColor = res.data
				})
		},
		// 2. 取色器颜色上传
		handleChange (color) {
			this.selectBg(color)
		},
		// 3. 颜色值数据上行父组件
		selectBg (color) {
			this.$emit('setTextColor', color)
		}
	},
	created () {
		this.getTeamColors()
	}
}
</script>


<style lang="scss" scoped>
.color-box {
	font-size: 14px;
}
.my-color {
	.color-icon {
		display: inline-block;
		width: 45px;
		height: 15px;
		background: url(/img/desicon/background/vip_small_icon.png) no-repeat;
		background-size: 100%;
		margin-left: 10px;
		margin-bottom: -3px;
	}
	.color-list {
		padding-top: 10px;
		display: flex;
		flex-wrap: wrap;
		width: 100%;
		.color-list-item {
			width: 26px;
			height: 26px;
			margin-right: 10px;
			margin-bottom: 10px;
			border-radius:3px;
			transition: all .3s;
			cursor: pointer;
			&:hover{
				transform: scale(1.1);
				box-shadow: 0 1px 8px 2px rgba(0, 0, 0, 0.3);
			}
		}
	}
}

.current-tempcolor {
	padding-top: 10px;
	.color-area{
		display: flex;
		flex-wrap: wrap;
		width: 100%;
		padding-top: 10px;
		.color-block{
			width: 26px;
			height: 26px;
			margin-right: 10px;
			margin-bottom: 10px;
			.color-item{
				width: 100%;
				height: 100%;
				border-radius:3px;
				transition: all .3s;
				cursor: pointer;
				&:hover{
					transform: scale(1.1);
					box-shadow: 0 1px 8px 2px rgba(0, 0, 0, 0.3);
				}
			}
			.color-item:first-child {
				border: 1px solid #ccc;
			}
		}
	}
}
</style>

