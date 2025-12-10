***
# rasberry_pi_ATM_project
# 라즈베리파이 atm기 만들기
***

# 라즈베리파이 핀번호
<img width="500" height="765" alt="image" src="https://github.com/user-attachments/assets/e70dbdbe-b357-41f3-91dd-aa7eb62bbf85" />
***

# 키패드 핀번호
* ### col = 21, 20 , 16, 12
* ### row = 19, 13, 6, 5
***

# 키패드 테스트 코드
* * ## *라즈베리파이 파이썬으로*
```
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)

ROWS = [19, 13, 6, 5]
COLS = [21, 20 , 16, 12]

KEYPAD = [
    ['1','2','3','A'],
    ['4','5','6','B'],
    ['7','8','9','C'],
    ['*','0','#','D']
]

PASSWORD = "0718"
input_buffer = ""

# GPIO Setup
for r in ROWS:
    GPIO.setup(r, GPIO.IN, pull_up_down=GPIO.PUD_UP)

for c in COLS:
    GPIO.setup(c, GPIO.OUT)
    GPIO.output(c, GPIO.HIGH)

def get_key():
    for ci, c in enumerate(COLS):
        GPIO.output(c, GPIO.LOW)

        for ri, r in enumerate(ROWS):
            if GPIO.input(r) == GPIO.LOW:
                while GPIO.input(r) == GPIO.LOW:
                    time.sleep(0.01)
                GPIO.output(c, GPIO.HIGH)
                return KEYPAD[ri][ci]

        GPIO.output(c, GPIO.HIGH)

    return None

try:
    print("비밀번호 입력 대기... (# = 확인, * = 지우기)")
    while True:
        key = get_key()

        if key:
            print("입력:", key)

            # 확인 버튼 (#)
            if key == "#":
                if input_buffer == PASSWORD:
                    print("good!")   # 성공
                else:
                    print("wrong!")  # 실패
                input_buffer = ""    # 비밀번호 입력 초기화

            # 지우기 버튼 (*)
            elif key == "*":
                input_buffer = ""
                print("입력 초기화")

            # 일반 숫자/문자 입력
            else:
                input_buffer += key

        time.sleep(0.07)

except KeyboardInterrupt:
    GPIO.cleanup()
    print("종료")
```
***
***
# dc모터 2개, L9110 모터 드라이버 코드, 라즈베리파이 파이썬
```
import RPi.GPIO as GPIO
import time

# 사용 핀 설정
IA1 = 17   # 방향 제어 핀 1
IA2 = 27   # 방향 제어 핀 2

GPIO.setmode(GPIO.BCM)
GPIO.setup(IA1, GPIO.OUT)
GPIO.setup(IA2, GPIO.OUT)

def forward():
    GPIO.output(IA1, GPIO.HIGH)
    GPIO.output(IA2, GPIO.LOW)
    print("정방향 회전")

def backward():
    GPIO.output(IA1, GPIO.LOW)
    GPIO.output(IA2, GPIO.HIGH)
    print("역방향 회전")

def stop():
    GPIO.output(IA1, GPIO.LOW)
    GPIO.output(IA2, GPIO.LOW)
    print("정지")

try:
    while True:
        forward()
        time.sleep(2)

        stop()
        time.sleep(1)

        backward()
        time.sleep(2)

        stop()
        time.sleep(1)

except KeyboardInterrupt:
    print("종료")
    GPIO.cleanup()
```
# 핀
* ### 라즈베리파이 L9110 : GPIO 17	IA1 / GPIO 27	IA2
* ### 라즈베리파이 L9110 : GPIO 17	IA1	모터 방향 제어 1 / GPIO 27	IA2	모터 방향 제어 2 / 5V	VCC	전원 / GND	GND	공통 그라운드
* ##### dc 모터, L9110핀 위로 기준하에 vcc 왼쪽, gnd 오른쪽
