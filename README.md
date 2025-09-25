# NASA_SPACE_APP_TEAM_VOIDS

link for information about dataset :- https://exoplanetarchive.ipac.caltech.edu/docs/API_kepcandidate_columns.html#transit_prop

# 🌌 Kepler Exoplanet Dataset (NASA)

This repository contains the **Kepler cumulative dataset** provided by NASA’s Exoplanet Archive.  
It includes all **confirmed exoplanets**, **planetary candidates**, and **false positives** determined from Kepler’s transit photometry.

---

## 📊 Dataset Overview
- **Rows:** 9,564  
- **Columns:** 49  
- **Target Column (for supervised learning):**  
  `koi_disposition` → Labels are:
  - `CONFIRMED` → Verified exoplanet  
  - `CANDIDATE` → Likely a planet but not confirmed  
  - `FALSE POSITIVE` → Signal due to other astrophysical/technical reasons  
  - `NOT DISPOSITIONED` → Not yet evaluated  

---

## 🔑 Identifier Columns
| Column       | Description | Notes |
|--------------|-------------|-------|
| `kepid`      | Kepler Input Catalog ID (unique star ID) | Primary key for stars |
| `kepoi_name` | Kepler Object of Interest (KOI) ID | Format KNNNNN.DD (star + planet index) |
| `kepler_name`| Official planet name if confirmed | Empty if not confirmed |

---

## 🪐 Disposition Columns
| Column           | Description | Values |
|------------------|-------------|--------|
| `koi_disposition` | **Final classification** (target label) | CONFIRMED, CANDIDATE, FALSE POSITIVE, NOT DISPOSITIONED |
| `koi_pdisposition` | Disposition using only Kepler data | CANDIDATE, FALSE POSITIVE, NOT DISPOSITIONED |
| `koi_score`      | Confidence score (0–1) from Robovetter Monte Carlo tests | Higher = stronger classification |
| `koi_fpflag_nt`  | Not transit-like flag | 1 = noise/artifact, 0 = valid |
| `koi_fpflag_ss`  | Stellar eclipse flag | 1 = binary star, 0 = valid |
| `koi_fpflag_co`  | Centroid offset flag | 1 = source is background star |
| `koi_fpflag_ec`  | Ephemeris match contamination flag | 1 = signal matches another object |

---

## 📈 Transit & Orbital Parameters
| Column         | Units | Description |
|----------------|-------|-------------|
| `koi_period`   | days  | Orbital period (planet’s year) |
| `koi_time0bk`  | BJD-2454833 (days) | Epoch of first detected transit |
| `koi_impact`   | 0–1   | Impact parameter (central = 0, grazing = 1) |
| `koi_duration` | hours | Transit duration |
| `koi_depth`    | ppm   | Transit depth (brightness drop) |
| `koi_prad`     | Earth radii | Planetary radius |
| `koi_teq`      | K     | Equilibrium temperature of planet |
| `koi_insol`    | Earth flux | Insolation flux relative to Earth |
| `koi_model_snr`| —     | Signal-to-noise ratio of transit |
| `koi_tce_plnt_num` | integer | Candidate number in system |
| `koi_tce_delivname` | text | Pipeline data release version |

---

## 🌟 Stellar Parameters
| Column       | Units | Description |
|--------------|-------|-------------|
| `koi_steff`  | K     | Stellar effective temperature |
| `koi_slogg`  | log10(cm/s²) | Stellar surface gravity (dwarfs ~4.0–4.6, giants <3.5) |
| `koi_srad`   | Solar radii | Stellar radius |
| `koi_kepmag` | mag   | Kepler-band magnitude (brightness; smaller = brighter) |
| `ra`, `dec`  | degrees | Sky position of star |

---

## ⚙️ Usage for Machine Learning
- **Classification Task:** Predict `koi_disposition` (planet vs. candidate vs. false positive).  
- **Strong Predictive Features:**  
  - Transit features: `koi_depth`, `koi_duration`, `koi_prad`  
  - Disposition flags: `koi_fpflag_nt`, `koi_fpflag_ss`, `koi_fpflag_co`, `koi_fpflag_ec`  
  - Stellar parameters: `koi_steff`, `koi_slogg`, `koi_srad`  
  - Transit SNR: `koi_model_snr`  
- **Supporting Features:**  
  - RA/Dec (positional metadata)  
  - Delivery name (pipeline version)  

---

## 📂 Source
- Data: [NASA Exoplanet Archive – Kepler Objects of Interest]([https://exoplanetarchive.ipac.caltech.edu/](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=cumulative))  
- Documentation: Batalha et al. (2012), DR24/DR25 Robovetter KOI flags.  

---

## 🚀 Example Applications
- Binary classification: Planet (`CONFIRMED`) vs. False Positive  
- Multi-class classification: CONFIRMED vs. CANDIDATE vs. FALSE POSITIVE  
- Feature importance analysis: Which astrophysical parameters are most useful in determining planet status  
- Exoplanet habitability studies: Using `koi_prad`, `koi_insol`, and `koi_teq`  
