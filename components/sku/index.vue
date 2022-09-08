<template>
	<view class="selectsku">
		<u-popup v-model="show" mode="bottom" height="90%">
			<view class="selectBox">
				<SKUDETAIL v-for="group in specGroups" :key="group.id" :group="group" 
				@change="changeSelectedSpecs" :selectedSpecs="selectedSpecs" :unSelectSpecs="unSelectSpecs"></SKUDETAIL>
			</view>
		</u-popup>
	</view>
</template>
<script>
	import SKUDETAIL from "./detail.vue"
	export default {
		props: {
			specGroups:{
				type:Array,
				default:()=>[]
			},
			skus:{
				type:Object,
				default:()=>{}
			}
		},
		components:{
			SKUDETAIL
		},
		data() {
			return {
				show: false,
				selectedSpecs:[],
				
			}
		},
		onLoad() {},
		watch:{
		},
		computed:{
			// 计算或者筛选不可选的状态
			unSelectSpecs(){
				let unArr = []
				if(this.selectedSpecs.length == 0){
					return
				}
				let selectId = this.selectedSpecs.map(item=>{return item.id})
				for (let key in this.skusArray) {
					let arr = this.getSame(this.skusArray[key].specs,selectId)
					if(arr.length > 0){
						if(this.skusArray[key].stock_num == 0){
							unArr.push(this.skusArray[key])
						}
					}
				}
				let newarr = unArr.map(item=> {return item.specs})
				return newarr.flatMap(item=>item)
			},
			// 计算所有规格组成的数组
			skusArray(){
				for (let key in this.skus) {
					this.skus[key].specs = key.split(';')
				}
				return this.skus
			}
		},
		methods: {
			// 比较数组相同的元素
			getSame(arr1,arr2){
				return [...new Set(arr1)].filter(item => 
				    arr2.includes(item)
				)
			},
			changeShow(item) {
				let arr = item.split(';') || []
				if(arr.length > 0){
					let newArr = []
					arr.forEach(item=>{
						newArr.push({
							id:item
						})
					})
					this.selectedSpecs = newArr
				}else{
					this.selectedSpecs = []
				}
				
				this.show = true
			},
			// 
			changeSelectedSpecs(item,group){
				// item为选中的规格，group为选中的规格所包含的数组
				const index = this.selectedSpecs.findIndex((i) => group.spec_items.findIndex((b) => b.id === i.id) > -1)
				 if (index > -1) {
				    if (item.id === this.selectedSpecs[index].id) {
				      this.selectedSpecs.splice(index, 1)
				    } else {
				      // this.selectedSpecs[index] = item
					  this.$set(this.selectedSpecs,index,item)
				    }
				  } else {
				    this.selectedSpecs.push(item)
				  }
			}
		}
	}
</script>
<style lang="scss" scoped>
	.selectsku {
		.selectBox{
			padding: 20rpx;
		}
	}
</style>
