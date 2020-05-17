![alt text](https://i.imgur.com/telFnTB.png)

# ⚡⚡⚡⚡⚡⚡⚡ Ziggy Stardust ⚡⚡⚡⚡⚡⚡⚡ </br> (An NFC bitcoin Lightning Network point of sale device) 

## *"There is life on Mars"*

A project to engage with the bitcoin Lightning Network over RFID/NFC, using an ESP32, RFID-RC522, OLED, Keypad

[![LN Slave Mod](https://i.imgur.com/aSmrQgv.png)](https://www.youtube.com/watch?v=5A0KK0k33cI)

**Wiring diagram**
![alt text](https://i.imgur.com/CtX7M1d.png)


**This project comprises of:**

1.	ESP32 & related Hardware
2.	Ziggy software
3.	Opennode.com account
4.	NFC Card Writer/Reader (initial build using the ESP32)
5.	PoS (final build upon 4. above)


**It is assumed that you have a working Arduino Development Environment for your ESP32.**

If not, please return when you have completed: [ArduinoDevEnvESP32](https://github.com/derrickr/ArduinoDevEnvESP32)

**1.	Hardware**

Here's a list of the hardware required, from Aliexpress.com (cheaper, but slower) & Amazon.co.uk (faster, more expensive):

1.	ESP32 Wireless WiFi Bluetooth Development Board
[AliExpress](https://www.aliexpress.com/wholesale?catId=0&SearchText=ESP32+Wireless+WiFi+Bluetooth+Development+Board) / 
[Amazon](https://www.amazon.co.uk/s?k=ESP32+Wireless+WiFi+Bluetooth+Development+Board)

	<img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/aliexpressESP32.png" width=200>
	
	<img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/amazonESP32.png" width=200>

2.	0.96 inch OLED White Display Module 128X64 SPI 7pin Driver Chip SSD1306 for Arduino Diy Kit
[AliExpress](https://www.aliexpress.com/wholesale?catId=0&SearchText=0.96+inch+OLED+White+Display+Module+128X64) / 
[Amazon](https://www.amazon.co.uk/s?k=0.96+Inch+7+Pin+128+x+64+SPI+OLED)

	<img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/aliexpressOLED.png" width=200>
	
	<img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/amazonOLED.png" width=200>

3.	MFRC522 RFID Module for Arduino SPI Writer / Reader
[AliExpress](https://www.aliexpress.com/wholesale?catId=0&SearchText=MFRC-522+RFID+Module+for+Arduino+SPI) / 
[Amazon](https://www.amazon.co.uk/s?k=MFRC522+RFID+Module+for+Arduino+SPI+Writer+%2F+Reader)

	<img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/aliexpressNFC.png" width=200>
	
	<img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/amazonNFC.png" width=200>

4.	16 key membrane
[AliExpress](https://www.aliexpress.com/wholesale?catId=0&SearchText=16+key+membrane) / 
[Amazon](https://www.amazon.co.uk/s?k=16+key+membrane)

	<img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/aliexpressKeypad.png" width=200>
	
	<img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/amazonKeypad.png" width=200>


**2.	Ziggy software**

This method slightly differs from that used in [Ben Arc's video](https://youtu.be/5A0KK0k33cI) in that all software is put in place ready for the project beforehand:

1.	Download the Ziggy code from [Ben's Github for the project ](https://github.com/arcbtc/Ziggy/archive/master.zip)

2.	Extract the downloaded Ziggy-master folder & save into your home Arduino directory

3.	Navigate to the writedata directory & rename main.ino to writedata.ino

4.	Navigate to the PoS directory & rename main.ino to PoS.ino
	This directory should also include the apicalls.h file


Next we will install all the required libraris:

1.	Open your Arduino IDE

2.	Click on: Sketch -> Include Libraries -> Manage Libraries
	![libraries](https://raw.githubusercontent.com/derrickr/ziggy/master/images/libraries.png)

3.	When loaded type: MFRC522

4.	Select & Install:
	MFRC522 by GithubCommunity
	Arduino RFID Library for MFRC522 (SPI)
	![mfrc522](https://raw.githubusercontent.com/derrickr/ziggy/master/images/mfrc522.png)

5.	Repeat for:

	* WiFi	by Arduino
	* U8g2	(by oliver)
	* Keypad	(by Mark Stnley, Alexander Brevig)
	* ArduinoJson	(by Benoit Blanchon) !Important - please ensure you select version 5.13.5 Newer versions (i.e. > 6+) will not work

	<img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/wifi.png" width=200> <img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/U8g2.png" width=200> <img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/keypad.png" width=200> <img src="https://raw.githubusercontent.com/derrickr/ziggy/master/images/arduinoJson.png" width=200>

6.	If any of the above libraries are unavailable in your environment, please carry out a web search to find the library, download and unzip to your Arduino library location

	e.g.	C:\Users\Alice\Documents\Arduino\libraries


**3.	Opennode.com account**

For this project we will be using [opennode.com](https://opennode.com)

Ben's youtube tutorial states that you should open a 'dev' account. However, the transactions woudn't show on my dev account, even after acquiring testnet bitcoins and I was utlimately advised by Opennode that I actually needed a production account for what I was trying to accomplish. After proceeding through Opennode's KYC to create an account I was then able to proceed.

* Once logged in to Opennode, you should see the 'Developers' option in the left hand menu
* Clicking on that will enable you to then click on the 'Integrations' option

* Now click on 'Add Key' to bring a popup for your 'New API Key'
* Enter a memorable label (e.g. NFC-Withdrawals) and select 'Withdrawals' from the Permissions dropdown
* Now enter your 2FA code and click Generate.
* You should now be presented with your new API Key: e.g. 64ab7f03-c9ef-2c0a-6bf8-73650c182f3a
* Make sure to copy your new key. You won't be able to see it again!
* This is your Customer API Key that will be used as the token on the NFC Card

Next, repeat the above but this time enter a different memorable label (e.g. NFC-Invoice) and select 'Invoice' from the permissions dropdown
* Again, make a note of the new generated API Key
* This is your Merchant API Key that is used to contact opennode to request an invoice and deposit the transaction's funds


**4.	NFC Card Writer**

1.	Navigate to your Ziggy writedata directory

2.	Click on the file 'writedata.ino', which should be associated with, and open the file in, Arduino

3.	Connect the MFRC522 RFID Module to your Arduino, as per the wiring digram
![](https://camo.githubusercontent.com/5aa99f36137b9d716ab9404698126c71302fecd7/68747470733a2f2f692e696d6775722e636f6d2f437458374d31642e706e67)

4.	Ensure the correct Com Port is selected

5.	Open the Arduino Serial Monitor

6.	Upload the code to the ESP32

7.	Place your NFC Card upon the NFC Writer module

8.	Remove the hyphens from your Customer (NFC-Withdrawals) API Key

9.	Paste in to the textarea at the top Serial Monitor, followed by the hash key #, then press enter
	![libraries](https://raw.githubusercontent.com/derrickr/ziggy/master/images/writer.png)

10.	The Serial Monitor should show that your API Key has sucessfully been written to the NFC Card
	![libraries](https://raw.githubusercontent.com/derrickr/ziggy/master/images/success.png)


**5.	PoS**

1.	Navigate to your Ziggy writedata directory

2.	Ensure the 'apicalls.h' file is located within this directory

3.	Click on the file 'PoS.ino', which should be associated with, and open the file in, Arduino.

	Note, the above mentioned apicalls.h should also be present in the Arduino IDE in a separate tab


4.	Enter you WiFi details for wifiSSID and wifiPASS into the code on lines 10 & 11
	![wifiCredentials](https://raw.githubusercontent.com/derrickr/ziggy/master/images/wifiCredentials.png)

5.	Enter your 'Merchant' (NFC-Invoice) API Key on line 17
	![merchantKey](https://raw.githubusercontent.com/derrickr/ziggy/master/images/merchantKey.png)

6.	Connect the Display and keypad to your Arduino, as per the wiring diagram above

7.	Ensure the correct Com Port is selected

8.	Open the Arduino Serial Monitor

9.	Upload the code to the ESP32

10.	Upon completion you may need to press the Boot button on the ESP32

11.	The OLED should then display "connecting..." and assuming your WiFi details are correct and present, it should connect and briefly display: "connected", followed by "Sats:"

12.	Using the keypad enter the amount of Sats you wish to charge, followed by the # key

13.	The OLED should display an elipsis (3 dots) "..." followed by "Tap card"

14.	Place your NFC card upon the NFC Card Reader

15.	After a few seconds the OLED should then display "Paid!"

16.	You should now have a working Point of Sale NFC system using Lightning.
