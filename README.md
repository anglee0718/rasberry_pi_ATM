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
