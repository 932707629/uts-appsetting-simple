### 打开原生APP系统设置插件 支持安卓/iOS/鸿蒙UTS组件

# 介绍

- AppSetting支持跳转系统设置、APP详情、WLAN、蓝牙、NFC、定位、开发者模式等设置项
- 提供完全免费的API支持，使用简单，集成方便
- 支持Vue2、Vue3使用
- 由于iOS系统未提供打开具体功能设置页面，仅支持iOS16以上通知页打开，其他方法仅能打开系统设置页
- 提供Github Demo集成实例，以及APK试用体验

### 开源不易，如果觉得插件不错，随手点个收藏，给个五星好评都可以哦！
[下载APK体验DEMO](https://www.pgyer.com/utsappsetting)

[去插件市场](https://ext.dcloud.net.cn/plugin?id=25070)


### API说明
API     | 参数    | 备注
| :-------- | :-----| :-----
openWIFISettings|true启动新的Activity实例，默认false|打开WIFI设置
openWirelessSettings|true启动新的Activity实例，默认false|打开无线和网络设置
openLocationSettings|true启动新的Activity实例，默认false|打开定位服务设置
openSecuritySettings|true启动新的Activity实例，默认false|打开隐私和安全设置
openLockAndPasswordSettings|true启动新的Activity实例，默认false|打开生物识别和密码设置
openBluetoothSettings|true启动新的Activity实例，默认false|打开蓝牙设置
openDataRoamingSettings|true启动新的Activity实例，默认false|打开移动数据设置
openDateSettings|true启动新的Activity实例，默认false|打开日期和时间设置
openDisplaySettings|true启动新的Activity实例，默认false|打开显示和亮度设置
openNotificationSettings|true启动新的Activity实例，默认false|打开APP通知设置
openSoundSettings|true启动新的Activity实例，默认false|打开声音和振动设置
openInternalStorageSettings|true启动新的Activity实例，默认false|打开存储设置
openBatteryOptimizationSettings|true启动新的Activity实例，默认false|打开电池优化设置
openAppSettings|true启动新的Activity实例，默认false|打开APP信息设置
openNFCSettings|true启动新的Activity实例，默认false|打开NFC设置
openDeviceSettings|true启动新的Activity实例，默认false|打开设备信息设置
openVPNSettings|true启动新的Activity实例，默认false|打开VPN设置
openAccessibilitySettings|true启动新的Activity实例，默认false|打开无障碍设置
openDevelopmentSettings|true启动新的Activity实例，默认false|打开开发者选项设置
openHotspotSettings|true启动新的Activity实例，默认false|打开个人热点设置

## Vue代码调用实例

```html
<template>
	<view>
		<page-head :title="title"></page-head>
		<scroll-view>
			<view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenWIFISettings">打开WIFI设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenWirelessSettings">打开无线网络设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenLocationSettings">打开定位服务设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenSecuritySettings">打开安全设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenLockAndPasswordSettings">打开锁屏密码设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenBluetoothSettings">打开蓝牙设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenDataRoamingSettings">打开移动数据设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenDateSettings">打开日期和时间设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenDisplaySettings">打开显示和亮度设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenNotificationSettings">打开APP通知设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenSoundSettings">打开声音和振动设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenInternalStorageSettings">打开存储设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenBatteryOptimizationSettings">打开电池优化设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenAppSettings">打开此APP信息设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenNFCSettings">打开NFC设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenDeviceSettings">打开设备信息设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenVPNSettings">打开VPN设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenAccessibilitySettings">打开无障碍设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenDevelopmentSettings">打开开发者选项设置</button>
				</view>
				<view class="uni-padding-wrap uni-common-mt">
					<button @tap="testOpenHotspotSettings">打开个人热点设置</button>
				</view>
			</view>
		</scroll-view>	
	</view>
</template>

<script>
	
	import { openWIFISettings,openWirelessSettings,openLocationSettings,openSecuritySettings,openLockAndPasswordSettings,openBluetoothSettings,openDataRoamingSettings,openDateSettings,openDisplaySettings,openNotificationSettings,openSoundSettings,openInternalStorageSettings,openBatteryOptimizationSettings,openAppSettings,openNFCSettings,openDeviceSettings,openVPNSettings,openAccessibilitySettings,openDevelopmentSettings,openHotspotSettings } from "@/uni_modules/cz-appsetting";
	
	export default {
		data() {
			return {
				title:"AppSetting 实例"
			}
		},
		methods: {
			testOpenWIFISettings(){
				openWIFISettings();
			},
			testOpenWirelessSettings(){
				openWirelessSettings();
			},
			testOpenLocationSettings(){
				openLocationSettings();
			},
			testOpenSecuritySettings(){
				openSecuritySettings();
			},
			testOpenLockAndPasswordSettings(){
				openLockAndPasswordSettings();
			},
			testOpenBluetoothSettings(){
				openBluetoothSettings();
			},
			testOpenDataRoamingSettings(){
				openDataRoamingSettings();
			},
			testOpenDateSettings(){
				openDateSettings();
			},
			testOpenDisplaySettings(){
				openDisplaySettings();
			},
			testOpenNotificationSettings(){
				openNotificationSettings();
			},
			testOpenSoundSettings(){
				openSoundSettings();
			},
			testOpenInternalStorageSettings(){
				openInternalStorageSettings();
			},
			testOpenBatteryOptimizationSettings(){
				openBatteryOptimizationSettings();
			},
			testOpenAppSettings(){
				openAppSettings();
			},
			testOpenNFCSettings(){
				openNFCSettings();
			},
			testOpenDeviceSettings(){
				openDeviceSettings();
			},
			testOpenVPNSettings(){
				openVPNSettings();
			},
			testOpenAccessibilitySettings(){
				openAccessibilitySettings();
			},
			testOpenDevelopmentSettings(){
				openDevelopmentSettings();
			},
			testOpenHotspotSettings(){
				openHotspotSettings();
			}
		}
	}
</script>

<style>

</style>

```

### 权限说明
未使用系统权限








