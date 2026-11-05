# NightMaa — Indoor Night-Heat Protection for Mothers

**One-line pitch:** NightMaa estimates tonight's *indoor* heat in a pregnant woman's home — from the live weather forecast and the kind of home she lives in — and turns it into a simple, safe plan for the night, in English and Hindi, on any phone.

Built for the **MIT Solve 2027 Global Climate Challenge** (Extreme Climate Hazard Protection — heat), by **Akarsh Pandey**, Indore, India.

## The problem

Every heat warning in India describes the *outdoor afternoon*. The newest research says the dangerous exposure is different: **indoors, at night, in the homes of pregnant women** who cannot leave.

- **Chennai sensor study (Climate Trends, Oct 2025–Apr 2026):** minute-level sensors in 50 low- and middle-income Chennai homes found indoor temperatures **frequently above 32 °C and rarely below 31 °C even at night**, peaking around **8–9 pm at ~34.7 °C** as concrete walls re-radiate stored heat. The worst homes logged **5,700–5,800 hours above 32 °C** — and the study period excluded peak summer. The researchers note indoor nighttime heat *"is not systematically monitored in any major city."*
  ([Ground Report coverage](https://www.groundreport.in/extreme-weather/inside-indias-homes-heat-never-leaves-not-even-at-night/))
- **Nature Medicine meta-analysis (198 studies, 66 countries):** odds of preterm birth rise **1.04 per +1 °C** and **1.26 during heatwaves**; odds of any obstetric complication 1.25 during heatwaves.
  ([Nature Medicine](https://www.nature.com/articles/s41591-024-03395-8))
- **Tamil Nadu cohort (BJOG 2024, n = 800 pregnant women doing physical work):** 47.3% had high occupational heat exposure; heat-exposed women had **adjusted OR 2.4 for miscarriage** and **2.3 for any adverse pregnancy outcome**.
  ([PubMed](https://pubmed.ncbi.nlm.nih.gov/37814395/))
- **The policy blind spot:** India has **300+ Heat Action Plans — none mandates indoor temperature monitoring.** No deployed tool converts an outdoor forecast into a pregnancy-specific, home-specific night risk, or tells an ASHA worker which of her mothers to visit first tomorrow.

## How it works

1. **Pick a place** — preset Indian cities, or search any town (Open-Meteo Geocoding API, free, no key).
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
5. **Outputs for tonight (18:00–07:00)** — an hour-by-hour chart of estimated indoor vs. outdoor temperature, the night's minimum indoor temperature, hours spent above 32 °C indoors, a **pregnancy heat-risk band** (Low / Moderate / High / Very High), a 3-night outlook, and a personalised advisory in **English and Hindi (Devanagari)** covering room cooling, hydration, sleep setup, ventilation timing and **danger signs** that mean "contact your ASHA worker or doctor immediately."
6. **For ASHA workers** — the whole check takes about a minute inside a routine home visit: no login, no install, no cost. Homes showing High or Very High risk are the next morning's first visits.

### Honesty limits

- The indoor model is a **calibrated heuristic, not a validated sensor model**. The offsets are informed by published housing-heat research (including the Chennai study's measured values), but any individual home's real temperature will differ. NightMaa shows its method openly ("How we estimate") and never presents the estimate as a measurement.
- The risk bands are **exposure guidance for action, not a medical diagnosis** (see disclaimer below). The pilot-stage roadmap is low-cost ESP32 temperature loggers in real homes to validate and refine the model.

## Run / deploy

NightMaa is a **single static HTML file** (`index.html`) — no build step, no backend, no API keys.

- **Locally:** just open `index.html` in a browser (internet needed for fonts and the weather APIs).
- **GitHub Pages:** push this folder to a repository → Settings → Pages → deploy from branch → the app is live at `https://<user>.github.io/<repo>/`.
- It is mobile-first and designed to work on inexpensive Android phones.

## MIT Solve 2027 context

- **Challenge:** MIT Solve 2027 Global Climate Challenge — focus area *Extreme Climate Hazard Protection* (protecting people from extreme heat), with a secondary link to climate-resilient housing/retrofitting.
- **Stage:** working prototype (this app, running on live data).
- **Prize alignment:** maternal & child health focus (Philips Foundation Social Impact Award); AI-for-good framing (AI for Humanity).
- **Delivery model:** through India's existing ASHA/ANM frontline health-worker system — the same channel the author has previously built AI tooling for (child-malnutrition screening) — free to mothers and ASHAs, with a per-district pathway via state health missions and municipal Heat Action Plan programmes.

## Medical disclaimer

NightMaa provides **heat-risk information to support decisions** by families and frontline health workers. It is **not a medical diagnosis**, does not provide medical advice for any individual, and does not replace consultation with a doctor, nurse or ASHA worker. If a pregnant woman experiences danger signs — dizziness or fainting, reduced baby movement, severe headache, sudden swelling, vomiting, fever, contractions or leaking fluid — she should contact her ASHA worker or doctor immediately or go to the nearest hospital.

## Credits

- **Created by:** Akarsh Pandey — AI / Computer Vision / Embedded Systems developer, Indore, Madhya Pradesh, India.
- **Weather data:** [Open-Meteo](https://open-meteo.com) (free APIs, CC BY 4.0).
- **Evidence:** Climate Trends (Chennai indoor-heat study, via Ground Report); Lakhoo et al., *Nature Medicine* (2024/25); Rekha et al., *BJOG* (2024).
