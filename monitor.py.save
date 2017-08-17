#! /usr/bin/env/ python

import RPi.GPIO as GPIO
import time
import subprocess
import os
import threading
#from multiprocessing import Process
import Adafruit_GPIO.SPI as SPI
import Adafruit_MCP3008
import PiFm

GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)
GPIO.cleanup()

### SPI Config
CLK = 18
MISO = 23
MOSI = 24
CS = 25
MCP = Adafruit_MCP3008.MCP3008(clk=CLK, cs=CS, miso=MISO, mosi=MOSI)

## GPIO Pins
DEPLOY = 20
GPIO.setup(DEPLOY, GPIO.OUT)

## Broadcast settings
STATION = "103.3"
# Allow time for start up
time.sleep(5)


def eSpeak(s):
    p = subprocess.Popen("espeak '{}'".format(s), shell=True)
    p.wait()

# Start up Status
value = MCP.read_adc(7)
if value > 100:
    eSpeak("please turn the lights off before we continue, I will wait 10 seconds to allow for this")
    os.system("espeak 'please turn the lights off before we continue, I will wait 10 seconds to allow for this'")
    time.sleep(10)

# Instructions
os.system("espeak 'when you initiate my LEDS with the C2 button on the back of the controller I will initiate broadcast and deploy leaflets'")
eSpeak("when you initiate my LEDS with the C2 button on the back of the controller "
       "I will initiate broadcast and deploy leaflets")


# Handler
if __name__ == "__main__":

    broadcast_process = None
    
    while True:
        value = 151 #MCP.read_adc(7)

        if value > 150:
            GPIO.output(DEPLOY, True)

            eSpeak("initiating broadcast on {} FM".format(STATION))

            if not broadcast_process:
                print(">>> Starting broadcast...")
                broadcast_process = subprocess.Popen("sudo ./pifm sound.wav {}".format(STATION),stdout=subprocess.PIPE, shell=True)
                broadcast_process = subprocess.Popen("sudo omxplayer sound.wav", stdout=subprocess.PIPE, shell=True)
            else:
                proc = broadcast_process.poll()
                if proc:  # process died
                    print(">>> Broadcast died..restarting broadcast...")
                    broadcast_process = subprocess.Popen("sudo ./pifm sound.wav {}".format(STATION), stdout=subprocess.PIPE, shell=True)

                    #broadcast_process = subprocess.Popen("sudo omxplayer sound.wav", stdout=subprocess.PIPE, shell=True)

        else:
            GPIO.output(DEPLOY, False)

        time.sleep(1)
