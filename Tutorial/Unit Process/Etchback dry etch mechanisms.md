<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Etchback dry etch mechanisms

The etchback dry etch process in semiconductor manufacturing employs plasma-based techniques to selectively remove excess tungsten from the wafer surface (field), especially after via or contact gapfill. The underlying mechanisms of dry etchback can be summarized as follows:

## Main Etchback Dry Etch Mechanisms

### 1. Physical Sputtering (Ion Bombardment)

- **Process:** Positively charged ions, accelerated in the plasma sheath, physically bombard the wafer surface.
- **Effect:** This bombardment leads to anisotropic (directional) removal of material, usually in the vertical direction, which is necessary to avoid lateral (sidewall) etching and protect the filled vias or features.
- **Role in Etchback:** Helps achieve clean removal of surface tungsten while minimally attacking the via fill, producing sharp feature profiles[^1][^3].


### 2. Chemical Reaction (Radical-Induced Etching)

- **Process:** Neutral radicals (such as fluorine species from SF₆ or NF₃) generated in the plasma chemically react with tungsten on the wafer surface.
- **Effect:** The reaction forms volatile byproducts (e.g., tungsten hexafluoride, WF₆) that can be easily pumped away from the chamber.
- **Control:** The degree of chemical vs. physical component varies with process gases, pressures, and plasma power.
- **Role in Etchback:** Ensures efficient and selective tungsten removal, especially when fine-tuning for the selective removal of field material without attacking underlying barriers or tungsten in the via[^3][^1].


### 3. Combined Mechanism: Reactive Ion Etching (RIE)

- **Mechanism:** Most modern etchback processes utilize a blend of the above two methods, known as reactive ion etching.
- **Outcome:** This combination allows precise control of anisotropy (verticality) and selectivity (preference to tungsten over barrier or dielectric).
- **Fine Control:** Adjusting parameters such as RF power, pressure, and gas mixture enables fine-tuning between chemical and physical etch contributions to optimize via fill retention, minimize residues, and yield vertical sidewalls[^3].


## Additional Mechanistic Considerations

- **Etch Profile Control:**
    - Lower chamber pressures and higher bias power produce more directional ion bombardment, resulting in vertical sidewalls and effective etchback.
    - Higher pressures favor isotropic (less directional) removal, possibly leading to unwanted lateral etching[^1].
- **Microloading \& Footing Effects:**
    - Dense regions of the wafer may etch more slowly than open areas (microloading).
    - Footing (or notching) can occur at the interface between tungsten and underlying layers; precise control of the dry etch recipe helps to minimize these artifacts[^1].
- **Residue Management:**
    - Overetch steps with specific chemistries (such as Cl₂/O₂) are often employed to clear residues and optimize the etched profile after the main etchback[^3].


## Summary Table: Key Aspects of Etchback Dry Etch Mechanisms

| Mechanism | Main Feature | Typical Impact |
| :-- | :-- | :-- |
| Physical Sputtering (Ions) | Anisotropic etching | Vertical profiles, controlled removal |
| Chemical Etching (Radicals) | Selectivity/Volatility | Efficient tungsten removal |
| RIE (Combined) | Fine profile control | Balance of directionality and selectivity |

In summary, the etchback dry etch process relies on a synergistic combination of ion-driven (physical) and radical-driven (chemical) material removal, optimized for selective and anisotropic tungsten field removal without damaging underlying structures[^1][^3].
<span style="display:none">[^2][^4][^5][^6][^7][^8]</span>

<div style="text-align: center">⁂</div>

[^1]: https://ocw.snu.ac.kr/sites/default/files/NOTE/5365.pdf

[^2]: https://www.ncabgroup.com/blog/pcb-etchback-processes/

[^3]: https://castorpollux1010.tistory.com/58

[^4]: https://www.jkps.or.kr/journal/download_pdf.php?doi=10.3938%2Fjkps.58.467

[^5]: https://link.springer.com/chapter/10.1007/978-3-319-10295-5_2

[^6]: https://spl.skku.ac.kr/_res/pnpl/etc/2015-03.pdf

[^7]: https://www.kssse.or.kr/html/?pmode=journal2\&smode=view\&seq=1313

[^8]: http://contents.kocw.net/KOCW/document/2014/Chungbuk/parkkeunhyung/11.pdf

