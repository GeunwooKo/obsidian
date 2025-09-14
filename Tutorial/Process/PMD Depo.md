# PMD Deposition (Pre-Metal Dielectric)

## 공정 개요
- **목적**: Gate electrode 상부 및 주변 절연층 형성으로 metal과의 절연 확보
- **구성**: PEOX(Plasma Enhanced Oxide) + BPSG(Boro-Phospho Silicate Glass) stack
- **중요성**: Inter-layer 절연, planarization, step coverage

## PMD Stack Structure
### PEOX (Plasma Enhanced Oxide) Layer
- **역할**: Conformal step coverage, dopant barrier
- **두께**: 3000-5000Å
- **장점**: 낮은 온도 deposition, 우수한 step coverage
- **단점**: 높은 stress, moisture absorption

### BPSG (Boro-Phospho Silicate Glass)
- **역할**: Gap filling, flow planarization
- **조성**: SiO2 + B2O3(2-4%) + P2O5(2-4%)
- **특징**: 낮은 flow temperature (850-950°C)

## PEOX Deposition Process
### PECVD (Plasma Enhanced CVD) Conditions
- **Chemistry**: SiH4 + N2O 또는 TEOS + O2
- **Temperature**: 350-450°C
- **Pressure**: 2-9 Torr
- **Power**: RF 13.56MHz, 200-800W

### Key Parameters
#### Gas Flow Control
- **SiH4 flow rate**: 100-300 sccm
- **N2O flow rate**: 1000-3000 sccm
- **N2O/SiH4 ratio**: Deposition rate vs. film quality 절충

#### Station Deposition Time (SDT)
- **Multi-station**: Uniformity 향상을 위한 rotation
- **Typical SDT**: 30-60초/station
- **Total thickness control**: Multi-pass deposition

### Film Properties Control
- **Refractive index**: 1.46 ±0.01 (film density 지표)
- **Stress**: <200 MPa compressive (wafer bow 최소화)
- **Wet etch rate**: BOE 대비 thermal oxide 기준

## BPSG Deposition and Flow
### Deposition Process
- **Method**: APCVD (Atmospheric Pressure CVD)
- **Temperature**: 400-450°C
- **Source**: TEOS + B2H6 + PH3

### Dopant Concentration Control
#### Boron Content (2-4%)
- **역할**: Flow temperature 저감, etch rate 제어
- **Too low**: 불충분한 flow characteristics
- **Too high**: Boron precipitation (boron rock)

#### Phosphorus Content (2-4%)
- **역할**: Moisture resistance, thermal stability
- **Interaction**: B-P complex formation
- **Optimization**: B/P ratio 최적화

### Flow Process
- **Temperature**: 850-950°C (B-P 농도에 따라)
- **Time**: 30-60분
- **Ambient**: N2 (oxidation 방지)
- **Ramp rate**: Controlled heating/cooling

## Critical Process Control
### Dopant Diffusion Management
- **Issue**: BPSG의 B, P가 하부막으로 확산
- **Solution**: PEOX barrier layer
- **Monitoring**: SIMS depth profiling

### Planarization Optimization
- **Flow temperature**: 충분한 유동성 확보
- **Film thickness**: Gap fill capability
- **Aspect ratio**: Step height/width 고려

### Boron Rock Prevention
- **Mechanism**: 과도한 B 농도로 인한 B2O3 precipitation
- **Prevention**: B 농도 <4%, 적절한 P 농도
- **Detection**: Surface roughness measurement

## Quality Control and Characterization
### Film Thickness Measurement
- **Tool**: Ellipsometry, Reflectometry
- **Uniformity**: 3σ < 3% (wafer 내)
- **Target**: Design rule에 따른 최적 두께

### Composition Analysis
- **FTIR**: B-O, P-O bond 분석
- **XPS**: Surface composition
- **SIMS**: Depth profile, dopant distribution

### Stress Measurement
- **Wafer bow**: Laser interferometry
- **Intrinsic stress**: Substrate curvature method
- **Target**: <200 MPa (wafer cracking 방지)

## Gap Fill and Planarization
### Gap Fill Mechanism
- **Viscous flow**: High temperature에서 glass transition
- **Surface tension**: 에너지 minimization
- **Wetting**: Substrate surface와의 interaction

### Planarization Efficiency
- **Definition**: (Step height reduction / Initial step height) × 100%
- **Target**: >80% planarization
- **Factors**: Film thickness, flow conditions, topology

## Advanced PMD Technologies
### High Density Plasma CVD
- **Advantage**: 향상된 step coverage, 낮은 damage
- **Chemistry**: SiH4/O2 또는 TEOS/O2
- **Process**: Decoupled source/bias power

### Spin-on Glass (SOG)
- **Application**: Gap fill enhancement
- **Material**: Siloxane-based polymer
- **Process**: Spin coating + thermal curing

### CMP (Chemical Mechanical Polishing)
- **목적**: Global planarization
- **Slurry**: SiO2 particle + alkaline solution
- **Endpoint**: Pattern recognition 또는 thickness monitoring

## Integration Challenges
### Thermal Budget Management
- **Issue**: Multiple high temperature steps
- **Solution**: Low temperature process 개발
- **Trade-off**: Film quality vs. thermal budget

### Contamination Control
- **Metal contamination**: Na, K < 10¹⁰ cm⁻²
- **Particle**: Class 1 environment
- **Moisture**: Controlled humidity <1%

### Pattern Loading Effects
- **Dense vs. isolated**: Deposition rate difference
- **Solution**: Process uniformity optimization
- **Monitoring**: Test pattern measurement

## Failure Analysis
### Poor Gap Fill
- **Symptom**: Void formation, incomplete fill
- **Cause**: 부족한 flow, 과도한 aspect ratio
- **Analysis**: Cross-section SEM, FIB

### Film Delamination
- **Symptom**: Film peeling, adhesion loss
- **Cause**: 과도한 stress, poor adhesion
- **Analysis**: Stress measurement, adhesion test

## 관련 공정
- **전단계**: [[13.0 GPoly]], Ion implantation
- **후단계**: [[17.0 SCONT]], [[21.0 GCONT]]

---
#SiC #PowerMOSFET #PMD #PEOX #BPSG #PECVD #FlowProcess #Planarization #GapFill #BoronRock #DopantDiffusion #StepCoverage #CMP #ProcessIntegration