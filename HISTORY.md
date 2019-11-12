Essai dans le cadre d'exploration de codage d'appli musicale sur navigateur

C'est partie de  la page https://www.onlinemusictools.com/kb/

Après avoir récupéré code et icone depuis le source de la page retour au script "piano" basé sur VMPK + FluidSynth

Ne génère aucun son sous FF lorsque lancé en local.

Utilise pour la partie figurant un clavier.


Web MIDI Keyboard on line
https://www.onlinemusictools.com/kb/

Getting Started With The Web MIDI API
March 19, 2018, Peter Anglea
https://www.smashingmagazine.com/2018/03/web-midi-api/
- indication pour polyfill pour autres que Chrome
  renvoie à la bilbiothèque WebMIDIAPIShim de Chris Wilson
  https://github.com/cwilso/WebMIDIAPIShim


rammmukul/web-MIDI-keyboard
A 2d MIDI keyboard https://rammmukul.github.io/web-MIDI-keyboard/examples/keyboard
https://github.com/rammmukul/web-MIDI-keyboard
plus un touchpad qu'un clavier mais au moins le son est là.


Connecting Midi Device to Browser with the Web MIDI API & Web Audio API
Web MIDI API + Web Audio API = A killer combo !
https://knowpapa.com/midi-device-browser/
Le support MIDI doit avoir été installé sur le poste.


djipco/webmidi
WebMidi.js helps you tame the Web MIDI API. Send and receive MIDI messages with ease. Control instruments with user-friendly functions (playNote, sendPitchBend, etc.). React to MIDI input with simple event listeners (noteon, pitchbend, controlchange, etc.). 
https://github.com/djipco/webmidi
Pour FF 52+ renvoie à 

    Jazz-MIDI extension v1.5.1+
    Web MIDI API extension


## Configurer FF pour MIDI

installer sous FF (70.0.1) :
- l'extension https://addons.mozilla.org/en-US/firefox/addon/jazz-midi/ (1.0.1.1)
- l'extension https://addons.mozilla.org/en-US/firefox/addon/web-midi-api/ (1.0.1.9)

instaler VMPK (0.7.2 du 2019-09-01.)

## Installation de base avec WMPK + qjackctl + qsynth

```bash

sudo apt-get install vmpk
# [...]
# Les paquets supplémentaires suivants seront installés : 
#   fluid-soundfont-gm jackd jackd2 jackd2-firewire libconfig++9v5 libffado2 libfluidsynth1 libqt4-svg libxml++2.6-2v5 python-dbus python-gi qjackctl qsynth
# Paquets suggérés :
#   fluid-soundfont-gs fluidsynth timidity jack-tools meterbridge python-dbus-dbg python-dbus-doc python-gi-cairo
# Les NOUVEAUX paquets suivants seront installés :
#   fluid-soundfont-gm jackd jackd2 jackd2-firewire libconfig++9v5 libffado2 libfluidsynth1 libqt4-svg libxml++2.6-2v5 python-dbus python-gi qjackctl qsynth vmpk
# 0 mis à jour, 14 nouvellement installés, 0 à enlever et 237 non mis à jour.
# Il est nécessaire de prendre 122 Mo dans les archives.
# Après cette opération, 164 Mo d'espace disque supplémentaires seront utilisés.
# Souhaitez-vous continuer ? [O/n] 
# Réception de:1 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 fluid-soundfont-gm all 3.1-5.1 [119 MB]
# Réception de:1 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 fluid-soundfont-gm all 3.1-5.1 [119 MB]
# Réception de:2 http://fr.archive.ubuntu.com/ubuntu bionic/main amd64 python-dbus amd64 1.2.6-1 [90,2 kB]        
# Réception de:3 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 jackd2 amd64 1.9.12~dfsg-2 [268 kB]    
# Réception de:4 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 jackd all 5 [2 072 B]                  
# Réception de:5 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 libconfig++9v5 amd64 1.5-0.4 [31,6 kB] 
# Réception de:6 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 libxml++2.6-2v5 amd64 2.40.1-2 [59,7 kB]
# Réception de:7 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 libffado2 amd64 2.3.0-5.1 [1 067 kB]    
# Réception de:8 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 jackd2-firewire amd64 1.9.12~dfsg-2 [18,9 kB] 
# Réception de:9 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 libfluidsynth1 amd64 1.1.9-1 [137 kB]         
# Réception de:10 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 libqt4-svg amd64 4:4.8.7+dfsg-7ubuntu1 [138 kB]
# Réception de:11 http://fr.archive.ubuntu.com/ubuntu bionic-updates/main amd64 python-gi amd64 3.26.1-2ubuntu1 [197 kB]   
# Réception de:12 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 qjackctl amd64 0.4.5-1ubuntu1 [429 kB] 
# Réception de:13 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 qsynth amd64 0.5.0-2 [191 kB] 
# Réception de:14 http://fr.archive.ubuntu.com/ubuntu bionic/universe amd64 vmpk amd64 0.4.0-3build1 [364 kB] 
# [...]

### Accepter l'option temps réel.

### A priori plus besoin d'installer séparemment qjackctl et un synthétiseur. 
#   Qsynth est utilisé pour exploiter fluid-soundfont-gs. Pas besoin de fluidsynth ou timidity .

### c'est long : la bibliothèque de son fluid-soundfont-gm fait 119 MB et se charge à ~ 100 kB/s

vmpk & qjackctl & qsynth
# [1] 20522
# [2] 20523
# Gtk-Message: 12:17:07.068: Failed to load module "canberra-gtk-module"
^C

```

Configurer Qsynth :
- sélectionner une configuration, par ex. "Qsynth1"
- cliquer sur "Configuration..." pour l'ouvrir
- ouvrir le volet "Banques de son"
- cliquer sur "Ouvrir..."
- sélectionner une banque, par ex. : /usr/share/sounds/sf2/FluidR3_GM.sf2

Configurer QjackCtl :
- cliquer sur "Connecter"
- ouvrir le volet "ALSA"
- dans la liste "Clients en lecture / Port de sortie", sélectionner "128:VMPK Output"
- dans la liste "Clients en écriture / Port d'entrée", sélectionner "131:FLUID Synth (20524)"

Et jouer ... depuis le clavier ou la souris.

OK

Connecter un clavier MIDI "Physique", ici un Akai LPK25

- brancher le clavier au PC
- configurer QjackCtl en ajoutant une connexion 
  - dans la liste "Clients en lecture / Port de sortie", sélectionner "24:LPK25"
  - dans la liste "Clients en écriture / Port d'entrée", sélectionner "131:FLUID Synth (20524)"

> Pas besoin de déconnecter l'existant

OK

## Essai d'automatisation

En ne conservant que la GUI de VMPK

```bash

vmpk & qjackctl & qsynth &
# [2] 22986
# [3] 22987
# [4] 22988
# Gtk-Message: 16:06:19.596: Failed to load module "canberra-gtk-module"

jack_lsp -c
# Cannot lock down 82280346 byte memory area (Cannot allocate memory)
# system:capture_1
# system:capture_2
# system:playback_1
#    qsynth:left
# system:playback_2
#    qsynth:right
# qsynth:left
#    system:playback_1
# qsynth:right
#    system:playback_2

killall vmpk qjackctl qsynth
# [7]   Complété              vmpk
# [8]   Complété              qjackctl
# [9]-  Complété              qsynth
# [10]+  Complété              vmpk

sudo apt-get install fluidsynth

sudo sed -i "/End of file/ i \\
@audio   -  rtprio      90 \\
@audio   -  memlock     unlimited \\
" /etc/security/limits.conf

id -u
# 1000

id -un
# pierre

cat /etc/passwd | grep pierre
pierre:x:1000:1000:pierre,,,:/home/pierre:/bin/bash

cat /etc/group | grep pierre
# adm:x:4:syslog,pierre
# cdrom:x:24:pierre
# sudo:x:27:pierre
# dip:x:30:pierre
# plugdev:x:46:pierre
# lxd:x:108:pierre
# lpadmin:x:115:pierre
# sambashare:x:122:pierre
# pierre:x:1000:
# vboxusers:x:133:pierre

cat /etc/group | grep audio
# audio:x:29:pulse

sudo usermod -a -G audio pierre

cat > start.sh << ___EOF
fluidsynth --server \
           --no-shell \
           --audio-driver=pulseaudio \
           --gain=1.0 \
           --reverb=0.42 \
           --chorus=0.42 \
           /usr/share/sounds/sf2/FluidR3_GM.sf2 &>/tmp/fluidsynth.out
sleep 2
vmpk
sleep 2
vmpkport=\$(aconnect -i |grep "client.*VMPK Output" | cut -d ' ' -f 2)0

synthport=\$(aconnect -i |grep "FLUID Synth" | cut -d ' ' -f 2)0 

echo "vmpk on \${vmpkport} & synth on \${synthport}"
___EOF

sudo apt-get install fluid-soundfont-gm

bash ./start.sh
```

KO : fluidsynth ne démarre pas 

TODO :
- fluidsynth avec --audio-driver=pulseaudio  ---> --audio-driver=alsa
- lancer le script en tant que sudoer ?

## Récupérer l'existant sur le portable

Voir [PREVIOUS](./PREVIOUS.md)

Certains éléments nécessaires pour le fonctionnement de ces scripts sont déjà installés :
- fluidsynth
- FluidR3_GM.sf2
- FluidR3_GS.sf2
- vmpk

```bash

sudo apt-get install timgm6mb-soundfont

cd /home/pierre/DevSpace/perso-sound/omt-keyboard

mkdir -pv scripts

cp /media/pierre/USB20FD/piano scripts
cp /media/pierre/USB20FD/piano.desktop scripts

sudo cp scripts/piano /usr/local/bin/piano

sudo chmod +x /usr/local/bin/piano

sudo desktop-file-install --dir=/usr/share/applications scripts/piano.desktop

sudo update-desktop-database

gtk-launch piano
# FluidSynth version 1.1.9
# Copyright (C) 2000-2018 Peter Hanappe and others.
# Distributed under the LGPL license.
# SoundFont(R) is a registered trademark of E-mu Systems, Inc.

# fluidsynth: warning: Failed to pin the sample data to RAM; swapping is possible.
# fluidsynth: warning: Failed to set thread to high priority
# fluidsynth: warning: Failed to set thread to high priority
# 1039
# 1245
# 26034
# 26608
# 26787
# Gtk-Message: 08:22:13.046: Failed to load module "canberra-gtk-module"
# 1259
# WMPK (132:0) is connected with Fluidsynth (128
# 129
# 130
# 131:0).
# /usr/local/bin/piano: ligne 45 : [: $KB_OUT : nombre entier attendu comme expression
# LPK 25 not available.


```

KO

Le remplacement de fluidsynth par qsynth ne laisse plus comme message d'avertissement que :
- Gtk-Message: 08:22:13.046: Failed to load module "canberra-gtk-module"
- LPK 25 not available   

L'installation de libcanberra-gtk-module:i386 n'y change rien :
            sudo apt-get install libcanberra-gtk-module:i386

idem avec libcanberra-gtk3-module:i386

Voir § suivant pour ce qui est de ce point

OK pour la connexion de VMPK avec fluidsynth mais :
- affichage de qsynth
- tjs pas de connexion à LPK 25

Les versions de vmpk et fluidsynth ne sont pas les mêmes :
- vmpk sur le portable : 0.6.2 contre 0.4.0 sur le Z420
- fluidsynth sur le portable : 1.1.6 contre 1.1.9 sur le Z420

Laisser tel quel pour le moment 

Oups, le script n'est pas bon : modifier le test portant 

    if [ '$KB_OUT' eq ':' ]

qui devient

    if [ '$KB_OUT' = ':' ]

en inversant l'ordre des instructions    

### canberra-gtk-module

Ce module est requis par vmpk.

```bash

# sudo apt-get install libcanberra-gtk-module:i386
# sudo apt-get install libcanberra-gtk3-module:i386

dpkg -l | grep canb
ii  gnome-session-canberra           0.30-5ubuntu1          amd64        GNOME session log in and log out sound events
ii  libcanberra-gtk-module:i386      0.30-5ubuntu1          i386         translates GTK+ widgets signals to event sounds
ii  libcanberra-gtk0:i386            0.30-5ubuntu1          i386         GTK+ helper for playing widget event sounds with libcanberra
ii  libcanberra-gtk3-0:amd64         0.30-5ubuntu1          amd64        GTK+ 3.0 helper for playing widget event sounds with libcanberra
ii  libcanberra-gtk3-0:i386          0.30-5ubuntu1          i386         GTK+ 3.0 helper for playing widget event sounds with libcanberra
ii  libcanberra-gtk3-module:amd64    0.30-5ubuntu1          amd64        translates GTK3 widgets signals to event sounds
ii  libcanberra-gtk3-module:i386     0.30-5ubuntu1          i386         translates GTK3 widgets signals to event sounds
ii  libcanberra-pulse:amd64          0.30-5ubuntu1          amd64        PulseAudio backend for libcanberra
ii  libcanberra0:amd64               0.30-5ubuntu1          amd64        simple abstract interface for playing event sounds
ii  libcanberra0:i386                0.30-5ubuntu1          i386         simple abstract interface for playing event sounds

piano stop
# Audio server(s) and virtual keyboard stopped.

vmpk &
# [1] 17164
# Gtk-Message: 12:35:36.411: Failed to load module "canberra-gtk-module"

sudo apt-get install libcanberra-gtk-module

vmpk &

piano stop
# qsynth: aucun processus trouvé
# Audio server(s) and virtual keyboard stopped.


```

Gag : il fallait prendre le message au pied de la lettre...

Retirer libcanberra-gtk0:i386 & C°

```bash

sudo apt-get remove libcanberra-gtk-module:i386 libcanberra-gtk3-module:i386

dpkg -l | grep canb
ii  gnome-session-canberra           0.30-5ubuntu1          amd64        GNOME session log in and log out sound events
ii  libcanberra-gtk-module:amd64     0.30-5ubuntu1          amd64        translates GTK+ widgets signals to event sounds
ii  libcanberra-gtk0:amd64           0.30-5ubuntu1          amd64        GTK+ helper for playing widget event sounds with libcanberra
ii  libcanberra-gtk0:i386            0.30-5ubuntu1          i386         GTK+ helper for playing widget event sounds with libcanberra
ii  libcanberra-gtk3-0:amd64         0.30-5ubuntu1          amd64        GTK+ 3.0 helper for playing widget event sounds with libcanberra
ii  libcanberra-gtk3-0:i386          0.30-5ubuntu1          i386         GTK+ 3.0 helper for playing widget event sounds with libcanberra
ii  libcanberra-gtk3-module:amd64    0.30-5ubuntu1          amd64        translates GTK3 widgets signals to event sounds
ii  libcanberra-pulse:amd64          0.30-5ubuntu1          amd64        PulseAudio backend for libcanberra
ii  libcanberra0:amd64               0.30-5ubuntu1          amd64        simple abstract interface for playing event sounds
ii  libcanberra0:i386                0.30-5ubuntu1          i386         simple abstract interface for playing event sounds

vmpk &
# [1] 19098

piano stop
# qsynth: aucun processus trouvé
# Audio server(s) and virtual keyboard stopped.

```

OK tjs

Par précaution laisser tel quel libcanberra0:i386 & C°. Avait été installé antérieurement par ???

## Lancement de Qsynth

Avec qsynth aussi bien 'alsa' que 'pulseaudio' marchent comme pilote audio

    qsynth -a alsa --no-shell -v &

mais préférer 'alsa' car avec 'pulseaudio' il semble qu'il y ait 2 problèmes :
- lors du 1er lancement de 'piano' après redémarrage de l'ordi et branchement du clavier, un bref son est généré
- après débranchement / rebranchement du clavier, piano n'émet plus de son.

Problèmes qui ne semblent pas arriver avec 'alsa'.

## FluidSynth

Il ne reste plus qu'un seul pb avec FluidSynth à la place de Qsynth :

```bash
piano start
# FluidSynth version 1.1.9
# Copyright (C) 2000-2018 Peter Hanappe and others.
# Distributed under the LGPL license.
# SoundFont(R) is a registered trademark of E-mu Systems, Inc.

# fluidsynth: error: Failed to bind server socket
# Failed to create the server.
# Continuing without it.
# There was a problem starting the audio server(s).
```

Or tout va bien :
- si on utilise Qsynth à la place de FluidSynth dans le script
- si on est sous Ubuntu Studio...

Qu'y a t il de plus dans la config de Ubuntu Studio ?
Que fait Qsynth pour compenser et qui manque à fluidsynth ?

    Que manque t il à fluidsynth pour fonctionner ?
    Egor Sanin, Nov 24, 2013
    http://linux-audio.4202.n7.nabble.com/Multiple-fluidsynth-instances-td87995.html

A priori seul shell.port est requis : prendre la même valeur que celle prise par défaut par Qsynth soit 9800

KO

Le port 9800 est de toute façon la valeur par défaut. La spécifier n'a de sens que si plusieurs instances de fluidsynth

Si on regarde le code de fluid_sys.c, si l'erreur "Failed to bind server socket" c'est que la création du serveur 
s'est correctement faite.

S'agirait il uniquement d'un mauvais paramétrage ?

Oups, non simplement d'un thead orphelin de fluidsynth

