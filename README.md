# Software-Defined-Radio-Reciever

This project was taken on with the goal to create a Software Defined Radio (SDR) in coolaboration with my partner Christian Williams. This project was completed for my Engineering Electronics II class and given to us by our professor, [Dr. Rob Frohne](https://github.com/frohro).

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

Our (SDR) receiver connects to an antenna (mounted ontop of the Engineering Building) and covers the filtering, mixing, and amplifying of the desired I and Q signals. Our I and Q signals are then passed through an external sound card and software handles the demodulation of the signal into audio. The mixing in our SDR is handled using an Arduino Nano running code provided to us by Dr. Rob Frohne which can be found in the [Arduino file](). The Arduino interfaces with Quisk, an SDR that controls our receiver through the Arduino. Using Quisk, the radio can be tuned to a certain frequency by adjusting the speed of the local oscillator used in the mixing process.

## Theory


## Rev 6 Schematic

Christian and I built and tested our SDR based on the 5th Rev of this project. These schematics can be found in the Rev 5 file. The follow schematics are our most updated after finding multiple errors while constructing out pcb. The Rev 6 schematics can be downloaded [here](). These schematics were all designed in [KiCad](https://www.kicad.org/) and will be broken down into seperate pieces below.

### BandPass Filter

![BandPass Filter and Voltage Divider](https://user-images.githubusercontent.com/103695977/171759491-6baa6ea1-8451-4fa1-af03-81ee3740693b.jpg)

We built this bandpass filter using the [LC Filter Design Tool](https://rf-tools.com/lc-filter/.). This is a 3rd-Order Conventional, Series First, Butterworth Filter that will only allow frequencies between 7MHz and 18MHz to pass through it. It has input and output impedances of 50 ohms.

### Mixer and Lowpass Filter

![Tayloe Mixer](https://user-images.githubusercontent.com/103695977/171759381-e49fd43a-300a-4075-a9eb-c46a4cfd6736.jpg)

The mixer that Joshua and I decided to use was  Tayloe Mixer. The Tayloe mixer uses switches to combined the signals that are 180 degrees out of phase with differential amplifiers so that signals above and below the oscilator frequecy can be detected.

Our Lowpass filter will block all frequencies greater that 100kHz.

### Voltage Smoother

![Voltage Smoother](https://user-images.githubusercontent.com/103695977/171760121-b3ce913d-ab7f-4082-ad9f-a03cbcf7cb58.jpg)

Our Voltage Smoother with take the 5V Vcc and remove as much noise as possible that comes in from the computer and outputs a smoother 5V for the SDR to use.

### Arduino and Si5351

![Arduino and Si5351](https://user-images.githubusercontent.com/103695977/171760493-9e65d962-7504-4b07-9917-c7026b6bcaa2.jpg)

Our Si5351 programmable oscillator will allow us to input two clock signals that are 90 degrees out of phase into S0 and S1 of the SN74CBT3253. The Arduino Nano will control the frequency the is put out by the Si5351 for tuning.

## Simulations

The following are the simulations of our filters as we have not yet fully simulated the SDR as a whole.

### Bandpass Filter

![image](https://user-images.githubusercontent.com/103695977/163806294-b24794c1-b576-47e0-a262-90af70f5b265.png)
![image](https://user-images.githubusercontent.com/103695977/163810045-d9e52c8f-7157-414a-8c3d-9e4d7e16fb39.png)

### Lowpass Filter

![image](https://user-images.githubusercontent.com/103695977/163810193-a9069945-3c7c-4594-b9f1-aa382b970d71.png)
![image](https://user-images.githubusercontent.com/103695977/163810300-0baa7ec9-fe0b-4580-a687-907685373d8e.png)


## Project Results

When testing to see the minimum discernible signal we hooked our board up to our signal generator, our system running quisk software, and our soundcard. We adjusted the attenuator on the signal generator to output a signal of 14MHz and adjusted the attenuator to see how small of a signal we could detect. In the image below, we see our radio discern a radio of 3.5uV!

![minDecV2](https://user-images.githubusercontent.com/103695977/171743162-00ffc03b-b354-496b-b6c2-f4261597c40c.png)

Next, we decided to see if we could find any bandwidths where people were talking and try to listen in. We found two bandwidths being used for communications.

The first was being used to communicate the weather from what we are assuming to be an air traffic control tower. 

![weather](https://user-images.githubusercontent.com/103695977/171746787-f60b8dcc-d91d-4fa2-9dc1-e9d8d79bb7dc.png)

"Wind 360 degrees, light drizzle, visibility more than 6 miles".

The second bandwidth was being used by two HAM Radio operators talking about how another station send out a signal that interfered with their own.

![listeningIn](https://user-images.githubusercontent.com/103695977/171746851-dc1fa95d-337f-405f-911d-0fd728108ca7.png)
