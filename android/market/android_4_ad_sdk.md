##### 自己实现一个广告SDK要考虑的是？？？

- 平台
  - Android
  - iOS
  - 其他


- 接口协议
  - 请求接口
  - 广告数据
- 网络请求
- 数据缓存
- 易用性，方便接入
- 稳定性
- 内存占用
- 包体大小

- 广告如何什么样形式展示，banner、全屏、视频？
- 广告如何保证投放成功率，如何精准投放？
- 广告如何做到防刷？

##### 市面上那么多广告SDK，一个SDK是否吸引人使用，产生有影响的点

1. 易用性，（注册、嵌入SDK是否方便） 
2. 推广 
3.  广告的丰富性与广告数 
4.  服务 
5. 最重要的收入 
6. 平台的稳定性 


另，补充一下网上摘录的部分移动端广告平台的知识

##### 广告基本知识

恶补一下广告基本知识： 

* CPM(每千次展示成本)，通过展示获取收益； 
* CPC（每点击成本），通过点击获取收益； 
* CPA（每行动成本），完成下载；安装；激活等方式计算收益 

##### 广告模式

- 广告条：最普遍的广告模式，嵌入在应用界面内，用户点击行为会带来收入。
-  积分墙：应用通过限制功能、去广告等引导用户进入积分墙页面下载广告应用得到积分来换取使用的模式，用户安装完推荐广告软件后开发者才有收入，该模式会被一些市场和发布渠道拒绝应用上架。
-  推送：通过类似短信通知的模式展示广告，此模式用户点击率奇高，所以也是最受市场排挤的模式，像应用汇、安智市场、安卓市场、N多网等等都拒绝发布带推送广告的软件。

##### 市面上广告平台

- 万普平台 [万普世纪（WAPS）](http://www.waps.cn/index.jsp)

  个人开发者觉得最给力的广告平台，广告单价最高，扣量也不多。其主要广告为广告条、积分墙、推送3种模式，主要以CPA广告为主。开发者注册地址：[http://www.waps.cn/?f=zhaostudy3](http://www.waps.cn/?f=zhaostudy3)，数据每1个小时更新一次左右，收入周结算，可以添加多个成员，人均收入低于800每月的，不收任何税费。超过的提现税率为6%。万普的收入明显比其他广告平台高很多、很多...。但是像应用汇、安智市场、安卓市场等大市场都拒绝带万普广告的应用。

- 多盟

  广告条模式，CPC单价0.21元左右，广告填充率很高很高。还算比较靠谱，实时数据、数据每2个小时更新一次左右，收入周结算，可以添加多个成员，人均收入低于800每月的，不收任何税费。超过的部分提现税率为20%。最近多盟系统升级，每次升级后，我的软件广告点击率都大幅下降，2月份点击率为1%，3月份降到了0.5%一下，太黑了，只有我自己统计的40%左右。不过其他广告平台都一样扣量。 

  注册地址:[http://www.domob.cn/](http://www.domob.cn/passport/user/register/recUser/zhaostudy3%40163.com) 

- 腾讯广告

  广告条模式，CPC单价0.1.3元左右，展示和扣量方面都也还可以，每天能查询昨天的数据每月16-20号结算收入，腾讯要收走30%的分成。虽然单价低，但是腾讯的广告都是热门的腾讯软件，点击率很好，我的软件的腾讯广告点击率大概为1.3%。是多盟的2.5倍-3倍的点击率。 

  注册地址:[http://dev.app.qq.com/loginInit.action](http://dev.app.qq.com/loginInit.action) 

- 哇棒（个人感觉：扣量很严重，广告很少，收入奇低）

- 百度联盟
  广告条模式，CPC单价动态计费，所以具体单价未知了，收入还行。百度审核周期很长，而且审核很严格的，经常因各种问题二通不过审核。 注册地址：[http://munion.baidu.com/](http://munion.baidu.com/)

- [小米广告 SDK](http://dev.xiaomi.com/docs/gameentry/%E6%89%8B%E6%9C%BA&pad%E6%B8%B8%E6%88%8F%E6%8E%A5%E5%85%A5%E6%96%87%E6%A1%A3/%E5%B0%8F%E7%B1%B3%E5%B9%BF%E5%91%8ASDK%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97/)

- 其他：adview,亿动，有米，wooboo,百分通联，万普世纪，芒果，果合，vpon,lense,wiyun,多盟，架势。



#### Audience Network and Admob对比
| AD SDK   	|  Audience Network  | Admob         
| :------- 	| :----------------- | ------
| 公司		| 	Facebook		 | Google
|特点	| 利用facebook的人群精准定位做为DMP用来做数据分析与人群定位，Audience Network 将广告展示的位置从facebook的自有广告位置上扩展到了第三方广告位置 具有DMP+DSP的趋势。facebook优势（准确的用户数据）+ DSP模式优势（大量的广告展示位置） |从用户的各种行为中分析用户的兴趣爱好，从而进行精准定位。google优势（品牌和平台带来的大量流量+强大技术支撑的数据分析）+ DC adexchange优势（更大更开放带来的更庞大的流量）
|广告来源	| 	Facebook 广告	 | 自家广告、Admob广告网络 中介广告网络
| 优势		| 	精准投放、CPM单价比Admob高	 	 | 可以接入第三方广告源，填充率高   	 |
|局限	|广告源只能来自Facebook、一个页面或者一个scroll中只能包含一个广告（国内很多广告平台专门提供应用墙SDK，一个页面全部是广告），以及必须显示”AD”或”sponsored”字样，必须显示广告title、CTA button（如install now, buy now）等。audience network只能在安装Facebook且登录的设备上显示广告，没有安装或登录的是无法显示的，返回一个1001错误代码。 | 广告必须和应用独立、不能诱导用户点击广告
|屏蔽竞争对手	|	屏蔽竞争对手AppId屏蔽广告| 屏蔽竞争对手AppId屏蔽广告
|支持平台|iOs、Android、Unity、移动网页（测试版）| Android、iOs、windowPhone
|横幅广告Banner Ads	| 	支持				 |	支持	
|插页广告Interstitial Ads	| 	支持				 |	支持
|Native Display Ads| 支持		 | 	支持
|可选择尺寸	|	320 * 50、320 * 90、320 * 250、 全屏   | 320 * 50、320 * 100、300 * 250、468 * 60、 728 * 90、全屏
|官网地址		| [https://developers.facebook.com/docs/audience-network?locale=en_US](https://developers.facebook.com/docs/audience-network?locale=en_US) | [https://firebase.google.com/docs/admob/?hl=zh-cn](https://firebase.google.com/docs/admob/?hl=zh-cn)

都面向海量的用户，Facebook以社交属性见长，Google以数据流量占优，
要看面向的目标市场目标人群，看怎么才达到效果最大化。

1.Facebook投放更精准，但是填充率不及Admob，应该怎么优化？

2.两家的广告如果都能填充，如何进行抉择？

​	Admob提供了比较完善的解决方案，那就是SDK集成SDK，你可以在使用Admob SDK时集成Facebook Audience Network，并且可以在Admob后台进行设置，进行广告效果的调优。

Admob其实可以集成非常多的第三方广告平台，但是效果最好的要数Facebook。Admob集成的优势在于它提供了一种竞价模式。据了解，Admob的竞价有两种：

- 一种就是InMobi那种，Admob和Inmobi后台已经打通，可以互通有无，Admob知道Inmobi的实时eCPM，因此总是能返回eCPM高的广告给调用方；
- 另一种就是Facebook这种，Admob并不知道Facebook的实时eCPM，但是我们可以在Admob后台设置一个Facebook的eCPM阀值，那么广告填充就会出现三种情况：
	>1）Facebook没有广告返回，直接返回Admob的广告，广告的eCPM就是Admob的平均eCPM；
	>
	2）Facebook有广告返回，但是Admob返回的广告eCPM大于Facebook预设的阀值，这里是15，那么返回Admob的广告，此时广告的eCPM会高于预设阀值；
	>
	>3）Facebook有广告返回，但是Admob返回的广告eCPM小于Facebook预设的阀值，那么就会使用Facebook返回的广告，此时广告的eCPM就会是你在Facebook平台上的平均eCPM。
	
	由此可以看出，要想实现收益最大化，我们应该适当调高在Admob后台设置的Facebook eCPM阀值，并在Facebook后台将广告设置偏向“价格优先”。例如经验阀值为15。实际情况需要实施者不断观察不断调整，从而达到整体最优。


###Audience Network示例代码：
    
	String facebookAdUnitId = "1605000876745596_1688342111425188";

	mFacebookNativeAd = new com.facebook.ads.NativeAd(mContext, facebookAdUnitId);

	mFacebookNativeAd.setAdListener(new AdListener() {

		@Override
		public void onError(Ad ad, AdError adError) {
		}

		@Override
		public void onAdLoaded(Ad ad) {
			// ad loaded
		}

		@Override
		public void onAdClicked(Ad ad) {
		}
	
	});

	mFacebookNativeAd.loadAd();
    

### Admob 示例代码

	String admobAdUnitId = "ca-app-pub-1301877944976160/5685349534";
	AdLoader.Builder adBuilder = new AdLoader.Builder(context, admobAdUnitId);

	adBuilder.forAppInstallAd(new NativeAppInstallAd.OnAppInstallAdLoadedListener() {
	    @Override
	    public void onAppInstallAdLoaded(NativeAppInstallAd nativeAppInstallAd) {
	
	    }
	});

	AdLoader adLoader = adBuilder.withAdListener(new AdListener() {
	    @Override
	    public void onAdFailedToLoad(int errorCode) {
	    }
	}).build();

	AdRequest adRequest = new AdRequest.Builder().build();
	adLoader.loadAd(adRequest);


### Facebook广告请求协议研究

初步研究了一下，难免错落之处，欢迎指正讨论。

##### 步骤

1. 下载demo体验；
   1. 需要注册相关的appid和新建广告位才能正常请求广告；
   2. 不想配置广告的话，可以直接开启debug模式体验；
2. 抓包；
   1. 显然，请求用的是Https，无法抓包，Facebook的工程师不是吃素的
3. 反编译Jar包；
   1. 通过跟踪，找到请求协议的相关参数，包括但不限于
      - VIEWABLE 不知道干嘛用
      - SCHEMA 像是数据类型
      - SDK  SDK类型
      - SDK_VERSION SDK版本
      - LOCAL 位置信息
      - DENSTTY 分辨率
      - SCREEN_WIDTH 屏幕宽度
      - SCREEN_HEIGHT 屏幕高度
      - IDFA 未知
      - IDFA_FLAG 位置
      - ATTRIBUTION_ID 未知ID 
      - ID_SOURCE 未知ID
      - OS 手机系统 Android/IOS
      - OSVERS 系统版本
      - BUNDLE 机型信息
      - APPNAME 应用名称
      - APPVERS 应用版本
      - APPBUILD 应用固件
      - CARRIER 未知
      - MAKE 未知
      - MODEL 手机机型信息
      - COPPA 未知
      - INSTALLER 未知
      - SDK_CAPABILITY 未知



![请求参数](./res/adsdk/screenshot 2016-07-28 16.18.05.png)


请求发起后下发的广告体：

![facebook广告体](./res/adsdk/screenshot 2016-07-28 17.13.33.png)



用户操作广告（点击、关闭等）后的数据统计请求参数：

![facebook广告操作后信息](./res/adsdk/screenshot 2016-07-28 17.15.07.png)





####用户属性Id相关调研

结论：Facebook是通过反射或者直接访问的方式获取到google服务中 "com.google.android.gms.ads.identifier.AdvertisingIdClient"从而直接获取到一个androidAdvertiserId，如果都无法获取，就通过 UUID.randomUUID().toString()随机生成一个作为用户的属性Id。

生成后会存在到 com.facebook.katana.provider.AttributionIdProvider 中供facebook的相关sdk共享。

研究方法：先反编译Audience NetWork 追踪，后面反编译Facebook Sdk进行进一步追踪。

一、反编译Audience NetWork，从接口参数的字段中可以看到有用户属性的字段 "ATTRIBUTION_ID"，那就跟踪进去看一下里面怎么获取。
package complie.com.facebook.ads.internal.dto; class e

	 private static Map<String, String> b(Context paramContext)
	  {
	    HashMap localHashMap = new HashMap();
	    localHashMap.put("VIEWABLE", "1");
	    localHashMap.put("SCHEMA", "json");
	    localHashMap.put("SDK", "android");
	    localHashMap.put("SDK_VERSION", "4.8.1");
	    localHashMap.put("LOCALE", Locale.getDefault().toString());
	    float f1 = paramContext.getResources().getDisplayMetrics().density;
	    int j = paramContext.getResources().getDisplayMetrics().widthPixels;
	    int k = paramContext.getResources().getDisplayMetrics().heightPixels;
	    localHashMap.put("DENSITY", String.valueOf(f1));
	    localHashMap.put("SCREEN_WIDTH", String.valueOf((int)(j / f1)));
	    localHashMap.put("SCREEN_HEIGHT", String.valueOf((int)(k / f1)));
	    localHashMap.put("IDFA", f.o);
	    localHashMap.put("IDFA_FLAG", f.p ? "0" : "1");
	    localHashMap.put("ATTRIBUTION_ID", f.n);
	    localHashMap.put("ID_SOURCE", f.q);
	    localHashMap.put("OS", "Android");
	    localHashMap.put("OSVERS", f.a);
	    localHashMap.put("BUNDLE", f.d);
	    localHashMap.put("APPNAME", f.e);
	    localHashMap.put("APPVERS", f.f);
	    localHashMap.put("APPBUILD", String.valueOf(f.g));
	    localHashMap.put("CARRIER", f.i);
	    localHashMap.put("MAKE", f.b);
	    localHashMap.put("MODEL", f.c);
	    localHashMap.put("COPPA", String.valueOf(AdSettings.isChildDirected()));
	    localHashMap.put("INSTALLER", f.h);
	    localHashMap.put("SDK_CAPABILITY", d.b());
	    return localHashMap;
	  }


二、"attributionId"的获取

- 先看sharedPreferences有没值，有则直接获取；
- sharedPreferences中没有则通过g.a()获取，然后保存在sharedPreferences中；
- 那么，g.a()又是什么？

package complie.com.facebook.ads.internal.dto; class f

	 public static void b(Context paramContext)
	  {
	    if (!r)
	      return;
	    try
	    {
	      com.facebook.ads.internal.f localf = null;
	      g.a locala = null;
	      SharedPreferences localSharedPreferences = paramContext.getSharedPreferences("SDKIDFA", 0);
	      if (localSharedPreferences.contains("attributionId"))
	        n = localSharedPreferences.getString("attributionId", "");
	      if (localSharedPreferences.contains("advertisingId"))
	      {
	        o = localSharedPreferences.getString("advertisingId", "");
	        p = localSharedPreferences.getBoolean("limitAdTracking", p);
	        q = f.c.a.name();
	      }
	      try
	      {
	        locala = g.a(paramContext.getContentResolver());
	      }
	      catch (Exception localException2)
	      {
	        c.a(b.a(localException2, "Error retrieving attribution id from fb4a"));
	      }
	      if (locala != null)
	      {
	        String str = locala.a;
	        if (str != null)
	          n = str;
	      }
	      try
	      {
	        localf = com.facebook.ads.internal.f.a(paramContext, locala);
	      }
	      catch (Exception localException3)
	      {
	        c.a(b.a(localException3, "Error retrieving advertising id from Google Play Services"));
	      }
	      if (localf != null)
	      {
	        localObject = localf.a();
	        Boolean localBoolean = Boolean.valueOf(localf.b());
	        if (localObject != null)
	        {
	          o = (String)localObject;
	          p = localBoolean.booleanValue();
	          q = localf.c().name();
	        }
	      }
	      Object localObject = localSharedPreferences.edit();
	      ((Editor)localObject).putString("attributionId", n);
	      ((Editor)localObject).putString("advertisingId", o);
	      ((Editor)localObject).putBoolean("limitAdTracking", p);
	      ((Editor)localObject).apply();
	    }
	    catch (Exception localException1)
	    {
	      localException1.printStackTrace();
	    }
	  }


三、通过跟踪g.a(),可以发现最终的数据来源是通过provider，从而获取aid,也就是**"attributionId"**
那么"content://com.facebook.katana.provider.AttributionIdProvider"又是怎么生成，猜测应该是Facebook Sdk中某一个jar包（Facebook Sdk中包含有 AccountKit、AudienceNetwork、facebook）


	package complie.com.facebook.ads.internal.util;
	class g{
  		private static final Uri a = Uri.parse("content://com.facebook.katana.provider.AttributionIdProvider");
	}

  	public static a a(ContentResolver paramContentResolver)
  	{
    	Cursor localCursor = null;
    	try
    	{
      	String[] arrayOfString = { "aid", "androidid", "limit_tracking" };
      	localCursor = paramContentResolver.query(a, arrayOfString, null, null, null);
      	if ((localCursor == null) || (!localCursor.moveToFirst()))
      	{
        	localObject1 = new a(null, null, false);
        	return localObject1;
	      }
	      localObject1 = localCursor.getString(localCursor.getColumnIndex("aid"));
	      String str = localCursor.getString(localCursor.getColumnIndex("androidid"));
	      Boolean localBoolean = Boolean.valueOf(localCursor.getString(localCursor.getColumnIndex("limit_tracking")));
	      a locala = new a((String)localObject1, str, localBoolean.booleanValue());
	      return locala;
	    }
	    catch (Exception localException)
	    {
	      Object localObject1 = new a(null, null, false);
	      return localObject1;
	    }
	    finally
	    {
	      if (localCursor != null)
	        localCursor.close();
	    }
	    throw localObject2;
	  }

四、反编译account-kit-sdk-4.12.1找了一下， 没发现。

五、反编译facebook sdk 通过找com.facebook.katana.provider.AttributionIdProvider发现了androidId的获取方式

优先看AttributionIdProvider中是否存在，无则

通过反射 "com.google.android.gms.ads.identifier.AdvertisingIdClient" 中 "getId"方法 获取androidAdvertiserId，如果没有，

则通过 GoogleAdInfo getAdvertiserId()方法获取。



package compliess.com.facebook.internal;

public class AttributionIdentifiers

	/*     */   public static AttributionIdentifiers getAttributionIdentifiers(Context context) {
	/* 165 */     if ((recentlyFetchedIdentifiers != null) && (System.currentTimeMillis() - recentlyFetchedIdentifiers.fetchTime < 3600000L))
	/*     */     {
	/* 168 */       return recentlyFetchedIdentifiers;
	/*     */     }
	/*     */ 
	/* 171 */     AttributionIdentifiers identifiers = getAndroidId(context);
	/* 172 */     Cursor c = null;
	/*     */     try {
	/* 174 */       String[] projection = { "aid", "androidid", "limit_tracking" };
	/*     */ 
	/* 178 */       providerUri = null;
	/* 179 */       if (context.getPackageManager().resolveContentProvider("com.facebook.katana.provider.AttributionIdProvider", 0) != null)
	/*     */       {
	/* 181 */         providerUri = Uri.parse("content://com.facebook.katana.provider.AttributionIdProvider");
	/* 182 */       } else if (context.getPackageManager().resolveContentProvider("com.facebook.wakizashi.provider.AttributionIdProvider", 0) != null)
	/*     */       {
	/* 184 */         providerUri = Uri.parse("content://com.facebook.wakizashi.provider.AttributionIdProvider");
	/*     */       }
	/* 186 */       String installerPackageName = getInstallerPackageName(context);
	/* 187 */       if (installerPackageName != null)
	/* 188 */         identifiers.androidInstallerPackage = installerPackageName;
	/*     */       AttributionIdentifiers localAttributionIdentifiers1;
	/* 190 */       if (providerUri == null) {
	/* 191 */         return cacheAndReturnIdentifiers(identifiers);
	/*     */       }
	/* 193 */       c = context.getContentResolver().query(providerUri, projection, null, null, null);
	/* 194 */       if ((c == null) || (!c.moveToFirst())) {
	/* 195 */         return cacheAndReturnIdentifiers(identifiers);
	/*     */       }
	/* 197 */       int attributionColumnIndex = c.getColumnIndex("aid");
	/* 198 */       int androidIdColumnIndex = c.getColumnIndex("androidid");
	/* 199 */       int limitTrackingColumnIndex = c.getColumnIndex("limit_tracking");
	/*     */ 
	/* 201 */       identifiers.attributionId = c.getString(attributionColumnIndex);
	/*     */ 
	/* 205 */       if ((androidIdColumnIndex > 0) && (limitTrackingColumnIndex > 0) && (identifiers.getAndroidAdvertiserId() == null))
	/*     */       {
	/* 207 */         identifiers.androidAdvertiserId = c.getString(androidIdColumnIndex);
	/* 208 */         identifiers.limitTracking = Boolean.parseBoolean(c.getString(limitTrackingColumnIndex));
	/*     */       }
	/*     */     }
	/*     */     catch (Exception e)
	/*     */     {
	/*     */       Uri providerUri;
	/* 212 */       Log.d(TAG, "Caught unexpected exception in getAttributionId(): " + e.toString());
	/* 213 */       return null;
	/*     */     } finally {
	/* 215 */       if (c != null) {
	/* 216 */         c.close();
	/*     */       }
	/*     */     }
	/* 219 */     return cacheAndReturnIdentifiers(identifiers);
	/*     */   }


	/*     */   private static AttributionIdentifiers getAndroidIdViaReflection(Context context)
	/*     */   {
	/*     */     try
	/*     */     {
	/*  89 */       if (Looper.myLooper() == Looper.getMainLooper()) {
	/*  90 */         throw new FacebookException("getAndroidId cannot be called on the main thread.");
	/*     */       }
	/*  92 */       Method isGooglePlayServicesAvailable = Utility.getMethodQuietly("com.google.android.gms.common.GooglePlayServicesUtil", "isGooglePlayServicesAvailable", new Class[] { Context.class });
	/*     */ 
	/*  98 */       if (isGooglePlayServicesAvailable == null) {
	/*  99 */         return null;
	/*     */       }
	/*     */ 
	/* 102 */       Object connectionResult = Utility.invokeMethodQuietly(null, isGooglePlayServicesAvailable, new Object[] { context });
	/*     */ 
	/* 104 */       if ((!(connectionResult instanceof Integer)) || (((Integer)connectionResult).intValue() != 0))
	/*     */       {
	/* 106 */         return null;
	/*     */       }
	/*     */ 
	/* 109 */       Method getAdvertisingIdInfo = Utility.getMethodQuietly("com.google.android.gms.ads.identifier.AdvertisingIdClient", "getAdvertisingIdInfo", new Class[] { Context.class });
	/*     */ 
	/* 114 */       if (getAdvertisingIdInfo == null) {
	/* 115 */         return null;
	/*     */       }
	/* 117 */       Object advertisingInfo = Utility.invokeMethodQuietly(null, getAdvertisingIdInfo, new Object[] { context });
	/*     */ 
	/* 119 */       if (advertisingInfo == null) {
	/* 120 */         return null;
	/*     */       }
	/*     */ 
	/* 123 */       Method getId = Utility.getMethodQuietly(advertisingInfo.getClass(), "getId", new Class[0]);
	/* 124 */       Method isLimitAdTrackingEnabled = Utility.getMethodQuietly(advertisingInfo.getClass(), "isLimitAdTrackingEnabled", new Class[0]);
	/*     */ 
	/* 127 */       if ((getId == null) || (isLimitAdTrackingEnabled == null)) {
	/* 128 */         return null;
	/*     */       }
	/*     */ 
	/* 131 */       AttributionIdentifiers identifiers = new AttributionIdentifiers();
	/* 132 */       identifiers.androidAdvertiserId = ((String)Utility.invokeMethodQuietly(advertisingInfo, getId, new Object[0]));
	/*     */ 
	/* 134 */       identifiers.limitTracking = ((Boolean)Utility.invokeMethodQuietly(advertisingInfo, isLimitAdTrackingEnabled, new Object[0])).booleanValue();
	/*     */ 
	/* 137 */       return identifiers;
	/*     */     } catch (Exception e) {
	/* 139 */       Utility.logd("android_id", e);
	/*     */     }
	/* 141 */     return null;
	/*     */   }
	/*     */ 
	/*     */   private static AttributionIdentifiers getAndroidIdViaService(Context context) {
	/* 145 */     GoogleAdServiceConnection connection = new GoogleAdServiceConnection(null);
	/* 146 */     Intent intent = new Intent("com.google.android.gms.ads.identifier.service.START");
	/* 147 */     intent.setPackage("com.google.android.gms");
	/* 148 */     if (context.bindService(intent, connection, 1)) {
	/*     */       try {
	/* 150 */         GoogleAdInfo adInfo = new GoogleAdInfo(connection.getBinder());
	/* 151 */         AttributionIdentifiers identifiers = new AttributionIdentifiers();
	/* 152 */         identifiers.androidAdvertiserId = adInfo.getAdvertiserId();
	/* 153 */         identifiers.limitTracking = adInfo.isTrackingLimited();
	/* 154 */         return identifiers;
	/*     */       } catch (Exception exception) {
	/* 156 */         Utility.logd("android_id", exception);
	/*     */       } finally {
	/* 158 */         context.unbindService(connection);
	/*     */       }
	/*     */     }
	/* 161 */     return null;
	/*     */   }




延伸阅读：

[精准互联网广告模式RTB、SSP、AdExchange、DMP、DSP-就看你想要的](http://www.litchi.me/litchi-20150687.html)

[Facebook广告陷入信任危机：虚假点击泛滥](http://www.litchi.me/litchi-201506520.html/6/)

[反作弊案例](http://www.litchi.me/litchi-201506520.html/7/)

[DSP基础算法与模型研究](http://www.litchi.me/litchi-201506518.html)

[代码级分析揭秘安卓广告SDK病毒化的事实](http://www.guokr.com/post/335516/)

[移动广告之广告平台选择（Admob&Facebook）](http://leotse90.com/2016/03/22/Mobile-AD-Admob-Integration/)

[Facebook原生广告接入总结](http://blog.csdn.net/figo0423/article/details/46914423)

[IOS Facebook接入](https://yq.aliyun.com/articles/29855)

[facebook sdk接入部分问题和解决方案](http://tjlibaoh.blog.163.com/blog/static/211226413201492922154362/)

[浅析Facebook 广告相关性评分](http://facebookjia.com/facebook-ads/%E6%B5%85%E6%9E%90facebook-%E5%B9%BF%E5%91%8A%E7%9B%B8%E5%85%B3%E6%80%A7%E8%AF%84%E5%88%86)

[Facebook 移动广告投放方式分析](http://facebookjia.com/facebook-ads/facebook-%E7%A7%BB%E5%8A%A8%E5%B9%BF%E5%91%8A%E6%8A%95%E6%94%BE%E6%96%B9%E5%BC%8F%E5%88%86%E6%9E%90)

[Custom Audiences和Lookalike Audiences广告形式浅析](http://facebookjia.com/facebook-ads/custom-audiences%E5%92%8Clookalike-audiences%E5%B9%BF%E5%91%8A%E5%BD%A2%E5%BC%8F%E6%B5%85%E6%9E%90)



#Facebook填充率低的原因分析

- 填充率是什么？
- 填充率如何计算？
- 有什么因素可以影响填充率？


###什么是填充率？它和展示次数有什么区别？

先来理解一些关键指标，以下为 Audience Network 计算这些关键指标的方式：

**请求**：当 Audience Network 收到初始广告请求时，即被计算在内。请求来源可以为应用或网站（如果您未使用其他广告网络），或者中介平台。某些请求可能未得到填充；例如，用户在请求完成前便关闭了应用。

**填充**：当 Audience Network 向应用或网站提供广告时，即被计算在内。但是这并不意味着广告会展示给用户；很多移动发行商通常会同时请求多个广告并缓存广告，等待广告版位可用后再展示广告。
注意：很多其他平台将填充的广告计为展示次数，即使无人看过该广告。

**展示次数**：当广告出现在屏幕可视区域时才被计算在内，因为我们希望确保用户能和看到的广告互动。通常，当用户和广告的互动为广告主带来转化时，广告版位将更具价值，从而为广告主带来更多收益。我们将通过 Advertiser Outcome Score 衡量版位价值。（Advertiser Outcome Score 不适用于移动网站版位。） 
注意：很多广告服务器和中介平台计算展示次数的方式不同。在某些情况下，当广告得到填充但并未被用户看到时，也会计为展示次数。而在另外一些情况下，只有当至少一半广告显示于屏幕可视区域，且持续时间不低于 1 秒钟时，才计为展示次数。

如果我们在特定时间段内收到太多来自同一广告版位的请求，Audience Network 将忽略这些广告请求。但其他平台可能视这些请求为有效请求。

[详见参考](http://zh-cn.id-id.m.fb.me/help/audiencenetwork/1028565693903694)


**填充率**指我们提供的广告次数和您请求投放的广告次数之间的比率。

填充率的计算方法为填充的广告次数除以请求投放的广告次数。

如何计算填充率？
填充率是指填充的广告与合格广告请求两个总数之间的比率。不计次数的请求情形包括：

频繁请求同一条广告。例如，在低于 15 秒的间隔时间内重复请求同一版位编号的广告
IDFA (iOS) 或广告编号 (Android) 不可用或不能用于广告投放
提出请求的应用尚未安装 Facebook 集成或用户最近未登录

[参考资料](https://developers.facebook.com/docs/audience-network/faq#a7)


展示次数指用户实际看到的广告次数。某些填充的广告次数可能未被用户看到。例如，如果您缓存了某些广告以减少页面加载时间，则用户可能在所有广告展示之前便退出了应用。

广告主只需支付展示次数（用户看到的广告次数）的费用。作为移动发行商，您将根据展示次数获得相应收益。

在 Android 设备上，Audience Network 可与适用于 Android SDK 的 API 版本 9 及以上的应用集成。

测试线上广告时，出现“1001 no fill”的错误提示，这意味着什么？
这意味着我们无法向此用户展示广告。出现这种错误的原因如下：

测试的用户未在移动设备上登陆原生 Facebook 应用
测试的用户在设备设置中启用了“限制广告追踪”（仅限 iOS），或者选择了“退出根据兴趣的广告”（仅限 Android）。
我们没有可向此用户展示的广告库存
请注意，如果您能看到测试广告，就证明您的嵌入运行正常，并且广告投放后用户能在您的应用中看到广告。

我的应用用户是否有权选择屏蔽 Facebook 广告？
只要用户在 iOS 和 Android 设备中开启广告屏蔽功能，我们就不会向他们展示广告。

填充率为什么会随着时间的推移而发生变动？
填充率可能会随 Audience Network 的持续演进而发生变动。许多因素都会改变和影响应用中能投放的广告数量，如用户群体的人口统计数据、地理数据和广告主的需求。

如果您的填充率突然发生变化，请检查并确保：

嵌入无误
未添加任何其他筛选条件
使用最新版的 SDK
没有过于频繁地提交请求（例如：在 15 秒的间隔时间内多次请求同一版位编号的广告）

###[使用中介服务时，如何最大程度提升版位填充率和收入？](https://www.facebook.com/help/audiencenetwork/821011588003559)


--- 

需求：聚合第三方SDK，同时尽量减少接入Size

现状：目前支持动态下发Jar包，纯代码的逻辑可以动态控制，对于资源（例如图片，xml，string.xml等）无法通过动态下发获取。

分析：

* 方案一 修改动态下发框架，支持下发资源，例如加载整个apk，修改成本大；

* 方案二 将资源相关的统计加到外壳里面，然后动态下发jar代码，通过代理的方式去调用jar里面的代码逻辑；

* 方案三 客户端直接接入 成本低，无法较少size；

https://realm.io/cn/news/oredev-ty-smith-building-android-sdks-fabric/



### 总结归纳
[随想录:开发一流Android SDK](http://blog.csdn.net/dd864140130/article/details/53558011)
[android日常开发总结的技术经验60条](http://www.vmatianyu.cn/summarization-of-technical-experience.html)
