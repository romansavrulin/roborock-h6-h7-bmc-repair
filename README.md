# What is is
This is a repo of analysis, tools and (some) guides to repair the battery pack of Roborock H6/H7 vacuum cleaner by yourself with a clean `bq40z80`

### The story
I had bad luck getting counterfeit and wrong model batteries for my VC from Ali. I’ve tried to replace the board inside the battery pack, but every time when you disconnect battery from the board the `bq40z80` brings up permanent failure flag and locks up. Because I don’t have an unseal key to reset the PF status, I’ve decided to replace `bq40z80` with new one (that have factory default unseal key). In order to work fine, Gauge should have default settings and pass calibration process. I haven’t done latter, but uploading config brought my VC back to life and I’m totally satisfied with the result.

### Precautions
- Cheap chinese batteries are dangerous! It has poor and risky connections, lacks of fuses, and my battery had power MOSFET issues and got into overheat protection just after 30 seconds of work (see photos)
- Do not short-circuit battery pack directly under any circumstances. You will likely to burn anyting.

### Tools needed
- [Cheap EV2400](https://aliexpress.ru/item/1005003308489798.html?spm=a2g2w.orderdetail.0.0.33e94aa6gQsmRD&sku_id=12000025129027661&_ga=2.229882334.319529504.1768079349-1585508259.1765472482) from ali express works fine
- bqStudio (latest stable from TI works fine)
### Useful notes
- Connection for EV2400 is shown in photos. Connect according to marking of EV2400 and you are good to go
- All IC’s datasheets that are met in battery pack are located in `IC Datasheets` folder
- Gel could be easily removed from PCB if gently heated to approx 100 degrees C. Peel it off with sharp needle or tweezers. Don’t try to solder ICs with gel on. It soaks the solder, making small bubbles of it that disconnects the component from PCB and makes it really hard to solder back.
- Before trying to upload `bq40z80` configuration, halt U3 or bring PRES pin of U2 to active state by disconnecting certain resistor from it. This all could be done If you don’t do that, all attempts may fail and PCB may look completely dead
- Upload only sections that is needed for `bq40z80` to function. Tune later. 
- Do not upload in bulk! Upload by small sections. One parameter set is OK to start.
- Use reset a lot! If something goes wrong or parameter is not applied, try to reset the device first
- Use `Golden.gg.csv` config. Upload in that sequence
	- Set and verify chemistry! The battery is present in chemistry db.
	- Calibration
	- Settings
	- Protections
	- Advanced Charge Algorithm
	- Gas Gauging
	- Power
	- SBS Configuration
	- LED Support
	- Do current/voltage calibration
	- Seal the device - not a data section, but action
- Verify what is written! Retry or study carefully what went wrong if you get certain parameter verification (or validation) error. 
- Sometimes default values in bqStudio are broken and what you read cannot be directly written back. Inspect what is happening, fix, retry.
- If `bq40z80` goes into PF or some werid states, use commands to temporarely disable some checks (flags)
- When really sure that everything works
	- Unseal the device
	- Upload Permanent Fail settings
- You can bring PLS debug header to the side of battery pack to communicate with battery at any time without have to disassemble the VC. It will save you tons of time
- If you try to debug gauge while VC LCD is on, you will likely get communication error due to 2 masters present on the bus. Wait for LCD to go blank, then communicate with `bq40z80`
- It is a good idea to bring reset of U3 to the debug header too, because it can halt and give unrecoverable error on LCD even if `bq40z80` reports OK flags. If that happens try to reset everything in a round. Sometimes error goes away after clearing PF on U2, U3 reset and LCD go off. If you tried everything but error still persist, disconnect/reconnect everything as a last resort.

### Need help?
I know, the info is very brief. I don’t have much time to provide a complete guide. But it highlights some key points that took me a lot of time to overcome. If you need help, feel free to contact me by email or here in issues
