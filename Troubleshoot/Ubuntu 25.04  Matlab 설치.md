우분투 25.04 버전에서 Matlab 설치 후 실행 시 실행 화면에서 1~2분 후 아무런 에러 메시지 없이 프로그램이 다운되는 문제 발생, 터미널 로그에서는 "unable to launch MVM server" 라고 뜸.

해당 문제는 매트랩의 라이선스를 읽는 서버와 연결이 되지 않아서 발생.

#### 1. Patchelf
[Mathwork 서비스 연결 안됨 해결법](https://www.reddit.com/r/matlab/comments/1k2vt4o/ubuntu_2504_startup_problems/)
위 링크로는 바로 해결이 안됨.

위의 링크는 'patchelf' 라는 프로그램을 통해 libmwfoundation_crash_handling.so 라는 파일을 이전 버전 glibc와 호환되게 만들어 주는 것 같음. 그대로 따라했지만 일단 MVM 문제는 해결되지 않음.

1. ``ldd --version`` 으로 glibc 버전을 확인하고, 2.41버전이면 다음 진행.
2. 홈 디렉토리의 /home/wonhyeok/.MathWorks/ServiceHost/Thinkbook/v2025.5.1.1/bin/glnxa64 에서 "libmwfoundation_crash_handling.so" 라는 파일을 찾는다.
3. patchelf 최신 버전을 설치 후 ``sudo patchelf --clear-execstack (디렉토리)`` 로 패치를 진행한다.

patchelf 설치 시 snap으로 설치하면 --clear-execstack 명령어를 지원하지 않는 오래된 버전이 설치되니 꼭 github를 통해 최신 버전을 설치.


#### 2. Mathworks Service Host 재설치

[MSH 재설치 하는법](https://kr.mathworks.com/matlabcentral/answers/1815365-how-do-i-uninstall-and-reinstall-the-mathworks-service-host)
위의 링크로 바로 해결됨.

어떠한 이유인지는 모르지만, 매트랩 설치 시 MSH가 제대로 설치되지 않거나, 오래된 버전으로 설치되는 이슈가 있음. 해당 이슈로 인해 MVM 서버 연결 실패 에러가 뜨는 것. MSH를 재설치 해주면 바로 해결됨.

1. ``wget https://www.mathworks.com/MathWorksServiceHost/glnxa64/ReinstallMathWorksServiceHost`` 명령어로 MSH 재설치 프로그램을 받음(현재 shell 디렉토리에 설치됨)
2. ``chmod +x ReinstallMathWorksServiceHost`` 명령어로 실행 권한 준 후 ``sudo ./ReinstallMathWorksServiceHost`` 를 실행해 설치.

Failed to load module "canberra-gtk-module" 라는 응답과 함께 굉장히 설치가 오래 걸리는데, 기다리다 보면 설치가 되니 인내심을 갖고 기다리자.

재설치 후 매트랩을 다시 실행해 보면 제대로 실행이 된다.