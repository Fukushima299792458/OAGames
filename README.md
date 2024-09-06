# Wireless Security Alarm
This is a Wireless motion-triggered alarm that uses a node.js webserver to serve the webpage with information from an ESP32 and its various sensors. The outputs of the ESP32 can also be controlled with the webpage. This page will focus on how to use the system as those who wish to make modifications can largely see the outputs and read the code to get an in-depth understanding that is outside the scope of this page. 
## ESP32 Hardware
![IMG_4801](https://github.com/Fukushima299792458/IoT-Weather/assets/132644178/8d4f013f-bea6-4465-b910-f30d38808d28)
# The Button!  
The Button in the above image serves only the purpose of appearing functional and inducing an involuntary press by any trespassers unlucky enough to meet its gaze and bringing them necessarily within the detection radius.
A number of studies and social experiments investigate the effectiveness of this method below and also serve as examples of how interested we are as a species in buttons. Undeniably this bait works often enough to make a noticeable difference to this device's effectiveness without the button itself needing to be a sensor input.
- [Love Buttons](https://techcrunch.com/2018/03/20/researchers-find-the-best-way-to-press-a-button/)
- [The Great Button!](https://www.vox.com/2015/4/10/8383165/reddit-button-explained)
- [Buttons](https://www.talkbass.com/threads/the-psychology-of-button-pushing-and-effective-responses-to-it.1319409/#:~:text=My%20opinion%20on%20this%20is,does%20the%20same%20to%20them.) 


## Sensors
### IR Motion Sensor
The Infrared motion sensor detects motion by detecting a change in the infrared signature of its detection field i.e. it sees warm objects using IR and then detects when they change position.  
![IMG_4801a](https://github.com/Fukushima299792458/IoT-Weather/assets/132644178/a8ae80d4-ae6d-4876-ac03-aa26b792de73)
(White dome in the above image, with the three power pins facing you (opposite of above image) and the white dome facing upwards, the leftmost pin is to be `gnd`, the middle is your data pin (attached to `17` in the example. `digitalRead(17)` returning `true` or `false` if there is or is not movement respectively), and the rightmost pin is to be `Vin` (5v))



### Ultrasonic Sensor
The Ultrasonic distance sensor sends out a 10µs pulse (from the `trig` pin (output pin `13` in the example)) and measures the time the pulse takes to bounce back (to the `echo` pin (input pin `12` in the example)). Using the speed of sound and the time (in ten-thousandths of a second as far as I can tell) the sound travelled we can calculate the distance travelled for the round trip and divide by two to get the approximate distance to the object.  
The pin labelled `trig` triggers the ultrasonic pulse (output pin `13` in the example), the `echo` pin receives the return pulse (input pin `12` in the example), and the `Vin` and `gnd` pins need to be plugged into the `Vin`(5v) pin and `gnd` pins respectively.  
  
### Average Distance Change
Every ten seconds the webpage requests an update and compares the new average to the old average distance. If the change in average distance is more than 5cm and there is no faster or warmer movement detected by the other functions then it changes the title and heading of the page to "Slow Movement Detected".  
  
### Fast Distance Change
Similar to the [IR motion sensor](https://github.com/Fukushima299792458/IoT-Weather/edit/main/README.md#ir-motion-sensor) if the time drops more than 100 (approximately 1.715 cm) below the average then, when every 10 seconds the webpage requests an update, it returns `true` and if there is no warm movement detected by the [IR motion sensor](https://github.com/Fukushima299792458/IoT-Weather/edit/main/README.md#ir-motion-sensor) it sets the title and heading  
  
![image](https://github.com/Fukushima299792458/IoT-Weather/assets/132644178/fa0f9c99-21d5-441e-938c-dc867a74a01c)
Note: 1 microsecond equivalent to 0.343 mm of sound travel time! That's 343 µm!! A total precision of ±686µm between 10s averages!!! However, < 1cm between individual measurements...


## UI
![image](https://github.com/Fukushima299792458/IoT-Weather/assets/132644178/7fd9b3ad-588a-45cd-a63b-b281a4c887b7)
Sensors display sensor data every 10 seconds as specified in the [sensors](https://github.com/Fukushima299792458/IoT-Weather/edit/main/README.md#sensors) section
outputs can be set to automatically change with the Virtual Alarm which can be in turn set to automatically change according to the sensor inputs. Alternatively, they can be controlled manually as in the [outputs](https://github.com/Fukushima299792458/IoT-Weather/edit/main/README.md#outputs) section. 

## outputs
All outputs or outputs can be set to automatic to be controlled by the Virtual alarm function or manually controlled individually including the virtual alarm which can be automatically triggered by the sensor inputs or manually. 

### Buzzer(s)
When activated automatically the buzzers produce a loud sound slowly rising in pitch up to 5kHz before dropping back down to the starting value of 50Hz. When activated manually the pitch of all three buzzers can be controlled.  
![image](https://github.com/Fukushima299792458/IoT-Weather/assets/132644178/2a1c2ed6-2c51-4141-b9e7-1fe3f17d24ba)  
The buzzer _module_ (above) needs the `Vin` pin to be attached to `Vin` (5v), the `gnd` pin attached to `gnd`, and the `i/o` pin is connected to pin `22`.
The buzzers need their negative (or not positive) pin to be connected to `gnd` with their positive pins connected to the control pins (pins `22` and `23` in the example).

### LEDs
#### Inbuilt LED
This LED is automatically set to max brightness when the Virtual alarm is activatedThe inbuilt LED is built into the circuit board and therefore requires no wiring. The blue LED is controlled with pin `2` so pin `2` shouldn't be used for any other component that you want to run independently.  
  
#### RGB LEDs 1 and 2
These LEDs are similar to the inbuilt LED, but RGB. They turn green on startup, and automatically respond to the Virtual alarm by becoming red, and when the virtual alarm is inactive but fast-cold movement is detected they turn blue.  
![RGB1](https://github.com/Fukushima299792458/IoT-Weather/assets/132644178/47aaa4de-4409-497e-836f-81712e34a56f)  
With the longest pin on the LEDs closest to your left (as above), the pins are Red (pins `32` and `33` for RGB1 and RGB2 respectively in the example), Ground (the longest pin)(needs to be connected to `gnd`), Green (pins `15` and `25` for RGB1 and RGB2 respectively in the example), and Blue (pins `4` and `26` for RGB1 and RGB2 respectively in the example).

### Virtual alarm
The virtual alarm is not a real alarm but a collection of functions that trigger the automatic alarm outputs. It can be triggered by sensors or be triggered manually. 


## an IoT project

![stealing](CoreStuff/public/assets/stealing.jpg)

### Development Roadmap

- [x] Setup repos - GitHub
- [x] setup ESP32 - Arduino IDE
- [x] Install dependencies - aREST.h and DHT.h
- [x] Test ESP32 - get json data via wifi
- [x] Initialize Node.js environment
- [x] setup server.js with express
- [x] set index route and views
- [x] link ESP32 to Node app with fetch

### Initializing a Node.js Environment

1. Ensure that Node.js and NPM are installed on your system:
    > node --version

    > npm --version

2. In terminal, inside the project directory:
    > npm init -y

3. This will add a package.json file

4. Install dependencies
    > npm install express ejs express-ejs-layouts

5. Install development dependencies
    > npm install --save-dev dotenv nodemon


### Initializing a Node.js Environment

1. Ensure that Node.js and NPM are installed on your system:
    > node --version

    > npm --version

2. In terminal, inside the project directory:
    > npm init -y

3. This will add a package.json file

4. Install dependencies
    > npm install express ejs express-ejs-layouts

5. Install development dependencies
    > npm install --save-dev dotenv nodemon

### Worklog and Commits

Date | Commit Message | Version
:-----|:----------------:|:--------:
31.08.23 | first commit | 0.0.0
31.08.23 | node env | 0.0.1
31.08.23 | update readme | 0.0.2
31.08.23 | readme | 0.0.3
31.08.23 | static ips | 0.0.4
04.09.23 | rm static ip | 0.0.5
07.09.23 | node server | 0.0.6
07.09.23 | index route | 0.0.7
07.09.23 | asset | 0.0.8
07.09.23 | fetching | 0.0.9
08.09.23 | pre-merge | 0.0.10
08.09.23 | merge | 0.0.11
13.10.23 | Update | 0.1.0
18.10.23 | Update README.md | 0.1.1
18.10.23 | Update | 0.1.2





### References
- [Web Page Layouts](https://www.youtube.com/watch?v=3C_22eBWpjg)
- [JS Native Fetch](https://www.youtube.com/watch?v=MBqS1kYzwTc)
- [Square Root in c++](https://www.scaler.com/topics/sqrt-in-cpp/)
- [WiFi Functionality](https://randomnerdtutorials.com/esp32-useful-wi-fi-functions-arduino/)
- [DOM Image Object](https://www.w3schools.com/jsref/dom_obj_image.asp)
- [Different Types of Buzzers](https://emariete.com/en/buzzer-active-or-passive-buzzer-for-arduino-esp8266-nodemcu-esp32-etc/)
- [Pinout ESP32](https://circuits4you.com/wp-content/uploads/2018/12/ESP32-Pinout.jpg)
- [Love Buttons](https://techcrunch.com/2018/03/20/researchers-find-the-best-way-to-press-a-button/)
- [The Great Button!](https://www.vox.com/2015/4/10/8383165/reddit-button-explained)
- [Buttons](https://www.talkbass.com/threads/the-psychology-of-button-pushing-and-effective-responses-to-it.1319409/#:~:text=My%20opinion%20on%20this%20is,does%20the%20same%20to%20them.)
- [UltraSonic Sensor](https://esp32io.com/tutorials/esp32-ultrasonic-sensor#google_vignette)
- [Piezo Buzzer](https://esp32io.com/tutorials/esp32-piezo-buzzer)
- [Buzzer](https://esp32io.com/tutorials/esp32-buzzer)
- [Motion Sensor](https://esp32io.com/tutorials/esp32-motion-sensor)


