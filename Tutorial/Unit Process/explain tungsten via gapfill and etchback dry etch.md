
The tungsten via gapfill and etchback dry etch process are crucial steps in forming reliable interconnections in semiconductor manufacturing, especially as device geometries shrink and aspect ratios grow.

## Tungsten Via Gapfill Process

**Gapfill** refers to filling high-aspect-ratio vias (vertical holes or trenches) with tungsten metal to create vertical electrical connections between wiring layers in an integrated circuit. The gapfill process sequence typically includes:

- **Liner Deposition:** A thin barrier/liner layer (commonly titanium nitride, TiN) is deposited via physical vapor deposition (PVD) or atomic layer deposition (ALD) to prevent tungsten from diffusing into surrounding materials and ensure good adhesion.
- **Tungsten Nucleation Layer:** Sometimes, a very thin tungsten or other nucleation layer is deposited by ALD to promote uniform tungsten growth.
- **Bulk Tungsten Deposition:** Tungsten is deposited by chemical vapor deposition (CVD), using precursors like tungsten hexafluoride (WF₆) and hydrogen. This step fills the via, ideally without seams or voids even in high-aspect-ratio structures (aspect ratio 8:1 to 15:1).
- **Rapid Thermal Process (RTP) Anneal:** After CVD, an RTP step may be applied to reduce film stress and lower resistivity in the tungsten fill. This anneal typically occurs at 700°C–900°C for 60 seconds or more[^1].

The deposition parameters—such as temperature, pressure, WF₆/H₂ ratio, and use of seam-suppressed (SS) CVD tungsten—are optimized to improve fill quality, minimize voids, and ensure the tungsten remains boron-free to prevent adhesion problems[^1].

## Etchback Dry Etch Process

After tungsten gapfill, excess tungsten deposits on the field (surface) areas above the intended vias. The purpose of **etchback** is to remove this excess (field) tungsten while leaving the fill inside the vias intact:

- **Dry Etch (Plasma Etch):** A plasma- or reactive-ion etch (RIE) process selectively removes field tungsten. This typically uses fluorine-based chemistries (e.g., SF₆ or NF₃ plasmas) that react with tungsten to form volatile byproducts like WF₆, which are pumped away. Process control ensures the via fill is not attacked or damaged during field removal.
- **Final Residue Removal:** Sometimes, an additional cleaning or overetch step is needed to clear any tungsten or residue from field or via top surfaces, often using a chlorine/oxygen (Cl₂/O₂) plasma or a short wet clean.

The combination of gapfill and etchback delivers an electrically reliable, void-free tungsten plug or via, with excellent topography for subsequent layers[^1].

## Key Takeaways

- **Gapfill** ensures the via is fully filled by tungsten, even for extremely narrow and deep features.
- **Etchback** selectively removes unwanted tungsten from the wafer surface without damaging the via fill.
- Both steps require careful recipe control for uniformity, minimal defects, and electrical integrity—especially critical as device scaling pushes aspect ratios higher.

This integrated process flow is a mainstay of advanced CMOS and memory device fabrication, and ongoing innovations allow for improved resistivity, stress control, and reliability in ever-smaller device nodes[^1].
<span style="display:none">[^2][^3][^4][^5][^6][^7][^8][^9]</span>

<div style="text-align: center">⁂</div>

[^1]: https://patents.google.com/patent/US20220336274A1/en

[^2]: https://news.skhynix.com/semiconductor-front-end-process-episode-6/

[^3]: https://www.sciencedirect.com/science/article/abs/pii/S0169433222013149

[^4]: https://patents.google.com/patent/KR100607756B1/en

[^5]: http://patentscope.wipo.int/search/en/WO2025165603

[^6]: https://www.ips.co.kr/jp/business/product.php?board_code=product\&product_category=Semiconductor\&page_type=view\&idx=337

[^7]: https://sst.semiconductor-digest.com/2016/08/extending-tungsten-metallization-for-next-generation-devices/

[^8]: https://www.sciencedirect.com/science/article/am/pii/S0169433222013149

[^9]: https://www.eetimes.com/applieds-tungsten-process-fights-resistance/

