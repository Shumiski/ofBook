#불멸의 설치 세팅법 - 리눅스

*by [Arturo Castro](http://arturocastro.net)*

리눅스를 사용하면 베어본 데스크탑을 사용하여 손쉽게 설치작업을 할 수 있습니다. 그중에서도 특히, 짜증나는 알림에 대해서 걱정할 필요가 없습니다. 또한 화면 깜빡임을 스크립트를 통해 끌 수 있습니다. 단순히 복사하여 붙여넣기 하면 되므로 자동화과정이 정말 쉽습니다.

1. 여러분이 원하는 리눅스 배포판을 설치합니다. 저의 경우 대체로 우분투를 선택하는데, 그래픽카드 드라이버가 미리 설치되어있기 떄문입니다. 설치할 때 자동로그인 옵션을 활성화하면, 컴퓨터를 켜자마자 나중의 설치를 진행할 수 있습니다.


2. 모든 패키지를 최신버전으로 업데이트합니다. 우분투에서는 소프트웨어 업데이트 도구를 사용하거나, 커맨드라인을 사용할 수 있습니다:

```bash
sudo apt-get update
sudo apt-get upgrade
```

3. nvidia 또는 ati(amd) 그래픽카드를 사용하는 경우라면, 그에 해당하는 드라이버를 설치해줍니다. 가장 최신의 우분투 버전이라면, "소프트웨어 & 업데이트"의 추가 드라이버 탭에서 설치할 수 있습니다.

4. 기본으로 사용되는 데스크톱 환경인 Ubuntu Unity는 일반적으로 설치용으로는 과도한 불필요한 기능을 갖고 있습니다. 저의 경우 최근에는 Openbox를 사용하고 있는데, Openbox를 사용하면 데스크톱 환경(바탕화면)을 사용하지 않으며 수직동기화 문제를 해결해주기도 하므로 OpenGL를 살짝 빠르게 해줍니다.

```bash
sudo apt-get install openbox
```

5. 또한 원격 접근을 위해 openssh 서버를 설치해줄 수도 있습니다.

```bash
sudo apt-get install openssh-server
```

6. 이제 오픈프레임웍스를 다운받고 install_dependencies.sh 스크립트를 사용하여 설치합니다.

7. 세션을 로그아웃하고 시작화면에서 unity 대신 openbox를 선택합니다.

8. You'll get a grey screen with no decorations, bars... you can access a context menu pressing with the right button of the mouse anywhere in the desktop although i find it easier at this point to just log in through ssh from my laptop.

8. 이제 아무런 장식이 없는 회색 화면이 보일것입니다. 콘텍스트 메뉴 등은 데스크톱에서 마우스 우측버튼을 눌러 접근할 수 있습니다. 또한 다른 노트북에서 ssh를 통해 로그인하여 보다 쉽게 접근할 수도 있습니다.

9. 여러분의 어플리케이션을 설치했다면, 컴퓨터가 부팅되었을 때 자동으로 시작되길 원할것입니다. Openbox에서는 ~/.config/openbox/autostart 스크립트 파일을 만들고 여기에 어플리케이션의 바이너리의 경로를 추가하면 됩니다.

```bash
~/openFrameworks/apps/myapps/myapp/bin/myapp &
```

    라인의 끝에 &을 붙이는것을 잊지 마십시오. 이렇게 해야 어플리케이션이 백그라운드에서 실행됩니다.

10. 같은 autostart 파일에 아래의 라인을 추가하여 화면이 검개 되는것을 방지합니다:

```bash
xset s off
xset -dpms
``` 

다 됐습니다. 이제 컴퓨터가 부팅될때마다 여러분의 앱이 자동으로 실행됩니다. 대부분의 PC에는 전원이 차단되었을 경우 자동으로 재시작할 수 있는 바이오스 옵션을 갖고 있습니다. 따라서 만약 설치되는 곳이 밤에 전원이 차단된다면, 컴퓨터는 아침에 전원이 다시 들어올 때 즉시 자동으로 부팅될 것입니다.

## 몇가지 추가적인 팁:

- 리눅스는 SD 카드에 설치할 수 있기 때문에 하드디스크 대신 16기가 SD 카드를 사용하면 견적을 아낄 수 있습니다. 대부분의 SD카드는 꽤 빠르므로 부팅시간이 정말 짧습니다. USB나 CD로 부팅할 떄 하드디스크 없이 SD카드 리더기만 연결해두면, 우분투 설치관리자는 SD카드에 우분투를 설치합니다. 우분투에서는 "Disks" 도구를 사용하여 디스크를 생성하거나 복원/백업할 수 있으며, 커맨드라인에서 아래의 명령어로도 같은 작업을 수행할 수 있습니다:

```bash
sudo dd bs=4M if=/dev/sdcardDevice of=sdcardimg.bin
```

    마운트했을때 nautilus에서 속성으로 sdcardDevice이라는 이름을 가진 장치의 내용을 백업하기 위해서는 아래와 같은 명령어를 사용하면 됩니다:

```bash
sudo dd bs=4M if=sdcardimg.bin of=/dev/sdcardDevice
```

    백업을 복원하려면, 2개의 SD카드 리더기를 사용하여 마운트하고, 원본의 내용을 dd명령어를 이용하여 복사하면 됩니다.


- ssh로 머신에 접근하여 작업할 때, 디스플레이를 설정해두면 그래픽환경 출력을 하는 어플리케이션을 실행할 수 있습니다:

```bash
export DISPLAY=:0
```

    또한 ssh 세션에서 -X 옵션을 사용하면 그래픽 출력을  자신의 컴퓨터로 터널링하여 출력할 수 있습니다.


- 앞서 언급한 autostart 스크립트를 약간만 수정하면, 어플리케이션이 죽을 경우 아주 손쉽게 재시작하게 할 수 있습니다

```bash
~/openFrameworks/apps/myapps/myapp/bin/myapp.sh &
```

    이제 myapp.sh 스크립트를 아래와 같이 수정합니다:

```bash
cd ~/openFrameworks/apps/myapps/myapp/bin/
ret=1
while [ $ret -ne 0 ]; do
    ./myapp
    ret=$?
done
```

    이렇게 하면 어플리케이션이 죽을 경우 재시작하며, (종료할 당시 앱이 죽지 않았자면) 여전히 esc키를 눌러 앱을 종료할 수 있습니다.

