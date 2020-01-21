e2-rpihddevice [![License: GPL v2](https://img.shields.io/badge/License-GPL%20v2-blue.svg)](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html)
==============
This is a "plugin" for Enigma2.

Written by:                  Thomas Reufer <thomas@reufer.ch>

Modified by:				 Open Vision Team			

Project's homepage:          https://github.com/OpenVisionE2/e2-rpihddevice

See the file COPYING for license information.

Description:

  Enigma2 HD output device for Raspberry Pi. The plugin makes use of the Raspberry
  Pi's VideoCore GPU and provides a lightweight implementation for Enigma2 output
  device.

Features:

  - MPEG-2 and H264 high-profile video codec up to 1080p30
  - MPEG-1 Layer II, (E)AC-3, AAC and DTS audio codec at 32kHz, 44.1kHz or 48kHz
    with 2.0 (Stereo) or 5.1 channels
  - HDMI multi channel LPCM audio output
  - HDMI digital audio pass-through
  - Analog stereo audio output
  - Box (letter-box/pillar-box), Crop and Stretch video display modes
  - True color OSD with GPU support
  - Video scaling and grabbing support

Requirements:

  - libavcodec, libavformat and libavutil for audio decoding, provided by ffmpeg
    or libav
  - libswresample when using ffmpeg-1.2 or newer
  - libavresample when using libav-9 or newer
  - freetype2 for GPU accelerated text drawing
  - valid MPEG-2 license when watching MPEG-2 streams
  - Raspberry Pi userland libraries: https://github.com/raspberrypi/userland
  - Raspberry Pi firmware version of 2015/01/18 or newer
 
Install:

  Get the source code either as archive or with git and compile like any other
  VDR plugin:

  $ cd /usr/src/vdr/PLUGINS/src
  $ git clone https://github.com/OpenVisionE2/e2-rpihddevice.git rpihddevice
  $ cd rpihddevice
  $ make
  $ make install
  
  If you want to link the plugin against a specific version of ffmpeg/libav, set
  EXT_LIBAV accordingly when compiling the plugin:
  
  $ make EXT_LIBAV=/usr/src/ffmpeg-1.2.6
  
Usage:

  To start the plugin, just add '-P rpihddevice' to the VDR command line.

  The plugin simply adds two new dispmanx layers on top of the framebuffer, one 
  for video and one for the OSD. The plugin does not clear the current console 
  or change any video mode settings. So it's the user's choice, what's being 
  displayed when no video is shown, e.g. during channel switches or for radio
  channels.

Options:

  -d, --disable-osd  Disables creation of OSD layer and prevents the plugin of
                     allocating any OSD related resources. If set, VDR's dummy
                     OSD is used, when selecting rpihddevice as primary device.
  -v, --video-layer  Specify the dispmanx layer for video (default 0)
  -o, --osd-layer    Specify the dispmanx layer for OSD (default 2)
      --display      display used for output:
                     0: default display (default)
                     4: LCD
                     5: TV/HDMI
                     6: non-default display

Plugin-Setup:

  Resolution: Set video resolution. Possible values are: "default",
  "follow video", "720x480", "720x576", "1280x720", "1920x1080"

  Frame Rate: Set video frame rate. Possible values are: "default",
  "follow video", "24p", "25p", "30p", "50i", "50p", "60i", "60p"

  When set to "default", the resolution/frame rate will not be changed and the
  plugin keeps the current setting of the framebuffer, normally set by the mode
  number in /boot/config.txt or changed with 'tvservice'. To let the plugin
  automatically set a value matching the current video stream, choose
  "follow video". In general, video setting are only applied if both, resolution
  and frame rate, fits to a video mode supported by the connected device,
  indicated by its EDID.

  EDID information can by overridden with various settings in /boot/config.txt,
  see the official documentation for further information:
  https://www.raspberrypi.org/documentation/configuration/config-txt.md

  Video Framing: Determines how the video frame is displayed on the screen in
  case the aspect ratio differs. "box" and "cut" will preserve the video's
  aspect ratio, while "box" (often called "letter box", however "pillar box" is
  used to show 4:3 videos on a wide screen) will fit the image on the 
  screen by adding transparent borders. On the other hand, "cut" is cropping 
  away the overlapping part of the video, so the entire display area is filled.
  When setting to "stretch", the videos' aspect ratio is adapted to the screen
  and the resulting image might appear distorted.
  
  Audio Port: Set the audio output port to "analog" or "HDMI". When set to
  analog out, multi channel audio is sampled down to stereo.
  
  Digital Audio Format: Specify the audio format when using the HDMI port as
  output. If set to "pass through", (E)AC-3 and DTS audio can be decoded by the
  connected HDMI device, if EDID indicates specific codec support. For local
  decoding, select "mutli channel PCM" or "Stereo PCM" if additional stereo
  dowmix of mutli channel audio is desired.
  
  Use GPU accelerated OSD: Use GPU capabilities to draw the on screen display.
  Disable acceleration in case of OSD problems to use VDR's internal rendering
  and report error to the author.
