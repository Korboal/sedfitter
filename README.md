This fork's version
------

This fork is based on the original code with some small changes (based on sedfitter 1.3.0). First of all, it works with more recent Python and packages. Tested and works specifically on my Windows 10 machine with:
- Python 3.8
- astropy 5.0
- numpy 1.21.4
- matplotlib 3.5.0

I expect that some things might break again in astropy 5.1, based on the warning message. However, this fork version worked for both Robitaille et al., 2007 and 2017 models. I have used a fast Windows machine for it.

Changes made from original code (with location so easy to revert):
- Plotting did not work because numpy array for "lines" did not support two different astropy units at once. Change: dividing by the unit before adding to the numpy array. That way there are no units, thus no problem. Plots seem to work fine, because it seems like they were plotting the actual value anyway. Not sure if it might be breaking anything else eventually though... (sedfitter/plot.py lines 322-325)
- Make directory function changed to os.mkdir, because the original implementation did not create a directory for me on Windows machine (but it did on MacOS?). Not sure if works in every case. (sedfitter/utils/io.py lines 16-17)
- If aperture is too small, then instead of raising Exception it increases those apertures to minimum value. That way implementation is exactly the as for the "too big" apertures: it is brought to a limit value. How good is it from physics/modelling? I will let you decide for yourself. Either way, it is not like using too small aperture allows one to run the code. But be careful that the Exception is not raised anymore for too small apertures. (sedfitter/convolved_fluxes/convolved_fluxes.py lines 314-315 AND sedfitter/sed/sed.py lines 359-360 and 382-383)
- Weights are not taken into account, at least for now. The change is purely experimental, but I did it while testing when the library was not working well. Can be easily turned back on in sedfitter/source/source.py on lines 178 and 189. Uncomment the last parts of the line to bring weights back. No weights might result in overfitting (and thus very low chi-squared). Weights result in quite big chi-squared and models that do not always go through all data points perfectly. I might bring them later, then I will delete this bullet point. (sedfitter/source/source.py lines 178, 189)
- Numpy change where logspace requires integer as one of the parameters, n_distances in this case. Change is exactly the same as in pull request by keflavich on 4th May 2021 (as of me writing this, the pull was not accepted yet). The change is to round off the float value of n_distances using int(np.ceil(n_distnances)). (sedfitter/models.py lines 173, 258)

If you use this fork, be aware of those changes. I do not claim that they are correct or not. But they make the code running with later package changes. Feel free to use, but I cannot gurantee that I will maintain this fork.

Now the original text from the library:

About
-----

This is a Python port of the SED fitter from [Robitaille et al., 2007, ApJS 169
328](http://adsabs.harvard.edu/abs/2007ApJS..169..328R). It replaces the
original Fortran code which can be found
[here](https://github.com/astrofrog/sedfitter-legacy).

At this stage, it should be used cautiously as it is still beta-quality
software and may contain bugs. Please make sure that you verify your results
using an independent method if at all possible.

The documentation is available at http://sedfitter.readthedocs.org


Status
------

[![Build Status](https://travis-ci.org/astrofrog/sedfitter.svg?branch=master)](https://travis-ci.org/astrofrog/sedfitter)
[![Coverage Status](https://coveralls.io/repos/astrofrog/sedfitter/badge.svg)](https://coveralls.io/r/astrofrog/sedfitter)
