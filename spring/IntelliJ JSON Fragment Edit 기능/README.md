그동안 TestCode를 작성 할 때, JSON 문자열을 일일이 입력하면서 불편하다는 생각을 했었습니다. 

그래서 이를 해결하기 위해 구글링 해보다가 알게된 사실입니다.

윈도우 기준으로

큰 따옴표 사이에 커서를 두고 alt + enter를 눌러 "Inject language or reference"를 누릅니다.

![1](https://user-images.githubusercontent.com/43127088/101363288-90532880-38e4-11eb-827c-6b4b68f99396.PNG)

그 후, JSON(JSON)을 클릭합니다.

![2](https://user-images.githubusercontent.com/43127088/101363382-afea5100-38e4-11eb-81f6-5a015d489d07.PNG)

큰 따옴표 사이에 커서를 두고 다시 alt + enter를 누르면 "Edit JSON Fragment" 라는 팝업이 추가되는데 눌러줍니다.

![3](https://user-images.githubusercontent.com/43127088/101363465-c7c1d500-38e4-11eb-8c77-92ee069933b4.PNG)

아래 JSON 편집 화면이 생성됩니다. Fragment 편집 화면에서 JSON을 직접 편집하면 편집한 내용이 자동으로 변수에 값 변환이 되어서 적용 됩니다.

![4](https://user-images.githubusercontent.com/43127088/101363584-ecb64800-38e4-11eb-98f3-9ee5e9e8952b.PNG)

작성 후 들여쓰기를 해줍니다.

![5](https://user-images.githubusercontent.com/43127088/101363668-00fa4500-38e5-11eb-9770-002693ef835f.PNG)



큰 따옴표 사이에 커서를 두고 alt + enter을 눌렀는데 "Inject language or reference" 가 보이지 않을 시,

1. Settings - Editor - Intentions 클릭

2. Language Injection 리스트 내에 있는 Inject language or reference를 체크


다시 큰 따옴표 사이에 커서를 두고 alt + enter를 누르면 "Inject language or reference" 팝업이 정삭적으로 추가됩니다.