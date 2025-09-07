@[TOC](华为云对象存储OBS 支持安卓/iOS/鸿蒙UTS组件)

# 介绍

华为云对象存储（OBS）组件，支持安卓、iOS、鸿蒙Next的UTS组件。
除了支持基础的文件上传下载之外，还支持了上传下载的进度获取，大文件上传下载的断点续传。
API概览：SDK初始化、创建存储桶、列举存储桶、存储桶是否存在、列举桶内对象、删除桶内对象、文件上传(可获取上传进度)、大文件上传(可断点续传)、文件下载(可获取下载进度)、大文件下载(可断点续传)、关闭Obs服务

[下载APK体验DEMO](https://www.pgyer.com/Dnu6WabN)
## 使用前须知
- 请确认您已经熟悉OBS的基本概念，如[桶（Bucket）](https://support.huaweicloud.com/productdesc-obs/obs_03_0207.html)、[对象（Object）](https://support.huaweicloud.com/productdesc-obs/obs_03_0206.html)、[访问密钥（AK和SK）](https://support.huaweicloud.com/productdesc-obs/obs_03_0208.html)等。
- 使用OBS客户端进行接口调用操作完成后，没有错误回调，则表明返回值有效；如果success返回false，则说明操作失败，此时应可从异常处理中获取错误信息
- 当前各区域特性开放不一致，部分特性只在部分区域开放，使用过程中如果接口HTTP状态码为405，请确认该区域是否支持该功能特性
## vue代码调用示例

```html
<template>
	<view>
		<page-head :title="title"></page-head>
		<scroll-view>
			<view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenClient">初始化配置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testCreateBucket">创建存储桶</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testListBuckets">列举存储桶</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testDeleteBucket">删除存储桶</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testHeadBucket">存储桶是否存在</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testListObjects">列举桶内对象</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testDeleteObject">删除桶内对象</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testUploadFile">文件上传(可获取上传进度)</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testUploadMultipartFile">大文件上传(可断点续传)</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testDownloadFile">文件下载(可获取下载进度)</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testDownloadMultipartFile">大文件下载(可断点续传)</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testCloseClient">关闭Obs服务</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					
				</view>
			</view>	
		</scroll-view>	
		
	</view>
</template>

<script>
	
	import { openClient,closeClient,createBucket,listBuckets,deleteBucket,headBucket,listObjects,deleteObject,uploadFile,uploadMultipartFile,downloadFile,downloadMultipartFile } from "@/uni_modules/cz-hwobs";

	export default {
		data() {
			return {
				title:"华为云OBS 示例"
			}
		},
		methods: {
			testOpenClient(){
				let res = openClient({
					accessKey:"你的accessKey",
					secretKey:"你的secretKey",
					endPoint:"你的endPoint，例如：obs.cn-east-5.myhuaweicloud.com",
					enableLog:true,
					androidConfig:{maxErrorRetry:5,connectionTimeout:60},
					iosConfig:{timeoutIntervalForCommand:100,timeoutIntervalForDownload:100,timeoutIntervalForUpload:100},
					harmonyConfig:{Timeout:300},
					});
				this.showToast(`初始化配置：${JSON.stringify(res)}`);	
			},
			testCreateBucket(){
				let self = this;
				createBucket({
					bucketName:"test.android2",
					location:"cn-east-5",
					completeListener(res) {
						self.showToast(`创建桶：${JSON.stringify(res)}`);
					}
				});
			},
			testListBuckets(){
				let self = this;
				listBuckets({
					completeListener(res) {
						self.showToast(`列举桶：${JSON.stringify(res)}`);
					}
				});
			},
			testDeleteBucket(){
				let self = this;
				deleteBucket({
					bucketName:"test.android2",
					completeListener(res) {
						self.showToast(`删除桶：${JSON.stringify(res)}`);
					}
				});
			},
			testHeadBucket(){
				let self = this;
				let res = headBucket({
					bucketName:"test.android2",
					completeListener(res) {
						self.showToast(`桶是否存在：${JSON.stringify(res)}`);
					} 
				});
			},
			testListObjects(){
				let self = this;
				listObjects({
					bucketName:"test.android1",
					completeListener(res) {
						self.showToast(`列举桶内对象：${JSON.stringify(res)}`);
					}
				});
			},
			testDeleteObject(){
				let self = this;
				let res = deleteObject({
					bucketName:"test.android2",
					objectKey:"1255122560-origin-IMG_0002.JPG",
					completeListener(res) {
						self.showToast(`删除桶内对象：${JSON.stringify(res)}`);
					}
				});
			},
			testUploadFile(){
				let self = this;
				uni.chooseImage({
					count:1,
					sourceType:['album'],
					sizeType:['original'],
					success(images) {
						console.log(`选择的文件：${JSON.stringify(images)}`);
						let res = uploadFile({
							bucketName:"test.android1",
							objectkey: images.tempFiles[0].path.split('/').pop(),
							path:images.tempFiles[0].path,
							expires:1,
							progressInterval:100*1024,
							progressListener(status) {
								console.log(`文件上传进度监听 ${status.transferPercentage} ${status.newlyTransferredBytes} ${status.transferredBytes} ${status.totalBytes}`);
							},
							completeListener(res) {
								self.showToast(`上传文件结果：${JSON.stringify(res)}`);
							}
						});
					},
					fail(error) {
						self.showToast(`选择文件失败：${JSON.stringify(error)}`);
					}
				})
			},
			testUploadMultipartFile(){
				let self = this;
				uni.chooseVideo({
					sourceType:['album'],
					compressed:false,
					success(video) {
						console.log(`选择的文件：${video.tempFilePath}`);
						uploadMultipartFile({
							bucketName:"test.android1",
							objectkey: video.tempFilePath.split('/').pop(),
							path:video.tempFilePath,
							enableCheckpoint:true,
							taskNum:5,
							partSize:10 * 1024 * 1024,
							progressInterval:500*1024,
							progressListener(status) {
								console.log(`文件上传进度监听 ${status.transferPercentage}`);
							},
							completeListener(res) {
								self.showToast(`上传文件结果：${JSON.stringify(res)}`);
							}
						});
					},
					fail(error) {
						self.showToast(`选择文件失败：${JSON.stringify(error)}`);
					}
				})
			},
			testDownloadFile(){
				let self = this;
				let name = `${Date.now()}.jpg`;
				downloadFile({
					bucketName:"test.android1",
					objectkey:"1748853027606.jpeg",
					savePath:`/storage/emulated/0/Android/data/uni.app.UNID5A0BCC/files/Download/${name}`,
					// savePath:`/Users/bill/Library/Developer/CoreSimulator/Devices/F719E191-2F58-41B8-918A-7A3A31976C90/data/Containers/Data/Application/8650EE45-01A9-4965-9068-06AC87FEC1C4/Documents/Pandora/apps/HBuilder/data/${name}`,
					// savePath:`/data/storage/el2/base/haps/entry/cache/${name}`,
					progressInterval:500*1024,
					progressListener(status) {
						console.log(`文件下载进度监听 ${status.transferPercentage}`);
					},
					completeListener(res) {
						self.showToast(`下载文件结果：${JSON.stringify(res)}`);
					}
				});
			},
			testDownloadMultipartFile(){
				let self = this;
				let name = `${Date.now()}.mp4`;
				downloadMultipartFile({
					bucketName:"test.android1",
					objectkey:"Screenrecorder-2025-05-08-17-40-09-363.mp4",
					// savePath:`/data/storage/el2/base/haps/entry/cache/${name}`,
					savePath:`/storage/emulated/0/Android/data/uni.app.UNID5A0BCC/files/Download/${name}`,
					enableCheckpoint:true,
					progressInterval:500*1024,
					taskNum:5,
					partSize:10 * 1024 * 1024,
					progressListener(status) {
						console.log(`文件下载进度监听 ${status.transferPercentage}`);
					},
					completeListener(res) {
						self.showToast(`下载文件结果：${JSON.stringify(res)}`);
					}
				});
			},
			testCloseClient(){
				closeClient();
			},
			showToast(msg){
				if(msg == ''){
					uni.showToast({
						icon:'none',
						title:'未获取到相关信息'
					})
				} else {
					uni.showToast({
						icon:'none',
						title: msg
					})
				}
			}
		}
	}
</script>

<style>

</style>

```

## 权限说明
安卓

```html
<uses-permission android:name="android.permission.INTERNET" /> 
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" /> 
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /> 
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.GET_TOP_ACTIVITY_INFO" />
```
iOS

```html
<key>NSAppTransportSecurity</key>
<dict>
	<key>NSAllowsArbitraryLoads</key>
	<true/>
</dict>	
```
鸿蒙 Next

```html
{
   "name": "ohos.permission.INTERNET",
}
```
## API调用说明

### 初始化配置（openClient）
系统     | 参数    | 返回    | 备注
| :-------- | :-----| :-----| :-----
安卓|accessKey 必填<br>secretKey 必填 <br>endPoint  必填<br> securityToken 可选<br>androidConfig 可选<br>enableLog 可选|示例：{success:true,data:true,error:null}|[参数说明](https://support.huaweicloud.com/sdk-android-devg-obs/obs_26_0203.html)<br>androidConfig 支持配置如下参数：<br>connectionTimeout:建立HTTP/HTTPS连接的超时时间（单位：毫秒）。默认为60000毫秒<br>maxConnections:最大允许的HTTP并发请求数。默认为1000。<br>maxErrorRetry:请求失败（请求异常、服务端报500或503错误等）后最大的重试次数。默认3次<br>socketTimeout:Socket层传输数据的超时时间（单位：毫秒）。默认为60000毫秒<br>endpointHttpPort:设置 HTTP 请求的端口号（默认为 80）<br>endpointHttpsPort:设置 HTTPS 请求的端口号（默认为 443）<br>httpsOnly:指定是否使用 HTTPS 进行 OBS 连接（默认情况下为“true”）<br>endPoint:连接OBS的服务地址。可包含协议类型、域名、端口号。示例：https://your-endpoint:443<br>pathStyle:是否要启用对 OBS 的路径式访问。“true”表示启用路径式访问，而“false”（默认值）表示启用虚拟托管式访问。注意：如果启用路径式访问，则不支持 OBS 3.0 的新存储桶功能。<br>validateCertificate:是否验证服务端证书。默认为false<br>verifyResponseContentType:是否验证响应头信息的ContentType。默认为true<br>isStrictHostnameVerification:是否严格验证服务端主机名。默认为false。<br>socketWriteBufferSize:Socket发送缓冲区大小（单位：字节），对应java.net.SocketOptions.SO_SNDBUF参数。默认为-1表示不设置<br>socketReadBufferSize:Socket接收缓冲区大小（单位：字节），对应java.net.SocketOptions.SO_RCVBUF参数。默认为-1表示不设置<br>readBufferSize:从Socket流下载对象的缓存大小（单位：字节），-1表示不设置缓存。默认为-1<br>writeBufferSize:上传对象到Socket流时的缓存大小（单位：字节），-1表示不设置缓存。默认为-1<br>idleConnectionTime:如果空闲时间超过此参数的设定值，则关闭连接（单位：毫秒）。默认为30000毫秒<br>maxIdleConnections:连接池中最大空闲连接数，默认值：1000<br>
iOS|accessKey 必填<br>secretKey 必填 <br>endPoint  必填<br> securityToken 可选<br>iosConfig 可选<br>enableLog 可选|示例：{success:true,data:true,error:null}|[参数说明](https://support.huaweicloud.com/sdk-ios-devg-obs/obs_27_0203.html)<br>iosConfig 支持配置如下参数：<br>trustUnsafeCert:是否信任不安全证书，默认为“NO”<br>maxConcurrentCommandRequestCount:允许的最大的命令请求并发数，默认为3<br>maxConcurrentUploadRequestCount:允许的最大的上传请求并发数，默认为3<br>maxConcurrentDownloadRequestCount:允许的最大的下载请求并发数，默认为3<br>httpMaximumCommandPerHost:允许打开的最大的命令请求连接数, ios系统中默认为4<br>httpMaximumUploadPerHost:允许打开的最大的上传请求连接数, ios系统中默认为4<br>httpMaximumDownloadPerHost:允许打开的最大的下载请求连接数, ios系统中默认为4<br>httpMaximumBackgroundUploadPerHost:允许打开的最大的后台上传请求连接数, ios系统中默认为4<br>httpMaximumBackgroundDownloadPerHost:允许打开的最大的后台下载请求连接数, ios系统中默认为4<br>timeoutIntervalForCommand:配置命令请求的超时时间；（单位秒）<br>timeoutIntervalForUpload:配置上传相关请求的超时时间；（单位秒）<br>timeoutIntervalForDownload:配置下载相关请求的超时时间；（单位秒）<br>
鸿蒙|accessKey 必填<br>secretKey 必填 <br>endPoint  必填<br> securityToken 可选<br>harmonyConfig 可选<br>enableLog 可选|示例：{success:true,data:true,error:null}|[参数说明](https://support.huaweicloud.com/sdk-harmony-devg-obs/obs_34_0101.html)<br>harmonyConfig 支持配置如下参数:<br>IsSecure<br>Timeout:HTTP/HTTPS请求的总超时时间。默认300，单位：秒。<br>IsCname:是否通过自定义域名访问OBS服务。默认为"false"<br>Port<br>IsSignatureNegotiation<br>PathStyle<br>
### 创建桶（createBucket）
桶是OBS全局命名空间，相当于数据的容器、文件系统的根目录，可以存储若干对象。

|系统| 参数 |返回|备注
|--|:--|:--|:--|
|安卓、iOS、鸿蒙| bucketName:桶名称 必填<br>location:桶区域 可选<br>completeListener:回调 必填|示例：{success:true,data:{"bucketName":"xxxxxx","location":"xxxxx"},error:null}|bucketName:桶名称<br>location:桶区域

### 列举桶（listBuckets）
|系统| 参数 |返回|备注
|:--|:--|:--|:--|
|安卓、iOS、鸿蒙| completeListener:回调 必填|示例：{success:true,data:[{"bucketName":"xxxxxx","location":"xxxxx","creationDate":1748851102664}],error:null}|bucketName:桶名称<br>location:桶区域<br>creationDate:创建时间
### 删除桶（deleteBucket）
|系统| 参数 |返回|备注
|:--|:--|:--|:--|
|安卓、iOS、鸿蒙| bucketName:桶名称 必填<br>completeListener:回调 可选|示例：{success:true,data:true,error:null}|data:true 成功，false 失败
### 桶是否存在（headBucket）
|系统| 参数 |返回|备注
|:--|:--|:--|:--|
|安卓、iOS、鸿蒙| bucketName:桶名称 必填<br>completeListener:回调 必填|示例：{success:true,data:true,error:null}|data:true 存在，false 不存在
### 列举桶内对象（listObjects）
|系统| 参数 |返回|备注
|:--|:--|:--|:--|
|安卓、iOS、鸿蒙| bucketName:桶名称 必填<br>completeListener:回调 必填|示例：{success:true,data:[{"objectKey":"xxxxxx","ownerName":"xxxxx"}],error:null}|objectKey:对象名<br>ownerName:归属人
### 删除桶内对象（deleteObject）
|系统| 参数 |返回|备注
|:--|:--|:--|:--|
|安卓、iOS、鸿蒙| bucketName:桶名称 必填<br>objectKey:对象名 必填<br>completeListener:回调 可选|示例：{success:true,data:true,error:null}|data:true 成功，false 失败

### 文件上传-可获取上传进度（uploadFile）
|系统| 参数 |返回|备注
|:--|:--|:--|:--|
|安卓、iOS、鸿蒙| bucketName:桶名称 必填<br>objectKey:对象名 必填<br>path:为待上传的本地文件路径，需要指定到具体的文件名 必填<br>expires:设置对象过期时间，单位天 可选<br>progressInterval:默认每上传100 * 1024L(100kb)数据反馈上传进度 可选<br>progressListener:上传进度回调 可选<br>completeListener:回调 可选|progressListener示例:<br>{transferPercentage:50,<br>newlyTransferredBytes:100,<br>transferredBytes:500,<br>totalBytes:1000}<br>completeListener示例：{success:true,data:true,error:null}|transferPercentage:上传进度百分比<br>newlyTransferredBytes:本次接收的字节transferredBytes:累计接收的字节totalBytes:总字节<br>data:true 成功，false 失败<br>鸿蒙sdk目前处于公测阶段，无上传进度回调，，且单次最大文件支持0-5G，需要等后续更新

### 大文件上传-可断点续传（uploadMultipartFile）
|系统| 参数 |返回|备注
|:--|:--|:--|:--|
|安卓、iOS、鸿蒙| bucketName:桶名称 必填<br>objectKey:对象名 必填<br>path:为待上传的本地文件路径，需要指定到具体的文件名 必填<br>taskNum:设置分段上传时的最大并发数 可选<br>partSize:设置分段大小，例如10 * 1024 * 1024（10MB） 可选<br>enableCheckpoint:是否开启断点续传模式，默认为false，表示不开启 可选<br>progressInterval:默认每上传100 * 1024L(100kb)数据反馈上传进度 可选<br>progressInterval:默认每上传100 * 1024L(100kb)数据反馈上传进度 可选<br>progressListener:上传进度回调 可选<br>completeListener:回调 可选|progressListener示例:<br>{transferPercentage:50,<br>newlyTransferredBytes:100,<br>transferredBytes:500,<br>totalBytes:1000}<br>completeListener示例：{success:true,data:true,error:null}|transferPercentage:上传进度百分比<br>newlyTransferredBytes:本次上传的字节transferredBytes:累计上传的字节totalBytes:总字节<br>data:true 成功，false 失败<br>鸿蒙sdk目前处于公测阶段，无上传进度以及断点续传功能，且单次最大文件支持0-5G，需要等后续更新
### 文件下载-可获取下载进度（downloadFile）
|系统| 参数 |返回|备注
|:--|:-------|:--|:--|
|安卓、iOS、鸿蒙| bucketName:桶名称 必填<br>objectKey:对象名 必填<br>savePath:文件保存路径，需要确定有存储到该位置的权限，需要指定到具体的文件名 必填<br>progressInterval:默认每下载100 * 1024L(100kb)数据反馈下载进度 可选<br>progressListener:下载进度回调 可选<br>completeListener:回调 可选|progressListener示例:<br>{transferPercentage:50,<br>newlyTransferredBytes:100,<br>transferredBytes:500,<br>totalBytes:1000}<br>completeListener示例：{success:true,data:true,error:null}|transferPercentage:下载进度百分比<br>newlyTransferredBytes:本次接收的字节transferredBytes:累计接收的字节totalBytes:总字节<br>data:true 成功，false 失败<br>鸿蒙sdk目前处于公测阶段，无下载进度回调，需要等后续更新
### 大文件下载-可断点续传（downloadMultipartFile）
|系统| 参数 |返回|备注
|:--|:--|:--|:--|
|安卓、iOS、鸿蒙| bucketName:桶名称 必填<br>objectKey:对象名 必填<br>path:为待上传的本地文件路径，需要指定到具体的文件名 必填<br>taskNum:设置分段下载时的最大并发数 可选<br>partSize:设置分段大小，例如10 * 1024 * 1024（10MB） 可选<br>enableCheckpoint:是否开启断点续传模式，默认为false，表示不开启 可选<br>progressInterval:默认每下载100 * 1024L(100kb)数据反馈下载进度 可选<br>progressInterval:默认每下载100 * 1024L(100kb)数据反馈上传进度 可选<br>progressListener:下载进度回调 可选<br>completeListener:回调 可选|progressListener示例:{<br>transferPercentage:50,<br>newlyTransferredBytes:100,<br>transferredBytes:500,<br>totalBytes:1000}<br>completeListener示例：{success:true,data:true,error:null}|transferPercentage:下载进度百分比<br>newlyTransferredBytes:本次下载的字节transferredBytes:累计下载的字节totalBytes:总字节<br>data:true 成功，false 失败<br>鸿蒙sdk目前处于公测阶段，无下载进度以及断点续传功能，需要等后续更新

### 关闭OBS服务（closeClient）
|系统| 参数 |返回|备注
|:--|:--|:--|:--|
|安卓、iOS、鸿蒙| -| -|-

## 异常处理
组件无自定义错误码，SDK错误码请参考如下文档处理：
- [安卓-OBS服务端错误码](https://support.huaweicloud.com/sdk-android-devg-obs/obs_26_1601.html)
- [安卓-SDK自定义异常](https://support.huaweicloud.com/sdk-android-devg-obs/obs_26_1608.html)
- [iOS-OBS服务端错误码](https://support.huaweicloud.com/sdk-ios-devg-obs/obs_27_1601.html)
- [iOS-SDK自定义异常](https://support.huaweicloud.com/sdk-ios-devg-obs/obs_27_1604.html)
- [鸿蒙-HTTP状态码](https://support.huaweicloud.com/sdk-harmony-devg-obs/obs_34_0609.html)
- [鸿蒙-OBS服务端错误码](https://support.huaweicloud.com/sdk-harmony-devg-obs/obs_34_0608.html)
- [鸿蒙-SDK公共结果对象](https://support.huaweicloud.com/sdk-harmony-devg-obs/obs_34_0104.html)

