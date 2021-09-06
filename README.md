# Introduction 
Open Card Tracking System (OCTS) is personal hobby to build a tracking system for my card.  

# HW Components 
1. `GSM/GPRS` module (for 2G Internet communcation, API call and ...)
2. `GPS` module to get current location 
3. `STM32F103` as main controller
4. `Li-ion` backup battery when car main battery is offline
5. `Microphone` to listen inside car on demand 
6. `µSD` to keep system log when `GSM` is not available 

# Overview 
1. Current location is sent to remote server (via RESTful API) every 10 minutes.
   - In case of main battery loss this interval may extend to decrease backup battery usage 
   - It's possible to request current location on demand via specific `SMS-CMD` 
3. Initial communication to device is handled via `GSM IMEI` as secret value 
   - System accepts commands via SMS in JSON format with specific feilds 
   - With SMS commands, remote server URL, emergency call number and ... are set 
5. System is powered via main **12V** car supply.
   - `Li-ion` backup battery is kept charged with main supply
   - Main supply voltage is monitored 
   - In case of loss of main supply, backup battery kicks in
   - Loss of main supply is reported to remote server 
7. `µSD` is used when connection to remote server is lost
   - Every thing which must be sent to remote server is kept is `µSD`
   - After connection is availble those logs in `µSD` is sent to remote server too

# SW components 
1. [lwgps](https://github.com/MaJerle/lwgps) for parsing GPS data
2. [lwgsm](https://github.com/MaJerle/lwgsm) to handle SIM800 module 
3. FreeRTOS
4. FatFS

