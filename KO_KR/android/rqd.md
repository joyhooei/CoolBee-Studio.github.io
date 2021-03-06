MSDK Crash데이터 전송 모듈
===
개요
---
Crash데이터 전송은 MSDK2.5a이전 버전（MSDK2.5a 불포함)에서는 RQD전송을 사용하며 전송 성공 완료 후  crash 상세한 스택은  http://rdm.wsd.com/ 에서만 확인할 수 있습니다. 또한 텐센트 직원이 RTX로 로그인하여야만 확인할 수 있어 퍼블리싱 게임은 확인하는데 많은 불편함이 있었습니다. MSDK2.5 및 이후 버전에서는 bugly전송을 사용하며  http://bugly.qq.com/ 에서 해당 정보를 확인할 수 있습니다. 또한 QQ계정으로 해당 앱을 바인딩할 수 있어 퍼블리싱 게임도 쉽게 확인할 수 있게 되었습니다. 물론 http://rdm.wsd.com/ 에서도 여전히 확인할 수 있니다다. 게임에서 별도의 조작이 필요 없으며 Crash데이터 전송 스위치를 닫을 뿐입니다. 상세 내용은 **RQD전송 스위치 설정**과**Bugly전송 스위치 설정**을 참조 바랍니다.

MSDK2.5a이전 버전이 2.5a 및 그 이후 버전으로 업데이트 한 뒤의 주의 사항
---
Android Library Project를 사용할 수 없는 게임일 경우,MSDKLibrary디렉터리에 있는 libs를 게임 공정 대응 디렉터리로 카피하여야 합니다.2.5a이전 버전에는 armeabi,armeabi-v7a,mips 및 x86디렉터리에 있는 libNativeRQD.so를 카피하여 게임 공정의 대응 디렉터리로 넣는 것이지만 2.5a및 이후 버전에는 libBugly.so를 카피하여 게임 공정의 대응 디렉터리에 넣습니다.만약 libNativeRQD.so이미 존재할 경우 삭제하여도 됩니다.

RQD전송 스위치 설정
---
rdm 데이터 전송 on/off 설정 함수:

     public static void WGEnableCrashReport(boolean bRdmEnable, boolean bMtaEnable)

WGPlatform에 이 함수가 있으며 bRdmEnable을 false(bMtaEnable은 false로 설정 가능）로 설정하면 rdm crash 전송를 off합니다. crash 전송은 디폴트로 오픈되어 있으므로 이 함수를 호출할 필요가 없습니다.

RDM플랫폼에서 Crash데이터 확인
---
####1.등록 바인딩

DEV 등록 게임은 자동으로 RDM을 등록하므로 수동으로 등록할 필요가 없습니다. 수동 등록은 직접 RDM에 로그인하여 에러 전송 모듈을 클릭하고 게임의 BoundID를 설정하면 됩니다.

스텝: [http://rdm.wsd.com/](http://rdm.wsd.com/) 로그인 후 게임 선택 -> 에러 전송 선택. 등록되지 않았으면 아래와 같은 안내 메세지가 출력됩니다.

![rdmregister](./rmdregister.png)

그중 boundID는 게임 자체의 AndroidManifest중 packageName 입니다. 등록하지 않은 게임일 경우 데이터 전송 시 데이터가 분실됩니다.

자세한 내용은 운영팀을 통하여 rdm 도우미한테 문의 바랍니다.

####2.전송 데이터 확인 방법
- 사이트:[http://rdm.wsd.com/](http://rdm.wsd.com/)->에러 전송->문제 리스트

![rdmwsd](./rdmwsd.png)
![rdmdetail](./rdmdetail.png)


Bugly전송 스위치 설정
---
bugly전송 스위치 on/off는 /assets/msdkconfig.ini에서 설정 필요

      ;bugly전송 스위치 off하면 디폴트로 false로 설정되고，true로 설정하면 Crash데이터 전송 기능 off합니다.
      CLOSE_BUGLY_REPORT=false

Bugly플랫폼에서 Crash데이터 확인
---
- 사이트:[http://bugly.qq.com/](http://bugly.qq.com/)->QQ계정으로 로그인->해당 App를 선택합니다.

![bugly](./bugly1.png)

App Proguard 난독화에 관련 처리
---
코드 안전을 감안하여 App발포하기 전에 Java코드에 대해 난독화 처리를 진행할 것입니다.난독화 처리할 때 Bugly에 난독화하면 안되는 클래스 및 방법을 난독화를 피하여야 하는 점은 특별히 주의하여야 합니다.

**방안 1：msdk*.jar 난독화 처리 안 한다（권장）**

bugly.jar이미 MSDK*.jar에서 탑재하기에 난독화 처리할 때 MSDK*.jar（如MSDK_Android_2.7.0a_svn53805.jar）에 대해 난독화 진행하지 않은 것을 권장 드립니다.
유저는 직접-libraryjars를 통해 MSDK*.jar인용할 수 있습니다.

![bugly](./bugly_proguard1.png)

**방안 2：MSDK*.jar 난독화 처리 진행할 때 SDK의 keep정보를 추가**

만약 유저가 MSDK*.jar를 난독화 처리 필요할 경우 아래의 keep정보를 App 난독화 설정에서 추가 바랍니다：

    -keep class * extends android.app.Activity{*;}
    -keep class * extends android.app.Service{*;}

    #Bugly인터페이스
    -keep public class com.tencent.bugly.crashreport.crash.jni.NativeCrashHandler{public *; native <methods>;}
    -keep public interface com.tencent.bugly.crashreport.crash.jni.NativeExceptionHandler{*;}


App 코드 복원 테이블 설정
---
**코드 복원 테이블 설정 관련은 우선 http://www.jikexueyuan.com/course/406.html?hmsr=bugly_androidcrash 에 있는 8장의 비디오 가이드를 보기 권장**, 해당 비디오 가이드를 통해 코드 복원 테이블을 파악할 수 있다.

**1.Java Progurad코드 복원 테이블 생성：**

progurad 툴로 난독화 작업을 진행할 때, 코드 복원 테이블 파일 mapping.txt를 출력할 수 있어 Bugly는 mapping.txt파일에 의하여 Java스택에 대해 복원을 진행합니다.

![bugly](./bugly_proguard2.png)

eclipse에서 progurad실행 후，release컴파일한 다음에 progurad디렉터리에서 코드 테이블 파일 mapping.txt를 생성할 것입니다.

![bugly](./bugly_proguard3.png)


**2.Native symbol파일 생성：**

SymbolTool.jar코드 테이블 툴을 통해 SO파일에 있는 debug정보를 추출하여 symbol생성합니다.Bugly는 symbol파일에 의하여 Native스택에 대해 복원 작업 진행합니다.

**3.NDK build결과에：**

libs디렉터리->cpu구조 디렉터리->XX.so 는 debug정보를 포함하지 않고 발포할 데 쓰는SO이며 크기가 작습니다.
obj디렉터리->cpu구조 디렉터리->XX.so 는 debug정보(debug_info , debug_line)를 포함한 debug할 때 쓰는 SO이며 크기가 큽니다.

![bugly](./bugly_proguard4.png)

Bugly코드 테이블 툴은 debug의 SO파일에서 함수의 코드정보를 추출하여 스택 복원 진행하여야 합니다.

    <cmd>
    java -jar AndroidSymbolTools.jar -i debugSoPath -a armeabi
    </cmd>

debugSo의 같은 디렉터리에서 buglySymbol&cpu구조&SO이름.zip 코드테이블 파일 생성할 것입니다.

**4.코드 테이블 성정：**

bugly플랫폼에서 게임->설정->버전 관리 페이지에 들어가서 버전에 때라 코드테이블을 직접 올리면 됩니다！

![bugly](./bugly_proguard5.png)

코드테이블 설정은 버전에 따라 설정하는 것이다：

한 버전에는 단 하나의 Java코드테이블（mapping.txt）밖에 없습니다.

한 버전의 +Cup구조에는 단 하나의 Native코드테이블（bulgySymbol&cpu구조&SO이름.zip）밖에 없습니다.

중복적으로 설정할 경우는 조작을 덮어쓰는 것입니다.



Crash 전송에 별도 업무 로그 추가
---

프로그램이 Crash발생 시 별도의 업무 로그를 추가하여 crash로그와 함께 http://rdm.wsd.com/ 플랫폼으로 전송하여야 합니다.이럴 경우 보다 쉽게 crash원인을 파악할 수 있고 rdm플랫폼에서 에러 상세 내역을 확인할 수 있습니다. 그 중 별도의 업무 로그는 extraMessage.txt파일에 저장됩니다. 현재 bugly는 extraMessage.txt 확인 기능을 오픈하지 않았으며 현재 기능 개발중입니다.

![rqd](./rqd_extramsg.png)

해당 기능을 완료하려면 전반observer(즉WGPlatformObserver)에서 콜백 함수OnCrashExtMessageNotify를 추가하여야 합니다. java는 아래 방식대로 호출합니다.

    @Override
    public String OnCrashExtMessageNotify() {
      // 여기에 crash 시 별도 정보를 추가하여 crash발생 원인을 분석하는 데 사용됩니다.
      // 예를 들어，String str = "test extra crash upload!";
      // 필요 없을 경우 String str = ""를 기입
      String str = "test extra crash upload!";
      return str;
    }

cpp는 아래 방식으로 호출합니다.

    virtual std::string OnCrashExtMessageNotify() {
    	// 여기에 crash 시 별도 정보를 추가하여 crash발생 원인을 분석하는 데 사용됩니다.
    	// 예를 들어，std::string str = "test extra crash upload!";
    	// 필요 없을 경우std::string str = ""를 기입
    	std::string str = "test extra crash upload!";
    	LOGD("OnCrashExtMessageNotify test %s", str.c_str());
    	return str;
    }

