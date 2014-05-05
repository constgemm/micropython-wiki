Here's a sample little tight counter loop, running for 10 seconds on 3 different setups:

**MicroPython on Teensy 3.1: (96Mhz ARM)**
Damien gave me a hint, and told me to put the performance test inside a function, rather than global. It almost doubles the execution speed, which used to run 396,505 times. With another hint, its up over a million now:

    def performanceTest():
        millis = pyb.millis
        endTime = millis() + 10000
        count = 0
        while millis() < endTime:
            count += 1
        print("Count: ", count)
    performanceTest()

**Count:  1,098,681**

On pyboard it's 2,890,723

***

**C++ on Teensy 3.1: (96Mhz ARM)**

    #include <Arduino.h>  
    void setup() {  
        Serial1.begin(115200);  
        uint32_t endTime = millis() + 10000;  
        uint32_t count = 0;  
        while (millis() < endTime)  
            count++;  
        Serial1.print("Count: ");  
        Serial1.println(count);  
    }  
    void loop() {  
    }  

**Count: 95,835,923**

***

**C++ on Arduino Pro Mini: (16 MHz Atmega328)**

    #include <Arduino.h>  
    void setup() {  
        Serial.begin(115200);  
        uint32_t endTime = millis() + 10000;  
        uint32_t count = 0;  
        while (millis() < endTime)  
            count++;  
        Serial.print("Count: ");  
        Serial.println(count);  
    }  
    void loop() {  
    }  

**Count: 4,970,227**

***

Some more looping examples and numbers: https://www.kickstarter.com/projects/214379695/micro-python-python-for-microcontrollers/posts/664832
