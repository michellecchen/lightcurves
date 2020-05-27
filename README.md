##### ASTR15 Final: Pt. 2 (DRAFT)

## Reproducing phase-folded light curves via aperture photometry

The False Positive Working Group (FPWG) produced its light curves for verified planet Kepler-1649b and planet candidate Kepler-1649c through **aperture photometry.** For each quarter, they selected — from a set of 20 different apertures — the one that produced the most photometrically precise curve. A noteworthy part of this process was accounting for potentially diluting flux from nearby stars, otherwise known as "contaminants."

This paper will perform **Simple Aperture Photometry** on a variety of cases to produce and compare light curves to those from Vanderburg et al. It uses `lightkurve`, a package for Kepler & TESS time series analysis, as well as `matplotlib`, a data visualization library, to do so.  Both run in Python 3.7. Because manual aperture optimization is beyond the scope of the following calculations, pixel optimization will be the focus of these calculations. In investigating why Kepler-1649c failed the Robovetter's Model-Shift Uniqueness test, the FPWG concluded that the fully automated pixel selection algorithm employed by the Kepler pipeline failed to account for the faintness of the host star, as well as its high proper motion. As a result, half-pixel errors resulted in Kepler-1649 remaining within the photometric aperture in some quarters, while falling outside of it entirely in others.

### Procedure: Simple Aperture Photometry

In this simplified rendition of aperture photometry,

1. All pixels from every quarter of the planet candidate's data set are downloaded;
2. A select few are chosen programmatically, eliminating as much flux contamination as possible;
3. The chosen pixels are summed to a singular flux value, as well as for every image per time slice;
4. The light curve is cleaned up: outliers are removed via simple sigma clipping, and long-term trends are flattened;
5. The light curve is finalized.

All data is extracted from the NASA Exoplanet Archive's Kepler database.

### Selecting cases of study

The primary intention of these calculations is to compare light curves of Kepler-1649b & c to representatives from the following categories:

1. A certified false positive (FP);
2. A confirmed planet whose host star exhibits *low* proper motion (ease of pixel optimization);
3. A confirmed planet whose host star exhibits *high* proper motion (difficluty in pixel optimization, much like Kepler-1649 itself);
4. And, lastly, an unclassified planet candidate (PC), so as to conclude with a non-automated dispositioning, modeling the work of Vanderburg et al.

### Case 1: Certified FP

[**KIC 892772**](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/DisplayOverview/nph-DisplayOverview?objname=KOI-1009&type=KEPLER_HOST) is a certified FP with a transit period of *5.09246539±4.39e-05* days.

Its initial light curve without pixel optimization revealed little about the FP's disposition.

![Initial light curve for KIC 892772](https://raw.githubusercontent.com/michellecchen/lightcurves/master/892772/892772-1.png)

Despite KIC 892772's certified FP status, one cannot discount the possibility that the signal has been nonetheless contaminated by a nearby star. Here, it becomes important to plot the target pixel files.

![Initial pixels](https://raw.githubusercontent.com/michellecchen/lightcurves/master/892772/892772-2.png)

The preselected region of pixels lie in the upper-left quadrant. This is unideal; as indicated by the legend, the flux is highest in the centre quadrant. Upon reselecting the pixels, selecting those in the center, a distinct improvement is made:

![Optimal pixels](https://raw.githubusercontent.com/michellecchen/lightcurves/master/892772/892772-3.png)

Upon "decontamination," or optimization, of pixel selection, the light curve can be rebuilt and cleaned.

![Optimal lightcurve](https://raw.githubusercontent.com/michellecchen/lightcurves/master/892772/892772-4.png)

This monotonically downward-sloping wave with low amplitude does not provide enough evidence for a planet signal. Therefore, the results from this finalized lightcurve are in agreement with the dispositioning of KIC 892772 as an FP.

### Case 2: Confirmed planet (low proper motion)



### Case 3: Confirmed planet (high proper motion)



### Case 4: Unclassified PC

This final case focuses on reproducing the rationales used by Vanderburg et al. in manually dispositioning Kepler-1649c as an exoplanet. It uses **KIC 6679295**, an unclassified PC, to do so.

[KIC 6679295](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/DisplayOverview/nph-DisplayOverview?objname=KOI-2862.01&type=KEPLER_CANDIDATE) has a transit period of *24.5752524±0.0001782* days. Its initial, unoptimized light curve, demonstrates a somewhat promising sinusoidal pattern.

![Initial lightcurve](https://raw.githubusercontent.com/michellecchen/lightcurves/master/6679295/6679295-2.png)

This lightcurve was produced using prior preselections of the central pixel quadrant. In this case, it is the unoptimal one. A brighter, "decontaminated" alternative lies in the upper-left quadrant.

![Initial pixels](https://raw.githubusercontent.com/michellecchen/lightcurves/master/6679295/6679295-1.png)

Pixel reselection reveals that the center quadrant was a good option, but not the best possible one. Therefore, upon optimization,

![Optimized pixels](https://raw.githubusercontent.com/michellecchen/lightcurves/master/6679295/6679295-3.png)

It is then possible to reproduce a cleaner lightcurve.

![Optimized light curve](https://raw.githubusercontent.com/michellecchen/lightcurves/master/6679295/6679295-4.png)