# Websocket_SD_Logger_Multisensor
Using an SDMicro to log data streaming over a websocket

Hello and welcome to another episode of Karl Fiddles Around!
This time I’m taking what I have built so far (ESP32 based Websocket streaming server/client setup with multiple sensors), and adding an SD card to the client end. This will add in the ability to store and analyze data after a trial is completed. The goal is to get the data to stream to the SD card in .csv format, so we’ll see how close we can get to that goal. 

Starting off, I’m using the Adafruit Micro-SD Breakout Board, so I’ll need to wire that up and select a sample sketch to merge. Because of the high logging speed, I’ll need to choose a low latency logger option. 

Wiring turned out to be a bit of a trick, because of the multiple labels for similar pins in the SPI wiring scheme. After worrying whether I’d cross-wired or shorted the chip a couple times, I got the chip wired correctly, and checked the install with a stock read/write code. 

After that I merged the ESP32_Boot_Client code with a simplified write only SD code from a prior project, and checked the compile. On the first attempt, I didn’t have the directory in the SD filesystem written correctly (missed an “/” lol). So after finding that, the Server successfully wrote to the SD card on the client!...but it overwrote the file every time, so I needed to change the FILE_WRITE to FILE_APPEND and now it works!  Again, sorta. Good news is, I have a new friend who’s going to help me start factor-checking the whole system to see what I’ve got going on under the hood as far as processing timeouts and lag. 

Yet, after that, I had a thought…if we’re post-processing through an ML, we might be able to just add a timestamp to the data chunks that are available, and use the time stamps to allocate the data and train on it regardless of intermittent data loss...a question to ask the boss for sure!

But, just to make sure the formatting is setup for easy charting in Google Sheets, I’m going to edit the header info for the file in the void setup, and remove the data labels in the rows so that the CSV formatter can identify the integer values. 

And, that’s it! Here’s the chart for reference:

The one second interval logging is an issue, and the data isn’t really that clean at all, but it’s there, which means the components will wire together. Yay!

Next up I’ll wire in the pH sensor, just to make sure that won’t crash the system, and then it’s time to go back through the code to optimize. Excelsior!

Oh, and here’s the client side code so far:
