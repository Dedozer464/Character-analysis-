# Character Personality Analysis & Longevity Prediction System

A Python-based psychometric modelling tool that scores fictional character traits, classifies personality types, estimates health risk, and produces longevity predictions — all surfaced through a ranked comparative report.

---

## Overview

This project applies rule-based psychometric scoring and actuarial-style longevity estimation to structured character profiles. It is designed as an exploratory exercise in personality modelling, demonstrating how qualitative trait data can be quantified, aggregated, and used to generate predictive outputs.

The system is intentionally transparent: every score is traceable back to a weighted trait mapping, making it easy to audit, extend, or challenge the underlying assumptions.

---

## Features

- **Maturity scoring** — weights positive and negative personality traits on a 0–100 scale
- **Personality classification** — maps trait combinations to archetypes (Caregiver, Analyst, Entertainer, Sensitive, Altruist, Achiever, Innovator)
- **Health risk estimation** — combines age-based baseline risk with lifestyle factor adjustments
- **Longevity prediction** — integrates health risk and maturity scores against a global life expectancy baseline to estimate years remaining and predicted age at death
- **Comparative ranking** — generates a ranked leaderboard across all characters sorted by predicted longevity
- **Statistical summary** — mean, median, and range of maturity, risk, and longevity scores across the character set
- **CSV export** — saves the comparison table to `character_analysis_results.csv`

---

## Character Profiles

The system ships with four distinctly positive characters designed to produce meaningfully different scoring outcomes:

| Character | Age | Archetype | Key Traits |
|---|---|---|---|
| **Amara** | 25 | Community Builder | Empathetic, Collaborative, Optimistic |
| **Miriam** | 50 | Wise Nurturer | Caring, Emotionally Intelligent, Patient |
| **Thabo** | 35 | Resilient Leader | Disciplined, Honest, Adaptable |
| **Lerato** | 24 | Creative Thinker | Curious, Analytical, Open-Minded |

Each character has a distinct trait profile and lifestyle pattern, which results in differentiated maturity scores, health risk levels, and predicted longevity — making the comparative output meaningful rather than uniform.

---

## Project Structure

```
Psychometricsforfun/
│
└── character_analysis.py       # Main script — all logic and execution
```

Output (generated on run):
```
character_analysis_results.csv  # Comparison table exported automatically
```

---

## How It Works

### 1. Character Definition

Each character is defined using the `Character` dataclass:

```python
@dataclass
class Character:
    name: str
    age: int
    traits: List[str]
    lifestyle_factors: List[str]
```

Traits and lifestyle factors are normalised to lowercase with underscores on instantiation, making comparisons reliable regardless of input formatting.

### 2. Scoring Pipeline

Each character passes through a sequential scoring pipeline:

| Step | Function | Output |
|---|---|---|
| Maturity scoring | `score_maturity()` | Float (0–100) |
| Personality classification | `assess_personality()` | List of archetypes |
| Character value rating | `calculate_character_value()` | Categorical label |
| Health risk estimation | `calculate_health_risk()` | Float (0–100) |
| Risk level classification | `get_risk_level()` | Categorical label |
| Longevity prediction | `predict_longevity()` | Years remaining + predicted death age |

### 3. Scoring Logic

**Maturity score** starts at 50 (base) and adjusts based on trait weights:

- Positive traits (e.g. `emotionally_intelligent: +18`, `caring: +15`, `resilient: +14`) push the score up
- Negative traits (e.g. `narcissistic: -30`, `passive_aggressive: -25`) pull it down
- Score is clamped to [0, 100]

**Health risk score** starts with an age-based baseline (`age / 100 * 50`) and adjusts based on lifestyle:

- Risk factors (e.g. `smokes_cigarettes: +20`, `social_isolation: +15`) increase risk
- Protective factors (e.g. `strong_relationships: -12`, `exercises_regularly: -10`, `mindfulness: -8`) reduce it
- Score is clamped to [5, 95]

**Longevity prediction** uses a global average life expectancy baseline of 78 years and applies:

- Risk adjustment: up to −25 years at maximum risk
- Maturity bonus: up to +12 years at maximum maturity

---

## Installation

### Requirements

- Python 3.7+
- pandas

Install dependencies:

```bash
pip install pandas
```

### Run

```bash
python character_analysis.py
```

---

## Sample Output

```
================================================================================
CHARACTER PERSONALITY ANALYSIS & LONGEVITY PREDICTION SYSTEM
================================================================================

CHARACTER PROFILE ANALYSIS: MIRIAM
================================================================================

BASIC INFORMATION:
  Age: 50 years old
  Traits: caring, nurturing, loves_everybody, emotionally_intelligent, patient
  Lifestyle Factors: strong_relationships, caring_for_others, active, low_stress

CHARACTER ASSESSMENT:
  Maturity Score: 96.0/100
  Personality Types: Caregiver, Altruist, Sensitive
  Overall Character Value: Very High

HEALTH & LONGEVITY PREDICTION:
  Health Risk Score: 9.0/100
  Risk Level: Low Risk
  Predicted Years Remaining: 37.5 years
  Predicted Age at Death: 87.5 years old
  Confidence Level: Medium (based on general health patterns)
```

---

## Extending the System

To add new characters, define them in `main()` using the `Character` dataclass and append to the `characters` list:

```python
new_char = Character(
    name="New Character",
    age=30,
    traits=["Curious", "Collaborative", "Honest"],
    lifestyle_factors=["Active", "Good_diet", "Low_stress"]
)
characters.append(new_char)
```

To add new traits or lifestyle factors, update the weight dictionaries in `score_maturity()` or `calculate_health_risk()` directly.

---

## Limitations & Assumptions

- Scoring weights are manually assigned and reflect subjective judgements — they are not derived from empirical psychometric data
- Life expectancy baseline (78 years) is a global average and does not account for country, sex, socioeconomic status, or genetic factors
- The confidence level on all longevity predictions is intentionally flagged as **Medium** — this is a modelling exercise, not a clinical tool
- The system uses rule-based lookup rather than machine learning; predictions do not generalise beyond the defined trait vocabulary

---

## Skills Demonstrated

- Python dataclasses and type annotations
- Modular function design with clear separation of concerns
- Weighted scoring systems and rule-based classification
- Pandas DataFrame construction and CSV export
- Statistical summarisation using the `statistics` module

---

## Author

Retshidisitswe Dedozer  
[GitHub: Dedozer464](https://github.com/Dedozer464)
