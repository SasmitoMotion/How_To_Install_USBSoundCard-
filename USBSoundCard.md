# How_To_Install_USBSoundCard-
Getting Audio Work with instalation ALSA configuration

check your driver connected on Raspberry Pi and my device example  :
  
    lsusb
    Bus 001 Device 006: ID 0d8c:013c C-Media Electronics, Inc. CM108 Audio Controller

if your device connected internet with wireless don't forget check:

    sudo apt-get update
    sudo apt-get upgrade
    sudo rpi-update

Install the ALSA utilities:

    sudo apt-get install alsa-utils
    
Install MP3 tools:

    sudo apt-get install mpg321
    
Install WAV to MP3 conversion tool:

    sudo apt-get install lame
    
Load the sound driver:
    
    sudo modprobe snd-bcm2835
    
To check if the driver is loaded you can type:

    sudo lsmod | grep 2835
    
# Saving The ALSA state

The operating system saves the ALSA sound configuration for other command in this file:

    sudo nano /etc/init.d/alsa-utils
    
The state is store and restored in this file:

    sudo nano /var/lib/alsa/asound.state
    
Now you can check and change the default value to record that sound with this command #

    state.ALSA {
        control.1 {
                iface MIXER
                name 'PCM Playback Volume'
                value 399			          # the default value -1725
                comment {
                        access 'read write'
                        type INTEGER
                        count 1
                        range '-10239 - 400'
                        dbmin -9999999
                        dbmax 400
                        dbvalue.0 399		# the default value 0 -1725
                }
        }
        control.2 {
                iface MIXER
                name 'PCM Playback Switch'
                value true
                comment {
                        access 'read write'
                        type BOOLEAN
                        count 1
                }
        }
        control.3 {
                iface MIXER
                name 'PCM Playback Route'
                value 2		    	        # default value 0
                comment {
                        access 'read write'
                        type INTEGER
                        count 1
                        range '0 - 2'
                }
        }
  }

if we mute it hthen have another look like line 5 change to "value -10239", line 13 changes to "dbvalue.0 -9999999" and line 19 changes to "false"

you can check the device in this command :

    alsamixer
    
click F6: Select sound card 

    x1 USB Pnp Sound Devices
    
enter and then you can watch the Speaker and Mic, you can put right or left to put the volume before you record, CTRL + C to exit

# This Function in ALSA

alsamixer ------ GUI for setting the recording and playback levels

amixer    ------ Command Line for setting the recording and playback levels

alsactl   ------ for saving the settings set in alsamixer or amixer to use again after a reboot

arecord   ------ For recording the sound

aplay     ------ For playing back your recording

# WAV and MP3 Conversion

The MP3 player mpg321 can convert MP3 files to WAV files but the WAV player, aplay, can not do a conversion.  To make a MP3 file from a WAV file, you’ll need the tool lame.

    To convert from WAV to MP3: lame input.wav output.mp3
    To convert from MP3 to WAV:  mpg321 -w output.wav input.mp3

Reference :
http://cagewebdev.com/index.php/raspberry-pi-getting-audio-working/

http://blog.scphillips.com/posts/2013/01/sound-configuration-on-raspberry-pi-with-alsa/

https://learn.adafruit.com/usb-audio-cards-with-a-raspberry-pi/updating-alsa-config

https://jeffskinnerbox.wordpress.com/2012/11/15/getting-audio-out-working-on-the-raspberry-pi/
