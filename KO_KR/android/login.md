#MSDK 로그인 모듈

##개요

MSDK 모든 인증 관련 모듈을 정리하는 바 인증 로그인,자동 로그인,퀵로그인,토큰 업데이트,읽기 등 모듈에 대한 자세한 설명이 포함됩니다. 게임에서 우선 해당 모듈을 참조하여 MSDK의 모든 인증 관련 모듈을 이해 한 후 게임 자체 수요에 따라 해당한 인터페이스를 사용하여 인증 등 기능을 구현할 수 있습니다.

## 로그인 연동 작업 내역（개발자 필독）

**`게임 개발사에서 아래 절차에 따라 MSDK 로그인 모듈의 연동을 진행하여 연동 비용과 처리 로직 누락 이슈를 줄일 수 있습니다.이 부분 내용을 숙지하고 모든 관련 로직을 처리할 것을 권장드립니다.！！！`**

1. 유저 인증이 필요한 권한 설정:
	- 게임은 MSDK 초기화 후 모바일QQ 권한 설정 인터페이스를 호출하여 유저 인증이 필요한 게임 플랫폼 권한을 설정합니다. 구체 방법[클릭](#유저 인증이 필요한 권한 설정 WGSetPermission).
- **인증 로그인 처리**：
	1. 로그인 버튼의 클릭 이벤트의 처리 함수에서 `WGLogin`을 호출하여 인증 로그인 진행. 상세 내용[클릭](#인증 로그인 처리 WGLogin)
- **자동 로그인 처리**：
	1. 메인 Activity의 onCreate에서 MSDK 초기화 후 `WGLoginWithLocalInfo`를 호출하여 게임 실행시 자동 로그인 진행. 상세 내용[클릭](#자동 로그인 처리 WGLoginWithLocalInfo)
	 - 메인 Activity의 onResume에서 게임이 백그라운드로 전환하는 시간 판단. 30분을 초과하면 자동으로 `WGLoginWithLocalInfo`를 호출하여 자동 로그인 진행
		- 게임에서 백그라운드로 전환하는 시간 판단은 MSDK의 demo 방식을 참조 바라며 전환 시 타임 스탬프를 1개 기록하고 리턴 후 시간차이를 계산합니다.
- **유저 로그아웃 처리**:
	- 로그아웃 버튼 클릭 이벤트의 처리 함수에서 WGLogout를 호출하여 인증 로그인 진행. 상세 내용[클릭](#유저 로그아웃 처리 WGLogout)
- **MSDK의 로그인 콜백 처리**:
	- 게임에서 MSDK 콜백 처리 로직에 onLoginNotify에 대한 처리 추가. 상세 내용[클릭](#MSDK의 로그인 콜백 처리)
- **플랫폼 실행 처리**:
	- 게임 메인 activity의 onCreate와 onNewIntent에서 handleCallback를 호출하여 플랫폼 실행에 대한 처리 진행. 상세 내용[클릭](#플랫폼 실행 처리 handleCallback)
- **MSDK 실행 콜백 처리**:
	- 게임의 MSDK 콜백 처리 onWakeUpNotify에서 플랫폼 실행에 대한 처리 추가. 상세 내용[클릭](#MSDK의 실행 콜백 처리)
- **Diff 계정 처리 로직**:
	- 게임이 Diff 계정에 대한 처리 로직,상세 내용은 [MSDK diff계정 연동](diff-account.md# diff계정 처리 로직(개발자 주목)) 참조 바랍니다.
- **기타 특수 로직 처리**:
	- 저메모리 설비에서 인증 진행시 게임 강제 종료된 후의 로그인 방안. 상세 내용[클릭](#모바일QQ 저메모리 설비에서 인증 진행시 게임 강제 종료된 후의 로그인 방안)
	- **` MSDKWechat 토큰 기한 만료시 자동 업데이트 메커니즘.`**상세 내용[클릭](#Wechat 토큰 자동 업데이트)
	- 로그인 데이터 업로드 인터페이스 호출 요구. 상세 내용[클릭](#로그인 데이터 업로드)
	
**`MSDK2.7.0a버전부터 로그인 프로세스에 대해 최적화하였으며 일부 호출을 수정하였으므로 주의 할 부분은 아래와 같습니다. 숙지 필요！！！`**

- **자동 로그인 처리**：

   - 1.MSDK2.7.0a 및 이후 버전에서, WGLoginWithLocalInfo() 호출을 WGLogin(EPlatform.ePlatform_None)로 수정하였지만 WGLoginWithLocalInfo도 겸용할 수 있습니다.

   - 2.MSDK2.7.0a 및 이후 버전에 게임 시작 및 포그라운드로 리턴 시 정기적으로 토큰을 업데이트하므로 게임은 시작시 onResume에거 자동 로그인을 호출하면 됩니다. 토큰 업데이트 프로세스[클릭](#MSDK2.7.0a 및 그 이후 버전 토큰 자동 업데이트 프로세스)

- **MSDK 위챗 토큰 기간 만료 자동 업데이트 매커니즘**：

   - MSDK2.7.0a 부터는 기존의 로그인 프로세스를 지원하는 동시에 프로세스를 최적화하여 토큰을 정기적으로 업데이트하므로 필히 assets/msdkconfig.ini내의 WXTOKEN_REFRESH를 WXTOKEN_REFRESH=true로 설정하거나 기입하지 않고 삭제（디폴트로 오픈）바랍니다.
   
  
- **로그인 데이터 전송 인터페이스 호출 요구**：
  - 자신의 launchActivity에 있는 onRestart에서 WGPlatform.onRestart를 호출하여야 합니다. 이와 같은 방식으로 차례로 onResume,onPause,onStop,onDestroy를 호출합니다.
  
- **WGGetLoginRecord호출 특별 설명**：
  - 2.7.0a 버전부터 'WGGetLoginRecord'이 eFlag_Checking_Token（5001）리턴할 경우 토큰 검증중이며 eFlag_WX_AccessTokenExpired（2007）로 리턴할 경우는 위챗 토큰 기간 만료입니다. 이때 WGLogin(EPlatform.ePlatform_None)를 호출하여 onLoginNotify에서 토큰 비동기 업데이트 결과를 처리합니다.

  
##용어 해석,인터페이스 설명
	
###로그인 관련 용어 해석:
| 명칭| 설명 |지원 플랫폼| 호출 인터페이스 |
|: ------------- :|
| 인증 로그인 | 플랫폼의 인증 UI를 호출하여 유저가 게임에 인증하고 로그인에 필요한 토큰을 받도록 안내| 모바일QQ/Wechat | WGLogin |
| 퀵로그인 | 유저의 조작은 플랫폼에서 게임 실행시 로그인 관련 토큰 정보를 투과 전송하여 게임에 로그인하게 함| 모바일QQ| 없음 |
| 자동 로그인 | 게임 시작시 유저의 최근 게임 로그인 토큰 정보를 직접 사용하여 게임에 로그인| MSDK에서 기능 제공| WGLoginWithLocalInfo |
| 자동 업데이트 | MSDK가 Wechat 토큰 자동 업데이트 인터페이스 제공| MSDK가 기능 제공 | 없음 |
| Diff계정 | 현재 게임에 로그인한 계정과 플랫폼에 로그인한 계정 불일치 | 플랫폼/MSDK 모두 지원| WGSwitchUser |

### 로그인 관련 인터페이스 개요

로그인과 관련된 호출 인터페이스 중 `WGGetLoginRecord`, `WGLogout`는 Synchronization 인터페이스이고 기타 인터페이스는 모두 Asynchronous로 구현되었으므로 callback 형식으로  OnLoginNotify(LoginRet)를 통하여 최종 결과를 게임에 콜백합니다. diff 계정과 관련된 인터페이스는 MSDK의 diff 계정 모듈에서 별도로 설명합니다. 상세한 내용은 아래와 같습니다.

| 명칭| 설명 |비고 |
|: ------------- :|
| WGGetLoginRecord | 로컬에 저장된 현재 유저의 로그인 토큰 획득 |  |
| WGSetPermission | 게임이 유저 인증을 통해 획득할 플랫폼 정보 설정 | |
| WGLogin | 플랫폼을 실행하여 인증 로그인 진행 |  |
| WGLogout | 현재 로그인한 계정의 로그인 정보 모두 삭제 |  |
| WGLoginWithLocalInfo | 로컬에 저장된 로그인 토큰으로 로그인 시도|  |
| handleCallback | 각 플랫폼 호출 처리 |  |
| WGRefreshWXToken | WechatrefreshToken으로 업데이트하여 accessToken 획득 |  MSDK 2.0부터 게임에서 스스로 Wechat 토큰을 업데이트하는 방식은 권장하지 않음 |

### 추천 로그인 프로세스

![login](./recommend_login.png) 

###로그인 관련 인터페이스 권장 사용법

1. 인증 로그인: 직접 `WGLogin`을 호출하여 해당한 플랫폼의 인증 획득
- 게임 부팅 혹은 게임이 백그라운드에서 포어그라운드로 전환시 토큰 유효성 검증: `WGLoginWithLocalInfo`를 호출하여 토큰 유효성 검증
- 토큰 획득: 직접 `WGGetLoginRecord`를 호출하여 로컬에서 읽기
- 로그아웃: 직접 `WGLogout`를 호출하여 현재 유저의 로그인 정보 삭제

**특별 설명：**
WGGetLoginRecord는 로컬 토큰을 획득하기 위하여 사용하는 인터페이스이며 전에 로그인한 적이 없으면 WGLogin 호출하여야 합니다. 성공 후 MSDK는 로컬에서 토큰을 저장합니다.그 후 게임 시작 시 자동 로그인 WGLoginWithLocalinfo를 사용할 수 있습니다. 자동 로그인 로직은 아래와 같이 구현할 것을 제안드립니다.

![login](./login_new1.png)

일부 게임은 게임 시작 시 WGGetLoginRecord 호출을 통하여 로컬 토큰의 유효성을 판단합니다. 로컬 토큰이 유효일 경우 토큰과 Game 서버 간 직접 통신을 하지만 `이와 같은 방식은 권장하지 않으며 자동 로그인 인터페이스 WGLoginWithLocalinfo를 사용하시기 바랍니다.`

##유저 인증이 필요한 WGSetPermission 권한 설정

####개요

게임은 MSDK 초기화 후 모바일QQ 권한 설정 인터페이스를 호출하여 유저 인증이 필요한 게임 플랫폼 권한을 설정하여야 합니다.

####인터페이스 설명

	/**
	 * @param permissions ePermission [Pascal fatal error] 혹은 연산 결과, 필요한 인증 항목 표시
	 * @return void
	 */
	void WGSetPermission(int permissions);

#### 인터페이스 호출:

	// QQ 실행시 필요한 유저 인증 항목 설정
	WGPlatform.WGSetPermission(WGQZonePermissions.eOPEN_ALL); 

#### 주의사항:

1. 게임은 MSDK 초기화 이후에 이 인터페이스를 호출하여야 하며 인터페이스 파라미터는 **`WGQZonePermissions.eOPEN_ALL`**을 입력할 것을 제안드립니다. 이 항목이 부족할 경우 게임이 일부 인터페이스 호출 시 권한이 없다는 안내가 출력합니다.


##인증 로그인 처리 WGLogin

#### 개요:

**모바일QQ/Wechat 클라이언트 혹은 web페이지(모바일QQ 미설치)를 실행하여 유저가 인증 후 onLoginNotify를 통하여 게임에 openID, accessToken, payToken, pf, pfkey 등 로그인 정보를 획득하였다는 내용을 통보합니다.**

#### 효과 전시:

![beacon_1](./loginpage.png)
![login](./lg1.png)
![login](./lg2.png)


####인터페이스 설명:

	/**
	 * @param platform 게임이 전송한 플랫폼 유형, 가능한 값: ePlatform_QQ, ePlatform_Weixin
	 * @return void
	 *   게임이 설정한 전역 콜백의 OnLoginNotify(LoginRet& loginRet) 메소드를 통하여 데이트를 게임에 리턴
	 */
	void WGLogin(ePlatform platform);

#### 인터페이스 호출:

	WGPlatform::GetInstance()->WGLogin(ePlatform_QQ); 

#### 주의사항:

- **통용**:
	- **Wechat와 모바일QQ 각각의 bug로 인하여 게임이 일부 상황에서 콜백을 받지 못하는 문제가 발생할 수 있습니다. 게임은 WGLogin을 호출한 후 카운트를 진행하며 카운트가 끝난 후에도 콜백을 받지 못하면 타임아웃으로 간주하여 로그인창으로 안내합니다. 카운트 시간은 30s로 권장하지만 게임에서 자체로 설정할 수 있습니다.**콜백을 받지 못하는 상황:
		- Wechat에 로그인하지 않은 상황에서 게임이 Wechat을 호출하여 유저명과 비밀번호를 입력하고 로그인하면 로그인 콜백이 없을 수 있습니다. 이는 Wechat 클라이언트에서 이미 알고 있는 BUG입니다.
		- Wechat 인증 과정에서 좌측 상단의  돌아가기 버튼을 클릭하면 인증 콜백이 없을 수 있습니다.
		- openSDK 2.7 (MSDK 2.5) 이하 버전은 web 인증을 통하여 인증 취소를 클릭한 후 콜백이 없습니다.
- **모바일QQ 관련**：
	1. 모바일QQ가 설치되지 않을 경우, 프리미엄 게임은 Web페이지를 호출하여 인증을 진행할 수 있습니다. AndroidMenifest.xml의 AuthActivity 설명에서  intent-filter 여기에  <data android:scheme="***" />를 설정하여야 합니다. 자세한 내용은 본 장의 모바일QQ 관련 AndeoidMainfest 설정을 참조 바랍니다. **응용보 게임은 페이지를 호출하여 인증하는 방식은 지원하지 않습니다.**. WGIsPlatformInstalled 인터페이스를 통하여 모바일QQ 설치 여부를 판단할 수 있으며 모바일QQ가 설치되지 않을 경우 유저한테 인증할 수 없음을 안내합니다.
	- **간혹 OnLoginNotify 콜백을 받지 못하는 경우: **`com.tencent.tauth.AuthActivity`와 `com.tencent.connect.common.AssistActivity`는 `AndroidManifest.xml`에서 모바일QQ 액세스 권한 선언([클릭하여 보기]())과 반드시 일치해야 한다.
	- 게임 Activity가 Launch Activity일 경우 게임 Activity 설명에 android:configChanges="orientation|screenSize|keyboardHidden"를 추가하여야 합니다. 아닐 경우 로그인창이 없고 콜백이 없는 문제가 발생할 수 있습니다.

- **Wechat 관련**:

	1. Wechat 인증은 Wechat 버전이 4.0이상이여야 합니다.
	- Wechat 실행시 Wechat은 앱 시그네쳐와 Wechat 백그라운드의 시그네쳐가 매칭되는지 검증합니다.(이 시그네쳐는 WechatappId를 신청 시 제출). 매칭되지 않을 경우 이미 인증된 Wechat 클라이언트를 실행할 수 없습니다.
	- `WXEntryActivity.java` 위치가 틀리면(폴드명 /wxapi 디렉토리 아래) 콜백을 받을 수 없습니다.


##자동 로그인 처리 WGLoginWithLocalInfo

#### 개요:

이 인터페이스는 기존에 로그인하였던 게임에 적용됩니다. 유저가 게임을 재접속할 경우 게임은 이 인터페이스를 우선 호출하여 백그라운드에서 토큰을 검증 후 OnLoginNotify를 통하여 결과를 게임에 콜백합니다. 

####인터페이스 설명:

	/**
	  *  @since 2.0.0
	  *  이 인터페이스는 기존에 로그인하였던 게임에 적용됩니다. 유저가 게임에 재접속할 경우 게임은 이 인터페이스를 우선 호출하여 백그라운드에서 토큰을 검증 시도합니다.
	   *  이 인터페이스는 OnLoginNotify를 통하여 결과를 게임에 콜백하고 eFlag_Local_Invalid와 eFlag_Succ 등 2개 flag만 리턴합니다.
	  *  로컬에 토큰이 없거나 로컬 토큰 검증 실패 시 리턴하는 flag는 eFlag_Local_Invalid입니다. 게임에서 이 flag를 받으면 유저들한테 인증 페이지로 안내하여 인증하도록 합니다.
	  *  로컬에 토큰이 있고 검증 성공 완료되면 flag는 eFlag_Succ입니다. 게임에서 이 flag를 받으면 재차 검증할 필요가 없이 sdk에서 제공한 토큰을 직접 사용할 수 있습니다.
	  *  @return void
	  *   Callback: 검증 결과는 OnLoginNotify를 통하여 리턴
	  */
 	void WGLoginWithLocalInfo();

####주의사항:

1. 게임에서 `WGLoginWithLocalInfo`를 사용하여 로그인 시 획득한 토큰은 게임 백그라운드에 전송하여 유효성 검증을 진행할 필요가 없으며 직접 MSDK에서 검증 후 게임에 콜백합니다.
2. **MSDK 2.7.0a버전 이후 WGLoginWithLocalInfo()호출을 WGLogin(EPlatform.ePlatform_None)로 수정하였지만 현재까지는 WGLoginWithLocalInfo 겸용 가능합니다. 추후 해당 인터페이스를 제거할 것입니다.**。

##유저 로그아웃 처리 WGLogout

#### 개요:

이 인터페이스를 호출하면 현재 로그인 계정의 로그인 정보를 삭제할 수 있습니다.

####인터페이스 설명:

	/**
	 * @return bool 리턴값을 이미 폐기, 모두 true 
	 */
	bool WGLogout();

####호출예시:

    WGPlatform.WGLogout();

####주의사항:

1. 게임이 **로그아웃 버튼을 클릭하거나 로그인창이 팝업되는 로직에서 WGLogout를 호출하여 로컬 로그인 정보를 모두 삭제하여야 합니다**. 아닐 경우 인증 실패 등 문제가 발생할 수 있습니다.

##MSDK의 로그인 콜백 처리

#### 개요:

MSDK의 로그인 콜백은 아래 상황에서 발생합니다.

- WGLogin 인증에서 돌아오기
- WGLoginWithLocalInfo 로그인에서 돌아오기
- 플랫폼 실행을 처리한 후(토큰을 갖고 실행)

#### 상세처리:

	OnLoginNotify(LoginRet ret) {
        Logger.d("called");
        switch (ret.flag) {
            case CallbackFlag.eFlag_Succ:
				 CallbackFlag.eFlag_WX_RefreshTokenSucc
            	//인증 성공후의 처리 로직
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
**여기에서 중요한 loginNotify 로직만 처리하였으며 완전한 콜백 flag 정보는 [콜백표시eFlag](const.md#콜백표시eFlag)에서 확인 바랍니다. 게임에서 자체 수요에 따라 처리할 수 있습니다**

##플랫폼 실행 처리handleCallback

#### 개요:

플랫폼 실행은 플랫폼 혹은 채널(모바일QQ/Wechat/게임로비/응용보 등)을 통하여 게임을 실행하는 방식을 의미합니다. 플랫폼은 일부 상황에서 토큰을 갖고 게임을 실행하여 게임에 직접 로그인할 수 있으므로 게임은 플랫폼의 호출을 처리하여야 합니다.

#### 상세 처리:
게임은 자체의 `launchActivity`의 `onCreat()`와 `onNewIntent()`에서 handleCallback를 호출하여야 하며 아닐 경우 로그인시 콜백이 없는 등 문제가 발생할 수 있습니다.

- **onCreate**:

        if (WGPlatform.wakeUpFromHall(this.getIntent())) {
        	// 실행 플랫폼은 게임 로비
        	Logger.d("LoginPlatform is Hall");
        } else {  
        	// 실행 플랫폼은 게임 로비가 아님
            Logger.d("LoginPlatform is not Hall");
            WGPlatform.handleCallback(this.getIntent());
        }

- **onNewIntent**

		if (WGPlatform.wakeUpFromHall(intent)) {
            Logger.d("LoginPlatform is Hall");
        } else {
            Logger.d("LoginPlatform is not Hall");
            WGPlatform.handleCallback(intent);
        }
#### 주의사항:

- 게임 로비를 호출하려면 게임 로비에 해당한 설정을 미리 등록하여야 지원할 수 있으므로 게임이 로비를 연동하지 않을 경우 `WGPlatform.wakeUpFromHall`을 호출하여 게임 로비를 호출하였는지 판단하여야 합니다. 게임 로비에서 등록하였으면 handleCallback를 호출하지 않습니다. 게임 로비에서 토큰을 갖고 호출하는 방법은 아래 내용을 참조 바랍니다.[클릭](qqgame.md)

##MSDK의 실행 콜백 처리

#### 개요

게임이 플랫폼 실행에 대한 처리는 주로 diff 계정과 관련된 로직을 처리하는 것입니다. 상세한 내용은 아래와 같습니다.

#### 상세처리：

        if (CallbackFlag.eFlag_Succ == ret.flag
                || CallbackFlag.eFlag_AccountRefresh == ret.flag) {
            //실행 후 로컬 계정을 통하여 게임에 로그인하는 것을 표시. 처리 로직은 onLoginNotify와 일치
            
        } else if (CallbackFlag.eFlag_UrlLogin == ret.flag) {
            // MSDK는 계정을 호출하여 토큰을 갖고 인증 로그인 시도. 결과는 OnLoginNotify에서 콜백. 이럴 경우 게임은 onLoginNotify 콜백 대기합니다.

        } else if (ret.flag == CallbackFlag.eFlag_NeedSelectAccount) {
            // 현재 게임에 Diff계정이 존재하므로 게임은 대화창을 팝업하여 유저에게 로그인할 계정을 선택하도록 합니다.

        } else if (ret.flag == CallbackFlag.eFlag_NeedLogin) {
            // 유효 토큰이 없어 게임 로그인 실패. 이럴 경우 게임은 WGLogout를 호출하여 로그아웃 후 다시 로그인하도록 안내합니다.

        } else {
            //기본 처리 로직은 게임이 WGLogout를 호출하여 게임에서 로그아웃한 후 유저가 다시 로그인하도록 안내합니다.
        }

##Diff계정 처리 로직

Diff계정 관련 모듈은 [MSDK Diff계정 연동](diff-account.md#Diff계정 처리 로직(개발자 주목）)을 참조 바랍니다.

##기타 특수 로직 처리

### 모바일QQ를 저메모리 설비에서 인증 시 게임을 강제 종료된 후의 로그인 방안

대부분 게임은 메모리를 많이 차지하므로 인증 과정에서 모바일QQ를 호출하여 인증 진행 시 android 메모리 복구 메커니즘으로 인하여 백그라운드의 게임 프로세스를 강제로 종료하므로 모바일QQ 인증이 없어 게임을 접속할 수 없습니다. 하여 게임에서 메인 Activity에 아래 코드를 추가하여 프로세스가 종료되에도 토큰을 갖고 게임에 접속할 수 있도록 하여야 합니다.


	// TODO GAME 은 onActivityResult에서 WGPlatform.onActivityResult를 호출 필요
    @Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
		WGPlatform.onActivityResult(requestCode, resultCode, data);
		Logger.d("onActivityResult");
	}

**주의사항：해당 인터페이스는 2.5.0a 및 이상 버전에서 추가, 전 버전에는 호출하지 않습니다.**

### 로그인 데이터 업로드

로그인 데이터를 정확히 업로드하기 위하여 게임 연동시 자체 `launchActivity`의 `onResume`에서 `WGPlatform.onResume`를 호출하고 `onPause`에서 `WGPlatform.onPause`를 호출하며 `onRestart`에서 `WGPlatform.onRestart`호출하고 `onStop`에서 `WGPlatform.onStop`를 호출하고 `onDestroy`에서 `WGPlatform.onDestroy`호출하여야 합니다.

### Wechat 토큰 자동 업데이트

1. MSDK2.0.0부터 게임 운행 기간에 정기적으로 Wechat 토큰을 검증하고 업데이트합니다. 업데이트가 필요할 경우 MSDK에서 자동으로 업데이트을 진행하고 OnLoginNotify를 통하여 게임에 통보합니다. flag는 eFlag_WX_RefreshTokenSucc와 eFlag_WX_RefreshTokenFail입니다.(이미 onLoginNotify의 콜백에 포함됨).
- **게임이 새로운 토큰을 받으면 게임 클라이언트에 저장된 토큰과 서버의 토큰을 동기적으로 업데이트하여 새로운 토큰으로 향후 절차를 진행할 수 있어야 합니다.**
- **MSDK2.7.0a이전 버전에서 게임이 위챗 토큰 자동 업데이트 기능이 필요 없을 경우 `assets\msdkconfig.ini`에서 `WXTOKEN_REFRESH`를 `false`로 설정하면 됩니다.이럴 경우 게임은 스스로 위챗 토큰 기간만료 로직을 처리합니다. 상세한 내용은 **[위챗 토큰 업데이트 인터페이스](login.md#위챗 토큰 업데이트：WGRefreshWXToken)를 참조 바랍니다.
- **MSDK2.7.0a 이후 버전에는 기존 버전의 로그인 프로세스를 지원하는 동시에 프로세스를 최적화하여 토큰에 대해 정기적으로 업데이트합니다. assets/msdkconfig.ini에 있는 WXTOKEN_REFRESH를 아래와 같이 WXTOKEN_REFRESH=true 설정하거나  기입하지 않고 삭제（즉 디폴트로 기능 오픈）바랍니다.**

##위챗 토큰 업데이트：WGRefreshWXToken

#### 개요：

- 위챗accessToken는 유효기간은 2시간, refreshToken의 유효기간은 30일입니다. refreshToken만 유효 기간 지나지 않으면 refreshToken를 통하여 accessToken를 업데이트할 수 있습니다. 업데이트 후 새로운 accessToken및 refreshToken를 획득.게임이 MSDK제공한 토큰 자동 업데이트 인터페이스 사용하지 않았을 경우 WGRefreshWXToken()인터페이스를 사용하여 accessToken기간 연장을 진행하여야 합니다.
- 게임이 `WGGetLoginRecord`를 호출하여 받은 flag가 `eFlag_WX_AccessTokenExpired`일 경우 해당 인터페이스를 호출하여 위챗 토큰을 업데이트합니다. 업데이트 결과는 `OnLoginNotify`를 통하여 게임에 리턴하며`eFlag_WX_RefreshTokenSucc` 일 경우 token 업데이트 성공.`eFlag_WX_RefreshTokenFail` 일 경우 token 업데이트 실패.

####인터페이스 설명：

	/**
	 * 해당 인터페이스는 위챗 accessToken 업데이트용.
	 * refreshToken는 accessToken 업데이트용이며 refreshToken기간 만료되지 않으면 refreshToken를 통하여 accessToken 업데이트 가능.
	 * 두 가지 상황에서 accessToken업데이트 필요.
	 * @return void
	 *   게임에서 설정하는 전반 콜백의 OnLoginNotify(LoginRet& loginRet)방법으로 데이터를 게임에 리턴.
	 *     위챗 플랫폼에만 refreshToken가 있으므로 loginRet.platform의 값은 ePlatform_Weixin.
	 *     loginRet.flag 값은 리턴 상태를 표시하며 가능한 값(eFlag열거)은 아래와 같습니다.
	 *       eFlag_WX_RefreshTokenSucc: 토큰 업데이트 성공.게임에서 이 flag를 받은 후 직접 LoginRet구조체에 있는 토큰을 호출하여 게임 인증 프로세스를 진행.
	 *       eFlag_WX_RefreshTokenFail: WGRefreshWXToken호출 도중에 네트워크 에러로 인해 업데이트 실패. 게임에서 WGRefreshWXToken 재 시도 여부를 결정.
	 */
	void WGRefreshWXToken();

####호출예시：

	WGPlatform::GetInstance()->WGRefreshWXToken()

#### 주의사항：

1. 각 refreshToken는 한번만 사용 가능하며 업데이트 시 새로운 refresh 획득.

##로컬 토큰 호출：WGGetLoginRecord

#### 개요:

이 인터페이스를 호출하면 현재 계정의 로그인 정보를 획득할 수 있습니다.

####인터페이스 설명:

	/**
	 * @param loginRet 리턴한 기록
	 * @return 리턴 값은 플랫폼 id,유형은 ePlatform.ePlatform_None을 리턴하면 로그인 기록이 없음을 표시.
	 *   loginRet.platform(유형 ePlatform) 플랫폼id 표시, 가능한 값은 ePlatform_QQ, ePlatform_Weixin, ePlatform_None.
	 *   loginRet.flag(유형 eFlag) 현재 로컬 토큰의 상태 표시, 가능한 값과 설명은 아래와 같습니다.
	 *     eFlag_Succ: 인증 토큰 유효
	 *     eFlag_QQ_AccessTokenExpired: 모바일QQ accessToken 기한 만료, 인증 인터페이스 표시, 유저가 다시 인증하도록 안내
	 *     eFlag_WX_AccessTokenExpired: WechataccessToken 토큰 기간 만료, WGRefreshWXToken을 호출하여 업데이트
	 *     eFlag_WX_RefreshTokenExpired: WechatrefreshToken, 인증 인터페이스 표시, 유저가 다시 인증하도록 안내
	 *   ret.token은 하나의 Vector<TokenRet>이고 그중에 저장된 TokenRet에 type와 value가 있습니다. Vector 순회를 통하여 type를 판단하여 필요한 토큰을 호출합니다.
     *
	 */
	int WGGetLoginRecord(LoginRet& loginRet);

####호출 예시:

	    LoginRet lr = new LoginRet();
        WGPlatform.WGGetLoginRecord(lr);
	    if(lr.flag == CallbackFlag.eFlag_Succ) {
	        // 토큰 획득
	    } 
	    // TODO Game MSDK2.7.0a버전부터 eFlag_Checking_Token 및 eFlag_WX_AccessTokenExpired에 대해 아래와 같은 방식으로 호출하여야 합니다.
	    else if(lr.flag == CallbackFlag.eFlag_Checking_Token || lr.flag == CallbackFlag.eFlag_WX_AccessTokenExpired) {
	        // eFlag_Checking_Token（5001）토큰 검증 중，eFlag_WX_AccessTokenExpired（2007）위챗 토큰 기간 만료, 다시 한번 체크 및 새로고침
            WGPlatform.WGLogin(EPlatform.ePlatform_None);
        } else {
            // 로그인 상태 실효, 유저 재로그인하여 인증하게 안내
        }

획득한 LoginRet에 있는 flag가 eFlag_Succ일 경우 로그인 유효라고 볼 수 있고 유효 토큰 정보를 호출할 수 있습니다.**MSDK2.7.0a이후 버전  만약 'WGGetLoginRecord' 5001 및 2007로 리턴할 경우 WGLogin(EPlatform.ePlatform_None)를 호출하여 onLoginNotify에서 토큰 Asynchronous 업데이트 결과를 처리합니다.** 그 중에 token는 아래 방식으로 획득할 수 있습니다.

Wechat 플랫폼:

    std::string accessToken = "";
    std::string refreshToken = "";
    for (int i = 0; i < loginRet.token.size(); i++) {
             if (loginRet.token.at(i).type == eToken_WX_Access) {
                 accessToken.assign(loginRet.token.at(i).value);
             } else if (loginRet.token.at(i).type == eToken_WX_Refresh) {
                 refreshToken.assign(loginRet.token.at(i).value);
             }
    }

QQ 플랫폼:

    std::string accessToken = "";
    std::string payToken = "";
    for (int i = 0; i < loginRet.token.size(); i++) {
        if (loginRet.token.at(i).type == eToken_QQ_Access) {
            accessToken.assign(loginRet.token.at(i).value);
        } else if (loginRet.token.at(i).type == eToken_QQ_Pay) {
            payToken.assign(loginRet.token.at(i).value);
        }
    }

#### 주의사항:

없음

##자주 발생하는 문제

1. 결제시 paytoken 기간 만료일 경우 로그인 인터페이스를 호출하여 다시 인증 후 결제할 수 있습니다.  paytoken 기간이 만료되면 다시 인증하여야 합니다.

1. `QQ 설치하지 않았을 경우 webQQ로 로그인할 때 계속 "네트워크 비정상"알림을 노출`
게임이 Unity를 사용하여 직접 Apk패키지를 패킹 시 해당 문제 발생할 경우 MSDK jar패키지에 있는 assets를 압축 풀어서 Android/assets에 넣어야 합니다. 만약 다른 패킹 방식을 사용할 경우 MSDK jar패키지에 있는 so파일 및 리소스 파일을 Apk패키지내에 넣어야 합니다.


##MSDK2.7.0a 및 그 이후 버전에서 토큰 자동 업데이트 프로세스
**개요**

MSDK2.7.0a 이상 버전에서 기존 버전의 로그인 프로세스를 지원하는 동시에 프로세스를 최적화하여 토큰에 대해 정기적으로 업데이트합니다. msdkconfig.ini에 있는 WXTOKEN_REFRESH를 아래와 같이 `WXTOKEN_REFRESH=true`설정하거나 혹은 설정하지 않습니다(즉 디폴트로 오픈). 상세한 내용은 운영팀에 문의 바라며 게임에서  'WGLogin' 'WGGetLoginRecord'만 주목하면 로그인 및 토큰 처리를 완료할 수 있습니다.

* 게임 로그인 토큰이 필요할 시의 호출 로직：
  
![login_new](./new_login_1.jpg)
  `만약 'WGGetLoginRecord'5001 및 2007로 리턴할 경우 WGLogin(EPlatform.ePlatform_None)를 호출하여 onLoginNotify에서 토큰 Asynchronous 업데이트 결과를 처리합니다.`

* 로컬 토큰을 사용하여 로그인할 때 'WGLoginWithLocalInfo'호출할 필요가 없으며 WGLogin(EPlatform.ePlatform_None) 를 호출하여 onLoginNotify의 결과를 대기하면 됩니다.
* MSDK 내부의 토큰 정기적 업데이트 로직：
  
  ![login_new](./new_login_2.jpg)

