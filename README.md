# 자세한 설명
## 아두이노
```ino
void setup() {
  Serial.begin(9600);
}
```
시리얼 통신을 초기화하고 데이터 전송 속도를 9600 bps로 설정합니다.
```ino
double th(int v) {
  double t;
  t = log(((10240000/v) - 10000));
  t = 1 /(0.001129148 + (0.000234125*t) + (0.0000000876741*t*t*t));
  t = t - 273.15;
  return t;
}
```
입력 값을 사용하여 초기 계산을 수행하고 결과를 t에 저장합니다.<br>두 번째 계산을 수행하여 온도 값을 보정합니다.<br>화씨를 섭씨로 변환하는 식을 사용하여 온도 값을 섭씨로 변환합니다.
```ino
void loop() {
  int a = analogRead(A0);
  Serial.println(th(a));
  delay(500);
}
```
아날로그 핀 A0에서 센서 데이터를 읽어와 a에 저장합니다.<br>함수를 호출하여 센서 값을 섭씨 온도로 변환하고 시리얼 모니터를 통해 출력합니다.<br>500밀리초(0.5초) 동안 대기합니다. 이렇게 하면 온도 값이 0.5초마다 시리얼 모니터에 표시됩니다.
## 프로세싱
```pde
import processing.serial.*;
Serial p;
```
시리얼 통신 라이브러리를 Processing에 가져옵니다.<br>클래스를 사용하여 시리얼 통신을 관리하는 객체를 선언합니다.
```pde
void setup(){
  size(400,400);
  p = new Serial(this, "COM4", 9600);
}
```
그래픽 창의 크기를 400x400 픽셀로 설정합니다.<br>시리얼 객체를 생성하고, "COM4"라는 시리얼 포트와 9600 bps의 통신 속도로 초기화합니다.
```pde
void draw(){
  if(p.available() > 0) {
    String m = p.readStringUntil('\n');
    if(m != null) {
      print(m);
      if(float(m) > 28) {
        background(255,0,0);
      }
      else {
        background(0,255,0);
      }
    }
  }
}
```
시리얼 포트로부터 데이터를 읽을 수 있는지 확인합니다.<br>시리얼 포트에서 개행 문자('\n')를 기준으로 문자열을 읽어와 m 변수에 저장합니다.<br>읽은 데이터가 null이 아닌 경우 실행합니다.<br>읽은 데이터를 Processing 창에 출력합니다.<br>읽은 데이터를 부동 소수점 숫자로 변환하고, 이 값이 28보다 크면 실행합니다.<br>배경색을 빨간색으로 변경합고, 그렇지 않으면 초록색으로 변경합니다.
