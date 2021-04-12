# SonarLint 설치 및 구성
---

1. Eclipse Marketplace 에서 sonarlint 검색후 아래와 같이 설치를 진행하시면 됩니다.

![sonarlint marketplace](images/Markerplace_sonarlint.png)

> 사내먕에서 설치가 안 될 때 다운로드 후 압축해제하여 plugins 디렉토리 파일을 sts plugins 디렉토리로 복사하고 sts restart함
  [sonarlint 다운로드](http://binaries.sonarsource.com/SonarLint-for-Eclipse/releases/

#
* 사내망에서 차단되지 않고 정상적인 설치 화면 출력됨.

![](images/Eclipse_sonarlint.png)

![](images/Eclipse_sonarlint2.png)

#
* 설치 오류 화면(사내망에서 다운로드가 막힐 때)

![](images/sonarlint_install_error.png)

다음과 같이 Window > Show View > Other 에서 보면 SonarLint가 정상적으로 설치된걸 볼 수 있습니다.

<img src="images/sonarlint_showview.png" alt="sonarlint_showview" width="300"/>

#
* on-the-fly 적용

![](images/sonarlint_onthefly_setup.png)

![](images/sonarlint_onthefly.png)

#
* Rule Description

![](images/Rule_Description.png)

#
* SonarQube서버와 Binding 하기

![](images/SonarLint_Bindings.png)
![](images/Connect_SonarQube.png)
![](images/SonarQube_Server_URL.png)
![](images/Choose_Auth_Method.png)
![](images/SonarQube_Auth_Token.png)
![](images/SonarQube_Connetin_Identifier.png)
![](images/Configuration_Completed.png)

* 프로젝트 바인딩

![](images/Bind_SonarQube_project.png)
![](images/Bind_SonarQube_project2.png)
![](images/Bind_SonarQube_project3.png)
![](images/Bind_SonarQube_project4.png)



# 
> 참고 사이트

[Eclipse SonarLint 코드 분석 플러그인 사용방법](https://www.mynotes.kr/eclipse-sonarlint-%EC%BD%94%EB%93%9C-%EB%B6%84%EC%84%9D-%ED%94%8C%EB%9F%AC%EA%B7%B8%EC%9D%B8-%EC%82%AC%EC%9A%A9%EB%B0%A9%EB%B2%95/)
[SonarLint(소나린트) 코드 분석 플러그인(코드리뷰)](https://blog.naver.com/zzang9ha/222048392057)
