# 1. repo init 준비과정

## 1) Vmware 설정

![1](/assets/images/1/1.PNG)

먼저 저번 주차에서 vmware에 ubuntu를 설치를 했었는데 조금 수정할 부분이 있다.

vmware 설정을 진행할 때 memory는 최소 8GB 이상 Processors는 processor 4개에 processor당 core수를 2개로 하여 총 8개 그리고 하드디스크 크기는 200GB이상으로 설정을 해준뒤 진행하는 것이 좋다. 그래야 빌드 과정에서 오류가 발생하지 않는다.

____

## 2) AOSP 빌드 필수 패키지 다운로드

```
sudo apt-get update
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 libncurses5 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig vim python2.7 make -y
```

먼저 위 명령어를 이용하여 AOSP 빌드에 필요한 package들을 다운받아 준다.

이때 만약 

![2](/assets/images/1/2.PNG)

이런 오류가 뜬다면 아래의 명령어들을 차례로 입력하여 오류를 잡아준다.

```
sudo killall apt apt-get
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock*
sudo dpkg --configure -a
```

---

## 3) repo 최신 버전 다운받기

```
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/repo
chmod a+x ~/repo
sudo mv ~/repo /usr/local/bin
```

기존 apt-get을 이용하여 repo 패키지를 다운받으면 나중에 쓰이게 될 몇몇 옵션들을 쓸 수 가 없어서 위 명령어들을 이용하여 repo 최신 버전을 직접 다운받고 /usr/local/bin에 넣어서 명령어로 쓸 수 있도록 해준다.

___

## 4) git 사용자 설정

```
git config --global user.name "[user name]"
git config --global user.email "[user Email]"
```

git global 설정에 user이름과 이메일을 넣어준다.

---

## 5) project 폴더 생성

```
sudo mkdir ~/project
cd ~/project
```

작업을 진행할 폴더를 생성해준다.

---

## 6) python 버전 설정

```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2
```

위 명령어를 이용하여 python2.7과 python3.6버전을 번갈아가며 이용할 수 있도록 설정해준다.

```
sudo update-alternatives --config python
```

그리고 해당 명령어를 쳐보면

![3](/assets/images/1/3.PNG)

위 그림과 같이 python 버전을 선택할 수 있게 나오는데 일단 지금은 python3으로 선택해준다.

---

# 2. Android 소스 다운로드

## 1) repo init

```
repo init -u https://android.googlesource.com/platform/manifest -b [빌드 태] -c --depth=1 --no-tags
```

위 명령어를 이용하여 Android 소스를 다운받아준다. 하지만 그냥 repo init으로 초기화를 시켜주면 소스의 용량이 너무 커지게 된다. 그래서 소스의 필요한 부분만 가져올 수 있도록 옵션을 줄 것이다.

repo init 옵션 중에 코드 사이즈를 줄일 수 있는 옵션은 -c , --depth, --no-tags 옵션이다. 

> 1) -c  옵션:  working branch 만 다운로드 하는 것으로 하나의 branch에서만 코드 작업을 하는 경우 유용하다. -c  옵션 사용하는 경우 branch 간 전환은 할 수 없다. 
> 
> 2) --depth 옵션:  git log 로 확인한 가능한 history 개수를 제한한다. 예를 들어,  --depth=100 으로 설정했다면 git log로 100개 까지의 history 를 다운로드 한다.  소스를 처음 받는 경우 repo init 의 depth 옵션은 git clone 시 depth 옵션으로 전달된다. 
> 
> 3) --no-tags 옵션:  tag 정보없이  소스를 다운로드 하는 것이다.  Working 소스에서 tag 가 전환이 필요 없는 경우 유용하다.

또한 안드로이드 버전별로 다운받을 수 있는데 이때 -b옵션으로 어떤 버전을 선택할 것인지 빌드 태그를 명시할 수 있다. 안드로이드 버전별 명칭은 [코드명, 태그 및 빌드 번호](https://source.android.google.cn/setup/start/build-numbers?hl=ko#source-code-tags-and-builds)에서 확인할 수 있다.

필자는 **android 10**버전을 선택했다. 원래는 android 6버전을 선택하여 진행했었는데 오류가 너무 많아서 빌드하기에 어려움이 있었다. 그래서 10버전으로 선택한 후 진행하였다.

![4](/assets/images/1/4.PNG)

---

## 2) repo sync

```
repo sync -q -c --no-tags
```

repo ysnc 옵션에서 repo init 과 유사하게 -c 와 --no-tags 옵션을 사용할 수 있다. 

> 1) -c 옵션: 현재 설정된 branch 의 소스만 다운로드 한다.
> 2) --no-tags 옵션:  Tag 정보 없이 소스를 다운로드 한다. 

-jN(N은 쓰레드의 개수)로 작업의 속도를 조정할 수 있지만 생략하면 그냥 알아서 적당한 쓰레드수를 자동으로 조절해준다.

-q 옵션은 진행로그를 띄워주지 않게 해주는 옵션이다. 써도되고 안써도 된다.

소스가 다운로드가 완료되면 repo start <branch> --all 명령오 모든 git을 android-10.0.0_r36 branch 로 전환(check out)시킨다. 

```
repo start android-10.0.0_r36 --all
```

---

## 3) Mac OS 빌드용 tool chain 삭제

AOSP는 Linux 와 Mac OS용 tool chain이 모두 포함되어 있다. Mac OS 와 관련한 tool chain은 삭제한다. MAC OS 용 tool chain은 대략 10 GB정도이며,  /prebuilt 와  /.repo 폴더 포함되어 있다.  git을 삭제하기 때문에 main branch에서 적용할때는 주의해야 한다.   

```
find ./prebuilts/ -name "*darwin-x86*" -type d
find ./prebuilts/ -name "*darwin-x86*" -type d -exec rm -rf {} \;

find ./.repo -name "*darwin-x86*" -type d 
find ./.repo -name "*darwin-x86*" -type d -exec rm -rf {} \;
```

---

# 3. Android 소스 빌드

## 0) 권한 부여

빌드를 할 때 Permission denied라는 오류를 정말 많이 보았는데 관리자 권한으로 할 수 없는 작업들이 있고 관리자 권한으로 하면 안되는 작업들도 있어서 그냥 애초에 모든 사용자에 대한 읽고쓰기 권한을 부여하여 진행할 것이다.

```
cd ~/project
sudo chmod -R 777 .
```

project폴더로 이동한 후 해당 폴더 내에 있는 모든 하위 폴더 및 파일들에게 777권한을 부여한다.

## 1) envsetup.sh 스크립트로 환경을 초기화

```
. build/envsetup.sh
```

## 2) 기기 빌드 선택

Android 10 소스를 다운로드한  폴더(/project)로 이동해서  build/envsetup.sh 와 빌드하고자 하는 target device 와 release 옵션으로 lunch 옵션을 설정해야 한다.  AOSP Tag 정보는 [여기](https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds) 에서 확인할 수 있고, Target Device Name 은 [여기](https://source.android.google.cn/setup/build/running?hl=ko) 에서 선택할 수 있다. 

![5](/assets/images/1/5.PNG)

![6](/assets/images/1/6.PNG)

android-10.0.0_r36 tag에 해당하는 단말은 Pixel2 와 Pixel 2XL 이다. Pixel 2XL 의 빌드 ID인 asop_taimen 와, 개발용으로 루팅이 가능한 userdebug 를 선택하여 aosp_taimen-userdebug 로 lunch 한다.

```
lunch aosp_taimen-userdebug
```

## 3) 코드 빌드

```
m
```

m 명령어를 이용하여 코드를 빌드해준다.

---

# 4. 결과

![7](/assets/images/1/7.PNG)

또 실패했다.... 이때 실패한 이유인 "ninja failed with: exited status 137"로 검색을 해보니 

[Building image failed with exit code: 137 · Codefresh | Docs](https://codefresh.io/docs/docs/troubleshooting/common-issues/error-code-137/) 이 사이트에서 "This issue occurs where you are low on pipeline resources. The build step does not have enough memory to finish building." 이런 말이 나왔다. 그래서 빌드간에 memory의 용량을 늘리려고 찾아보니 swap 크기를 늘리면 된다는 정보가 있었다. 그래서 swap의 크기를 늘리려면 다음의 명령어들을 수행해야 한다.

```
# swap 비활성
sudo swapoff -v /swapfile

# swap 을 12GB 로 조정한 경우 
sudo fallocate -l 12G /swapfile

#권한 설정
sudo chmod 600 /swapfile

#swap file 만들기
sudo mkswap /swapfile

#swap file 활성화 : 리부티하지 않아도 swap file이 활성화 된다.
sudo swapon /swapfile

# swap이 정상동작되는지 free 명령어로 확인하다.
free -m
```

Android의 시스템 requirement를 보면 Swap 포함하여 16GB 이상이니 안전하게 20 GB 로 설정한다.

![8](/assets/images/1/8.PNG)

그 후 다시 m 명령어를 이용하여 빌드를 시도하였다.

![9](/assets/images/1/9.PNG)

이번엔 OutofMemoryError가 발생했는데 이는 java의 heap space의 부족으로 발생한 것이다. api-stub-docs 의 Java 실행 시 HeapSize 에 대한 명시적 선언없이 default 값으로 사용되다보니 이런 에러가 발생한 것 같다. JDK 9 의 Java Heap Size의 default는 2GB 로 설정되어 있고, 이를 아래 명령어를 사용하여 4GB를 늘려서 수정하였다. 

```
export _JAVA_OPTIONS=-Xmx4g
```

 Java Heap size는

```
./prebuilts/jdk/jdk9/linux-x86/bin/java -XX:+PrintFlagsFinal -version | grep 'HeapSize'
```

로 확인할 수 있다. java Max Heap은 실제 메모리은 1/4 로 설정된다.



수정 전

![10](/assets/images/1/10.PNG)

수정 후

![11](/assets/images/1/11.PNG)

다시 m 명령어를 실행하였다.

![](C:\Users\Don%20Oh\AppData\Roaming\marktext\images\2022-02-28-11-54-10-image.png)

드디어!!!! 성공하였다.

![12](/assets/images/1/12.PNG)

```
emulator
```

위 명령어를 실행한 결과 위 스크린샷처럼 어떤 프로그램이 실행되는데 android가 실행되진 않았다.

이 이후에 과정들은 다음 스터디에서 진행하도록 하겠다.
  
**## 참고한 사이트**

[Building the Android Open Source Project](https://www.raywenderlich.com/10197539-building-the-android-open-source-project)
  
[우분투에서 파이썬 버전 변경하기](https://seongkyun.github.io/others/2019/05/09/ubuntu_python/)
  
[[AOSP] Code Download 안드로이드 P 프리뷰 코드 다운로드](https://spoiler.tistory.com/107?category=662650)
  
[Android 소스 최적화 (100GB에서 65GB로 줄이기)](https://kibua20.tistory.com/51)
  
[JDK 7(Java 1.7) 설치하기 in Ubuntu 18.04](http://www.wearedev.net/209)
  
[OpenJDK 7 Download and Installation on Ubuntu ](https://techoral.com/blog/java/install-openjdk-7-ubuntu.html)
  
[building android error java version](https://stackoverflow.com/questions/36885029/building-android-error-java-version)
  
[Android 6 compile](https://github.com/nettee/nettee.github.io/issues/2)
  
[[리눅스] 우분투 업데이트 에러 Could not get lock /var/lib/dpkg/lock-frontend - open (11: resource temporarily unavailable)](https://bigbigpark.tistory.com/39)
  
[Exit code 137 - Out of memory](https://support.circleci.com/hc/en-us/articles/115014359648-Exit-code-137-Out-of-memory)
  
[AOSP build error ninja failed with: exit status 1](https://stackoverflow.com/questions/52617313/aosp-build-error-ninja-failed-with-exit-status-1)
  
[AOSP Building Failed - ckati failed with: exit status 1](https://stackoverflow.com/questions/64027042/aosp-building-failed-ckati-failed-with-exit-status-1)
  
[안드로이드 소스 다운로드](https://source.android.google.cn/setup/build/downloading?hl=ko)
