그동안 TestCode를 작성 할 때, JSON 문자열을 일일이 입력하면서 불편하다는 생각을 했었습니다. 

그래서 이를 해결하기 위해 구글링 해보다가 알게된 사실입니다.

윈도우 기준으로

큰 따옴표 사이에 커서를 두고 alt + enter를 눌러 "Inject language or reference"를 누릅니다.

![1](https://user-images.githubusercontent.com/43127088/101462612-bbd72100-397f-11eb-9743-4deb641ac7ce.PNG)

그 후, JSON(JSON)을 클릭합니다.

![2](https://user-images.githubusercontent.com/43127088/101462643-c42f5c00-397f-11eb-85e8-a46d5d484d97.PNG)

큰 따옴표 사이에 커서를 두고 다시 alt + enter를 누르면 "Edit JSON Fragment" 라는 팝업이 추가되는데 눌러줍니다.

![3](https://user-images.githubusercontent.com/43127088/101462669-cb566a00-397f-11eb-9477-a34925292ea9.PNG)

아래 JSON 편집 화면이 생성됩니다. Fragment 편집 화면에서 JSON을 직접 편집하면 편집한 내용이 자동으로 변수에 값 변환이 되어서 적용 됩니다.

![4](https://user-images.githubusercontent.com/43127088/101462716-d7422c00-397f-11eb-8e5e-843f76a7ccb9.PNG)

작성 후 들여쓰기를 해줍니다.

![5](https://user-images.githubusercontent.com/43127088/101462733-dd380d00-397f-11eb-84ba-938947c75ff5.PNG)



큰 따옴표 사이에 커서를 두고 alt + enter을 눌렀는데 "Inject language or reference" 가 보이지 않을 시,

1. Settings - Editor - Intentions 클릭

2. Language Injection 리스트 내에 있는 Inject language or reference를 체크


다시 큰 따옴표 사이에 커서를 두고 alt + enter를 누르면 "Inject language or reference" 팝업이 정삭적으로 추가됩니다.