# A piano under Linux

A very simple piano keyboard:
- no GUI for sound server (such as [QjackCtl](https://qjackctl.sourceforge.io/)) or synthesizer (such as[Qsynth](https://qsynth.sourceforge.io/qsynth-index.html)).
- if a Akai LPK 25 is connected, it will control the piano.

It only uses [VMPK](), [Alsa](http://www.alsa-project.org) and [FluidSynth](http://www.fluidsynth.org/).

Tested under Ubuntu 18.04 and Ubuntu Studio 16.04.1 LTS.

> There is some online keyboard:
* [onlinemusictools](https://www.onlinemusictools.com/kb/)
* [Keith Horwood's one](https://keithwhor.com/music/)

## TODO

* after launching "Virtual Piano", sometime no sound is emitted

* working with any MIDI device, not only Akai LPK 25.

## References

[Ted's Linux MIDI Guide](http://tedfelix.com/linux/linux-midi.html) by Ted Felix, July 2018 

## Install

```bash

sudo dpkg -l alsa-utils ### provide tools such as aplaymidi

sudo apt-get install vmpk 

sudo apt-get install libcanberra-gtk-module

sudo apt-get install fluidsynth  ### To avoid warning message from VMPK

sudo sed -i "/End of file/ i \\
@audio   -  rtprio      90 \\
@audio   -  memlock     unlimited \\
" /etc/security/limits.conf

sudo usermod -a -G audio $(id -un)

sudo apt-get install fluid-soundfont-gm fluid-soundfont-gs timgm6mb-soundfont

sudo cp ./scripts/piano /usr/local/bin/piano

sudo chmod +x /usr/local/bin/piano

piano start  ### OK

piano stop

sudo desktop-file-install --dir=/usr/share/applications scripts/piano.desktop

sudo update-desktop-database

gtk-launch piano ### OK

sudo netstat -tulpn|grep 9800
# tcp6       0      0 :::9800                 :::*                    LISTEN      20345/fluidsynth    


```

> BUG After launching "Virtual Piano", sometime no sound is emitted. With Qjack check the server status and stop it if needed.

## French keyboard

A file azerty.xml is provided that binds :
- a first series of notes with:
    - line QSFG... bound to black keys,
    - line WXCVB... bound to white keys,
- a second series with:
    - line _, ç, à, ...
    - line yuiop...
    
> There is an overlap from pair "A" / "," to pair "T" / "*". 

Some changes will help to get a more regular mapping. All of them can be set up from the UI. Otherwise:

```bash

### Create a new keyboard mapping file:

sudo su -

cp /usr/share/vmpk/azerty.xml /usr/share/vmpk/fr-azerty.xml

### add key "&":

sed -i '/key="\$"/  a\
    <mapping key="&amp;" note="30"/>' /usr/share/vmpk/fr-azerty.xml

exit

### To get the expected tune with key "-", for the feature "Controller", replace
#   "+" (Up) and "-" (Down) with Alt + "+" and Alt + "-":

sed -i 's|^\(\s*actionControllerUp=\).*$|\1Alt++|' ~/.config/vmpk.sourceforge.net/"Virtual MIDI Piano Keyboard.conf"
sed -i 's|^\(\s*actionControllerDown=\).*$|\1Alt+-|' ~/.config/vmpk.sourceforge.net/"Virtual MIDI Piano Keyboard.conf"

### Load the new file:

sed -i 's|^\(\s*MapFile=\).*$|\1/usr/share/vmpk/fr-azerty.xml|' ~/.config/vmpk.sourceforge.net/"Virtual MIDI Piano Keyboard.conf"

piano stop

piano start

```

There are still two discrepencies:
- key "*" after key "ù" to get a D,
- key "^" remplaced by ")" to get a C.
  