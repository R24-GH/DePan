Note
===========
Should be the last version of Depan when the repo was deleted. But I'm not 100% sure if the binary version matches the current source code. Forked from theChaosCoder's Jan 26, 2022 version to rebuild with the latest Vapourkit headers. You'll still need to change the paths in DePan.vcxproj to yours, since I used a portable install from Vapourkit to build it (I don't have a full install of Vapoursynth). The new build files for make were vibe coded to make it so you don't need to include a bunch of dll dependencies (no libgcc, libstdc++, or libwinpthread), it used to require to build with visual studio otherwise and I didn't want to update my visual studio installs at the moment.

Also note, I'm not a coder, so I won't be maintaining this in the future. I made this branch to have a public repo for a dll that works in the latest VS because the 2022 version from theChaosCoder was no longer working, and I needed the range parameter but I didn't want to continue using the Avisynth plugin in VS since it kept throwing prefetch errors and causing performance issues.

Feel free to fork for further development.

Description
===========

Tools for estimation and compensation of global motion (pan).

Apparently no longer requires libfftw3f-3.dll to be in the search path, purely by accident, but if it throws an error then it does (I have it in the path but ldd DePan.dll doesn't show it as dependency). http://www.fftw.org/install/windows.html

Ported from AviSynth plugin http://avisynth.org.ru/


Usage
=====

    depan.DePanEstimate(clip clip[, int range=0, float trust=4.0, int winx, int winy, int wleft=-1, int wtop=-1, int dxmax=winx/4, int dymax=winy/4, float zoommax=1.0, float stab=1.0, float pixaspect=1.0])

* range: Number of previous (and also next) frames (fields) near requested frame to estimate motion.

* trust: Limit of relative maximum correlation difference from mean value at scene change. (0.0 to 100.0)

* winx: Number of columns (width) of fft window. (default to the maximum power of 2 within frame width)

* winy: Number of rows (height) of fft window. (default to the maximum power of 2 within frame height)

* wleft: Left position (offset) of fft window. (-1 for auto centered window)

* wtop: Top position (offset) of fft window. (-1 for auto centered window)

* dxmax: Limit of x shift.

* dymax: Limit of y shift.

* zoommax: Maximum zoom factor. (if =1, zoom is not estimated)

* stab: Decreasing of calculated trust for large shifts.
  * 0.0 = not decrease
  * 1.0 = half at dxmax, dymax

* pixaspect: Pixel aspect.

---

    depan.DePan(clip clip, clip data[, float offset=0.0, float pixaspect=1.0, bint matchfields=True, int mirror=0, int blur=0, int[] planes])

* data: Service clip with coded motion data, produced by `depan.DePanEstimate`.

* offset: Value of compensation offset for all input frames (fields). (-10.0 to 10.0)
  * 0.0 = null transform
  * -1.0 = full backward motion compensation of next frame (field) to current
  * 1.0 = full forward motion compensation of previous frame (field)
  * -0.5 = backward semi-compensation of next frame (field)
  * 0.5 = forward semi-compensation of previous frame (field)
  * 0.3333333 = forward one-third compensation of previous frame (field)
  * -1.5 = backward semi-compensation of next next frame (field)
  * 2.0 = full forward motion compensation of previous previous frame (field)
  * and so on

* pixaspect: Pixel aspect.

* matchfields: Match vertical position of interlaced fields for preserve fields order, better denoising etc.

* mirror: Fill empty borders with mirrored from frame edge pixels (instead of black).
  * 0 = no mirror
  * 1 = top
  * 2 = bottom
  * 4 = left
  * 8 = right
  * sum any of above combination (15 = all)

* blur: Blur mirrored zone by using given max blur length (0 is no blur, the good value is above 30)

* planes: A list of the planes to process. By default all planes are processed.
