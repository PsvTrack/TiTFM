# TiT FM - use Raspberry Pi as FM transmitter
Copyright (c) 2020, Amos Ndeto
All rights reserved.
Just get an FM receiver, connect 20 - 40 cm plain wire to Raspberry Pi's GPIO4 (PIN 7 on GPIO header) to act as an antena, and You are ready for broadcasting.
This project uses the general clock output to produce frequency modulated radio communication. It is based on idea originally presented by [Oliver Mattos and Oskar Weigl](http://icrobotics.co.uk/wiki/index.php/Turning_the_Raspberry_Pi_Into_an_FM_Transmitter) at [PiFM project](http://icrobotics.co.uk/wiki/index.php/Turning_the_Raspberry_Pi_Into_an_FM_Transmitter).
## How to use it
To use this project You will have to build executable. First, clone this repository, then use "make" command as shown below:
```
git clone https://github.com/PsvTrack/TiTFM
cd TiTFM
make
``` 
After successful build You can start transmitting by executing "tit_fm" program:
```
sudo ./tit_fm -f 102.0 test.wav
```
Where:
* -f frequency - Specifies the frequency in MHz, 100.0 by default if not passed
* test.wav - Sample WAVE file, You can use your own

Other options:
* -d dma_channel - Specifies DMA channel to be used (0 by default), type 255 to disable DMA transfer, CPU will be used instead
* -b bandwidth - Specifies the bandwidth in kHz, 100 by default
* -r - Loops the playback

After transmission has begun, simply tune an FM receiver to chosen frequency, You should hear the playback.
### Supported audio formats
You can transmitt uncompressed WAVE (.wav) files directly or read audio data from stdin, eg.:
```
sudo apt-get install sox
sox example.wav -r 22050 -c 1 -b 16 -t wav - | sudo ./tit_fm -f 100.6 -
```
Please note only uncompressed WAVE files are supported. If You expire "corrupted data" error try converting file, eg. by using SoX:
```
sudo apt-get install sox libsox-fmt-mp3
sox my-audio.mp3 -r 22050 -c 1 -b 16 -t wav my-converted-audio.wav
sudo ./tit_fm -f 100.6 my-converted-audio.wav
```
### USB microphone support
In order to use a USB microphone input use arecord command, eg.:
```
arecord -D hw:1,0 -c1 -d 0 -r 22050 -f S16_LE | sudo ./tit_fm -f 100.6 -
```
In case of performance drop down use ```plughw:1,0``` instead of ```hw:1,0```.
## Legal note
Please keep in mind that transmitting on certain frequencies without special permissions may be illegal in your country.
## Extra Features
* DMA peripheral support
* Allows custom frequency and bandwidth settings
* works on every Raspberry Pi model
* reads mono and stereo files
* reads data from stdin

