저번주에 Android 10 버전의 소스를 가지고 빌드를 하는데에 성공하였다. 이번엔 저번의 빌드 성공 경험을 되살려서 Android 6 빌드를 다시 한 번 도전해볼 것이다.

---

# 1. Repo init 준비과정

repo init 준비과정은 [이전 포스트](https://donohhhh.github.io/%EC%8A%A4%ED%84%B0%EB%94%94-1%EC%A3%BC%EC%B0%A8-Android-Platform-Source-%EB%B9%8C%EB%93%9C%ED%95%B4%EB%B3%B4%EA%B8%B02/)와 동일하니 참고하면 된다.

---

# 2. Android source 다운

## 1) repo init

```
repo init -u https://android.googlesource.com/platform/manifest -b android-6.0.1_r79 -c --depth=1 --no-tags
```

이번엔 저번 포스트와는 다르게 android-6.0.1_r79 버전을 선택하여 소스를 다운로드하였다.

![1.PNG](/assets/images/2/1.PNG)

## 2) repo sync

```
repo sync -q -c --no-tags
```

위 명령어로 소스를 다운받아준다.

![2.PNG](/assets/images/2/2.PNG)

다 끝이 나면 

```
repo start android-6.0.1_r79 --all
```

위 명령어로 모든 git의 브랜치를 android-6.0.1_r79로 변경해준다. 이 작업은 소스 용량을 줄이기 위하여 불필요한 부분을 덜어내는 작업이다.

## 3) Mac OS 빌드용 tool chain 삭제

```
sudo find ./prebuilts/ -name "*darwin-x86*" -type d
sudo find ./prebuilts/ -name "*darwin-x86*" -type d -exec rm -rf {} \;

sudo find ./.repo -name "*darwin-x86*" -type d 
sudo find ./.repo -name "*darwin-x86*" -type d -exec rm -rf {} \;
```

그리고 위 명령어들로 Mac OS 빌드용 tool chain들을 찾거나 삭제해주면 된다.

![3.PNG](/assets/images/2/3.PNG)

---

# 3. Android source 빌드

## 0) 권한 부여

```
cd ~/project
sudo chmod -R 777 .
```

저번과 마찬가지로 permission denied라는 에러를 잡기 위해 폴더내 모든 파일의 권한을 777로 변경한다.

## 1) envsetup.sh 스크립트로 환경을 초기화

```
. build/envsetup.sh
```

## 2) 기기 빌드 선택

빌드하고자 하는 target device 와 release 옵션으로 lunch 옵션을 설정해야 한다. AOSP Tag 정보는 [여기](https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds) 에서 확인할 수 있고, Target Device Name 은 [여기](https://source.android.google.cn/setup/build/running?hl=ko) 에서 선택할 수 있다.

![4.PNG](/assets/images/2/4.PNG)

![5.PNG](/assets/images/2/5.PNG)

우리는 이번에 android-6.0.1_r79로 했으므로 aosp_shamu-userdebug를 선택하면 된다.

```
lunch aosp_shamu-userdebug
```

위 명령어를 이용하여 빌드하고자 하는 타겟을 설정한다.

## 3) 소스 빌드

```
m -j4
```

위 명령어를 이용하여 소스를 빌드해준다. 쓰레드를 4개만 쓰는 이유 [여기]([android - AOSP build error ninja failed with: exit status 1 - Stack Overflow](https://stackoverflow.com/questions/52617313/aosp-build-error-ninja-failed-with-exit-status-1) 에서 언급된것처럼 outofmemoryerror가 너무 많은 쓰레드가 동시에 실행되다보니 발생하는 거라고하여 일부러 줄여주었다.

---

# 4. 결과

![6.PNG](/assets/images/2/6.PNG)

먼저 크게 에러가 2가지 발생하였는데 첫 번째는 python 문법이 맞지 않다는 것이고 두 번째는 깔려있는 java가 없어서 발생하는 것이다.



```
sudo update-alternatives --config python
```

![7.PNG](/assets/images/2/7.PNG)

첫 번재 에러는 위 그림처럼 해당 명령어를 이용하여 python3 -> python2로 변경해주면 해결된다.



두 번째 에러는 [building android error java version - Stack Overflow](https://stackoverflow.com/questions/36885029/building-android-error-java-version) 를 참고하여 openjdk 1.7.x버전 설치를 진행하면 된다.



혹시 몰라서 다운받는 명령어를 적어놓겠다.

```
# /usr/lib/jvm이라는 폴더를 만듦.
sudo mkdir /usr/lib/jvm

# 해당 폴더로 이동
cd /usr/lib/jvm

# openjdk 1.7.x 버전을 다운받음.
sudo wget https://download.java.net/openjdk/jdk7u75/ri/jdk_ri-7u75-b13-linux-x64-18_dec_2014.tar.gz

# 다운받은 파일을 압축해제함.
sudo tar zxvf jdk_ri-7u75-b13-linux-x64-18_dec_2014.tar.gz

# java 환경변수를 설정함.
export PATH="/usr/lib/jvm/java-se-7u75-ri/bin:$PATH"

# repo init한 폴더로 이동
cd ~/project

# 만약 vi가 깔려있지 않다면 설치
sudo apt-get install vim -y

# 수정할 파일을 엶.
sudo vi build/core/main.mk
```

위 명령어를 수행하면 어떤 파일을 열게 된다. vi editor가 있으면 그냥 "153G"를 타이핑하거나 "/required_version"을 타이핑하면 아래와 같은 화면이 나온다.

![8.PNG](/assets/images/2/8.PNG)

위 캡쳐에서 java_version에 java라고 써져있는 것을 아래 캡쳐처럼 openjdk로 변경해준다.

![9.PNG](/assets/images/2/9.PNG)

그리고 :wq!로 저장하고 빠져나온다.

![10.PNG](/assets/images/2/10.PNG)

그리고 위 명령어로 java와 javac의 버전이 1.7.x로 변경된 것이 확인되면 빌드 준비가 끝난 것이다.



이제 다시 "m -j4" 명령어로 빌드를 시도해본다.



### 1) 첫 번째 에러

![11.PNG](/assets/images/2/11.PNG)

그랬더니 위와 같은 에러가 발생했다. 차마 캡쳐할 생각을 못해서 해당 에러가 발생한 블로그에서 캡쳐를 하였다. 그래서 해당 블로그에서는 

```
export LC_ALL=C
```

명령어를 사용하여 오류를 해결하였고 나 또한 해결할 수 있었다.



### 2) 두 번째 에러

![12.PNG](/assets/images/2/12.PNG)

위와 같은 에러는 [여기](https://blog.krybot.com/a?ID=01200-2e7eb1e7-ebc5-44ac-b621-691f6d33fed6) 에서 확인하여 고칠 수 있었다. 영어로 되어있는데 대충

```
vi ./art/build/Android.common_build.mk
```

위 명령어를 이용하여 파일을 열고 "/WITHOUT_HOST_CLANG"를 입력하여 해당 라인을 찾은 다음

```
# Host.ART_HOST_CLANG := false
ifneq ($(WITHOUT_HOST_CLANG),true)
 # By default, host builds use clang for better warnings.

  ART_HOST_CLANG := true
endif
```

위와 같은 내용에서 ART_HOST_CLANG := true를 

```
# Host.ART_HOST_CLANG := false
ifneq ($(WITHOUT_HOST_CLANG),true)
 # By default, host builds use clang for better warnings.

  ART_HOST_CLANG := false
endif
```

ART_HOST_CLANG := false로 바꾸면 된다는 내용이었다.



이제 다시 "m -j4" 명령어로 빌드를 시도해본다. 그랬더니...

![13.PNG](/assets/images/2/13.PNG)

# 뙇!!!

성공해버렸다.

위와 같은 방법을 사용하면 다른 android 버전들도 빌드가 가능할 듯하다.

## **참고 사이트**

[Android 源码编译aidl_language_l 相关错误解决](https://blog.csdn.net/xljxiang/article/details/83044067)

[https://blog.krybot.com/a?ID=01200-2e7eb1e7-ebc5-44ac-b621-691f6d33fed6](https://blog.krybot.com/a?ID=01200-2e7eb1e7-ebc5-44ac-b621-691f6d33fed6)
