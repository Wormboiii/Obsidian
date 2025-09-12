
[AMD 공식 문서 참조](https://docs.amd.com/r/en-US/ug1400-vitis-embedded/Manage-Source-Code)


# 1. 소스, 헤더파일 생성

![[Images/Pasted image 20250912163852.png]]
현재 작업 중인 어플리케이션 내부 경로 중 하나를 선택(혹은 새로 폴더를 생성) 후 우클릭, New File로 .c 혹은 .h 파일을 생성해 준다.

외부 IDE나 텍스트 에디터 등으로 작업 후 파일을 디렉토리에 끌고 오는 식으로 하면 인식을 못한다.
꼭 바이티스 내부에서 파일을 생성해야 함.

# 2. 어플리케이션 설정에서 경로 추가

![[Images/Pasted image 20250912164059.png]]
현재 작업 중인 어플리케이션 탭의 우측에 설정 버튼을 눌러 설정 창을 키고

![[Images/Pasted image 20250912164212.png]]
Compiler Settings 하단의 UserConfig.cmake를 선택해 컴파일러 설정창을 열어 준다.

![[Images/Pasted image 20250912164335.png]]
좌측 탭에서 Directory를 선택 후, Include paths와 Compile sources 란에 경로를 추가해 주면 된다.

![[Images/Pasted image 20250912164433.png]]
내가 만든 .c .h 파일이 있는 경로에 마우스 우클릭 후 Copy Path를 선택한 후,

![[Images/Pasted image 20250912164544.png]]
Include paths에 Add item을 선택한 후 뜨는 창에 붙여넣기 해 준다.

![[Images/Pasted image 20250912164714.png]]
Compile source에는 Add item을 선택한 후 뜨는 창에 소스 파일(.c)의 경로를 추가한다.
Include paths에 추가한 경로를 기준, "추가한 경로/소스이름.c" 와 같이 추가하면 된다.

만약 적용이 되지 않을 시 Vitis를 재시작 해 본다.
