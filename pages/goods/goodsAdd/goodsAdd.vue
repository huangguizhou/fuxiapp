<template>
	<view class="v-add-goods-view">
		<view class="cgh-head-con">
			<view class="left">
				<view class="goods-img"  @click="onSelImg">
					<image @error="handleError()" :src="goodImg"></image>
				</view>
			</view>
			<view class="right">
				<view class="head-item">
					<view class="head-title">
						<text class="mark">*</text>货号
					</view>
					<view class="head-val">
						<input type="text" v-if="typeRq === 1"  v-model="goodsInfo.code" maxlength="100" />
						<input type="text" v-if="typeRq === 2 "  disabled v-model="goodsInfo.code" />
					</view>
				</view>
				<view class="head-item">
					<view class="head-title">
						<text class="mark">*</text>品名
					</view>
					<view class="head-val">
						<input type="text" v-model="goodsInfo.name" maxlength="100" />
					</view>
				</view>
				<view class="head-item">
					<view class="head-title">
						<text class="mark">*</text>类别
					</view>
					<view class="head-val"  @click="opentType(1)">
						<input type="text" disabled v-model="selRadioList.classType" maxlength="100" />
					</view>
					<view class="content-right">
						<image src="../../../static/base/right.png"></image>
					</view>
				</view>
			</view>
		</view>
		<view class="v-goods-content">
			<view class="v-goods-brand" @click="opentType(2)">
				<view class="v-input-title">
					<text>品牌</text>
				</view>
				<view class="v-input">
					<input type="text"  v-model="selRadioList.brandType" disabled="true" />
				</view>
				<view class="content-right"><image class="base-right" src="../../../static/base/right.png"></image></view>
			</view>
			<view class="v-goods-brand" @click="opentType(3)">
				<view class="v-input-title">
					<text>年份</text>
				</view>
				<view class="v-input">
					<input type="text"  v-model="selRadioList.ageType" disabled="true" />
				</view>
				<view class="content-right"><image class="base-right" src="../../../static/base/right.png"></image></view>
			</view>
			<view class="v-goods-brand" @click="opentType(4)">
				<view class="v-input-title">
					<text>季节</text>
				</view>
				<view class="v-input">
					<input type="text"  v-model="selRadioList.seasonType" disabled="true" />
				</view>
				<view class="content-right"><image class="base-right" src="../../../static/base/right.png"></image></view>
			</view>
			<view class="v-goods-brand" @click="opentType(5)">
				<view class="v-input-title">
					<text>厂商</text>
				</view>
				<view class="v-input">
					<input type="text"   v-model="selRadioList.supplierType" disabled="true" />
				</view>
				<view class="content-right"><image class="base-right" src="../../../static/base/right.png"></image></view>
			</view>
			<view class="v-goods-brand">
				<view class="v-input-title">
					<text>厂商货号</text>
				</view>
				<view class="v-input">
					<input type="text" v-model="goodsInfo.suppliercode" />
				</view>
			</view>
			<view class="v-goods-brand">
				<view class="v-input-title">
					<text>进货价</text>
				</view>
				<view class="v-input">
					<input type="number"  v-model="goodsInfo.purchaseprice" />
				</view>
			</view>
			<view class="v-goods-brand">
				<view class="v-input-title">
					<text>零售价</text>
				</view>
				<view class="v-input">
					<input type="number"  v-model="goodsInfo.retailsales" />
				</view>
			</view>
			<view class="v-goods-brand">
				<view class="v-input-title">
					<text>批发价</text>
				</view>
				<view class="v-input">
					<input type="number" v-model="goodsInfo.tradeprice" />
				</view>
			</view>
			<view class="v-goods-brand" @click="opentType(6)">
				<view class="v-input-title">
					<text class="mark">*</text><text>颜色</text>
				</view>
				<view class="v-input">
					<input type="text"  v-model="selRadioList.colorType" disabled="true" />
				</view>
				<view class="content-right"><image class="base-right" src="../../../static/base/right.png"></image></view>
			</view>
		</view>
		<view class="v-save" v-if="!isShowType" @click="save" >
			<text>保存</text>
		</view>
		<!-- 点击图片选择 -->
		<view class="cgh-img-alet-con" v-if="isShowImg">
			<view class="alert-black" @click="isShowImg = false"></view>
			<view class="img-con">
				<view class="alert-btn">
					<view class="btn"  @click="uploadImg">上传图片</view>
					<view class="btn" @click="previewImage">查看大图</view>
					<view class="btn" @click="isShowImg = false">取消</view>
				</view>
			</view>
		</view>
		<!-- 单选 -->
		<view  v-if="isShowType" >
			<radioItemsSearch  v-if="typeNumber === 1"  :list="classTypeInfo"  title="货品类别" placeholderTitle="关键字搜索"  @closeAlert="closeAlert" @search="search"  @okRadioValue="okRadioValue()"></radioItemsSearch>
			<radioItemsSearch  v-if="typeNumber === 2" :list="brandTypeInfo"  title="所属品牌" placeholderTitle="关键字搜索"  @closeAlert="closeAlert" @search="search"  @okRadioValue="okRadioValue()"></radioItemsSearch>
			<radioItem v-if="typeNumber === 3" :list="ageTypeInfo" @closeAlert="closeAlert"  @okRadioValue="okRadioValue"></radioItem>
			<radioItem v-if="typeNumber === 4" :list="seasonTypeInfo" @closeAlert="closeAlert"  @okRadioValue="okRadioValue"></radioItem>
			<radioItemsSearch  v-if="typeNumber === 5" :moreType="typeNumber" :list="supplierTypeInfo" :isPage="isPage" :isMore="supplierisMore"  title="所属厂商" placeholderTitle="关键字搜索"  @closeAlert="closeAlert" @search="search"  @okRadioValue="okRadioValue()" @moreTypeInfo="moreTypeInfo"></radioItemsSearch>
			<selType v-if="typeNumber === 6" :selTypeChidenItem="selTypeChidenItem" :classTypeInfo="colorTypeInfo" @onType="onType" @resetType="resetType"  @closeMoreType="closeMoreType" @okType="okType" :moduleType="moduleType"></selType>
		</view> 
	</view>
</template>

<script>
	import radioItem from '../../../components/radioItem.vue';
	import selType from '../../../components/selType.vue';
	import { pricePass } from '../../../components/filter/index.js';
	import radioItemsSearch from '../../../components/radioItemsSearch';
	export default {
		data() {
			return {
				goodImg: '../../../static/err_img.png',
				goodsInfo: {
					  age: "",
					  brandid: "",
					  code: "",
					  goodscolor: "",
					  goodsid: "",
					  goodstypeid: "",
					  name: "",
					  purchasePrice: '',
					  retailsales:'',
					  season: "",
					  suppliercode:"",
					  supplierid: "",
					  tradePrice: '',
				},
				isShowRadio: false,
				selRadioList: {classType: '', brandType: '', ageType: '', seasonType: '', supplierType: '', colorType:''},
				//
				classTypeInfo: [],
				brandTypeInfo: [],
				ageTypeInfo: [],
				seasonTypeInfo: [],
				supplierTypeInfo: [],
				colorTypeInfo: [],
				isShowType: false,
				typeNumber: 1,
				moduleType: 10,
				selTypeChidenItem: [],
				goodsId: '',
				url: '/fuxi/goods/add-goods',
				typeRq: 1, // 1: 新增 2: 修改
				isonSave: false,
				keyword: '',
				// 弹框分页
				isPage: true,
				size: 10,
				supplierPage: 1,  // 厂商
				supplierisMore: true,
				isShowImg: false
			}
		},
		onBackPress(options) {  
		    if (options.from === 'navigateBack') {  
		        return false;  
		    } 
			this.$API.tab('../../tab/main/main');
		    return true;  
		}, 	
		onLoad(option) {
			 this.goodsId = option.id;
			if (this.goodsId !== '' && this.goodsId !== undefined && this.goodsId !== null) {
				uni.setNavigationBarTitle({title: '编辑货品'});
				this.isonSave = true;
				this.url = '/fuxi/goods/update-goods';
				this.typeRq = 2;
				this.getDetailsInfo();
			}
			if (this.goodsId === '' || this.goodsId === undefined || this.goodsId === null) {
				this.$API.generateNo(1).then(res => {
					this.goodsInfo.code = res;
				});
			}
		},
		methods: {
			handleError () { // 图片加载错误
			    this.goodImg = '../../../static/err_img.png';  
			},
			getDetailsInfo () { // 获取商品详情
				this.$API.get('/fuxi/goods/query-goods', {goodsId: this.goodsId}).then(res => {
					if (res.code === 'success') { 
						this.goodsInfo = res.data;
						this.goodImg = this.$URL + res.data.code + '.jpg';
						this.selRadioList = {classType: res.data.goodsType, brandType: res.data.brand, ageType: res.data.age, seasonType: res.data.season, supplierType: res.data.supplierName, colorType: res.data.goodscolorName};
					}
				});
			},
			closeAlert () {
				this.isShowType = false;
				this.keyWord = '';
			},
			search (str) { // 弹框搜索
				this.keyword = str;
				this.opentType(this.typeNumber);
			},
			okRadioValue (val) { // 确定选择类别
				this.isShowType = false;
				if (this.typeNumber === 1) {
					this.goodsInfo.goodstypeid = val.id;
					this.selRadioList.classType = val.name;
					if (this.goodsInfo.name === '') {
						this.goodsInfo.name = val.name;
					}
				} else if (this.typeNumber === 2) {
					this.goodsInfo.brandid = val.id;
					this.selRadioList.brandType = val.name;
				} else if (this.typeNumber === 3) {
					this.goodsInfo.age = val.id;
					this.selRadioList.ageType = val.name;
				} else if (this.typeNumber === 4) {
					this.goodsInfo.season = val.id;
					this.selRadioList.seasonType = val.name;
				} else if (this.typeNumber === 5) {
					this.goodsInfo.supplierid = val.id;
					this.selRadioList.supplierType = val.name;
				}
			},
			uploadImg () { // 选择图片
				this.$API.uploadChooseImage(this.goodsInfo.code).then(res => {
					this.goodImg = res;
					this.isShowImg  = false;
				});
			},
			onSelImg () { // 打开图片选择
				this.isShowImg = true;
			},
			previewImage () {  // 预览图片
				if (this.goodImg === '../../../static/err_img.png' || this.goodImg === '') {
					uni.showToast({
						title: '请先选择一张图片!',
						icon: 'none'
					});
					return;
				}
				let imgs = [this.goodImg];
				uni.previewImage({
				    urls: imgs
				});
				this.isShowImg = false;
			},
			onType (index) { // 选择颜色
				this.colorTypeInfo[index].flg = !this.colorTypeInfo[index].flg;
			},
			okType () { // 确定选择颜色
				this.isShowType = false;
				let selArr = '';
				let id = '';
				for (let i = 0; i < this.colorTypeInfo.length; i++) {
					if (this.colorTypeInfo[i].flg === true) {
						if (selArr === '') {
							selArr = selArr + this.colorTypeInfo[i].name;
							id = id + this.colorTypeInfo[i].id;
						} else {
							selArr = selArr + ','+ this.colorTypeInfo[i].name;
							id = id + ','+ this.colorTypeInfo[i].id;
						}
					}
				}
				this.goodsInfo.goodscolor = id;
				this.selRadioList.colorType = selArr;
			},
			opentType (index) { // 打开筛选类型
				this.typeNumber = index;
				this.isShowType = true;
				uni.showLoading({
					title: '加载中...'
				});
				if (index === 1) {
					this.$API.get('/fuxi/select/query-goods-type', {keyword: this.keyword}).then(res => {
						uni.hideLoading();
						if (res.code === 'success') {
							this.classTypeInfo = this.$API.fmtSelData(res.data, 11);
						}
					});
				} else if (index === 2) {
					this.$API.get('/fuxi/select/query-brand', {keyword: this.keyword}).then(res => {
						uni.hideLoading();
						if (res.code === 'success') {
							this.brandTypeInfo = this.$API.fmtSelData(res.data, 2);
						}
					});
				} else if (index === 3) {
					if (this.ageTypeInfo.length > 0) {
						uni.hideLoading();
						return;
					}
					this.$API.get('/fuxi/select/query-age').then(res => {
						uni.hideLoading();
						if (res.code === 'success') {
							this.ageTypeInfo = this.$API.fmtSelData(res.data, 1);
						}
					});
				} else if (index === 4) {
					if (this.seasonTypeInfo.length > 0) {
						uni.hideLoading();
						return;
					}
					this.$API.get('/fuxi/select/query-season').then(res => {
						uni.hideLoading();
						if (res.code === 'success') {
							this.seasonTypeInfo = this.$API.fmtSelData(res.data, 12);
						}
					});
				} else if (index === 5) {
					this.supplierPage = 1;
					this.supplierisMore = true;
					this.$API.get('/fuxi/select/query-supplier',  {keyword: this.keyword, pageNum: this.supplierPage, pageSize: this.size}).then(res => {
						uni.hideLoading();
						if (res.code === 'success') {
							this.supplierTypeInfo = this.$API.fmtSelData(res.data.list, 13)
							if (this.supplierPage === res.data.pages) {
								this.supplierisMore = false;
							} else {
								this.supplierisMore = true;
							}
						}
					});
				} else if (index === 6) {
					this.colorTypeInfo = [];
					this.$API.get('/fuxi/select/query-color',  {keyword: this.keyword}).then(res => {
						uni.hideLoading();
						if (res.code === 'success') {
							let arr =  this.$API.fmtSelData(res.data, 3);
							if (this.goodsInfo.goodscolor !== '' ) {
								let selColor = (this.goodsInfo.goodscolor).split(',');
								for (let i = 0; i < arr.length; i++) {
									let info = {id: arr[i].id, name: arr[i].name, flg: false};
									for (let j = 0; j < selColor.length; j++) {
										if (arr[i].id === selColor[j]) {
											info.flg = true;
										}
									}
									this.colorTypeInfo.push(info);
								}
							} else {
								this.colorTypeInfo =  this.$API.fmtSelData(res.data, 3);
							}
						}
					});
				}
			},
			moreTypeInfo (index) { // 更新弹框信息
				 if (index === 5) {
					this.supplierPage++;
					this.$API.get('/fuxi/select/query-supplier',  {keyword: this.keyword, pageNum: this.supplierPage, pageSize: this.size}).then(res => {
						if (res.code === 'success') {
							let list = this.$API.fmtSelData(res.data.list, 13);
							this.supplierTypeInfo = this.supplierTypeInfo.concat(list);
							if (this.supplierPage === res.data.pages) {
								this.supplierisMore = false;
							} else {
								this.supplierisMore = true;
							}
						}
					});
				} 
			},
			closeMoreType () { // 关闭多选
				let list = this.selTypeChidenItem;
				for (let i = 0; i < list.length; i++) {
					this.colorTypeInfo[list[i]].flg = false;
				}
				this.okType();
			},
			resetType () {// 重置多选
				for (let i = 0; i < this.colorTypeInfo.length; i++) {
					this.colorTypeInfo[i].flg = false;
				}
				this.okType();
			},
			save () {
				if (this.goodsInfo.code === '' || this.goodsInfo.code === undefined || this.goodsInfo.code === null ) {
					uni.showToast({
						title: '请输入货号',
						icon: 'none'
					});
					return;
				}
				if (this.goodsInfo.name === '' || this.goodsInfo.name === undefined || this.goodsInfo.code === null) {
					uni.showToast({
						title: '请输入品名',
						icon: 'none'
					});
					return;
				}
				if (this.goodsInfo.goodstypeid === '' || this.goodsInfo.goodstypeid === undefined || this.goodsInfo.goodstypeid === null) {
					uni.showToast({
						title: '请输入类别',
						icon: 'none'
					});
					return;
				}
				let purchasePrice = this.goodsInfo.purchaseprice;
				if (purchasePrice !== '' && purchasePrice !== undefined && purchasePrice !== null ) {
					if (!pricePass(purchasePrice)) {
						uni.showToast({
							title: '请输入正确的进货价!',
							icon: 'none'
						});
						return;
					}
				}
				let retailsales = this.goodsInfo.retailsales;
				if (retailsales !== '' && retailsales !== undefined && retailsales !== null ) {
					if (!pricePass(retailsales)) {
						uni.showToast({
							title: '请输入正确的零售价!',
							icon: 'none'
						});
						return;
					}
				}
				let tradePrice = this.goodsInfo.tradeprice;
				if (tradePrice !== '' && tradePrice !== undefined && tradePrice !== null ) {
					if (!pricePass(tradePrice)) {
						uni.showToast({
							title: '请输入正确的批发价!',
							icon: 'none'
						});
						return;
					}
				}
				if (this.goodsInfo.goodscolor === '') {
					uni.showToast({
						title: '请选择颜色!',
						icon: 'none'
					});
					return;
				}
				let method = 'POST';
				if (this.typeRq === 2) {
					method = 'PUT'
				}
				this.$API.post(this.url, this.goodsInfo, method).then(res => {
					if (res.code === 'success') {
						this.$API.upload(this.goodsInfo.code, this.goodImg);
						if (this.typeRq === 2) {
							uni.showToast({
								title: '编辑成功!'
							});	
							
							setTimeout(() => {
								this.$API.to('../../goods/goodsList/goodsList');
							}, 2000)
						} else {
							uni.showToast({
								title: '添加成功!'
							});	
							this.isonSave = true;
							this.resetData();
						}
						
					}
				});
			},
			resetData () {
				this.goodImg = '../../../static/err_img.png';
				this.goodsInfo =  {
					  age: "",
					  brandid: "",
					  code: "",
					  goodscolor: "",
					  goodsid: "",
					  goodstypeid: "",
					  name: "",
					  purchasePrice: '',
					  retailsales:'',
					  season: "",
					  suppliercode:"",
					  supplierid: "",
					  tradePrice: ''
				};
				this.isShowRadio = false;
				this.selRadioList = {classType: '', brandType: '', ageType: '', seasonType: '', supplierType: '', colorType:''};
				this.classTypeInfo = [];
				this.brandTypeInfo =  [];
				this.ageTypeInfo = [];
				this.seasonTypeInfo = [];
				this.supplierTypeInfo = [];
				this.colorTypeInfo = [];
				this.isShowType = false;
				this.typeNumber = 1;
				this.moduleType = 10;
				this.selTypeChidenItem = [];
				this.goodsId = '';
				this.url = '/fuxi/goods/add-goods';
				this.typeRq = 1;
				this.$API.generateNo().then(res => {
					this.goodsInfo.code = res;
				});
			}
		},
		components: {
			radioItem,
			selType,
			radioItemsSearch
		}
	}
</script>

<style lang="scss">
	@import "../../../components/mixin.scss";
	.v-add-goods-view {
		width: 100%;
		overflow: hidden;
		font-size: 30upx;
		padding-bottom: 110upx;
		// 头部
		.cgh-head-con {
			width: 94%;
			overflow: hidden;
			padding: 0upx 3%;
			display: flex;
			justify-items: center;
			align-content: center;
			padding-top: 20upx;
			background: #fff;
			.left {
				width: 33%;
				margin-right: 2%;
				image {
					width: 98%;
					height: 220upx;
					vertical-align: middle;
				}
			}
			.right{
				width: 66%;
				overflow: hidden;
				color: #333;
				font-size: 28upx;
				.head-item {
					display: flex;
					align-content: center;
					border-bottom: 1upx solid $boder-se;
					.head-title {
						width: 15%;
						line-height: 80upx;
					}
					.head-val {
						width: 78%;
						input {
							width: 100%;
						}
					}
				}
			}
		}
		.content-right {
			width: 5%;
			margin-left: 2%;
			image {
				@include cgh-right-img();
				vertical-align: middle;
				margin-top: 30upx;
			}
		}
		// 中间内容
		.v-goods-content {
			width: 100%;
			overflow: hidden;
			background: #fff;
			.v-goods-brand view:last-child {
				border-bottom: 0upx;
			}
			.v-goods-brand {
				width: 100%;
				overflow: hidden;
				display: flex;
				align-items: center;
				border-bottom: 1upx solid $boder-se;
				.v-input-title {
					width: 20%;
					margin-left: 15upx;
					line-height: 80upx;
				}
				.v-input {
					width: 69%;
					height: 80upx;
					margin-top: 20upx;
					input {
						width: 100%;
					}
				}
			}
		}
		.mark {
			color: red;
		}
		.v-save {
			background-color: #FFFFFF;
			position: fixed;
			bottom: 0;
			width: 100%;
			box-shadow: 5upx 0 0 0 #ECECEC;
			width: 100%;
			height: 90upx;
			font-size: 36upx;
			color: #FFFFFF;
			background-color: #427CAC;
			text-align: center;
			line-height: 90upx;
			z-index: 9;
		}
		input {
			text-align: right;
			height: 80upx;
			line-height: 80upx;
		}
		// 弹框查看图片
		.cgh-img-alet-con {
			.alert-black  {
				@include cgh-alert-black();
			}
			.img-con {
				width: 70%;
				height: 380upx;
				position: fixed;
				overflow:auto;
				top: 20%;
				left: 15%;
				z-index: 11;
				background: #fff;
				.btn {
					width: 90%;
					height: 70upx;
					margin-left: 5%;
					background: $themeColor;
					text-align: center;
					color: #fff;
					font-size: 30upx;
					margin-top: 40upx;
					line-height: 70upx;
				}
				.btn:first-child {
					background: orange;
				}
				.btn:last-child {
					background: #0EA391;
				}
			}
		}
	}
</style>
