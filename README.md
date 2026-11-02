# mitchoster

I build native VST2 plugins for Akai MPC and Force **standalone** devices — the instruments and
effects the built-in plugin host never shipped with.

MPC OS 2.x and 3.x contain a JUCE-based VST2 host, but there was nothing to load into it. A plugin that
fits this hardware has to cross-compile for arm32v7 against glibc 2.32, run inside a 2.9 ms audio block,
and draw its own touchscreen pages in MPC's skin format, with Q-Link assignments that make sense under
your hands while you play.

## Projects

| Repository | What it is |
| --- | --- |
| [mpc-vst-djfx2](https://github.com/mitchoster/mpc-vst-djfx2) | One-slot DJ insert effect: 3-band isolator, DJ filter, beat repeat, delay, plate/hall reverb, through-zero flanger, grain delay, true-peak limiter, and eight macros fitted to measurements of the original rack. Three interfaces, plus a companion screen in the browser. Device tested on an MPC Live II. |
| [mpc-vst-tape-echo](https://github.com/mitchoster/mpc-vst-tape-echo) | Three-head tape echo with a spring tank: 12 head and reverb modes, tape saturation, wow and flutter, tempo sync. Measured at 19.8% of a 2.9 ms audio block on an MPC Live II. |
| [mpc-vst-multihead-echo](https://github.com/mitchoster/mpc-vst-multihead-echo) | Multi-head tape echo with a joggable playhead: three preamp characters, chorus, tape wear (drift, dropouts, hiss), stereo spread, tempo sync with drift and a spring reverb — plus a companion screen in the browser. Device tested on an MPC Live II. |

Every one of them also serves a **companion screen**: a page the plugin itself hands out on the LAN, so
the dense part of the interface lives on a phone or tablet while the device keeps drawing its own skin.

## How these are built

- C, no framework runtime. One shared object per plugin, running on the host's own 128-frame block size.
- Build container `arm32v7/gcc:11-bullseye`, linked with `-Wl,--no-undefined`, so a missing symbol fails
  the build instead of crashing the MPC on load.
- Two gates before anything reaches a device: an offline x86 host test under ASan/UBSan, then a CPU bench
  on the hardware against the real-time block budget.
- Built on the open porting framework [sd88me/mpc-vst-plugins](https://github.com/sd88me/mpc-vst-plugins)
  (MIT), which supplies the VST2 wrapper, the parameter generator and the skin tooling.
- Every device claim in these repositories names the device and the firmware version, and the date it was
  tested.

Not affiliated with Akai Professional or inMusic. Product names are used only to say what the software
runs on.
