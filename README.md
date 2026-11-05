# NightMaa — Indoor Night-Heat Protection for Mothers

**One-line pitch:** NightMaa estimates tonight's *indoor* heat in a pregnant woman's home — from live weather data and the kind of home she lives in — and turns it into a simple, safe plan for the night, in **eight languages**, on any phone, anywhere in the world.

Built for the **MIT Solve 2027 Global Climate Challenge** (Extreme Climate Hazard Protection — heat), by **Akarsh Pandey**, Indore, India.

## The problem

Every heat warning describes the *outdoor afternoon*. The newest research says the dangerous exposure is different: **indoors, at night, in the homes of pregnant women** who cannot leave.

- **Chennai sensor study (Climate Trends, Oct 2025–Apr 2026):** minute-level sensors in 50 low- and middle-income Chennai homes found indoor temperatures **frequently above 32 °C and rarely below 31 °C even at night**, peaking around **8–9 pm at ~34.7 °C** as concrete walls re-radiate stored heat. The worst homes logged **5,700–5,800 hours above 32 °C** — and the study period excluded peak summer. The researchers note indoor nighttime heat *"is not systematically monitored in any major city."*
  ([Ground Report coverage](https://www.groundreport.in/extreme-weather/inside-indias-homes-heat-never-leaves-not-even-at-night/))
- **Nature Medicine meta-analysis (198 studies, 66 countries):** odds of preterm birth rise **1.04 per +1 °C** and **1.26 during heatwaves**; odds of any obstetric complication 1.25 during heatwaves.
  ([Nature Medicine](https://www.nature.com/articles/s41591-024-03395-8))
- **Tamil Nadu cohort (BJOG 2024, n = 800 pregnant women doing physical work):** 47.3% had high occupational heat exposure; heat-exposed women had **adjusted OR 2.4 for miscarriage** and **2.3 for any adverse pregnancy outcome**.
  ([PubMed](https://pubmed.ncbi.nlm.nih.gov/37814395/))
- **The policy blind spot:** India has **300+ Heat Action Plans — none mandates indoor temperature monitoring.** No deployed tool converts an outdoor forecast into a pregnancy-specific, home-specific night risk, or tells a frontline health worker which of her mothers to visit first tomorrow.

## How it works

1. **Pick a place** — preset cities across India and the world (Indore, Chennai, Delhi, Ahmedabad, Nagpur, Patna, Dhaka, Karachi, Lagos, Phoenix), search **any city in the world** (Open-Meteo Geocoding API), or tap **"Use my location"** (browser geolocation; the place label comes from the free BigDataCloud reverse-geocode service, with a graceful fallback to coordinates).
2. **Describe the home** — roof type, floor, ventilation, cooling available (four taps).
3. **Live forecast** — hourly temperature, humidity and feels-like temperature for 7 days from the Open-Meteo Forecast API (free, no key).
4. **Indoor estimate** — a transparent offset model converts the outdoor curve into an estimated indoor curve for that home:

   | Factor | Adjustment |
   |---|---|
   | Tin / asbestos roof | +2.5 °C |
   | Concrete slab roof | +1.5 °C |
   | Tile / thatch roof | +0.5 °C |
   | First floor / second floor or above | +0.4 / +0.8 °C |
   | Poor ventilation / good cross-ventilation | +0.7 / −0.5 °C |
   | Cooler / AC | −2 / −4 °C |
   | Fan only | −0.3 °C (perceived cooling) |

   The estimate is clamped so it never falls more than 5 °C below the outdoor temperature.
5. **Outputs for tonight (18:00–07:00)** — an hour-by-hour chart of estimated indoor vs. outdoor temperature, the night's minimum indoor temperature, hours spent above 32 °C indoors, a **pregnancy heat-risk band** (Low / Moderate / High / Very High), a 3-night outlook, and a personalised advisory covering room cooling, hydration, sleep setup, ventilation timing and **danger signs** that mean "contact your ASHA worker or doctor immediately."
6. **Eight languages, °C or °F** — English, हिन्दी, বাংলা, Español, Français, Português, اردو and العربية, with a full right-to-left layout for Urdu and Arabic. The °C/°F toggle converts every displayed temperature (chart, tiles, outlook, shared plan); all risk maths stays in °C internally.
7. **Share tonight's plan** — one tap builds a concise text summary (place, night, estimated indoor minimum, risk band, hours above 32 °C, three key actions, disclaimer) to **copy** or send via **WhatsApp** — built for a health worker forwarding the plan to a family.
8. **For frontline health workers** — the whole check takes about a minute inside a routine home visit: no login, no install, no cost. In India this is the ASHA worker; the same check is written for community health workers, midwives and Anganwadi workers everywhere. Homes showing High or Very High risk are the next morning's first visits.

## New in v2

### Hot-night replay
In the cool season, "tonight" looks safe everywhere and the tool's value is invisible. **Replay mode** runs the *same* indoor model, chart and risk band on a **real recorded hot night** from the Open-Meteo Archive API (ERA5 reanalysis, free, no key): the chosen date plus the following days are fetched so the full 18:00→07:00 window and a 3-night outlook are computed from recorded data, clearly labelled *"Replay of a real recorded night — ERA5 data via Open-Meteo Archive."*

Verified preset nights (checked against the Archive API before being hardcoded — ERA5 outdoor night minimum in brackets):

| Preset | Night of | Outdoor night min |
|---|---|---|
| Chennai, India | 20 May 2026 | 31.4 °C |
| Delhi, India | 11 Jun 2025 | 31.1 °C |
| Karachi, Pakistan | 29 May 2025 | 30.0 °C |
| Phoenix, USA | 9 Jul 2025 | 33.9 °C |

A date picker also allows **any past date** for the currently selected place (ERA5 lags real time by roughly five days; the picker is capped accordingly).

### Checked against published measurements
A validation section compares NightMaa's estimate with what the Chennai study's sensors actually measured, for the same kind of home (tin roof + poor ventilation, replayed night of 20 May 2026, computed with the app's own model):

| | Chennai study (sensors, 50 homes) | NightMaa estimate |
|---|---|---|
| Coolest point of the night, indoors | Rarely below 31 °C | ≈34.6 °C — never leaves the danger zone |
| Evening peak, indoors | ≈34.7 °C around 8–9 pm | ≈36.8 °C at 8 pm |
| Time above 32 °C indoors | Frequently; worst homes 5,700–5,800 hours | All 14 hours of the night window |

The app states the limits next to the table (see *Honesty limits*): the agreement claimed is the **pattern** — this kind of home does not cool below the danger zone at night — not exact degrees.

## Honesty limits

- The indoor model is a **calibrated heuristic, not a validated sensor model**. The offsets are informed by published housing-heat research (including the Chennai study's measured values), but any individual home's real temperature will differ. NightMaa shows its method openly ("How we estimate") and never presents the estimate as a measurement.
- The **replay validation is indicative, not a validation of exact degrees.** The study measured real homes with in-home sensors over Oct 2025–Apr 2026 (excluding peak summer); the NightMaa column is an estimate from outdoor weather for a single recorded peak-summer night (20 May 2026) that was hotter than the study period — the outdoors never fell below 31.4 °C — so the estimate runs hotter too. ERA5 is also a neighbourhood-scale grid average, not a street-level measurement. The stated next step is field validation with low-cost **ESP32 temperature loggers inside real homes**.
- The risk bands are **exposure guidance for action, not a medical diagnosis** (see disclaimer below).
- Geolocation place labels come from a free third-party reverse-geocode service and may occasionally name a neighbouring locality; the weather itself always uses the exact GPS coordinates.

## Run / deploy

NightMaa is a **single static HTML file** (`index.html`) — no build step, no backend, no API keys.

- **Locally:** just open `index.html` in a browser (internet needed for fonts and the weather APIs).
- **GitHub Pages:** push this folder to a repository → Settings → Pages → deploy from branch → the app is live at `https://<user>.github.io/<repo>/`.
- It is mobile-first and designed to work on inexpensive Android phones.

## MIT Solve 2027 context

- **Challenge:** MIT Solve 2027 Global Climate Challenge — focus area *Extreme Climate Hazard Protection* (protecting people from extreme heat), with a secondary link to climate-resilient housing/retrofitting.
- **Stage:** working prototype (this app, running on live data, with a replay mode that demonstrates peak-summer behaviour on recorded data).
- **Prize alignment:** maternal & child health focus (Philips Foundation Social Impact Award); AI-for-good framing (AI for Humanity).
- **Delivery model:** through existing frontline health-worker systems — India's ASHA/ANM network (the same channel the author has previously built AI tooling for: child-malnutrition screening), and community health workers, midwives and Anganwadi workers globally — free to mothers and health workers, with a per-district pathway via state health missions and municipal Heat Action Plan programmes.

## Medical disclaimer

NightMaa provides **heat-risk information to support decisions** by families and frontline health workers. It is **not a medical diagnosis**, does not provide medical advice for any individual, and does not replace consultation with a doctor, nurse or ASHA worker. If a pregnant woman experiences danger signs — dizziness or fainting, reduced baby movement, severe headache, sudden swelling, vomiting, fever, contractions or leaking fluid — she should contact her ASHA worker or doctor immediately or go to the nearest hospital.

## Credits

- **Created by:** Akarsh Pandey — AI / Computer Vision / Embedded Systems developer, Indore, Madhya Pradesh, India.
- **Weather data:** [Open-Meteo](https://open-meteo.com) Forecast, Geocoding and Archive (ERA5) APIs (free, CC BY 4.0). Place labels for GPS lookups: BigDataCloud reverse geocoding (free client API).
- **Evidence:** Climate Trends (Chennai indoor-heat study, via Ground Report); Lakhoo et al., *Nature Medicine* (2024/25); Rekha et al., *BJOG* (2024).
