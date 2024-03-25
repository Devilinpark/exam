# 1 program
```ruby
# 1. Read your name and print Hello message with name
name = input("Enter your name: ")
print("Hello, " + name + "!")

# 2. Read two numbers and print their sum, difference, product and division.
num1 = float(input("Enter the first number: "))
num2 = float(input("Enter the second number: "))
sum_result = num1 + num2
difference_result = num1 - num2
product_result = num1 * num2
try:
    division_result = num1 / num2
except ZeroDivisionError:
    division_result = "Cannot divide by zero"
print("Sum:", sum_result)
print("Difference:", difference_result)
print("Product:", product_result)
print("Division:", division_result)

# 3. Read two numbers and print their sum, difference, product and division.
num1 = float(input("Enter the first number: "))
num2 = float(input("Enter the second number: "))
print("Sum:", num1 + num2)
print("Difference:", num1 - num2)
print("Product:", num1 * num2)
if num2 != 0:
    print("Division:", num1 / num2)
else:
    print("Cannot divide by zero")

# 4. Word and character count of a given string
input_string = input("Enter a string: ")
word_count = len(input_string.split())
char_count = len(input_string)
print("Word Count:", word_count)
print("Character Count:", char_count)

# 5. Area of a given shape (rectangle, triangle and circle) reading shape and appropriate values from standard input
import math
shape = input("Enter the shape (rectangle, triangle, or circle): ")
if shape == "rectangle":
    length = float(input("Enter the length: "))
    width = float(input("Enter the width: "))
    area = length * width
    print("Area of the rectangle:", area)
elif shape == "triangle":
    base = float(input("Enter the base: "))
    height = float(input("Enter the height: "))
    area = 0.5 * base * height
    print("Area of the triangle:", area)
elif shape == "circle":
    radius = float(input("Enter the radius: "))
    area = math.pi * radius ** 2
    print("Area of the circle:", area)
else:
    print("Invalid shape entered.")

# 6. Print a name „n‟ times, where name and n are read from standard input, using for and while loops.
name = input("Enter a name: ")
n = int(input("Enter the number of times to print the name: "))
print("Using for loop:")
for _ in range(n):
    print(name)
print("\nUsing while loop:")
count = 0
while count < n:
    print(name)
    count += 1

# 7. Handle Divided by Zero Exception.
numerator = float(input("Enter the numerator: "))
denominator = float(input("Enter the denominator: "))
try:
    result = numerator / denominator
    print("Result:", result)
except ZeroDivisionError as e:
    print(f"Error: {e}. Cannot divide by zero.")
except Exception as e:
    print(f"An unexpected error occurred: {e}")

# 8. Print current time for 10 times with an interval of 10 seconds.
import time
from datetime import datetime
for _ in range(10):
    current_time = datetime.now().strftime("%H:%M:%S")
    print("Current Time:", current_time)
    time.sleep(10)

```
# 2 program
```yard
from machine import Pin
import time

# Define LED pins and button pins
led1 = Pin(18, Pin.OUT)  
led0 = Pin(2, Pin.OUT)   
button1 = Pin(35, Pin.IN)  
button0 = Pin(0, Pin.IN)  

while True:  
    if button1.value(): 
        led1.value(1)    
    else:
        led1.value(0)     

    if not button0.value():  
        led0.value(1)        
    else:
        led0.value(0)        

    time.sleep(0.1) 

#1. LED anode (+) end – GPIO 18 (ESP32)
#2. LED anode (+) end – GND (Ground- ESP32)
#Execution:
#1. Tools – Serial – COM3
#2. Upload Prog.py file
#3. Press
#a. Default button (35) – external LED ‘ON’ b. BOOT button – internal LED ‘ON
```
# 3 program
```yard
from machine import Pin
from time import sleep
led = Pin(18, Pin.OUT)
try:
    with open('delay.txt', 'r') as file:
        line = file.readline()
except FileNotFoundError:
    print('No such file.')
else:
    delay = line.split()
    on_time = float(delay[0])
    off_time = float(delay[1])
    print('ON:', on_time)
    print('OFF:', off_time)

    while True:
        led.value(1) 
        sleep(on_time)  
        led.value(0) 
        sleep(off_time)

#Connection steps:
#1. LED anode (+) end – GPIO 18 (ESP32)
#2. LED anode (+) end – GND (Ground- ESP32)
#Execution:
#1. Tools – Serial – COM3
#2. Upload delay.txt and prog.py file  
```
# 4 program

```yard
from machine import Pin
from time import sleep
relay = Pin(13, Pin.OUT)
button = Pin(35, Pin.IN)
while True:
    if button.value():
        relay.value(1)  
    else:
        relay.value(0) 

#Connection steps:
#1. (Bread board) LED anode (+) end – NO (ESP32)
#2. (ESP32) 3.3V – COM (ESP32)
#3. (Bread board) LED cathode (-) end – GND (Ground- ESP32)
#Execution:
#1. Tools –> Serial –> COM3
#2. Upload Prog.py file
```
# 5 program
```yard
#include <WebServer.h>
#include <WiFi.h>
#include <esp32cam.h>

const char* WIFI_SSID = "JSSATEB";
const char* WIFI_PASS = Nil;

WebServer server(80);

static auto loRes = esp32cam::Resolution::find(320, 240);
static auto midRes = esp32cam::Resolution::find(350, 530);
static auto hiRes = esp32cam::Resolution::find(800, 600);

void serveJpg()
{
    auto frame = esp32cam::capture();
    if (frame == nullptr) {
        Serial.println("CAPTURE FAIL");
        server.send(503, "", "");
        return;
    }
    Serial.printf("CAPTURE OK %dx%d %db\n", frame->getWidth(), frame->getHeight(), static_cast<int>(frame->size()));
    server.setContentLength(frame->size());
    server.send(200, "image/jpeg");
    WiFiClient client = server.client();
    frame->writeTo(client);
}

void handleJpgLo()
{
    if (!esp32cam::Camera.changeResolution(loRes)) {
        Serial.println("SET-LO-RES FAIL");
    }
    serveJpg();
}

void handleJpgHi()
{
    if (!esp32cam::Camera.changeResolution(hiRes)) {
        Serial.println("SET-HI-RES FAIL");
    }
    serveJpg();
}

void handleJpgMid()
{
    if (!esp32cam::Camera.changeResolution(midRes)) {
        Serial.println("SET-MID-RES FAIL");
    }
    serveJpg();
}

void setup()
{
    Serial.begin(115200);
    Serial.println();

    {
        using namespace esp32cam;
        Config cfg;
        cfg.setPins(pins::AiThinker);
        cfg.setResolution(hiRes);
        cfg.setBufferCount(2);
        cfg.setJpeg(80);
        bool ok = Camera.begin(cfg);
        Serial.println(ok ? "CAMERA OK" : "CAMERA FAIL");
    }

    WiFi.persistent(false);
    WiFi.mode(WIFI_STA);
    WiFi.begin(WIFI_SSID, WIFI_PASS);
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
    }

    Serial.print("http://");
    Serial.println(WiFi.localIP());
    Serial.println(" /cam-lo.jpg");
    Serial.println(" /cam-hi.jpg");
    Serial.println(" /cam-mid.jpg");

    server.on("/cam-lo.jpg", handleJpgLo);
    server.on("/cam-hi.jpg", handleJpgHi);
    server.on("/cam-mid.jpg", handleJpgMid);
    server.begin();
}

void loop()
{
    server.handleClient();
}

```
# 6 program
```yard
def web_page():
    if relay.value() == 1:
        relay_state = ''
    else:
        relay_state = 'checked'
    
    html = """<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        body {
            font-family: Arial;
            text-align: center;
            margin: 0px auto;
            padding-top: 30px;
        }
        .switch {
            position: relative;
            display: inline-block;
            width: 120px;
            height: 68px;
        }
        .switch input {
            display: none;
        }
        .slider {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            border-radius: 34px;
        }
        .slider:before {
            position: absolute;
            content: "";
            height: 52px;
            width: 52px;
            left: 8px;
            bottom: 8px;
            background-color: #fff;
            transition: .4s;
            border-radius: 68px;
        }
        input:checked + .slider {
            background-color: #2196F3;
        }
        input:checked + .slider:before {
            transform: translateX(52px);
        }
    </style>
    <script>
        function toggleCheckbox(element) {
            var xhr = new XMLHttpRequest();
            if (element.checked) {
                xhr.open("GET", "/?relay=on", true);
            } else {
                xhr.open("GET", "/?relay=off", true);
            }
            xhr.send();
        }
    </script>
</head>
<body>
    <h1>ESP Relay Web Server</h1>
    <label class="switch">
        <input type="checkbox" onchange="toggleCheckbox(this)" %s>
        <span class="slider"></span>
    </label>
</body>
</html>""" % (relay_state)
    
    return html

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(('', 80))
s.listen(5)

while True:
    try:
        if gc.mem_free() < 102000:
            gc.collect()
        
        conn, addr = s.accept()
        conn.settimeout(3.0)
        print('Got a connection from %s' % str(addr))
        request = conn.recv(1024)
        conn.settimeout(None)
        request = str(request)
        print('Content = %s' % request)
        relay_on = request.find('/?relay=on')
        relay_off = request.find('/?relay=off')
        
        if relay_on == 6:
            print('RELAY OFF')
            relay.value(0)
        if relay_off == 6:
            print('RELAY ON')
            relay.value(1)
        
        response = web_page()
        conn.send('HTTP/1.1 200 OK\n')
        conn.send('Content-Type: text/html\n')
        conn.send('Connection: close\n\n')
        conn.sendall(response)
        conn.close()
    except OSError as e:
        conn.close()
        print('Connection closed')

#Connection steps:
#1. (Bread board) LED anode (+) end – NO (ESP32)
#2. (ESP32) 3.3V – COM (ESP32)
#3. (Bread board) LED cathode (-) end – GND (Ground- ESP32)
#Execution:
#1. Tools – Serial – COM3
#2. Device – click boot.py file – give ssid, password – upload boot.py
#3. Upload Prog.py file (your code)
#4. IP address displayed
```
# 7 program
```yard
import umail
import network
from machine import Pin
from time import sleep

motion = False
ssid = 'JSSATEB'
password = None
sender_email = 'iot22mcal37@gmail.com'
sender_name = 'iotlab'  # sender name
sender_app_password = 'mrub pmhi dtdj xozy'
recipient_email = 'Enter Your/any Email Address'
email_subject = 'intruder system'

def handle_interrupt(pin):
    global motion
    motion = True
    global interrupt_pin
    interrupt_pin = pin
    buzzer = Pin(14, Pin.OUT)
    pir = Pin(0, Pin.IN)  # boot (push) button
    pir.irq(trigger=Pin.IRQ_FALLING, handler=handle_interrupt)

def connect_wifi(ssid, password):
    station = network.WLAN(network.STA_IF)
    station.active(True)
    station.connect(ssid, password)
    while station.isconnected() == False:
        pass
    print('Connection successful')
    print(station.ifconfig())

connect_wifi(ssid, password)

while True:
    print(pir.value())
    sleep(1.0)
    if motion:
        print('Motion detected! Interrupt caused by:', interrupt_pin)
        buzzer.value(1)
        sleep(1)
        buzzer.value(0)
        print('Motion stopped!')
        motion = False
        smtp = umail.SMTP('smtp.gmail.com', 465, ssl=True)  # Gmail's SSL port
        smtp.login(sender_email, sender_app_password)
        smtp.to(recipient_email)
        smtp.write("From:" + sender_name + "<"+ sender_email+">\n")
        smtp.write("Subject:" + email_subject + "\n")
        smtp.write("Message from ESP32- Motion detected! ")
        smtp.send()
        smtp.quit()

#Connection steps:
#1. (ESP32) GND – GND (PIR)
#2. (ESP32) GPIO 18 or 26 – output (PIR)
#3. (ESP32) 5V VCC – 5V VCC (PIR)
#Execution:
#Note: Switch - SW1 (GPIO26) - DIP 3 – ON
#1. Tools – Serial – COM3
#2. Upload Umail.py and Prog.py file (your code)
```
# 9 program
```yard
from machine import Pin, ADC
from time import sleep

smoke = ADC(Pin(26))
smoke.atten(ADC.ATTN_11DB)
buzzer = Pin(14, Pin.OUT)

while True:
    smoke_value = smoke.read()
    print(smoke_value)
    if smoke_value > 1500:
        buzzer.value(1)
        sleep(0.5)
    else:
        buzzer.value(0)
        sleep(0.5)

#Connection steps:
#1. (MQ-2) VCC – 5V VCC (ESP32)
#2. (MQ-2) AO – GPIO 26(ESP32)
#3. (MQ-2) GND – GND (ESP32)
#Execution:
#1. Tools – Serial – COM3
#2. Upload Prog.py file
```
