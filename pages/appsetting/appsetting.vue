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
				title:"AppSetting 示例"
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
