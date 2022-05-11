---
layout: single 
title: "NVIDIA Jetson AGX Xavier 보드, PCA9685모듈, Jetsonhack 소스를 이용하여 서보모터 사용하기!"
categories: 
 - servomotor 
tags:
 - xavier 
 - servomotor
 - motor
 - module
 - 서보모터
 - nvidia
 - 모터
excerpt: "Xavier 보드를 이용한 서보모터 동작 테스트"
case date: 2019-11-19 23:36:00 +0900 
last_modified_at: 2021-02-05 12:14:00 +0900 
toc: true
toc_sticky: true
toc_label: "Contents"
toc_icon: "cog"
---

nvidia Jetson 보드 중 Xavier 보드를 이용하여 서보모터 동작 시켰습니다.

PCA9685 모듈을 이용하여 모터를 구동했습니다.

---

# 하드웨어 구성

## [PCA9685 정보]

- PCA9685 : [https://www.adafruit.com/product/815](https://www.adafruit.com/product/815)

    - (한국어 번역 : [https://www.icbanq.com/P007406152](https://www.icbanq.com/P007406152)\)

- 모듈 데이터시트 : [https://cdn-shop.adafruit.com/datasheets/PCA9685.pdf](https://cdn-shop.adafruit.com/datasheets/PCA9685.pdf)

    - (위 링크로 데이터시트 다운이 안되는 분은 [https://learn.adafruit.com/16-channel-pwm-servo-driver/downloads](https://learn.adafruit.com/16-channel-pwm-servo-driver/downloads) 들어가서 데이터시트를 받으시면 됩니다.)

## [Xavier 정보]

우선 연결을 위해 Xavier의 핀맵을 확인합니다.

[Xavier 핀맵 Link](https://www.jetsonhacks.com/nvidia-jetson-agx-xavier-gpio-header-pinout/)

(핀 갯수가 많아 이미지 업로드시 전체 핀 맵은 한눈에 안들어오므로 사이트 확인 부탁드립니다)

제가 사용한 핀은 아래와 같습니다.

### < 사용핀 목록 >

| number  | usage     |
| ----------- | ------------|
| 1                | 3.3 VDC |
| 6 | GND |
| 27 | **I2C_GP2_DAT /** *General I2C #2 Data _ 1.8/3.3V, I2C Bus 1* |
| 28 | **I2C_GP2_CLK /** *General I2C #2 Clock _ 1.8/3.3V, I2C Bus 1* |

모듈의 VCC와 보드의 1번핀을,

모듈의 GND와 보드의 6번 핀을 연결해줍니다.

모듈의 SCL과 보드의 28번핀을,

모듈의 SDA와 보드의 27번핀을 연결해줍니다.

“왜 이렇게 연결하는가?” 는 I2C 통신을 우선 알고 와야 합니다.

잘 정리되어있는 블로그 강좌를 추천합니다. [몽구스 네이버블로그 Link](https://blog.naver.com/yuyyulee/220323559541){: .btn .btn--success}

모듈에 윗부분의 **전원**과 **서보모터**도 연결합니다.

(서보모터는 갈색 선이 GND, 빨간선이 V+, 노란선이 PWM입니다)

**note:** 외부 전원은 최대 6V까지 사용할 수 있다고 합니다.
{: .notice--warning}
---

# 코드 다운 및 수정

## 소스 다운

하드웨어 연결이 끝났으니 소스가 필요하겠죠?

[https://github.com/jetsonhacks/JHPWMDriver](https://github.com/jetsonhacks/JHPWMDriver)

이 소스를 사용할 겁니다. git clone 명령어를 이용하여 다운받습니다.

## 소스 Xavier 버전으로 수정

**그러나!** TX1보드 기준으로 작성되어있으므로 약간의 수정이 필요합니다!

(제가 참고한 사이트 : [https://devtalk.nvidia.com/default/topic/1045330/jetson-agx-xavier/i2c-library-not-working-properly/](https://devtalk.nvidia.com/default/topic/1045330/jetson-agx-xavier/i2c-library-not-working-properly/)\)

그냥 소스를 돌리면 i2c_smbus_read_byte_data’ was not declared in this scope 라는 에러가 뜹니다!

버전이 달라서 그렇다고 합니다.

### JHPWMPCA9685.h 파일

**/JHPWMDriver/src/JHPWMPCA9685.h 파일** 28번째 줄에 아래 3줄을 추가해줍니다.

```cpp
    extern "C" {
        #include <i2c/smbus.h>
    }
```

아래의 사진과 같이 추가를 합니다.

![1번그림-소스파일](https://lh5.googleusercontent.com/YN6iUJpNGlBALdmUjXx07YsCz2VcF8j8M-12tvKrZwacQmAQYRWzHTVYrdWbM69ogq5pvGcPNKxCTN0ootQcOjj01hTw2k-YsO18zomVl5VhwEVjU65vG9ijXfxmmXSubvzfI-99)

그 후 소스를 돌리면 undefined reference to 'i2c_smbus_read_byte_data' 라는 에러가 뜹니다.

i2c 라이브러리가 첨부가 안되어서 그렇다고 합니다.

### Makefile 수정

**/JHPWMDriver/example/Makefile** 파일을 아래과 같이 수정해줍니다.

```bash
g++ displayExample.cpp ../src/JHPWMPCA9685.cpp -I../src -li2c -o [출력파일명]
```

아래와 같이 추가를 합니다.

![2번그림-소스파일](https://lh6.googleusercontent.com/fP3hPjvVDYF8VySVRf3xlXhl1GcFM3c2SbNfPehc46Or87GvlAI0v-LEyqhgGKy4qlWeuVWbiwTjrnBi-lChzsoaYEpYpcEpMgyr3gzvCoFXUSb0zLPm5FFm-QeQRDuW96MdEXSc)

이제 make 명령어를 입력하면 소스가 잘 구동이 됩니다!

**Note:** (혹시 i2c-tool은 설치하셨죠? 설치 명령어는 오른쪽과 같아요 sudo apt-get install libi2c-dev i2c-tools)
{: .notice--warning}
**Note:** (혹시 servoMax Min 변수 에러가 나오나요? /JHPWMDriver/example/example.cpp의 36, 37번줄의 주석처리를 해제합니다.)
{: .notice--warning}
**Note:** 위 괄호 두개는 메뉴얼에 있는 공통된 내용이랍니다. github.com/jetsonhacks/JHPWMDriver메뉴얼 읽어보시는걸 추천합니다.)
{: .notice--warning}


# I2C통신 설정

만들기만 하면 완성이냐?!? 아니죠~! 그 후 실행이 또 문제입니다

(제가 참고한 사이트 : [https://github.com/jetsonhacks/JHPWMDriver/issues/1](https://github.com/jetsonhacks/JHPWMDriver/issues/1)\)

## i2c 주소 확인

자비어 핀맵 아래 I2C예제에서 볼 수 있듯 자비어 에서는 1번 과 8번 버스를 사용합니다.

그러나 제가 연결한 핀은 1번 버스를 사용합니다. 

**Note:** (8번 버스를 사용하고 싶다면 자비어의 다른 핀을 사용하셔야 합니다. 핀맵을 보고 연결해보세요!)
{: .notice--info}

다음 명령어를 이용하여 i2c주소를 확인할 수 있습니다.

```bash
sudo i2cdetect -y -r [버스 숫자]
```

![3번그림-i2c](https://lh5.googleusercontent.com/QxKtAwz79roDx5CDV10HqGxci1HqgrWZNQjcbxHC0wen1A-KmyY4ATxo4crUVTQAIHzp6lf8o0Sx6eqeV5zSUk6LVO9wwXQXRYNT0gmmqE93b8FuthNQpZweEMFo33P9zmLWVHqN)

숫자가 떠있는 부분이 사용 가능한 주소입니다. 저는 70번 주소를 사용하겠습니다.

우선! 버스가 다르다면 아래와 같이 설정해주세요. 1로 되어있는지 확인하세요. (아마 기본적으로 1로 되어있을거에요.)

## JHPWMPCA9685.cpp 수정

**/JHPWMDriver/src/JHPWMPCA9685.cpp** 파일의 커서부분의 숫자를 본인의 버스에 맞게 설정합니다

![4번그림-JHPWMPCA9685.cpp소스파일](https://lh4.googleusercontent.com/-2D4qzNCdtI1fpnQePlmEGVNFF7BOAFtyEA717BJ6Gb182TGquFtDnsnuUdCaUxpHGuZ4-6D1OcVuVxG0X9CEWkvesIIENdecDhCD_YZyFiL5gOSjhiTeNHGP8yjIObh3CePYZp9)

그러면 주소를 변경하러 가겠습니다.

## JHPWMPCA9685.h 수정

**/JHPWMDriver/src/JHPWMPCA9685.h** 파일의 커서부분의 숫자를 본인의 주소에 맞게 바꿔줍니다. 기본은 0x40으로 되어있을겁니다. 바꿔주세요!

![5번그림-JHPWMPCA9685.h소스파일](https://lh5.googleusercontent.com/ib5HxV-cPaiqsixRjovRrzeAShtTnsPlJjS-GdI5xxHArG2Dm93mj46syQYL3OfLeU1-XzXMNIwyB8MpI3euhkTWNay-UBgE9yA84o0IvRy8cdK00NCFnsnZ7S2RmAdN5PgMCT-N)

이제 완료되었습니다!

## 실행

다시 **/JHPWMDriver/example/** 폴더로 이동하여 make 명령어를 입력하면 실행파일이 생깁니다.

실행파일을 실행하면 아래와 같이 잘 실행될겁니다!

![6번그림-실행](https://lh6.googleusercontent.com/fJgtEa8xqoi-aGYB1rpR7N4wMf_-NGlY4Od1VYj-rccVQi6rpoHTYihg5JeFruIsQPbIPUZkHHxIqzbDQsv0zxAY3w3XIx3ZGRlXmj7uTcE18wNEiTv02oZhwYMsC_84prVK7AlG)

{% include video id="sp1ICbyFcVY" provider="youtube" %}
