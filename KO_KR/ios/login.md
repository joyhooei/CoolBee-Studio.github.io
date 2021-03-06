#MSDK 토큰

##개요

이 모듈은 MSDK 모든 인증 관련 모듈을 정리하고 인증 로그인, 자동 로그인, 퀵로그인, 토큰 업데이트, 읽기 등 모듈에 대한 자세한 설명이 포함됩니다. 게임에서 우선 해당 모듈을 참조하여 MSDK의 모든 인증 관련 모듈을 이해한 후 게임 자체 수요에 따라 해당한 인터페이스를 사용하여 인증 등 기능을 구현할 수 있습니다.

##용어 해석, 인터페이스 설명
	
###로그인 관련 용어 해석:
| 명칭| 설명 |지원 플랫폼| 호출 인터페이스 |
|: ------------- :|
| 인증 로그인 | 플랫폼의 인증 UI를 호출하여 유저가 게임에 인증하고 로그인에 필요한 토큰을 받도록 안내| 모바일QQ/Wechat | WGLogin |
| 퀵로그인 | 유저의 조작은 플랫폼에서 게임 실행시 로그인 관련 토큰 정보를 투과 전송하여 게임에 로그인하게 함| 모바일QQ| 없음 |
| 자동 로그인 | 게임 시작시 유저의 최근 게임 로그인 토큰 정보를 직접 사용하여 게임에 로그인| MSDK에서 기능 제공| WGLoginWithLocalInfo |
| 자동 업데이트 | MSDK가 Wechat 토큰 자동 업데이트 인터페이스 제공| MSDK에서 기능 제공 | 없음 |
| Diff계정 | 현재 게임에 로그인한 계정과 플랫폼에 로그인한 계정 불일치 | 플랫폼/MSDK 모두 지원| WGSwitchUser |

### 로그인 관련 인터페이스 개요

로그인과 관련된 호출 인터페이스 중 `WGGetLoginRecord`, `WGLogout`는 Synchronization 인터페이스이고 기타 인터페이스는 모두 Asynchronous로 구현되었으므로 callback 형식으로  OnLoginNotify(LoginRet)를 통하여 최종 결과를 게임에 콜백합니다. diff 계정과 관련된 인터페이스는 MSDK의 diff 계정 모듈에서 별도로 설명합니다. 상세한 내용은 아래와 같습니다.

| 명칭| 설명 |비고 |
|: ------------- :|
| WGGetLoginRecord | 로컬에 저장된 현재 유저의 로그인 토큰 획득 |  |
| WGSetPermission | 게임이 유저 인증을 통하여 획득할 플랫폼 정보 설정 | |
| WGLogin | 플랫폼을 실행하여 인증 로그인 진행 |  |
| WGLogout | 현재 로그인한 계정의 로그인 정보 전부 제거 |  |
| WGLoginWithLocalInfo | 로컬에 저장된 로그인 토큰으로 로그인 시도|  |
| handleCallback | 각 플랫폼 호출 처리 |  |
| WGRefreshWXToken | WechatrefreshToken으로 업데이트하여 accessToken 획득 |  MSDK 2.0부터 게임에서 스스로 Wechat 토큰을 업데이트하는 방식을 권장하지 않음 |

### 추천 로그인 프로세스（2.6.0i이전 버전에）

![login](./recommend_login.png)

**스텝 설명：**

* 스텝1：게임 시작 후에 우선 MSDK를 초기화합니다.
* 스텝2：자동 로그인 인터페이스 `WGLoginWithLocalInfo()`호출하여, 해당 인터페이스는 MSDK 백그라운드에서 로컬 토큰 유효 여부를 체크합니다.
* 스텝3：자동 로그인 인터페이스는 `OnLoginNotify(LoginRet ret)`를 통하여 로그인 결과를 게임에 콜백하며`ret.flag`를 통하여 로그인 결과를 판단합니다.`eFlag_Succ`(0)혹은 `eFlag_WX_RefreshTokenSucc`(2005)일 경우는 성공이며 기타 경우는 실패입니다.
* 스텝4：로그인 인증 성공되면 스텝9로 이동합니다.
* 스텝5：자동 로그인 실패하면,` WGLogin(EPlatform platform)`를 호출하여, QQ 혹은 위챗 플랫폼 로그인 인증을 호출합니다.
* 스텝6：로그인 결과는 `OnLoginNotify(LoginRet ret)`를 통하여 콜백.`ret.flag`를 통하여 결과 판단.`eFlag_Succ`(0)일 경우는 성공이고 기타 경우는 실패.
* 스텝7：플랫폼 호출하여 로그인 인증 실패하면, 유저로 하여금 로그인 재시도를 안내하여 스텝5로 이동합니다.
* 스텝8：로그인 인증 성공.
* 스텝9：클라이언트에 있는 최신 토큰을 게임 서버에 동기화합니다.서버에서 로그인 토큰을 필요할 경우 **로그인 성공 콜백을 받은 후 최신 토큰을 동기화하여야 합니다.** 하여 서버에서 실효된 토큰을 사용하는 상황을 피할 수 있습니다. 

###로그인 관련 인터페이스 권장 사용법

1. 인증 로그인: 직접 `WGLogin`을 호출하여 해당한 플랫폼의 인증을 획득합니다.
- 게임 시작 혹은 게임이 백그라운드에서 포어그라운드로 전환시 토큰 유효성 검증: `WGLoginWithLocalInfo`를 호출하여 토큰 유효성을 검증합니다.
- 토큰 획득: 직접 `WGGetLoginRecord`를 호출하여 로컬에서 읽기.
- 로그아웃: 직접 `WGLogout`를 호출하여 현재 유저의 로그인 정보를 삭제합니다.

## 로그인 연동 구체 작업(개발자 필독)

**게임 개발자는 아래 제공한 절차에 따라 MSDK로그인 모듈 연동를 진행하여 연동 비용과 누락된 처리 로직을 줄일 수 있습니다. 이 부분 내용은 숙지 바랍니다.**

1. 유저 인증이 필요한 권한 설정:
	- 게임은 MSDK 초기화 이후 모바일QQ 권한 설정 인터페이스를 호출하여 유저 인증이 필요한 게임 플랫폼 권한을 설정하여야 합니다. 상세 내용은 [클릭](#유저 인증이 필요한 권한 설정 WGSetPermission)참조 바랍니다.
- **인증 로그인 처리**：
	1. 로그인 버튼 클릭 이벤트의 처리 함수에서 `WGLogin`을 호출하여 인증 로그인을 진행합니다. 상세 내용은 [클릭](#인증 로그인 처리 WGLogin)참조 바랍니다.
- **자동 로그인 처리**：
	1. 게임이 포그라운드로 이동한 후 `WGLoginWithLocalInfo`를 호출하여 게임 실행시 자동 로그인 진행. 구체 방법[클릭하여 보기](#자동 로그인 처리 WGLoginWithLocalInfo)
	- AppDelegate의 applicationDidEnterBackground에서 게임이 백그라운드로 전환하는 시간을 판단합니다. 30분을 초과하면 자동으로 `WGLoginWithLocalInfo`를 호출하여 자동 로그인을 진행합니다.
		- 게임에서 백그라운드로 전환하는 시간 판단에 대해서는 MSDK의 demo 방식을 참조 바랍니다. 전환시 타임 스탬프를 1개 기록하며 리턴 후 시간차를 계산합니다.
- **유저 로그아웃 처리**:
	- 로그아웃 버튼 클릭 이벤트의 처리 함수에서 WGLogout를 호출하여 인증 로그인 진행합니다. 상세 내용은 [클릭](#유저 로그아웃 처리 WGLogout)참조 바랍니다.
- **MSDK의 로그인 콜백 처리**:
	- 게임의 MSDK 콜백 처리 로직에 onLoginNotify에 대한 처리를 추가합니다. 상세 내용은 [클릭](#MSDK의 로그인 콜백 처리)참조 바랍니다.
- **MSDK의 실행 콜백 처리**:
	- 게임의 MSDK 콜백 처리 onWakeUpNotify에서 플랫폼 실행에 대한 처리 추가. 상세 내용은 [클릭](#MSDK의 실행 콜백 처리)참조 바랍니다.
- **Diff계정 처리 로직**:
	- 게임이 Diff계정에 대한 처리 로직, 상세 내용은 [MSDK Diff계정 연동](diff-account.md#Diff계정 처리 로직(개발자 주목))참조 바랍니다.
- **기타 특수 로직 처리**:
	- 저 메모리 설비에서 인증 진행시 게임이 강제 종료된 후의 로그인 방안은 [클릭](#모바일QQ 저 메모리 설비에서 인증 진행시 게임 강제 종료된 후 로그인 방안)참조 바랍니다.
	- MSDKWechat 토큰 기한 만료시 자동 업데이트 메커니즘. 구체방법[클릭하여 보기](#Wechat 토큰 자동 업데이트)
	- 로그인 데이터 업로드 인터페이스를 호출하는 요구는 [클릭](#로그인 데이터 업로드)참조 바랍니다.

##유저 인증이 필요한 권한 설정 WGSetPermission

####개요

게임은 MSDK 초기화 이후 모바일QQ 권한 설정 인터페이스를 호출하여 유저 인증이 필요한 게임 플랫폼 권한을 설정하여야 합니다.

####인터페이스 설명

	/**
	 * @param permissions ePermission 열거값 혹은 연산 결과, 필요한 인증 항 표시
	 * @return void
	 */
	void WGSetPermission(int permissions);

#### 인터페이스 호출:

	// QQ 실행시 필요한 유저 인증 항 설정
	WGPlatform.WGSetPermission(WGQZonePermissions.eOPEN_ALL); 

#### 주의사항:

1. 게임은 MSDK 초기화 이후에 이 인터페이스를 호출하여야 하며 인터페이스 파라미터는 **`eOPEN_ALL`**을 입력할 것을 권장합니다. 이 내용이 부족하면 게임이 일부 인터페이스 호출 시 권한이 없다는 안내가 팝업할 수 있습니다.


##인증 로그인 처리 WGLogin

#### 개요:

**모바일QQ/Wechat 클라이언트 혹은 web페이지(모바일QQ 미설치)를 실행하여 인증을 진행하며 유저가 인증 완료된 후 onLoginNotify를 통하여 openID, accessToken, payToken, pf, pfkey 등 로그인 정보를 획득하였다는 통보를 게임에 전송합니다.**


####인터페이스 설명:

	/**
	 * @param platform 게임에서 전송한 플랫폼 유형, 가능한 값: ePlatform_QQ, ePlatform_Weixin
	 * @return void
	 *   게임이 설정한 전역 콜백의 OnLoginNotify(LoginRet& loginRet) 메소드를 통하여 데이트를 게임에 리턴
	 */
	void WGLogin(ePlatform platform);

#### 인터페이스 호출:

	WGPlatform::GetInstance()->WGLogin(ePlatform_QQ); 

#### 주의사항:

- **통용**:
	- **OnLoginNotify/OnWakeupNotify콜백을 받지 못합니다.
		- AppDelegate의 didFinishLaunchingWithOptions함수가 YES로 return함을 확보하여야 합니다.
		- info.plist에 QQ와 위챗 appid/appkey설정이 정확하여야 합니다.
		- Url Schemes는 wiki 요구에 따라 설정하여야 합니다.
	
##자동 로그인 처리 WGLoginWithLocalInfo

#### 개요:

이 인터페이스는 전에 로그인했던 게임에서 사용되며 유저가 다시 게임을 접속했을 시 게임은 이 인터페이스를 우선 호출하여 백그라운드에서 토큰을 검증한 후 OnLoginNotify를 통하여 결과를 게임에 콜백합니다. 게임에서 위챗 토큰 리셋과  QQ/위챗 AccessToken검증 등을 처리할 필요가 없습니다.

####인터페이스 설명:

	/**
	  *  @since 2.0.0
	  *  이 인터페이스는 전에 로그인했던 게임에 사용됩니다. 유저가 다시 게임을 접속했을 시 게임은 이 인터페이스를 우선 호출하여 백그라운드에서 토큰 검즘을 진행합니다.
	   *  이 인터페이스는 OnLoginNotify를 통하여 결과를 게임에 콜백하고 결과가`eFlag_Succ`(0)혹 `eFlag_WX_RefreshTokenSucc`(2005)일 경우 로그인 성공임을 의미하며 기타 결과일 경우 로그인 실패임을 의미합니다.
	  *  로컬에 토큰이 없거나 로컬 토큰 검증 실패시 반환하는 flag는 eFlag_Local_Invalid이다. 게임이 이 flag를 받으면 유저를 인증 페이지로 안내하여 인증을 진행하면 된다.
	  *  로컬에 토큰이 있고 검증에 성공되면 flag는 eFlag_Succ입니다. 게임에서 이 flag를 받으면 재차 검증할 필요가 없이 sdk에서 제공한 토큰을 직접 사용할 수 있습니다.
	  *  @return void
	  *   Callback: 검증 결과는 OnLoginNotify를 통하여 리턴합니다.
	  */
 	void WGLoginWithLocalInfo();

####주의사항:

1. 게임에서 `WGLoginWithLocalInfo`를 사용하여 로그인시 획득한 토큰은 게임 백그라운드에 전송하여 유효성 검증를 진행할 필요가 없이 MSDK가 검증한 후 게임에 콜백합니다.

##유저 로그아웃 처리 WGLogout

#### 개요:

이 인터페이스를 호출하면 현재 로그인 계정의 로그인 정보를 삭제할 수 있습니다.

####인터페이스 설명:

	/**
	 * @return bool 리턴값을 이미 폐기, 모두 true 반환
	 */
	bool WGLogout();

####호출 예시:

    WGPlatform.WGLogout();

####주의사항:

1. 게임이 **로그아웃 버튼을 클릭하거나 로그인창이 팝업되는 로직에서 WGLogout를 호출하여 로컬 로그인 정보를 모두 삭제하여야 합니다.**. 아닐 경우 인증 실패 등 문제를 초래할 수 있습니다.

##MSDK의 로그인 콜백 처리

#### 개요:

MSDK의 로그인 콜백은 아래 몇가지 상황에서 발생합니다.

- WGLogin 인증에서 돌아오는 상황
- WGLoginWithLocalInfo 로그인에서 돌아오는 상황
- 플랫폼 실행을 처리한 후(토큰을 갖고 실행하면)

#### 상세 처리:

	OnLoginNotify(LoginRet ret) {
        Logger.d("called");
        switch (ret.flag) {
            case CallbackFlag.eFlag_Succ:
				 CallbackFlag.eFlag_WX_RefreshTokenSucc
            	//인증 성공의 처리 로직
				break;
            case CallbackFlag.eFlag_WX_UserCancel:
				 CallbackFlag.eFlag_QQ_UserCancel
				//유저가 인증을 취소하는 로직
				break;
			case CallbackFlag.eFlag_WX_UserDeny
				//유저가 Wechat인증을 거절하는 로직
				break;
            case CallbackFlag.eFlag_WX_NotInstall:
				//유저 설비에 Wechat 클라이언트가 설치되지 않은 로직
				break;
			case CallbackFlag.eFlag_QQ_NotInstall:
				//유저 설비에 QQ 클라이언트가 설치되지 않은 로직
				break;
            case CallbackFlag.eFlag_WX_NotSupportApi:
				//유저 Wechat 클라이언트가 이 인터페이스 로직을 지원하지 않음
				break;
            case CallbackFlag.eFlag_QQ_NotSupportApi:
				//유저 모바일QQ 클라이언트가 이 인터페이스 로직을 지원하지 않음
				break;
            case CallbackFlag.eFlag_NotInWhiteList
				//유저 계정이 화이트리스트에 없는 로직
				break;
            default:
                // 기타 로그인 실패 로직
                break;
        }
    }

#### 주의사항:
**여기에서 다만 중요한 loginNotify의 로직만 처리했을 뿐입니다. 완전한 콜백 flag 정보는 [콜백표시eFlag](const.md#콜백표시eFlag)를 클릭하여 확인할 수 있습니다. 게임에서 자체 수요에 따라 처리할 수 있습니다.**

##MSDK의 실행 콜백 처리

#### 개요

게임에서 플랫폼 실행에 대한 처리는 주로 Diff계정과 관련된 로직을 처리하는 것입니다. 상세 처리는 다음과 같습니다.

#### 상세 처리：

        if (CallbackFlag.eFlag_Succ == ret.flag
                || CallbackFlag.eFlag_AccountRefresh == ret.flag) {
            //실행 후 로컬 계정을 통하여 게임에 로그인하는 것을 표시. 처리 로직은 onLoginNotify와 일치합니다.
            
        } else if (CallbackFlag.eFlag_UrlLogin == ret.flag) {
            // MSDK은 계정을 불러와 토큰을 갖고 인증 로그인 시도. 결과는 OnLoginNotify에서 콜백. 이럴 경우 게임은 onLoginNotify 콜백을 대기합니다.

        } else if (ret.flag == CallbackFlag.eFlag_NeedSelectAccount) {
            // 현재 게임에 Diff계정이 존재하므로 게임은 대화창을 팝업하여 유저에게 로그인할 계정을 선택하도록 안내하여야 합니다.

        } else if (ret.flag == CallbackFlag.eFlag_NeedLogin) {
            // 유효 토큰이 없어 게임 로그인 실패. 이럴 경우 게임은 WGLogout를 호출하여 로그아웃하여 유저가 다시 로그인하도록 안내합니다.

        } else {
            //기본 처리 로직은 게임이 WGLogout를 호출하여 게임에서 로그아웃한 후 유저가 다시 로그인할 것을 권장합니다.
        }

##Diff계정 처리 로직

Diff계정 관련 모듈은 [MSDK Diff계정 연동](diff-account.md#Diff계정 처리 로직(개발자 주목）)을 참조 바랍니다.

##기타 특수 로직 처리

### 모바일QQ 저메모리 설비에서 인증 진행시 게임이 강제 종료된 후의 로그인 방안

대부분 게임이 메모리를 많이 차지하므로 인증 과정에서 모바일QQ를 호출하여 인증 진행 시 iOS 메모리 복구 메커니즘이 백그라운드의 게임 프로세스를 강제로 종료시켜 게임 모바일QQ 인증이 게임에 접속 못하는 문제를 초래할 수 있습니다. 게임은 AppDelegate에서 아래 코드를 추가하여 프로세스가 종료된 후에도 토큰을 갖고 게임에 접속할 수 있도록 하여야 합니다.
```ruby
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
    NSLog(@"!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!url == %@",url);
    WGPlatform* plat = WGPlatform::GetInstance();
    WGPlatformObserver *ob = plat->GetObserver();
    if (!ob) {
        MyObserver* ob = new MyObserver();
        ob->setViewcontroller(self.viewController);
        plat->WGSetObserver(ob);
    }
	//광고 기능을 구현되지 않을 경우 WGADObserver 설정하지 않아도 됩니다.
    WGADObserver *adOb = plat->GetADObserver();
    if (!adOb) {
        MyAdObserver *adObserver = new MyAdObserver();
        plat->WGSetADObserver(adObserver);
    }
     return  [WGInterface  HandleOpenURL:url];
}
```
### Wechat 토큰 자동 업데이트

1. MSDK2.0.0부터 게임 운행 기간에 주기적으로 Wechat 토큰을 검증하고 업데이트합니다. 업데이트 필요할 경우 MSDK가 자동으로 업데이트을 진행하고 OnLoginNotify를 통하여 게임에 통보합니다. flag는 eFlag_WX_RefreshTokenSucc와 eFlag_WX_RefreshTokenFail입니다.(이미 onLoginNotify의 콜백에 포함됨).
- **게임에서 신규 토큰을 받으면 게임 클라이언트에 저장된 토큰과 서버의 토큰을 동기적으로 업데이트하여 신규 토큰으로 향후 절차를 진행할 수 있도록 하여야 합니다.**
- 게임에 Wechat 토큰 자동 업데이트 기능이 필요하지 않으면 `info.plist`에서 `AutoRefreshToken`를 `NO`로 설정하면 된다.

## 기타 인터페이스 리스트

###WGGetLoginRecord

#### 개요:

이 인터페이스를 호출하면 현재 계정의 로그인 정보를 획득할 수 있습니다.

####인터페이스 설명:

	/**
	 * @param loginRet 리턴한 기록
	 * @return 리턴 값은 플랫폼 id, 유형은 ePlatform, ePlatform_None을 리턴하면 로그인 기록이 없음을 표시합니다.
	 *   loginRet.platform(유형 ePlatform) 플랫폼id 표시, 가능한 값은 ePlatform_QQ, ePlatform_Weixin, ePlatform_None.
	 *   loginRet.flag(유형 eFlag) 현재 로컬 토큰의 상태 표시, 가능한 값과 설명은 다음과 같습니다.
	 *     eFlag_Succ: 인증 토큰 유효
	 *     eFlag_QQ_AccessTokenExpired: 모바일QQ accessToken 기한 만료, 인증 인터페이스 표시, 유저가 다시 인증하도록 안내합니다.
	 *     eFlag_WX_AccessTokenExpired: WechataccessToken 토큰 기한 만료, WGRefreshWXToken을 호출하여 업데이트하여야 합니다.
	 *     eFlag_WX_RefreshTokenExpired: WechatrefreshToken, 인증 인터페이스 표시, 유저가 다시 인증하도록 안내합니다.
	 *   ret.token은 하나의 Vector<TokenRet>이고 그중에 저장된 TokenRet에 type와 value가 있습니다. Vector 순회를 통하여 type를 판단하여 필요한 토큰을 획득합니다.
     *
	 */
	int WGGetLoginRecord(LoginRet& loginRet);

####호출 예시:

    LoginRet ret = new LoginRet();
    WGPlatform.WGGetLoginRecord(ret);

획득한 LoginRet 중 flag가 eFlag_Succ이면 로그인이 유효하다고 인정할 수 있으며 유효한 토큰 정보를 읽을 수 있습니다. 그중 token은 아래 방식으로 획득할 수 있습니다.

Wechat 플랫폼:

   NSString *accessToken = "";
    NSString *refreshToken = "";
    for (int i = 0; i < loginRet.token.size(); i++) {
             if (loginRet.token.at(i).type == eToken_WX_Access) {
                 accessToken = [NSString stringWithCString:loginRet.token.at(i).value.c_str() encoding:NSUTF8StringEncoding];
             } else if (loginRet.token.at(i).type == eToken_WX_Refresh) {
                 payToken = [NSString stringWithCString:loginRet.token.at(i).value.c_str() encoding:NSUTF8StringEncoding];
             }
    }

QQ 플랫폼:

    NSString *accessToken = "";
    NSString *payToken = "";
    for (int i = 0; i < loginRet.token.size(); i++) {
        if (loginRet.token.at(i).type == eToken_QQ_Access) {
            accessToken = [NSString stringWithCString:loginRet.token.at(i).value.c_str() encoding:NSUTF8StringEncoding];
        } else if (loginRet.token.at(i).type == eToken_QQ_Pay) {
            payToken = [NSString stringWithCString:loginRet.token.at(i).value.c_str() encoding:NSUTF8StringEncoding];
        }
    }

#### 주의사항:

없음

##자주 발생하는 문제

1. 결제시 paytoken 기한 만료를 제시하면 로그인 인터페이스를 호출하여 다시 인증하여야 결제할 수 있습니다. paytoken 기한이 만료되면 다시 인증하여야 합니다.

## MSDK2.6.0i 및 이후 버전에 토큰 자동 업데이트 프로세스
**개요**

MSDK2.6.0i및 이후 버전에서 이전 버전의 로그인 프로세스 기반으로 새로운 프로세스를 최적화하였습니다. 게임은 'WGLogin' 'WGGetLoginRecord'만 주목하면 로그인과 토큰 처리를 완료할 수 있습니다.

* 게임이 토큰으로 로그인 하여야할 시의 호출 로직：
  ![login_1](./login_1.jpg)
  `만약'WGGetLoginRecord'비0으로 리턴할 경우 WGLogin() 를 호출하여（플랫폼 파라미터 첨부 불필요）onLoginNotify에서 토큰 비동기 업데이트 결과를 처리합니다.`
* 게임이 로컬 토큰을 사용하여 로그인하여야 할 시,'WGLoginWithLocalInfo'호출할 필요 없으며 WGLogin()를 호출하여 onLoginNotify의 결과만 대기하면 됩니다.
* MSDK내부 토큰 정시 업데이트 로직：
  ![login_4](./login_4.jpg)

**새로운 호출 스텝 설명：**

* 스텝1：게임 시작한 후 우선 MSDK초기화합니다.
* 스텝2：자동 로그인 인터페이스`WGLogin() 를 호출하여（플랫폼 파라미터 첨부 불필요）`，해당 인터페이스는 MSDK 백그라운에서 로컬 토큰 유효여부를 확인합니다.
* 스텝3：자동 로그인 인터페이스는 `OnLoginNotify(LoginRet ret)`를 통하여 로그인 결과를 게임에 콜백합니다. `ret.flag`를 통하여 로그인 결과를 판단하여 `eFlag_Succ`(0)일 경우 로그인 성공임을 의미하며 기타일 경우 실패임을 의미합니다.
* 스텝4：로그인 인증 성공하면 스텝9로 이동합니다.
* 스텝5：자동 로그인 실패하면,` WGLogin(EPlatform platform)`를 콜하여, QQ 혹은 위챗 플랫폼 로그인 인증을 호출합니다.
* 스텝6：로그인 결과는 `OnLoginNotify(LoginRet ret)`를 통하여 콜백합니다.`ret.flag`를 통하여 결과 판단하여`eFlag_Succ`(0)일 경우는 성공임을 의미하며 기타 경우 실패임을 의미합니다.
* 스텝7：플랫폼 호출하여 로그인 인증 실패하면, 유저로 하여금 로그인 재시도를 안내하여 스텝5로 이동합니다.
* 스텝8：로그인 인증 성공.
* 스텝9：클라이언트에 있는 최신 토큰을 게임 서버에게 동기화합니다.서버에서 로그인 토큰 필요할 경우 **로그인 성공의 콜백을 받은 후 최신 토큰을 동기화하여야 합니다.** 하여 서버에서 실효된 토큰을 사용하는 상황을 피할 수 있습니다. 
