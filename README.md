# 시타델 프린터 운용지침서
KUMS Citadel Printer Manual&amp;Config Files
이동현 매니저



## 0. 정보 등급



이 문서는 개조 가능성이 있는 프린터를 서술한다. 이는 하드웨어의 물리적 개조와 활발히 개발되는 소프트웨어의 업데이트를 포함한다. 따라서 아래 서술되는 모든 데이터는 3개의 등급으로 구분하여 인지하여야 한다.

* 불변 등급: 변하지 않는 사실의 서술, 또는 de facto 표준 사항 – 검은색으로 표시
* 가변 등급: 현재로서는 서술된 방향으로 개발되었으나 추후 변경될 가능성이 있는 사항 – *기울임체로 표시*
* 임시 등급: 개발 과정에서 뚜렷한 근거 없이 결정된 사항으로 반드시 변하게 될 사항 – ~~취소선으로 표시~~

이 프린터를 사용하기 위해서 다음의 지식이 필요:

-	3D 프린팅 기본지식
-	[PrusaSlicer](https://www.prusa3d.com/drivers/) 사용법
-	웹 브라우저 사용법

이 프린터를 유지보수 하기 위해서 다음의 지식이 필요:

-	리눅스 터미널
-	웹 마크업 언어(html, css 등)
-	파이썬 (일부 2.7, 대부분 3.0 이상)
-	기초적 전기전자 기술



## 1. 개요



> 시타델 프린터는 VORON 디자인 팀이 설계한 VORON 2.4를 기반으로 메이커스페이스의 환경에 적합한 여러 기능이 추가된 3D 프린터이다. VORON 2.4는 주로 Aliexpress나 Ebay 등 국제적인 쇼핑 사이트에서 구하기 용이한 부품을 사용할 것을 상정하고 설계되었고 110V 전원 소켓 등 전기전자적 부품이 구미권 표준에 맞춰져 있다. 이를 한국에서 주로 유통되는 부품 규격에 맞게 수정하고 웹캠, 챔버 가열기 등을 추가한 것이 시타델 프린터이다.
> 시타델의 원형인 VORON, 탑재된 Octoprint와 Klipper 등 여러 소프트웨어는 모두 세계의 걸출한 개발자들이 오픈 소스로 개발한 것이다. 이 프린터의 사용자는, 자신의 작업물을 거리낌 없이 무료로 공개하는 개발자들의 용단을 인지하고, 그들의 노고에 공감하며, 이를 사용하여 얻어낸 결과물 또한 그들의 정신을 계승하여 세상을 널리 이롭게 하는 데 쓸 것을 결심해야 한다.
>  - 2020년 이동현



## 2. 제원



* 구동방식: [Core-XY](https://corexy.com/)
* 온도 제한: 핫엔드 260도 / 히티드 베드 110도
* 압출 가능 소재: ABS, PETG, TPU, ASA, PLA *(PLA는 핫엔드 어셈블리를 교체하여 사용)*
* 속도 제한: *압출 속도 150mm/s, 이송 속도 300mm/s*
* 가속도 제한: *최대 3000mm/s^2, 권장 1500mm/s^2*
* 출력 좌표 제한: *(0, 0, 0) – (300, 300, 275)*
* 소비 전력: *700W (추정)*
* 프로세서: 라즈베리파이 4B 1개, SKR v1.3 2개
* 소프트웨어: 
  - [Octopi](https://octoprint.org/download/): 운영 체제
  - [Klipper](https://www.klipper3d.org/): 모션 플래너
  - [Octoprint](https://octoprint.org/): 웹 인터페이스



## 3. 하드웨어



이 프린터는 Core-XY라는 구동 방식을 사용한다. 이는 모터가 움직이지 않고 영리하게 배치된 벨트를 사용하여 툴헤드를 이송하는 방식이다. 자세한 사항은 [corexy.com](https://corexy.com/)을 참조한다.

![Core-XY Diagram](https://github.com/earthicko/Citadel/blob/main/materials/corexy.png)

이 방식은 모터가 움직이지 않기 때문에 매우 높은 가속도 및 속도를 가능케 하지만 벨트의 **장력과 각도**를 세심하게 설정해야 한다.

- 따라서 매니저는 시타델의 벨트가 풀리에 올바르게 안착하여 구동하는지 감시하여야 한다.

이 프린터는 **리니어 레일**을 사용하여 툴헤드의 이동 경로를 3개 축에 평행하도록 제한한다. 

- 따라서 매니저는 레일이 적절하게 윤활 되었는지 확인하고, 필요시 **재봉틀 윤활유 또는 *리튬 기반 그리스***를 도포한다.
- WD-40의 도포는 방청 피막을 형성해 레일을 망가뜨리므로 **엄격히 금지**한다.

이 프린터는 **E3D V6**를 핫엔드로 사용한다. 핫엔드의 중심에는 **히트 브레이크**라는 부품이 있는데, 이것은 히터 블록에서 발생하는 열이 상부로 전도되지 않게 막아주는 역할을 한다.

히트 브레이크는 PLA를 출력할 때 특히 중요하다. PLA는 불과 60도 정도의 온도에서도 급격히 연해져 히트 브레이크를 통과할 때 곧바로 들어가지 않고 좌굴되어 핫엔드를 막히게 한다. 

또한 PLA는 금속에 잘 달라붙는 성질이 있어 히트 브레이크 내벽에 붙어 핫엔드를 막히게 한다. 따라서 PLA를 출력하기 위해서는 내부가 **테플론**으로 코팅된 히트 브레이크를 사용해야 한다. 

하지만 테플론은 240도 이상의 온도에서 기능을 잃고 변형된다. 따라서 PLA를 출력할 수 있는 프린터는 ABS를 출력할 수 없고, 그 역 또한 성립된다. 


- *시타델은 대형 ABS 출력물을 최우선 목표로 하여 설계되었다.*

> Q: 큐비콘의 프린터는 모두 테플론 코팅이 된 노즐목을 사용하는데도 ABS를 출력할 수 있습니다. 시타델은 왜 그렇게 할 수 없습니까?
> 
> A: 테플론이 240도 이상의 환경에 노출된다고 하여 그 즉시 변성하는 것은 아닙니다. 다만 장기적으로 보았을 때 테플론은 반드시 변성하여 필라멘트에 섞이거나 녹아내려 노즐을 막히게 합니다. 이는 단순히 노즐을 교체하는 것으로 대처할 수 있지만 진행중인 출력물은 폐기해야 합니다. 이는 범용성 높은 프린터를 지향하는 큐비콘의 전략적 선택으로 이해해야 합니다.


반드시 필요한 경우 압출부 전체를 보관된 PLA 전용 핫엔드 어셈블리로 교체한 후 PLA 또는 PLA+를 출력할 수 있다. 동시에 익스트루더 우측의 배선 버스 PCB의 해당 배선 (히터 전력선, 써미스터 배선 등)을 조심히 연결한다.

-	~~핫엔드 어셈블리가 혼용되지 않도록 주의한다. 보관된 핫엔드 어셈블리의 먼지 유입에 주의한다.~~

이 프린터는 SN-04 인덕티브 센서를 프로브로 사용한다. 간트리 레벨링과 베드 레벨링에 이 센서를 사용한다. 자세한 사항은 배선도를 참조한다.

이 프린터는 2기의 100와트 PTC 히터를 사용해 챔버를 가열한다. *PTC의 특성상 과열로 인한 손상이 불가능하기 때문에 이는 베드 히터에 병렬로 결선되어 별도의 제어가 필요하지 않다.*

이 프린터는 후면의 필터를 통해 내부 공기를 여과해 배출한다. 필터에 붙은 팬이 멈추면 내부의 fume이 여과되지 않는다. ~~필터 유닛은 시중의 적당한 HEPA 필터를 구입해 절단하여 만들어야 한다.~~

이 프린터는 PEI 코팅된 스프링 철판을 출력면으로 사용한다. PEI는 물리적인 손상에 극히 약하므로 주의해서 관리해야 한다.

*	PEI 시트는 에틸알코올에 반응하므로 아이소프로판올로 세척한다.
*	PEI 시트는 소모품이므로 재료구매 기회가 있을 시 여분을 구매한다.



## 4. 소프트웨어



시타델은 SKR v1.3 보드를 마이크로컨트롤러로 사용하고 라즈베리파이 4B를 서버로 사용한다.

* Octoprint는 웹 인터페이스를 통해 설정을 변경하고 업데이트 한다. 
*	Klipper는 ssh 접속하여 `git pull` 등의 명령으로 업데이트한다.
*	Klipper는 `~/printer.cfg` 파일을 수정하여 설정을 변경한다.

*	자세한 사항은 각 소프트웨어의 github repo를 참조한다.
    * OctoPrint [홈페이지](https://octoprint.org/)
    * OctoPrint [Repository](https://github.com/OctoPrint/OctoPrint)
    * Klipper [홈페이지](https://www.klipper3d.org/)
    * Klipper [Repository](https://github.com/KevinOConnor/klipper)

![Data Flow Masterplan](https://raw.githubusercontent.com/earthicko/Citadel/main/diagrams/data%20flow.png)

시중의 프린터와 달리, 마이크로컨트롤러가 모션 플래닝/G코드 파싱/온도 모니터링 및 제어 등의 모든 일을 관리하지 않는다. 라즈베리파이에서 리눅스 서비스 형태로 작동하는 klipper가 G코드를 파싱하여 모션 플래닝(모터가 어느 시점에 어떻게 움직여야 할 지 계산하는 작업)을 하고 계산 결과를 마이크로컨트롤러에 전송한다. 마이크로컨트롤러는 단순히 펄스를 내보내고 받는 slave 역할만 맡게 된다.

-	라즈베리파이와 SKR v1.3 사이의 안정적인 USB 연결이 매우 중요하다.

*시타델은 시설 내 DHL이라는 SSID를 가진 라우터에 연결되어있다. *

-	Wi-Fi 연결 설정을 바꾸고 싶을 시 리눅스 터미널에서 raspi-config을 사용한다.

교내 네트워크 방화벽 정책에 따라 교외에서는 시타델에 접속할 수 없다.

-	KoreaUniv AP, eduroam 등의 Wi-Fi 네트워크는 학교 소유지만 교내 네트워크가 아니다.
-	*몇 가지 실험 결과, 교내 방화벽은 외부에서 내부로 보내는 HTTP GET은 차단하지만, 내부에서 외부로 보내는 HTTP POST는 차단하지 않는다. 이는 SSL 등의 암호화로 우회할 수 없다. ~~따라서 ngrok 등의 터널링 서비스를 사용하면 안전히 외부에서 접속할 수 있을 것이다.~~ ngrok 터널링 서비스가 적용 중이다.*


### 4-1. 소프트웨어 심화


이 부분에서는 각 구성요소별 자세한 작동 원리를 다룬다.



## 5. OctoPrint 사용법



![OctoPrint Overview](https://raw.githubusercontent.com/earthicko/Citadel/main/materials/octoprint-00.png)

Octoprint는 유저를 관리자와 일반 유저로 구분하여 특정 기능에 대한 접근을 통제하거나 허용한다. 따라서 상단 이미지에 표시된 기능이 본인이 보고 있는 화면에 표시되지 않는다면, 그 기능은 관리자 전용이다.



## 6. PrusaSlicer 사용법



![PrusaSlicer Overview](https://github.com/earthicko/Citadel/blob/main/materials/prusaslicer-01.png)


### 0. (최초 1회) 설정 파일 불러오기


준비된 설정 파일을 불러온다.

설정 파일은 [이 링크](https://github.com/earthicko/Citadel/blob/main/Citadel%20Config%20Bundle.ini)에서 다운로드할 수 있다.

![PrusaSlicer How to Load Config File](https://github.com/earthicko/Citadel/blob/main/materials/prusaslicer-02.png)


### 1. 3D 모델을 불러오기


출력할 3D 모델 (`stl`, `3mf`, `obj` 등)을 불러온다.

![PrusaSlicer How to Load 3D File](https://github.com/earthicko/Citadel/blob/main/materials/prusaslicer-03.png)

또는 작업 영역 위로 드래그&드롭할 수 있다.


### 2. 오브젝트 조작


원활한 출력을 위해 불러온 오브젝트를 조작할 수 있다.

![PrusaSlicer How to Manipulate Model](https://github.com/earthicko/Citadel/blob/main/materials/prusaslicer-04.png)

버튼의 기능은 다음과 같다.

- A.	이동
- B.	회전
- C.	크기 조절
- D.	제면회전
- E.	절단

자세한 사항은 PrusaSlicer [가이드](https://help.prusa3d.com/en/article/general-info_1910)를 참고한다.


### 3. 소재 지정


출력에 사용될 소재를 선택한다.

![PrusaSlicer How to select material](https://github.com/earthicko/Citadel/blob/main/materials/prusaslicer-05.png)


### 4. 출력 설정 조절


원하는 결과를 얻기 위해 다음의 사항을 조절할 수 있다.

- Layer 높이
- Extrusion 두께
- Infill 밀도

![PrusaSlicer How to adjust print settings](https://github.com/earthicko/Citadel/blob/main/materials/prusaslicer-06.png)

- 출력 속도에 관련된 것은 충분한 지식을 가지고 조절한다.
- Printer Settings 탭의 값은 조절하지 않는다.


### 5. G코드 생성


![PrusaSlicer How to generate Gcode](https://github.com/earthicko/Citadel/blob/main/materials/prusaslicer-07.png)

우측 하단의 `Slice Now` 버튼, `Export G-Code`버튼을 차례로 클릭한다.



## 7. 출력 개시 과정



### 1. 웹 서버 접속


교내 네트워크에 접속된 PC에서 웹 브라우저를 사용해 dhlprinters.iptime.org:33 으로 접속한다.

  - 시설에 비치된 데스크톱 PC는 모두 교내 네트워크에 연결되어 있다.
  - *개인 랩톱을 사용할 시 KoreaUniv AP, eduroam에 연결하지 않고, makerspaceHall 또는 makerspace office에 접속한다.*


### 2. 로그인


본인이 발급받은 ID와 패스워드를 사용해 로그인한다.

![Octoprint Login Screen](https://github.com/earthicko/Citadel/blob/main/materials/octoprint-01.png)

  - 아이디 양식: `[이름의 두문자 2개][성씨의 로마자 표기][순번 (동명이인이 있을 시)]`
  - 예시: `dhlee01 (동현 이 01)`


### 3. G-Code 업로드


본인이 슬라이싱한 G코드 파일을 업로드한다.

![Octoprint Upload File](https://github.com/earthicko/Citadel/blob/main/materials/octoprint-02.png)

  - 강조된 Upload 버튼을 누른 후 원하는 .gcode 파일을 선택한다.
  - 또는 브라우저 창 위로 파일을 드래그&드롭 할 수 있다.


### 4. 프린트 시작


파일 별로 하나씩 마련된 프린트 버튼을 누른다.

![Octoprint Print Start](https://github.com/earthicko/Citadel/blob/main/materials/octoprint-03.png)


### 5. 초기화 과정 주시


시타델 프린터는 출력 개시 전 자기 점검 및 자기 보정을 실시한다. 이것은 기계적 오차의 누적을 상쇄하여 일정한 출력물의 품질을 얻기 위함이다.

  - 히팅 (Heating)

  익스트루더와 히티드 베드의 온도를 설정값에 맞게 가열하는 과정이다.
  
  - 호밍 (Homing)

  프린터가 출력의 기준점을 설정하는 과정이다.
  
  - 간트리 레벨링 (Gantry Leveling)

  이 프린터는 4개의 Z축을 가지고 있다. 4개의 모터가 모두 같은 위치에 있다는 보장이 없으므로, 프로브를 이용해 베드와의 상대적 거리를 계측해 거리 차이를 보정한다. 결과적으로 간트리의 평면이 베드의 평면과 일치하게 된다.
  
  - 베드 레벨링 (Bed Leveling)

  베드는 완벽한 평면이 아니다. 미세하게 휘어진 형태에 맞춰 출력 경로를 보정하기 위해 프로브로 9개의 지점을 계측한다. 결과적으로 툴헤드가 항상 베드 표면과 일정한 거리를 유지하며 움직인다.
  
  - 필라멘트 퍼지 (Filament Purge)

  이전 출력에서 남은 필라멘트 찌꺼기를 밀어내고 사용자가 핫엔드 주변에 붙은 이물질을 제거할 기회를 준다. 이때 과정이 원활히 진행되도록 전면 커버를 열고 압출되는 필라멘트를 바깥으로 유도한다. 사용자 개입이 없을 시 툴헤드가 잔여물을 끌고 베드를 긁을 수 있다.

이후 첫번째 레이어가 안착되는 것을 확인 후 자리를 떠도 좋다.
