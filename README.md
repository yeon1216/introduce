# Introduce

안녕하세요. 

성장하는 개발자, 같이 일하고싶은 개발자 김성연입니다.

# Career
- 2012.03 ~ 2019.02 : 대학교 (졸업)
- 2013.10 ~ 2015.07 : 군대 (만기제대)
- 2017.09 ~ 2018.03 : 국비교육 웹 개발 과정 (수료)
- 2019.05 : 정보처리기사 취득
- 2019.05 ~ 2020.05 : 개발공부 (포트폴리오 : [여기](https://github.com/yeon1216/introduce/blob/main/portfolio.md))
- 2020.07 ~ : 볼트마이크로 재직중 (담당 업무 : android, react, nestjs)

## Troubleshooting Experience

[메모리 누수 원인을 찾다]

현재 회사 입사후 약 3개월차에 회사에서 서비스하는 앱에서 메모리 누수가 발생하여 저에게 메모리 누수의 원인을 찾아달라는 업무가 부여되었습니다. 참고로 현재 회사에서 서비스중인 앱은 라이브방송을 도와주는 '카메라파이라이브'앱입니다. 아직 프로젝트를 완전히 파악하지도 못한 상태에서 해당 업무가 부여되어 부담이 되었지만 천천히 방법을 생각해보았습니다. 우선 LeakCanary라는 안드로이드 메모리 누수를 탐지해주는 라이브러리를 사용하여 메모리 누수가 어디서 일어나는지 파악해보았습니다. 해당 라이브러리를 사용하니 메모리 누수가 어디에서 발생하는지는 금방 파악이 되었습니다. 여기에서 더 나아가 메모리 누수가 발생하는 원인을 좀 더 파악해보았습니다. 이 원인을 파악하면서 메모리의 static, heap 영역, Garbage Collector, Weak Reference에 대한 이해도가 높아졌습니다. 또한 안드로이드에서 Activity Context, Thread 사용시 메모리 누수에 대한 신경을 많이 써야된다는 점도 다시 한번 인식하였습니다.


[안드로이드 빌드 과정을 자동화 하다]

안드로이드 개발 후 release 빌드시 안드로이드 스튜디오를 통해서 release 빌드가 가능하지만 버튼을 몇번 눌러야하고, 추출된 apk를 찾아가 현재 버전이름으로 적용을 해야하는 등 불편함이 있었습니다. 그래서 고민을 하다 gradlew를 사용하여 shell script를 작성하여 release 빌드를 하도록 하였습니다. 이 과정에서 release 빌드를 하고 현재 버전을 android manifest에 접근하여 가져와 만들어진 apk 파일명을 수정하도록 하였습니다. 이제 script 파일만 한번 실행시키면 모든 작업이 자동으로 진행이 되도록 되었습니다. 추후에는 git remote의 master에 push 하였을 때 위의 과정이 서버에서 진행되도록 구현해볼 예정입니다.
