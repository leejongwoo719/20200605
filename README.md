# 20200605
1.디자이너 파트
1) 블루투스
목록선택버튼 – 통신연결
연결해제버튼 – 통신해제
레이블 – 연결정보 표시
2)LED ON/OFF 스위치
누를 때 마다 교차하면서
LED의 점등과 소등을 반복
밝기 조절 시 OFF상태일 땐
슬라이더를 움직여도 LED가
따라 점등하지 않도록 하는
것을 목표로 설정
3)LED 밝기조절
각각 RED, GREEN, BLUE 슬라
이더를 구성하여 각각의 LED
의 밝기를 제어할 수 있도록
구성
2.블록코딩
함수 on)
LED의 점등에 관여하는 함수
ON버튼을 누르면 블루투스 클라이언트에 “1”을 송신
이후 LED를 소등 시키는데 필요한 OFF버튼으로 변경
함수 off)
LED의 소등에 관여하는 함수
OFF버튼을 누르면 블루투스 클라이언트에 “2”을 송신
이후 LED를 점등 시키는데 필요한 ON버튼으로 변경
RGB1함수)
ON버튼을 눌렀을 시 LED가 점등할 수 있도록 만드는
함수.
텍스트 값: (R or G or B + 슬라이더 위치값을 반올림
한 값 + \n)  = LED의 밝기를 슬라이더로 조절하기 위한
방법
RGB0함수)
OFF 버튼을 눌렀을 경우 슬라이더의 위치를 0으로
정렬하여 LED가 모두 소등할 수 있도록 만든 함수
버튼의 TEXT = “OFF”	<-ON상태 일 때
슬라이더의 섬네일 위치값을 확인해가며 밝기조절
버튼의 TEXT = “ON”	<-OFF상태 일 때
Off함수 호출 = 슬라이더를 움직여도 LED의 동작변화 x
3.아두이노 스케치코드
#include <SoftwareSerial.h>

#define redPin 11
#define greenPin 10
#define bluePin 9

String rgbValue = ""; 

SoftwareSerial BTSerial(2, 3); 

void setup() {
  Serial.begin(9600);
  BTSerial.begin(9600);
 
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
 
  analogWrite(redPin, 0);
  analogWrite(greenPin, 0);
  analogWrite(bluePin, 0);
}

void loop() {
        switch(rgbValue[0]) {
        case '1' : 
          digitalWrite(redPin, HIGH);
          digitalWrite(greenPin, HIGH);
          digitalWrite(bluePin, HIGH); break;
        case '2' : 
          digitalWrite(redPin, LOW);
          digitalWrite(greenPin, LOW);
          digitalWrite(bluePin, LOW); break;
        }
  if (BTSerial.available()) {
    char rgbData = BTSerial.read();
    if(rgbData == '\n') {
      Serial.println(rgbValue);

      if(rgbValue.length() > 1) {
        int brightness = rgbValue.substring(1).toInt(); 
        switch(rgbValue[0]) {
        case 'R':
          analogWrite(redPin, brightness);
          Serial.print("RED : ");
          Serial.println(brightness);
          break;
        case 'G':
          analogWrite(greenPin, brightness);
          Serial.print("GREEN : ");
          Serial.println(brightness);
          break;
        case 'B':
          analogWrite(bluePin, brightness);
          Serial.print("BLUE : ");
          Serial.println(brightness);
          break;
        }
      }
      rgbValue = "";
    }
    else rgbValue = rgbValue + rgbData;
  }
}
