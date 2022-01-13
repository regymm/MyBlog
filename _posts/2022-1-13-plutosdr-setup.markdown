---
layout: post
title: "PlutoSDR Environment Setup on Linux as of 2022"
data: 2022-1-13 23:00:00 +800
comments: true
categories: [Experience]
---

A quick blog to document my setup for PlutoSDR software-defined radio "development". Written in a haste, to fight laziness, and fading memory. 

Especially, I want GNU Radio, iio-oscilloscope, and GQRX. 

#### Virtual Machine

My hardware is my [PYNQSDR](https://github.com/regymm/PYNQSDR) running PlutoSDR firmware, connected to Linux laptop via 1G ethernet. Real PlutoSDR with USB should be the same(and might easier). 

If you're running Arch Linux, go install a Ubuntu virtual machine, 20.04 would be good. Once upon a time I got everything settled down on Arch but a boost update(without which LibreOffice can't open) broke them all. Don't rely on AUR, don't try to play hero. 

Virtualbox, network set to NAT. PYNQSDR connected to host, should be able to ping in VM at 192.168.1.10. 

First libiio-utils from apt. Then `iio_info -u ip:192.168.1.10` should return SDR's status. 

https://wiki.analog.com/university/tools/pluto/drivers/linux

Then iio-oscilloscope, follow official guide, compile yourself, don't forget to `apt-get -y install libglib2.0-dev libgtk2.0-dev libgtkdatabox-dev libmatio-dev libfftw3-dev libxml2 libxml2-dev bison flex libavahi-common-dev libavahi-client-dev libcurl4-openssl-dev libjansson-dev cmake libaio-dev libserialport-dev`  first. 

https://wiki.analog.com/resources/tools-software/linux-software/iio_oscilloscope

Run it and you'll be able to see interface for AD936X control, and capture waveforms if your board's RX and TX IO's are connected to FPGA(which is always the case except you're making your own board). I'd also usually test if the TX LO can be found on RX spectrum. If you are still debugging hardware, stop here is enough. Also the above is quite easy on Arch as well, I have to admit. 

#### GNU Radio

Then GNU Radio(which is required for GQRX). Follow this strictly(if you were on Arch you can't and you'll suffer): 

https://wiki.analog.com/resources/tools-software/linux-software/gnuradio

Compile & install libiio & libad9361-iio, also clone gr-iio but you'll need GNU Radio installed to compile. 

Install GNU Radio 3.8 -- at least I got this working. Yes, you'll need to compile it yourself, install from PPA will fail. 

From source: https://wiki.gnuradio.org/index.php/InstallingGR#From_Source , jump to "For GNU Radio 3.8 or Earlier". It already pointed out what to do on Ubuntu 20.04: https://wiki.gnuradio.org/index.php/UbuntuInstall#Focal_Fossa_.2820.04.29_through_Impish_Indri_.2821.10.29

Compilation didn't take too long on my i7-8550u and 4-core 6 GB RAM allocated to VM. 

It will surely fail to start if you run gnuradio-companion directly, you'll need to follow the PYTHON_EXECUTABLE part. 

Maybe like this:

```
export PYTHONPATH=/usr/local/lib/python3/dist-packages
export LD_LIBRARY_PATH=/usr/local/lib
gnuradio-companion
```

Then you can compile gr-iio normally and install as said in the ADI wiki. 

You should be able to find PlutoSDR blocks when GNU Radio finally opens. 

#### GQRX

GQRX's support for PlutoSDR is... kinda not official, and I, as a struggling compile person, feel it very fragile. 

I'm using this: https://github.com/dk2ro/gr-osmosdr-pluto-spyserver

Clone and compile, install as usual, it *shouldn't* misbehave. It's just a library(.so) used by GQRX. So installed to /usr/local/lib, you should always take care of LD_LIBRARY_PATH. 

Then finally GQRX itself. Use the official repo and try to cmake and build. If cmake fails just install missing libraries like libqt5xxx. 

OK, I met a boost function(chrono) not found problem at the final linking, yet the libboost-chrono1.71-dev was already installed. 

My workaround is `VERBOSE=1 make` to see the failing command itself, and I manually add the chrono lib path. And I ran it in `build/src`:

```
/usr/bin/c++  -O3 -DNDEBUG  -rdynamic CMakeFiles/gqrx.dir/gqrx_autogen/mocs_compilation.cpp.o CMakeFiles/gqrx.dir/applications/gqrx/main.cpp.o CMakeFiles/gqrx.dir/applications/gqrx/mainwindow.cpp.o CMakeFiles/gqrx.dir/applications/gqrx/receiver.cpp.o CMakeFiles/gqrx.dir/applications/gqrx/remote_control_settings.cpp.o CMakeFiles/gqrx.dir/applications/gqrx/remote_control.cpp.o CMakeFiles/gqrx.dir/applications/gqrx/recentconfig.cpp.o CMakeFiles/gqrx.dir/applications/gqrx/file_resources.cpp.o CMakeFiles/gqrx.dir/dsp/afsk1200/cafsk12.cpp.o CMakeFiles/gqrx.dir/dsp/afsk1200/costabf.c.o CMakeFiles/gqrx.dir/dsp/filter/fir_decim.cpp.o CMakeFiles/gqrx.dir/dsp/rds/decoder_impl.cc.o CMakeFiles/gqrx.dir/dsp/rds/parser_impl.cc.o CMakeFiles/gqrx.dir/dsp/agc_impl.cpp.o CMakeFiles/gqrx.dir/dsp/correct_iq_cc.cpp.o CMakeFiles/gqrx.dir/dsp/downconverter.cpp.o CMakeFiles/gqrx.dir/dsp/fm_deemph.cpp.o CMakeFiles/gqrx.dir/dsp/lpf.cpp.o CMakeFiles/gqrx.dir/dsp/resampler_xx.cpp.o CMakeFiles/gqrx.dir/dsp/rx_agc_xx.cpp.o CMakeFiles/gqrx.dir/dsp/rx_demod_am.cpp.o CMakeFiles/gqrx.dir/dsp/rx_demod_fm.cpp.o CMakeFiles/gqrx.dir/dsp/rx_fft.cpp.o CMakeFiles/gqrx.dir/dsp/rx_filter.cpp.o CMakeFiles/gqrx.dir/dsp/rx_meter.cpp.o CMakeFiles/gqrx.dir/dsp/rx_noise_blanker_cc.cpp.o CMakeFiles/gqrx.dir/dsp/rx_rds.cpp.o CMakeFiles/gqrx.dir/dsp/sniffer_f.cpp.o CMakeFiles/gqrx.dir/dsp/stereo_demod.cpp.o CMakeFiles/gqrx.dir/interfaces/udp_sink_f.cpp.o CMakeFiles/gqrx.dir/qtgui/afsk1200win.cpp.o CMakeFiles/gqrx.dir/qtgui/agc_options.cpp.o CMakeFiles/gqrx.dir/qtgui/audio_options.cpp.o CMakeFiles/gqrx.dir/qtgui/bandplan.cpp.o CMakeFiles/gqrx.dir/qtgui/bookmarks.cpp.o CMakeFiles/gqrx.dir/qtgui/bookmarkstablemodel.cpp.o CMakeFiles/gqrx.dir/qtgui/bookmarkstaglist.cpp.o CMakeFiles/gqrx.dir/qtgui/ctk/ctkRangeSlider.cpp.o CMakeFiles/gqrx.dir/qtgui/demod_options.cpp.o CMakeFiles/gqrx.dir/qtgui/dockaudio.cpp.o CMakeFiles/gqrx.dir/qtgui/dockbookmarks.cpp.o CMakeFiles/gqrx.dir/qtgui/dockfft.cpp.o CMakeFiles/gqrx.dir/qtgui/dockinputctl.cpp.o CMakeFiles/gqrx.dir/qtgui/dockrds.cpp.o CMakeFiles/gqrx.dir/qtgui/dockrxopt.cpp.o CMakeFiles/gqrx.dir/qtgui/dxc_options.cpp.o CMakeFiles/gqrx.dir/qtgui/dxc_spots.cpp.o CMakeFiles/gqrx.dir/qtgui/freqctrl.cpp.o CMakeFiles/gqrx.dir/qtgui/ioconfig.cpp.o CMakeFiles/gqrx.dir/qtgui/iq_tool.cpp.o CMakeFiles/gqrx.dir/qtgui/meter.cpp.o CMakeFiles/gqrx.dir/qtgui/nb_options.cpp.o CMakeFiles/gqrx.dir/qtgui/plotter.cpp.o CMakeFiles/gqrx.dir/qtgui/qtcolorpicker.cpp.o CMakeFiles/gqrx.dir/receivers/nbrx.cpp.o CMakeFiles/gqrx.dir/receivers/receiver_base.cpp.o CMakeFiles/gqrx.dir/receivers/wfmrx.cpp.o CMakeFiles/gqrx.dir/pulseaudio/pa_device_list.cc.o CMakeFiles/gqrx.dir/pulseaudio/pa_sink.cc.o CMakeFiles/gqrx.dir/pulseaudio/pa_source.cc.o CMakeFiles/gqrx.dir/qrc_icons.cpp.o CMakeFiles/gqrx.dir/qrc_textfiles.cpp.o  -o gqrx  -Wl,-rpath,/usr/local/lib: /usr/lib/x86_64-linux-gnu/libQt5Network.so.5.12.8 /usr/lib/x86_64-linux-gnu/libQt5Svg.so.5.12.8 /usr/lib/x86_64-linux-gnu/libboost_date_time.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_program_options.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_filesystem.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_system.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_regex.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_thread.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_unit_test_framework.so.1.71.0 /usr/local/lib/libgnuradio-osmosdr.so -lpulse -lpulse-simple /usr/local/lib/libgnuradio-digital.so.3.8.5.0 /usr/local/lib/libgnuradio-audio.so.3.8.5.0 /usr/lib/x86_64-linux-gnu/libQt5Widgets.so.5.12.8 /usr/lib/x86_64-linux-gnu/libQt5Gui.so.5.12.8 /usr/lib/x86_64-linux-gnu/libQt5Core.so.5.12.8 /usr/local/lib/libgnuradio-analog.so.3.8.5.0 /usr/local/lib/libgnuradio-filter.so.3.8.5.0 /usr/local/lib/libgnuradio-blocks.so.3.8.5.0 /usr/local/lib/libgnuradio-fft.so.3.8.5.0 -lfftw3f -lfftw3f_threads /usr/local/lib/libgnuradio-runtime.so.3.8.5.0 /usr/lib/x86_64-linux-gnu/libboost_program_options.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_filesystem.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_system.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_regex.so.1.71.0 /usr/local/lib/libvolk.so.2.0 -ldl -lorc-0.4 -lm /usr/local/lib/libgnuradio-pmt.so.3.8.5.0 /usr/lib/x86_64-linux-gnu/libboost_thread.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_atomic.so.1.71.0 /usr/lib/x86_64-linux-gnu/libboost_chrono.so.1.71.0 -lpthread -llog4cpp -lgmpxx -lgmp -lrt -lasound
```

And it just pass... And GQRX can start without segfault. 

#### avahi

Yes, avahi is used for identifying where the PlutoSDR network location is by GQRX, but in VM the pluto.local domain doesn't exist. So simply add `192.168.1.10 pluto.local` to /etc/avahi/hosts. Restart avahi-daemon if iio-osilloscope can't connect after PlutoSDR board crashed. 

Another tip: seems GQRX won't modify PlutoSDR's complex settings. So you can modify options in iio-oscilloscope while listening to GQRX, receiver options like slow_gain may help. 

#### Overkill

Now, you can finally enjoy some FM radio using the hundred-dollar high performance, high integrated RF agile transceiver and SoC with hardware and software programmability. 

*A local FM broadcast(with suppressed lower sideband?), and I put tape on camera*

![](/MyBlog/images/sdr.png)

