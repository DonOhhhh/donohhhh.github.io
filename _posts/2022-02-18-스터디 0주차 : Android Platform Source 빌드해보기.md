준비물 : Vmware or Virtual Box

# 1. Install Vmware Workstation or Virtual Box.

<center><img src="/assets/images/0/vmware_installation.PNG" width="100%" height="100%"></center>
위 사진은 vmware16이지만 필자는 vmware10 버전을 선택했다. 왜냐하면 나온지 오래됐기 때문에 체험판이 아닌 정식버전을 쓸 수 있기 때문이다.
vmware10 정식버전 사용법은 잘 찾아보면(?) 찾을 수 있다.

<center><img src="/assets/images/0/ubuntu_image_download.PNG" width="100%" height="100%"></center>
그리고 ubuntu 공식 홈페이지에서 ubuntu 18.04.6버전의 iso 파일을 다운로드 해준다.

---
# 2. Make new Virtual Machine by using Ubuntu 18.04.6 image.

<center><img src="/assets/images/0/making_virtual_machine.PNG" width="100%" height="100%"></center>
vmware에서 new virtual machine을 선택한다. 그리고 next, next, next를 선택하여 새로운 가상머신을 만들어준다.

---
# 3. Log in to the Virtual OS and download python2.7 and python3.6 and repo.

<center><img src="/assets/images/0/python_version_check.PNG" width="100%" height="100%"></center>
먼저 설치되어 있는 python 버전을 확인한다.

<center><img src="/assets/images/0/python2.7_download.PNG" width="100%" height="100%"></center>
그리고 python2.7이 깔려있지 않으면 python 2.7버전을 설치해준다.

<center><img src="/assets/images/0/ubuntu_python_version_change.PNG" width="100%" height="100%"></center>
그 후 위 명령어들을 쳐서 python3과 python2.7을 자유롭게 선택할 수 있도록 해준다.

<center><img src="/assets/images/0/select_python2.7.PNG" width="100%" height="100%"></center>
일단 지금은 python2.7을 선택해준다.

<center><img src="/assets/images/0/download_repo.PNG" width="100%" height="100%"></center>
마지막으로 위 명령어들을 통해 repo packge를 설치해준다.

---
# 4. Download Android Platform Sources

<center><img src="/assets/images/0/repo_init.PNG" width="100%" height="100%"></center>
이제 android platform source를 다운받기 위해 repo init을 진행시켜보면

<center><img src="/assets/images/0/syntax_error.PNG" width="100%" height="100%"></center>
이런 에러가 뜬다.

<center><img src="/assets/images/0/select_python3.PNG" width="100%" height="100%"></center>
이때 python3으로 버전을 변경해주고 다시 repo init을 진행시키면

<center><img src="/assets/images/0/repo_init_success.PNG" width="100%" height="100%"></center>
성공적으로 다운로드 된다.

우리팀은 안드로이드6 marshmallow 버전을 다운받기로 했는데 크기가 대략 4.1GB 정도 된다. Full source에 비하면 매우 작은 크기이다.

<center><img src="/assets/images/0/source_download_failed.PNG" width="100%" height="100%"></center>
그런데 default.xml이 없다면서 오류가 뜨고 제대로 다운로드가 되질 않는다.

이 오류는 아무리 찾아봐도 어떻게 해결하는지 도무지 방법을 알 수 없어서 이상의 과정을 진행할 수가 없었다.

---
# 5. Build Platform Sources
