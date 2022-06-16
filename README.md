# Software-Defined-Radio-Reciever

This project was taken on with the goal to create a Software Defined Radio (SDR) in coolaboration with my partner [Christian Williams](https://github.com/cwill713). This project was completed for my Engineering Electronics II class and given to us by our professor, [Dr. Rob Frohne](https://github.com/frohro).

## Design Specifications/Objectives

These are the design specifications and objectives that my partner and I decided on. Origianlly, we decided to build a commercial FM radio, but after discusstion with Dr. Frohne, we reconsidered. The final specifications we decided on are as follows:

#### Objectives

- $25 budget
- Less than -107 dBm (1uV) minimum discernible signal
- *Stretch Goal* -127 dBm (.1uV) minimum discernible signal
- Good image rejection
- Low noise figure

#### Specs

- Frequency Range: 7MHz - 18MHz
- Radio Type: Shortwave
- One Butterworth Bandpass Filter
- One Tayloe Mixer
- One Bessel Lowpass Filter
- One Power Supply Noise Filter

## Overview

Our (SDR) receiver connects to an antenna (mounted ontop of the Engineering Building) and covers the filtering, mixing, and amplifying of the desired I and Q signals. Our I and Q signals are then passed through an external sound card and software handles the demodulation of the signal into audio. The mixing in our SDR is handled using an Arduino Nano running code provided to us by Dr. Rob Frohne which can be found in the [Arduino file](https://github.com/JoshuaMularczyk/Software_Defined_Radio_Receiver/blob/main/Arduino/New_Quisk_Nano/New_Quisk_Nano.ino). The Arduino interfaces with Quisk, an SDR that controls our receiver through the Arduino. Using Quisk, the radio can be tuned to a certain frequency by adjusting the speed of the local oscillator used in the mixing process.

## Theory

<img width="620" alt="blockdia" src="https://user-images.githubusercontent.com/103919092/172453440-d2bb1faf-6f6f-4016-b2f9-7ac1197f9765.PNG">

The block diagram above shows the basic components of our SDR Receiver. The signal is received by an antenna and is passed through the bandpass filter which attenuates signals that are above or below our frequency range of 7-18 MHz. These signals are then passed through the Tayloe Quadrature Product Detector. This generates the I and Q signals for demodulation. Once the signals have been split they pass through the low noise filter which cuts off anything above 100 KHz, creating a smooth output signal. This signal is then send into an external soundcard which demodulates teh signal before it is read by Quisk software. Quisk interfaces to the Arduino Nana and allows for tuning of the desired frequency.

## Rev 6 Schematic

<img width="857" alt="completeschematic" src="https://user-images.githubusercontent.com/103919092/172456078-4ce87fb0-aaee-48cd-a33c-bca96f2128b5.PNG">

Christian and I built and tested our SDR based on the 5th Rev of this project. These schematics can be found in the Rev 5 file. The follow schematics are our most updated designs. These were created after finding multiple errors while constructing our pcb. The Rev 6 schematics can be downloaded [here](https://github.com/JoshuaMularczyk/Software_Defined_Radio_Receiver/tree/main/Schematics/SDRrecV6). These schematics were all designed in [KiCad](https://www.kicad.org/) and will be broken down into seperate pieces below.

### BandPass Filter

![BandPass Filter and Voltage Divider](https://user-images.githubusercontent.com/103695977/172123798-0871c234-771f-46bd-9502-651a71eb86e6.jpg)

We built this bandpass filter using the [LC Filter Design Tool](https://rf-tools.com/lc-filter/.). This is a 3rd-Order Conventional, Series First, Butterworth Filter that will only allow frequencies between 7MHz and 18MHz to pass through it. It has input and output impedances of 50 ohms.

### Mixer and Lowpass Filter

![Tayloe Mixer](https://user-images.githubusercontent.com/103695977/171759381-e49fd43a-300a-4075-a9eb-c46a4cfd6736.jpg)

The mixer that Christian and I decided to use was  Tayloe Mixer we referenced this [paper](http://www.norcalqrp.org/files/Tayloe_mixer_x3a.pdf) by Dan Tayloe when designing our mixer. The demultiplexer that we used was the [SN74CBT](http://www.ti.com/lit/gpn/sn74cbt3253) which converts the RF signal into four baseband signals. The Tayloe mixer uses switches to combine the signals that are 180 degrees out of phase with differential amplifiers so that signals above and below the oscilator frequecy can be detected.

Our Lowpass filter was designed using the [TI Filter Design Tool](). It was made using two [LT6231](https://www.analog.com/media/en/technical-documentation/data-sheets/623012fc.pdf) low noise op amps and is designed to block all frequencies greater that 100kHz.

### Voltage Smoother

![Voltage Smoother](https://user-images.githubusercontent.com/103695977/171760121-b3ce913d-ab7f-4082-ad9f-a03cbcf7cb58.jpg)

Our Voltage Smoother with take the 5V Vcc and remove as much noise as possible using a very large capacitace. Our schematic here shows the C3 having a value of 8200uF, however we ended up using a 1F capacitor that we already had in the lab. This takes the voltage that comes in from the computer and outputs a smoother 5V for the SDR to use.

### Arduino and Si5351

![Arduino and Si5351](https://user-images.githubusercontent.com/103695977/172481568-51654324-0069-432d-a443-24010c4640cb.jpg)

Our [Si5351](https://www.silabs.com/documents/public/data-sheets/Si5351-B.pdf) programmable oscillator allows us to input two clock signals that are 90 degrees out of phase into S0 and S1 of the SN74CBT3253. The Arduino Nano controls the frequency that is put out by the Si5351 for tuning. For our testing we had the [Aduino](http://www.mouser.com/pdfdocs/Gravitech_Arduino_Nano3_0.pdf) set the frequency to 14,190,000 Hz to match what was being output by a signal generator.

## Rev 5 Simulations

We conducted all of our simulations on [LTSpice](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html). My partner, Christian Williams, conducted the simulations of both filters and the Tayloe Mixer.

### Bandpass Filter

![Bandpass Filter Simulation Graph](https://user-images.githubusercontent.com/103695977/172128945-2436fb08-f1e6-4906-a7a6-28c304c560f3.jpg)
![Bandpass Filter Simulation](https://user-images.githubusercontent.com/103695977/172128985-f59a6ec8-1b1a-4b8a-a930-dfc2ace7611d.jpg)

Our Bandpass Filter was designed to allow a frequency range of 7-18MHz to pass through. As seen in the simulation above, curser 1 and cursor 2 mark the two freqency cutoff points. Inbetween them is the peak of our curve which is just where we want it.

### Lowpass Filter

![LowPass Filter Simulation](https://user-images.githubusercontent.com/103695977/172126680-7654ed1c-1729-4d28-b0ac-22c0453f8cd5.jpg)
![LowPass Filter Schematic](https://user-images.githubusercontent.com/103695977/172126698-47ae7323-1dd5-43b3-99ac-888cf9888982.jpg)

### Complete SDR

![Complete Simulation Graph](https://user-images.githubusercontent.com/103695977/172208557-2a06cd88-0c1e-46c1-88f6-4b392ef950bf.jpg)
![Complete SDR Simulation](https://user-images.githubusercontent.com/103695977/172208710-ec5d7ed2-cf76-4863-a852-ea123d72f428.jpg)

We designed the tayloe mixer using four voltage controlled switches, and four voltage pulses that control them. For our op amps, we added our LT6231, these are already Linear Technology componenets so they dont take long to simulate in LTSpice. The LT6231's had a much lower noise profile than the LM4578's we selected for our second LPF(which we ended up removing in Rev 6), so we knew that the first LPF would take care of mostly everything and the second would just perfect the output. In the simulation window above you can see the baseband signals and the I and Q outputs. They're exactly as expected, about 90 degrees out of phase.

## PCB Design

<img width="513" alt="rev6pcb" src="https://user-images.githubusercontent.com/103919092/172272703-23443340-6fdf-4e17-bb3b-1bf6cff80102.PNG">


## Rev 5 Testing and Fixed Issues

When our Rev 5 boards arrived we realized that there was no ground plane. There had been a mixup with the gerber files and no ground plane was present. To fix this, I went ahead and connected any ground connections that relied on the ground plane with external wires. This can be seen in the [Rev 5 photos on the wiki](https://github.com/JoshuaMularczyk/Software_Defined_Radio_Receiver/wiki/Rev-5-Photos). 

Next a college, [Nicholas Zimmerman](https://github.com/nickz12345), who had been helping us error check, noticed that we had wired our Tayloe Mixer incorrectly. I then cut three out of the four traces and jumped the wires to send the I and Q signals to the correct paths. This can also be seen in the wiki. 

The final error in our Rev 5 design was in our second low pass filter. This was discovered by Dr. Rob Frohne. We had not realized that the V- needed to be connected to a negative voltage instead of ground. Luckily this was a backup filter and most of the gain was in our first LPF. To fix this, I simply just removed the second LPF from the board and jumped the outputs of the first LPF to the inputs of our audio connector.

Luckily I was able to fix all of these mistakes with simple cutting and jumping techniques. We immediately fixed the schematics and Christian created Rev 6 of the PCB which was displayed above.

## Project Results

We first tested to see the minimum discernible signal by hooking our board up to a signal generator, our system running quisk software, and a soundcard. We adjusted the attenuator on the signal generator to output a signal of 14.19 MHz and then dropped the signal on the attenuator to see how small of a signal we could detect. In the image below, we see our radio has a minimum disernable signal of inbetween 0.3-0.4 uV!

![minDecV2](https://user-images.githubusercontent.com/103695977/171743162-00ffc03b-b354-496b-b6c2-f4261597c40c.png)

We then hooked up our SDR to a BNC connector that ran to the roof of our Engineering building into an antenna. The final test was to see if we could hear any radio opperators transmitting around the world. We first looked at the signals being read in to see that our radio was picking up signals as far as Russia and Italy. Next we decided to listen in and found several bandwidths being used for communications. Here are two that we took snippets of.

The first signal that we tuned into was being used to communicate the weather from what sounded like an air traffic control tower. 

## "Wind 360 degrees, light drizzle, visibility more than 6 miles".

![weather](https://user-images.githubusercontent.com/103695977/171746787-f60b8dcc-d91d-4fa2-9dc1-e9d8d79bb7dc.png)

The second was an automated message that would read off the Coordinated Universal time every minute.

## "At the tone ****** 5 hours 52 minutes coordinated universal time"

![listeningIn](https://user-images.githubusercontent.com/103695977/171746851-dc1fa95d-337f-405f-911d-0fd728108ca7.png)

We were also able to listen to a few different music stations as well as two men in Hawaii talk about hurricanes to each other.
