##### ASTR15 Final: Original Calculations

## Reproducing phase-folded light curves via aperture photometry

The False Positive Working Group (FPWG) produced its light curves for verified planet Kepler-1649b and planet candidate Kepler-1649c through **aperture photometry.** For each quarter, they selected — from a set of 20 different apertures — the aperture that produced the most photometrically precise curve. A noteworthy part of this process was accounting for potentially diluting flux from nearby stars, otherwise known as "contaminants."

This paper will perform **Simple Aperture Photometry** on a variety of cases to produce and compare light curves to those from Vanderburg et al.'s FPWG. To this end, it will use `lightkurve`, a package for Kepler & TESS time series analysis, as well as `matplotlib`, a data visualization library, to do so. Both run in Python 3.7. Because manual aperture optimization is beyond the scope of the following calculations — rather, these calculations will simply use the *Kepler* pipeline aperture — pixel optimization will be the focus of these calculations. In investigating why Kepler-1649c failed the Robovetter's Model-Shift Uniqueness test, the FPWG concluded that the fully automated pixel selection algorithm employed by the Kepler pipeline failed to account for the faintness of the host star, as well as its high proper motion. As a result, half-pixel errors resulted in Kepler-1649 remaining within the photometric aperture in some quarters, while falling outside of it entirely in others; thus, demonstrating the importance of optimizing pixels, both programmatically and via human inspection.

### Procedure: Simple Aperture Photometry (SAP)

To emulate aperture photometry via SAP,

1. All pixels from every quarter of the planet candidate's data set are downloaded;
2. A select few are chosen programmatically, eliminating as much flux contamination as possible;
3. The chosen pixels are summed to a singular flux value, as well as for every image per time slice;
4. The light curve is cleaned up via a set of functions:
    - `flatten()`, which removes long-term trends using a [Savitzky-Golay filter](https://en.wikipedia.org/wiki/Savitzky%E2%80%93Golay_filter);
    - `remove_outliers()`, which uses simple sigma clipping;
    - `remove_nans()`, which removes illegitimate infinite/Not-a-Number values;
    - `fold()`, which folds the data at a particular period;
    - and `bin()`, which calculates the average flux in each bin to reduce the overal time resolution of the plot;
5. And, at last, the light curve is finalized.

All data is extracted from the NASA Exoplanet Archive's cumulative [Kepler Objects of Interest (KOI)](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=cumulative) database.

### Selecting cases of study

The primary intention of these calculations is to compare the FPWG's light curve of Kepler-1649c to representatives from the following categories:

1. A certified false positive (FP);
2. A confirmed planet whose host star exhibits *low* proper motion (ease of pixel optimization);
3. A confirmed planet whose host star exhibits *high* proper motion (difficulty in pixel optimization, much like Kepler-1649 itself);
4. And, lastly, an unclassified planet candidate (PC), so as to conclude with a non-automated dispositioning, modeling the work of Vanderburg et al.

### Case 1: Certified FP

[**KIC 892772**](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/DisplayOverview/nph-DisplayOverview?objname=KOI-1009&type=KEPLER_HOST) is a certified FP with a transit period of *5.09246539±4.39e-05* days.

Its initial light curve contains highly inconsistent amounts of photometric scatter with little to no transiting signs. So far, the FP disposition goes unchallenged.

![Initial light curve for KIC 892772](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_1/892772-1.png)

Despite KIC 892772's certified FP status, it would be procedurally sound to account for the possibility that the signal has been contaminated by a nearby star. Therefore, it is important to plot the target pixel files.

![Initial pixels](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_1/892772-2.png)

The preselected region of pixels lie in the upper-left quadrant. However, high flux from the central quadrant indicates chances of contamination. By reselecting the pixels to cover this central quadrant, one can investigate the source of interference.

![Optimal pixels](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_1/892772-3.png)

This produces the following light curve:

![Optimal lightcurve](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_1/892772-4.png)

This monotonically downward-sloping wave with low amplitude similarly does not provide evidence for a planet signal. The results from these lightcurves are in agreement with the dispositioning of KIC 892772 as an FP.

### Case 2: Confirmed planet (low proper motion)

[**Kepler-227b**](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/DisplayOverview/nph-DisplayOverview?objname=K00752.01&type=KEPLER_CANDIDATE) (KIC 10797460) is a confirmed planet with an orbital period of *9.48803557±2.775e-05* days. Its host star, Kepler-227, carries a low proper motion of *5.00003* mas/yr. (This calculation employed data from [SIMBAD](http://simbad.u-strasbg.fr/simbad/sim-id?Ident=KIC+10797460&submit=submit+id), with a provided *μ<sub>α*</sub> of *1.853* mas/yr and μ<sub>δ</sub> of *4.644* mas/yr.) As a PC, Kepler-227b was dispositioned with a score of 1.0000, indicating high confidence in a PC classification.

![Initial lightcurve](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_2/10797460_updated.png)

Upon plotting the target pixel files, we find that the reading is isolated. Because there is no risk of contamination, no reselection is required.

![Pixels](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_2/10797460-2.png)

### Case 3: Confirmed planet (high proper motion)

[**Kepler-42c**](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/DisplayOverview/nph-DisplayOverview?objname=K00961.02&type=KEPLER_CANDIDATE) (KIC 8561063) is a confirmed planet with a short orbital period of *0.453287416±1.02e-07* days (10.9 hours) whose host star, located in the Cygnus constellation, carries a renownedly high proper motion of *427.682* mas/yr. (Again, this was calculated via [SIMBAD](http://simbad.u-strasbg.fr/simbad/sim-id?Ident=KIC+8561063&NbIdent=1&Radius=2&Radius.unit=arcmin&submit=submit+id), which provided μ<sub>α*</sub> of *93.126* mas/yr and μ<sub>δ</sub> of *-417.420* mas/yr.)

As a PC, Kepler-42c was initially assigned a disposition score of 0.0000, indicating high confidence in the automated FP classification, as well as further suggesting a direct correlation between high stellar proper motion and difficulties in aperture/pixel optimization. However, this classification was disproven by Muirhead et al. in the [2012 publication](https://ui.adsabs.harvard.edu/abs/2012ApJ...747..144M/abstract) "Characterizing the Cool KOIs. III. KOI 961: A Small Star with Large Proper Motion and Three Small Planets," which dispositioned all three transit signals detected around Kepler-42 as true planets. (They came to be known as Kepler-42b, Kepler-42c, and Kepler-42d, respectively.)

Due to low orbital period as well as high stellar PM, producing the lightcurve for Kepler-42c requires additional fine-tuning. Upon downloading pixel files from the fourth quarter, adjusting the x- and y-axes, and taking additional steps to flatten, fold, and remove outliers & infinite/NaN values, the following result was produced:

![Light curve](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_3/8561063-1.png)

There is a clear transit signal at phase ~0.325. To be certain of no interfering fluxes, the target pixel files are also plotted — again, with the aperture mask in red:

![Target pixel files](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_3/8561063-2.png)

There are dim fluxes adjacent to the mask, but nothing bright enough so as to be capable of a misleading signal. Therefore, all plots agree with the original dispositioning of Kepler-42c as a bona fide planet.

### Case 4: Unclassified PC

This final case focuses on reproducing the rationales used by Vanderburg et al. in manually dispositioning Kepler-1649c as an exoplanet. It uses **KIC 6679295**, an unclassified PC, to do so.

[KIC 6679295](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/DisplayOverview/nph-DisplayOverview?objname=KOI-2862.01&type=KEPLER_CANDIDATE) has a transit period of *24.5752524±0.0001782* days. Its initial, unoptimized light curve, demonstrates promising evidence of a transit.

![Light curve](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_4/6679295-4.png)

It becomes especially critical here to decontaminate the light curve. Upon plotting the target pixel files,

![Target pixel files](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_4/6679295-1.png)

The central quadrant has been masked; however, there is a nearby bright contaminant in the upper-leftmost quadrant. It is worth investigating, so one must remask to pursue this inquiry.

![Contaminant pixels](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_4/6679295-3.png)

Doing so allows one to then produce a light curve from the contaminant:

![Contaminant lightcurve](https://raw.githubusercontent.com/michellecchen/lightcurves/master/case_4/6679295-2.png)

Which reveals no signs of a transit.

It becomes clear that only KIC 6679295 has a planet signal. Therefore, it is with high confidence that one can disposition KIC 6679295 as a true planet, and not an FP.

### Analysis

Aperture photometry does require some element of human inspection for optimal performance: in selecting apertures (in the case of the FPWG), or in selecting target pixels, as shown prior. Additionally, stellar parameters such as high proper motion and faintness weaken the detectability of transit signals around host stars; these should be taken into account when producing phase-folded light curves, for as seen with Case 3's Kepler-42, such factors necessitate additional optimization of the curve. Otherwise, photometric scatter will obscure the transit. This is what led originally to Kepler-1649c being dispositioned with a score of 0.0000, after all.

### Final thoughts

While automated vetting procedures such as the Robovetter's Model-Shift Uniqueness Test are helpful in parsing hefty data sets, they sacrifice individual disposition correctness for statistical uniformity, leading to incorrect vettings such as Kepler-1649c — and Kepler-42c. Therefore, human inspection should be applied to strategically elected data samples from these sets. This would enable the field of astronomy to continually improve its algorithms so as to account for influential stellar parameters. By examining unique candidates from these samples, astronomers can learn more about the properties of exoplanets and their habitability.

### References

The FPWG referenced throughout is composed of Vanderburg et al.'s team of transatlantic astronomers; their 2020 publication, which was used as inspiration for these calculations, is "A Habitable-zone Earth-sized Planet Rescued from False Positive Status." It can be found [here](https://iopscience.iop.org/article/10.3847/2041-8213/ab84e5) at *The Astrophysical Journal Letters*.