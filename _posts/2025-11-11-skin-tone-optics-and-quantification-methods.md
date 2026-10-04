---
layout: post
title: Skin Tone Optics and Quantification Methods
date: 2025-11-11 12:00
description: Journal club on how light interacts with skin, why pulse oximeters under-perform on darkly pigmented patients, and how we can quantify skin pigmentation objectively.
tags: optics biophotonics colorimetry journal-club
categories: notes
featured: false
toc:
  beginning: true
---

I gave a journal club talk on **skin tone optics and quantification methods**, more specifically how light interacts with skin, why that interaction is wavelength-dependent and pigmentation-dependent, and what tools we actually have to measure pigmentation objectively. This post is a written-up version of that talk for anyone who'd like the highlights without the slide deck.

## Why this matters

The motivating fact for the entire talk is a 2020 [retrospective study from the University of Michigan](https://doi.org/10.1056/NEJMc2029240) showing a consistent **~2% measurement bias** in pulse oximetry between Black and white patients. The bias is small in absolute terms but matters clinically as it determines whether a patient is flagged as hypoxemic and whether they receive supplemental oxygen.

The bias arises because pulse oximetry uses two wavelengths (~660 nm red and ~940 nm NIR) and assumes a fixed relationship between absorption ratios and arterial oxygen saturation. **Melanin in the epidermis violates that assumption**: it absorbs strongly at 660 nm and only weakly at 940 nm, so for darker-pigmented skin the red channel is disproportionately attenuated *before* the light ever reaches blood. The device can't tell the difference between "less hemoglobin in the optical path" and "more melanin in the optical path."

<div class="row justify-content-center">
  <div class="col-md-8">
    {% include figure.liquid
       loading="eager"
       path="assets/img/posts/skin-tone/hemoglobin.png"
       class="img-fluid rounded z-depth-1"
       zoomable=true
       caption="Molar extinction coefficients of oxyhemoglobin (HbO₂) and deoxyhemoglobin (Hb), compiled by Scott Prahl from multiple sources. The vertical lines mark the 660 nm and 940 nm wavelengths used by pulse oximeters. Data: <a href=\"https://omlc.org/spectra/hemoglobin/\">omlc.org/spectra/hemoglobin</a>."
    %}
  </div>
</div>

This isn't an obscure problem. I run into the same issue building a smartphone-based [nailfold capillaroscopy](#) system for non-invasive hemoglobin estimation: I have to manually adjust exposure for darker-skinned subjects to keep capillary contrast within a usable range. The question that motivated the talk is: **how do we measure skin pigmentation objectively, so that we can correct for it rather than ignore it?**

## A 30-second tour of skin

<div class="row align-items-center mt-3">
    <div class="col-md-5 mb-3 mb-md-0">
        {% include figure.liquid
           loading="eager"
           path="assets/img/posts/skin-tone/skin-layers.jpg"
           class="img-fluid rounded z-depth-1"
           zoomable=true
        %}
        <div class="caption">
            Cross-section of human skin showing the epidermis, dermis,
            and hypodermis. <em>Image: Don Bliss / National Cancer Institute (public domain).</em>
        </div>
    </div>
    <div class="col-md-7" markdown="1">

Skin is a layered, turbid medium. From the surface down:

- **Epidermis** (~60–120 µm): protective barrier; contains melanocytes that produce **melanin** in melanosomes. Melanin is the dominant absorber in the visible range and the source of skin's color variation.
- **Dermis** (~90% of total thickness): collagen-and-elastin matrix, vascularized, this is where **hemoglobin** lives, and where most of the optical path length accumulates.
- **Hypodermis**: subcutaneous fat, larger nerves and vessels, and mostly invisible to short-wavelength visible light.

</div>

Light hitting skin does three things, in roughly this order: reflects off the surface, absorbs as it travels, and scatters along the way.


### Reflection (~4–7% of incident light)

The sebum layer of the epidermis has a refractive index of about 1.5; air is about 1.0. The Fresnel equations give roughly 4–7% specular reflection at this interface, which is small but makes skin look glossy and what creates the highlight problems in any handheld imaging system.

### Absorption 

Absorption is governed by the absorption coefficient $$\mu_a(\lambda)$$, and at visible wavelengths it's dominated by two chromophores:

**Melanin** (epidermis). Its absorption falls roughly exponentially with wavelength, high in the UV/blue, much lower in the NIR. Eumelanin (brown/black) and pheomelanin (red/yellow) are produced in different ratios across pigmentations. Critically, **melanocyte concentration doesn't change much with skin tone**, what changes is melanosome size, number, and the resulting volume fraction of melanin in the epidermis. Lower-pigmented skin sits around 1–2% melanosome volume fraction; darker skin can reach 40%+. That's a 20× swing in epidermal absorption from the same anatomy.

**Hemoglobin** (dermis). Both oxy- and deoxy-Hb have characteristic absorption peaks: a strong **Soret band around 400–420 nm** and **green–yellow bands around 540–600 nm**. At 660 nm, deoxy-Hb absorbs more than oxy-Hb (this is the foundation of pulse oximetry). At 940 nm, the relationship inverts.

Putting these together explains the pulse-ox bias quantitatively: at 660 nm, the red light has to traverse epidermal melanin first, and this loss is much larger for higher-pigmented skin than for lower. At 940 nm, melanin barely matters. So the ratio that the device interprets as oxygen saturation is partly driven by epidermal melanin, a signal it has no way to separate.

### Scattering

The remaining photons scatter on their way through tissue:

- **Mie scattering** dominates in the dermis, driven by filamentous proteins like collagen (and keratin in the epidermis) when scatterer size is comparable to wavelength.
- **Rayleigh scattering** dominates when scatterers are much smaller than wavelength.

The balance of Mie and Rayleigh, combined with absorption, determines the **effective optical path** that a photon takes, and most of that path is in the dermis.

## Measurement technique 1: Diffuse Reflectance Spectroscopy (DRS)

The cleanest way to characterize skin optics in the lab is diffuse reflectance spectroscopy. The setup is a broadband light source (halogen, xenon, or white LED), a fiber-optic probe with one delivery and one collection fiber, and a spectrometer.

You compute a calibrated, dimensionless diffuse reflectance:

$$
R_d(\lambda) = \frac{I_{\text{skin}}(\lambda) - I_{\text{background}}(\lambda)}{I_{\text{standard}}(\lambda) - I_{\text{background}}(\lambda)}
$$

where $$I_{\text{standard}}$$ is measured against a known reflectance standard. From $$R_d(\lambda)$$ you invert to recover $$\mu_a$$ and $$\mu_s'$$, typically using lookup tables built from Monte Carlo simulations of the specific probe geometry, since closed-form analytical models don't handle layered turbid media well.

DRS is **the reference technique**, accurate, well-characterized, but not exactly portable. Probes are handheld (the Konica Minolta CM700d is a common form factor), but you still need a spectrometer and a controlled measurement environment.

## Measurement technique 2: Colorimetry and the Individual Typology Angle (ITA)

Colorimeters trade spectral resolution for simplicity. They measure tristimulus values through broad-band filters, then convert to **CIELAB color space** under a standard illuminant (typically D65) and observer (10°). CIELAB encodes color as:

- $$L^*$$, lightness, 0 (black) to 100 (white)
- $$a^*$$, green (–) to red (+)
- $$b^*$$, blue (–) to yellow (+)

The widely used summary metric is the **Individual Typology Angle**:

$$
\mathrm{ITA} = \frac{180}{\pi}\arctan\!\left(\frac{L^* - 50}{b^*}\right)
$$

ITA is the angle subtended from the reference point $$(L^*=50, b^*=0)$$ to the measured $$(L^*, b^*)$$ in the $$L^*$$–$$b^*$$ plane. Empirically, skin tones cluster along a banana-shaped curve in this plane, lighter tones in the upper-right region, darker tones lower-left. As melanin content increases, ITA decreases.

### The catch: ITA is blood-confounded

This was the most interesting paper in the talk for me. Harunani et al. (SPIE 2025) used a [three-layer skin model](https://doi.org/10.1117/12.3044143), epidermis, dermis, background, driven by published absorption and scattering spectra and inverted via the adding-doubling method. They asked a clean causal question: hold one chromophore constant, sweep the other, and see where the resulting reflectance lands in $$L^*$$–$$b^*$$.

Two findings that change how you should think about ITA:

1. **Iso-melanin curves form the empirical banana.** Holding melanin fixed and sweeping blood volume fraction (0.2%–7%) traces a smooth curve in $$L^*$$–$$b^*$$. Each melanin level produces its own non-overlapping curve. Stack the curves together and you reproduce the empirically observed banana, with low-melanin skin at the top and high-melanin skin at the bottom.

2. **Blood volume slides points along those curves.** Vascular maneuvers (occlusion, congestion) change ITA without changing melanin at all. So ITA is a useful *summary*, but it's not a clean estimate of melanin specifically.

Layer thickness matters too, but asymmetrically. Epidermal thickness (60→120 µm) shifts the whole distribution noticeably, while dermis thickness (1→2 mm) has a much smaller effect. Epidermal structure dominates color variation.

The practical implication: if you actually want to infer chromophore concentrations from color, **use the iso-melanin / iso-blood family of curves rather than ITA alone**. ITA is a coarse one-dimensional projection of an inherently two-dimensional problem.

## Measurement technique 3: Smartphone colorimetry

Burrow et al. (Biophoton. Discovery 2025) showed you can [approximate a professional colorimeter with an iPhone 11](https://doi.org/10.1117/1.BIOS.2.3.032504), if you control the conditions carefully. Their pipeline (the "SITA" algorithm):

1. Capture an RGB image of the finger at fixed distance (~7 cm) and angle (perpendicular).
2. Pick a square ROI within the image (typically 3024×3024, 8-bit JPEG).
3. Normalize RGB to [0, 1] and gamma-linearize per the sRGB transfer function.
4. Convert linear RGB to **CIE XYZ** (1931 standard observer).
5. Normalize by the D65 reference white and convert to **CIELAB** with the standard nonlinearity.
6. Compute ITA per pixel and average across the ROI.

Compared against a benchtop DSM-4 colorimeter, smartphone-derived ITA agreed reasonably well, but only under controlled conditions:

- **Anatomic site matters.** The dorsal side of the finger spans a wider ITA range than the palmar side, making it more discriminative for skin-tone classification.
- **Exposure drift.** As exposure increases, the ITA distribution shifts toward more positive values and broadens, your skin "lightens" numerically even though biology hasn't changed.
- **Lighting matters.** The most stable agreement with the reference colorimeter came under **ambient lights off, flash off**, with exposure ≈ 0.7 in their setup.

The takeaway: smartphone colorimetry is a feasible path to scalable, low-cost skin-tone assessment, but you have to *fix* exposure, geometry, white balance, and ambient lighting, and ideally calibrate per-device. A free-running auto-exposure smartphone capture is essentially uncalibrated.

## A note on Fitzpatrick and Monk

Two qualitative skin-color scales worth knowing about:

- **Fitzpatrick (I–VI)**: classifies skin by its tanning/burning response to UV. Widely used in dermatology, but only six bins, with the upper end (V, VI) covering a huge range of darker pigmentations.
- **Monk Scale (1–10)**: developed by Ellis Monk, [adopted by Google](https://skintone.google/) for technology evaluation. Ten shades, with broader coverage of darker tones. Research suggests it's more inclusive and more reliable for human-rater classification of medical and consumer technologies.

Neither replaces a quantitative measurement (DRS, colorimeter, ITA), but Monk in particular is a reasonable choice when you need a discrete categorical variable, for stratifying clinical trials, training datasets, or human-rater protocols.

## Take Aways:

- **The pulse-ox bias has a clean physical explanation.** It isn't a black box. Once you've sat with the melanin and hemoglobin absorption spectra, the bias is almost predictable.
- **One number is rarely enough.** ITA is a one-dimensional summary of a two-dimensional space; collapsing $$L^*$$ and $$b^*$$ into a single angle confounds melanin with blood volume in ways that matter clinically.
- **Practical smartphone-based measurement is feasible.** The hard part is consistency, fixed geometry, fixed exposure, controlled lighting. Anything that varies the optical path or the camera response variance becomes an uncontrolled covariate.
- **Calibration is upstream of fairness.** A lot of the conversation around algorithmic bias in medical imaging starts with the model. The deeper problem is often that the *measurement itself* is pigmentation-dependent before any algorithm sees the data.

This is also why I find it useful in my own work to treat skin tone (and exposure, and finger curvature, and ambient light) as **first-class design variables**, not as nuisance factors to correct for downstream.

---

### Key references

- Sjoding et al. *Racial Bias in Pulse Oximetry Measurement.* NEJM, 2020. [doi:10.1056/NEJMc2029240](https://doi.org/10.1056/NEJMc2029240)
- Vasudevan et al. *Melanometry for objective evaluation of skin pigmentation in pulse oximetry studies.* Communications Medicine, 2024. [doi:10.1038/s43856-024-00550-7](https://doi.org/10.1038/s43856-024-00550-7)
- Bajrami et al. *Human Skin Models in Biophotonics.* Adv. Healthcare Materials, 2025. [doi:10.1002/adhm.202501894](https://doi.org/10.1002/adhm.202501894)
- Harunani et al. *Establishing a causal link between the physiologic range of skin chromophore concentrations and physiologically relevant regions of CIELAB color space.* SPIE 13317, 2025. [doi:10.1117/12.3044143](https://doi.org/10.1117/12.3044143)
- Burrow et al. *Smartphone tristimulus colorimetry for skin-tone analysis at common pulse oximetry anatomical sites.* Biophoton. Discovery, 2025. [doi:10.1117/1.BIOS.2.3.032504](https://doi.org/10.1117/1.BIOS.2.3.032504)
- Putcha et al. *Characterizing the influence of skin pigmentation on pulse oximetry.* Biophoton. Discovery, 2025. [doi:10.1117/1.BIOS.2.3.032506](https://doi.org/10.1117/1.BIOS.2.3.032506)
- Del Bino & Bernerd. *Variations in skin colour and the biological consequences of UV exposure.* British Journal of Dermatology, 2013. [doi:10.1111/bjd.12529](https://doi.org/10.1111/bjd.12529)

[Open Oximetry: Skin Color Quantification](https://openoximetry.org/skin-color-quantification/) is also a great living reference for this topic, with up-to-date reviews of the measurement landscape.
