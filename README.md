# Counter-on-7seg-display-using-buttons-.-micropython-esp32
Counter on 7seg display , using buttons . : micropython : esp32
wokwi link : https://wokwi.com/projects/475123565415454721
from machine import Pin
from time import sleep

segments = [
    Pin(23, Pin.OUT),
    Pin(22, Pin.OUT),
    Pin(21, Pin.OUT),
    Pin(19, Pin.OUT),
    Pin(18, Pin.OUT),
    Pin(5, Pin.OUT),
    Pin(4, Pin.OUT)
]

up_switch = Pin(32, Pin.IN, Pin.PULL_UP)
down_switch = Pin(33, Pin.IN, Pin.PULL_UP)

numbers = [
    [1,1,1,1,1,1,0],  # 0
    [0,1,1,0,0,0,0],  # 1
    [1,1,0,1,1,0,1],  # 2
    [1,1,1,1,0,0,1],  # 3
    [0,1,1,0,0,1,1],  # 4
    [1,0,1,1,0,1,1],  # 5
    [1,0,1,1,1,1,1],  # 6
    [1,1,1,0,0,0,0],  # 7
    [1,1,1,1,1,1,1],  # 8
    [1,1,1,1,0,1,1]   # 9
]

counter = 0

def display_number(num):
    pattern = numbers[num]

    for i in range(7):
        segments[i].value(pattern[i])


display_number(counter)

while True:
    
    if up_switch.value() == 0:
        counter = counter + 1
        if counter > 9:
            counter = 0
        display_number(counter)
        sleep(0.2)
        while up_switch.value() == 0:
            sleep(0.01)
            
    if down_switch.value() == 0:
        counter = counter - 1
        if counter < 0:
            counter = 9
        display_number(counter)
        sleep(0.2)
        while down_switch.value() == 0:
            sleep(0.01)
    sleep(0.01)
