우분투 25.04 버전에서 Matlab 설치 후 실행 시 실행 화면에서 1~2분 후 아무런 에러 메시지 없이 프로그램이 다운되는 문제 발생, 터미널 로그에서는 "unable to launch MVM server" 라고 뜸.

해당 문제는 매트랩의 라이선스를 읽는 서버와 연결이 되지 않아서 발생.

#### 1. Patchelf
[Mathwork 서비스 연결 안됨 해결법](https://www.reddit.com/r/matlab/comments/1k2vt4o/ubuntu_2504_startup_problems/)
위 링크로는 바로 해결이 안됨.

위의 링크는 'patchelf' 라는 프로그램을 통해 libmwfoundation_crash_handling.so 라는 파일을 이전 버전 glibc와 호환되게 만들어 주는 것 같음. 그대로 따라했지만 일단 MVM 문제는 해결되지 않음.

1. ``ldd --version`` 으로 glibc 버전을 확인하고, 2.41버전이면 다음 진행.
2. 홈 디렉토리의 