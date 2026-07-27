# Knowledge File 3: Career Matching Data & Methodology

## Matching Methodology

### How Fit Scores Work

A fit score compares a user's 24-subskill Skillprint against a career's skill requirement profile. Both are expressed as vectors of 24 values (0-100 scale).

**Fit Score Calculation:**

1. For each subskill, calculate the difference: `gap = requirement - user_score`
2. Positive gaps (user exceeds requirement) contribute positively but with diminishing returns
3. Negative gaps (user below requirement) are penalized proportionally to size
4. Weight each subskill by its importance to the specific career (see career profiles below)
5. Aggregate to a 0-10 fit score where:
   - 9.0-10.0 = Exceptional fit (top 5% match quality)
   - 7.5-8.9 = Strong fit (likely to succeed and enjoy the role)
   - 6.0-7.4 = Good fit with some development areas
   - 4.5-5.9 = Moderate fit, significant gaps to close
   - Below 4.5 = Poor current fit, but show the path

**Simplified calculation for the GPT:**

For each career, look at the top 8 most-important subskills for that role. Compare user scores to requirements. The fit score is approximately:

`Fit = 10 - (average weighted gap / 10)`

Where weighted gap = sum of (max(0, requirement - user_score) * weight) / sum of weights

This ensures someone who exceeds all requirements scores near 10, and someone with large gaps in critical areas scores lower.

### Important Rules:
- Never show a fit score below 2.0. Everyone has value; the floor communicates that.
- Always explain WHY the score is what it is in terms of specific subskills.
- If a user's score is low for a career they expressed interest in, frame it as: "You're at [X]. Here's what it would take to get to [Y]."

---

## O*NET to Edge Subskill Crosswalk

This maps O*NET dimensions to Futurenav Edge's 24 subskills for career profile generation.

| Edge Subskill | Primary O*NET Source | Secondary O*NET Source | Weight Method |
|---|---|---|---|
| Adaptability | Work Style: Adaptability/Flexibility | -- | WS Impact rating |
| Aesthetic Appreciation | -- | -- | Default 50 unless creative/design role |
| Assertiveness | Work Style: Leadership | Skill: Negotiation | WS Impact + Skill Importance |
| Collaboration | Work Style: Cooperation | Skill: Coordination | WS Impact + Skill Importance |
| Confidence | Work Style: Independence | Work Style: Leadership | Average of WS Impact |
| Creativity | Work Style: Innovation | Skill: Complex Problem Solving | WS Impact + Skill Level |
| Curiosity | Work Style: Achievement/Effort | Skill: Active Learning | WS Impact + Skill Importance |
| Emotional Regulation | Work Style: Self-Control | Work Style: Stress Tolerance | Average of WS Impact |
| Empathy | Work Style: Concern for Others | Skill: Social Perceptiveness | WS Impact + Skill Importance |
| Formality | Work Style: Attention to Detail | -- | WS Impact rating |
| Delivery | Skill: Speaking | Ability: Oral Expression | Skill Level + Ability Level |
| Initiative | Work Style: Initiative | Work Style: Achievement/Effort | Average of WS Impact |
| Language Use | Skill: Writing | Skill: Reading Comprehension | Average of Skill Importance |
| Nonverbal Communication | Skill: Social Perceptiveness | -- | Skill Importance |
| Organization | Work Style: Attention to Detail | Skill: Time Management | WS Impact + Skill Importance |
| Perspective Taking | Skill: Social Perceptiveness | Work Style: Concern for Others | Skill Importance + WS Impact |
| Persuasion | Skill: Persuasion | Skill: Negotiation | Average of Skill Importance |
| Resilience | Work Style: Stress Tolerance | Work Style: Persistence | Average of WS Impact |
| Responsibility | Work Style: Dependability | Work Style: Integrity | Average of WS Impact |
| Self-discipline | Work Style: Achievement/Effort | Work Style: Persistence | Average of WS Impact |
| Topic Development | Skill: Speaking | Ability: Oral Expression | Skill Level |
| Tone | Work Style: Social Orientation | Skill: Social Perceptiveness | WS Impact + Skill Importance |
| Trust | Work Style: Integrity | Work Style: Dependability | Average of WS Impact |
| Verbal Communication | Skill: Speaking | Ability: Oral Expression | Skill Importance + Ability Level |

### Conversion Formula

O*NET Work Styles use an Impact scale (-3.00 to +3.00).
O*NET Skills use Importance (1-5) and Level (0-7) scales.
O*NET Abilities use Importance (1-5) and Level (0-7) scales.

To convert to the 0-100 Edge scale:
- Work Style Impact: `score = ((impact + 3) / 6) * 100` (maps -3..+3 to 0..100)
- Skill/Ability Importance: `score = ((importance - 1) / 4) * 100` (maps 1..5 to 0..100)
- Skill/Ability Level: `score = (level / 7) * 100` (maps 0..7 to 0..100)
- When multiple sources: weighted average, primary source gets 0.6 weight, secondary gets 0.4

---

## Top 50 Career Profiles for Richmond Metro MVP

Each profile shows the 8 most critical Edge subskills for the role (importance-weighted), required level (0-100), and salary data.

Format: Career Title | O*NET Code | Top 8 Subskills (with required level) | Salary Range | Richmond Employers

### Healthcare

**1. Registered Nurse**
O*NET: 29-1141.00
Critical Subskills: Empathy (85), Emotional Regulation (82), Responsibility (88), Adaptability (78), Collaboration (80), Verbal Communication (75), Organization (76), Resilience (80)
Salary: $62,000-$92,000 (Richmond); $65,000-$98,000 (National)
Richmond Employers: VCU Health, HCA (Chippenham, Henrico Doctors), Bon Secours

**2. Medical & Health Services Manager**
O*NET: 11-9111.00
Critical Subskills: Organization (88), Responsibility (85), Collaboration (82), Initiative (80), Verbal Communication (78), Emotional Regulation (75), Persuasion (72), Self-discipline (80)
Salary: $78,000-$125,000 (Richmond)
Richmond Employers: VCU Health, HCA, Bon Secours, Virginia Department of Health

**3. Licensed Practical Nurse**
O*NET: 29-2061.00
Critical Subskills: Empathy (82), Responsibility (85), Organization (78), Collaboration (75), Emotional Regulation (78), Adaptability (72), Verbal Communication (70), Trust (80)
Salary: $40,000-$55,000 (Richmond)

**4. Pharmacy Technician**
O*NET: 29-2052.00
Critical Subskills: Organization (85), Responsibility (88), Self-discipline (80), Formality (78), Verbal Communication (68), Collaboration (65), Adaptability (62), Trust (82)
Salary: $32,000-$45,000 (Richmond)

**5. Medical Assistant**
O*NET: 31-9092.00
Critical Subskills: Empathy (78), Organization (80), Collaboration (75), Adaptability (72), Verbal Communication (68), Responsibility (80), Trust (78), Emotional Regulation (70)
Salary: $30,000-$42,000 (Richmond)

### Technology & Financial Services

**6. Data Analyst**
O*NET: 15-2051.00
Critical Subskills: Organization (85), Curiosity (80), Self-discipline (78), Creativity (72), Verbal Communication (70), Initiative (75), Perspective Taking (68), Language Use (72)
Salary: $58,000-$88,000 (Richmond)
Richmond Employers: Capital One, CarMax, Markel, Federal Reserve Bank of Richmond

**7. Software Developer**
O*NET: 15-1252.00
Critical Subskills: Creativity (82), Self-discipline (85), Curiosity (88), Initiative (78), Organization (80), Collaboration (72), Adaptability (75), Resilience (70)
Salary: $75,000-$130,000 (Richmond)
Richmond Employers: Capital One, CarMax, Hims & Hers, CoStar

**8. Financial Analyst**
O*NET: 13-2051.00
Critical Subskills: Organization (88), Self-discipline (85), Language Use (78), Verbal Communication (75), Initiative (72), Curiosity (70), Responsibility (82), Persuasion (68)
Salary: $60,000-$95,000 (Richmond)
Richmond Employers: Capital One, Markel, Genworth, Owens & Minor

**9. Cybersecurity Analyst**
O*NET: 15-1212.00
Critical Subskills: Self-discipline (88), Curiosity (85), Adaptability (82), Initiative (80), Organization (85), Responsibility (85), Resilience (78), Creativity (72)
Salary: $72,000-$115,000 (Richmond)
Richmond Employers: Capital One, Booz Allen Hamilton, SAIC, Virginia state government

**10. IT Support Specialist**
O*NET: 15-1232.00
Critical Subskills: Adaptability (80), Empathy (72), Verbal Communication (75), Collaboration (70), Organization (78), Initiative (72), Resilience (68), Curiosity (75)
Salary: $42,000-$65,000 (Richmond)

### Manufacturing & Skilled Trades

**11. Industrial Maintenance Technician**
O*NET: 49-9041.00
Critical Subskills: Initiative (82), Self-discipline (80), Adaptability (78), Organization (80), Responsibility (85), Resilience (75), Curiosity (72), Creativity (68)
Salary: $45,000-$68,000 (Richmond)

**12. CNC Machine Operator**
O*NET: 51-4011.00
Critical Subskills: Self-discipline (88), Organization (85), Responsibility (85), Formality (78), Adaptability (70), Initiative (65), Resilience (72), Trust (80)
Salary: $38,000-$58,000 (Richmond)

**13. Electrician**
O*NET: 47-2111.00
Critical Subskills: Responsibility (88), Self-discipline (85), Initiative (80), Organization (82), Adaptability (75), Resilience (78), Curiosity (72), Creativity (68)
Salary: $45,000-$72,000 (Richmond)

**14. HVAC Technician**
O*NET: 49-9021.00
Critical Subskills: Adaptability (82), Initiative (80), Self-discipline (80), Responsibility (85), Organization (78), Verbal Communication (68), Resilience (75), Curiosity (72)
Salary: $42,000-$68,000 (Richmond)

**15. Welder**
O*NET: 51-4121.00
Critical Subskills: Self-discipline (88), Responsibility (85), Organization (80), Aesthetic Appreciation (72), Resilience (78), Adaptability (68), Initiative (65), Trust (80)
Salary: $38,000-$60,000 (Richmond)

### Business & Professional Services

**16. Project Manager**
O*NET: 11-9199.00
Critical Subskills: Organization (90), Collaboration (85), Verbal Communication (82), Initiative (80), Responsibility (85), Assertiveness (78), Adaptability (75), Persuasion (72)
Salary: $68,000-$110,000 (Richmond)

**17. Human Resources Specialist**
O*NET: 13-1071.00
Critical Subskills: Empathy (82), Verbal Communication (80), Collaboration (78), Perspective Taking (80), Emotional Regulation (78), Organization (75), Trust (82), Assertiveness (72)
Salary: $52,000-$78,000 (Richmond)

**18. Marketing Specialist**
O*NET: 13-1161.00
Critical Subskills: Creativity (85), Verbal Communication (80), Persuasion (82), Curiosity (78), Initiative (75), Adaptability (72), Collaboration (70), Aesthetic Appreciation (72)
Salary: $48,000-$78,000 (Richmond)

**19. Accountant**
O*NET: 13-2011.00
Critical Subskills: Organization (90), Self-discipline (88), Responsibility (88), Formality (82), Language Use (75), Trust (85), Initiative (68), Verbal Communication (65)
Salary: $55,000-$85,000 (Richmond)

**20. Sales Representative**
O*NET: 41-3091.00
Critical Subskills: Persuasion (88), Verbal Communication (85), Confidence (82), Resilience (80), Initiative (85), Adaptability (78), Empathy (72), Assertiveness (80)
Salary: $45,000-$90,000+ (Richmond, varies heavily with commission)

### Government & Public Service

**21. Public Administration Manager**
O*NET: 11-1011.00
Critical Subskills: Organization (85), Responsibility (88), Collaboration (82), Verbal Communication (80), Perspective Taking (78), Initiative (75), Trust (85), Emotional Regulation (78)
Salary: $65,000-$110,000 (Richmond)
Richmond Employers: Virginia state government, City of Richmond, Henrico County, Chesterfield County

**22. Social Worker**
O*NET: 21-1021.00
Critical Subskills: Empathy (90), Emotional Regulation (85), Perspective Taking (85), Resilience (82), Verbal Communication (78), Collaboration (78), Assertiveness (72), Trust (80)
Salary: $42,000-$62,000 (Richmond)

**23. Urban & Regional Planner**
O*NET: 19-3051.00
Critical Subskills: Organization (82), Creativity (78), Perspective Taking (85), Verbal Communication (80), Collaboration (80), Curiosity (78), Language Use (78), Initiative (72)
Salary: $55,000-$82,000 (Richmond)

**24. Compliance Officer**
O*NET: 13-1041.00
Critical Subskills: Responsibility (90), Organization (88), Self-discipline (85), Formality (85), Language Use (80), Trust (88), Initiative (72), Assertiveness (70)
Salary: $58,000-$92,000 (Richmond)

**25. Emergency Management Specialist**
O*NET: 13-1061.00
Critical Subskills: Adaptability (88), Emotional Regulation (85), Organization (85), Collaboration (82), Initiative (85), Resilience (85), Verbal Communication (80), Responsibility (85)
Salary: $52,000-$80,000 (Richmond)

### Education & Training

**26. Training & Development Specialist**
O*NET: 13-1151.00
Critical Subskills: Verbal Communication (85), Delivery (85), Empathy (78), Creativity (75), Organization (78), Collaboration (75), Adaptability (72), Curiosity (75)
Salary: $52,000-$82,000 (Richmond)

**27. K-12 Teacher**
O*NET: 25-2031.00
Critical Subskills: Empathy (85), Verbal Communication (85), Delivery (82), Organization (80), Adaptability (78), Resilience (80), Creativity (75), Emotional Regulation (80)
Salary: $45,000-$70,000 (Richmond)

**28. Instructional Coordinator**
O*NET: 25-9031.00
Critical Subskills: Organization (85), Creativity (80), Collaboration (82), Verbal Communication (78), Initiative (78), Curiosity (78), Language Use (78), Persuasion (72)
Salary: $55,000-$80,000 (Richmond)

### Data Centers & Infrastructure (NIDCM-aligned)

**29. Data Center Technician**
O*NET: 15-1244.00
Critical Subskills: Self-discipline (85), Organization (85), Responsibility (88), Adaptability (78), Initiative (75), Curiosity (72), Resilience (72), Collaboration (68)
Salary: $52,000-$75,000 (Richmond)
Richmond Employers: QTS Data Centers, Iron Mountain, Aligned Data Centers, AWS

**30. Network Administrator**
O*NET: 15-1244.00
Critical Subskills: Organization (85), Self-discipline (82), Curiosity (80), Adaptability (80), Initiative (78), Responsibility (85), Creativity (68), Collaboration (72)
Salary: $60,000-$90,000 (Richmond)

### Logistics & Supply Chain

**31. Supply Chain Analyst**
O*NET: 13-1081.00
Critical Subskills: Organization (88), Self-discipline (82), Curiosity (75), Adaptability (78), Collaboration (75), Initiative (72), Verbal Communication (70), Creativity (68)
Salary: $55,000-$82,000 (Richmond)
Richmond Employers: Owens & Minor, CarMax, Amazon, Altria

**32. Logistics Coordinator**
O*NET: 43-5011.00
Critical Subskills: Organization (90), Adaptability (82), Collaboration (78), Responsibility (85), Self-discipline (80), Verbal Communication (72), Initiative (70), Resilience (72)
Salary: $40,000-$58,000 (Richmond)

### Customer-Facing & Service

**33. Customer Service Manager**
O*NET: 43-1011.00
Critical Subskills: Empathy (82), Verbal Communication (80), Emotional Regulation (80), Collaboration (78), Organization (78), Adaptability (78), Assertiveness (72), Resilience (78)
Salary: $45,000-$68,000 (Richmond)

**34. Insurance Claims Adjuster**
O*NET: 13-1031.00
Critical Subskills: Organization (85), Verbal Communication (78), Empathy (72), Perspective Taking (75), Self-discipline (80), Responsibility (82), Assertiveness (70), Emotional Regulation (75)
Salary: $48,000-$72,000 (Richmond)

### Emerging & Nontraditional

**35. UX Designer**
O*NET: 15-1255.00
Critical Subskills: Creativity (88), Empathy (82), Curiosity (85), Aesthetic Appreciation (85), Collaboration (78), Perspective Taking (80), Initiative (75), Verbal Communication (72)
Salary: $65,000-$105,000 (Richmond)

**36. Technical Writer**
O*NET: 27-3042.00
Critical Subskills: Language Use (90), Organization (85), Self-discipline (82), Curiosity (78), Formality (80), Topic Development (82), Creativity (68), Collaboration (65)
Salary: $55,000-$85,000 (Richmond)

**37. Environmental Compliance Specialist**
O*NET: 13-1199.00
Critical Subskills: Responsibility (88), Organization (85), Self-discipline (82), Initiative (75), Curiosity (78), Language Use (78), Formality (82), Collaboration (70)
Salary: $52,000-$78,000 (Richmond)

**38. Renewable Energy Technician**
O*NET: 49-9081.00
Critical Subskills: Adaptability (82), Initiative (80), Self-discipline (80), Responsibility (85), Curiosity (78), Organization (78), Resilience (75), Collaboration (68)
Salary: $42,000-$65,000 (Richmond)

**39. Paralegal**
O*NET: 23-2011.00
Critical Subskills: Organization (90), Language Use (85), Self-discipline (85), Responsibility (88), Formality (85), Trust (85), Initiative (72), Verbal Communication (72)
Salary: $45,000-$68,000 (Richmond)

**40. Construction Manager**
O*NET: 11-9021.00
Critical Subskills: Organization (88), Assertiveness (82), Responsibility (88), Initiative (82), Collaboration (78), Verbal Communication (78), Resilience (80), Adaptability (75)
Salary: $62,000-$100,000 (Richmond)

### Agriculture & Natural Resources (Norwood-aligned)

**41. Farm Manager**
O*NET: 11-9013.00
Critical Subskills: Initiative (88), Responsibility (88), Organization (85), Adaptability (85), Resilience (85), Self-discipline (82), Creativity (72), Collaboration (70)
Salary: $45,000-$75,000 (Richmond/Powhatan area)

**42. Conservation Scientist**
O*NET: 19-1031.00
Critical Subskills: Curiosity (85), Responsibility (82), Organization (80), Initiative (78), Adaptability (78), Language Use (75), Collaboration (72), Creativity (72)
Salary: $52,000-$78,000 (Virginia)

### Entry-Level / High-Volume

**43. Administrative Assistant**
O*NET: 43-6014.00
Critical Subskills: Organization (88), Responsibility (85), Formality (82), Verbal Communication (75), Self-discipline (78), Collaboration (72), Adaptability (70), Trust (80)
Salary: $32,000-$48,000 (Richmond)

**44. Bank Teller**
O*NET: 43-3071.00
Critical Subskills: Responsibility (85), Verbal Communication (78), Empathy (72), Organization (78), Self-discipline (80), Trust (88), Formality (80), Emotional Regulation (72)
Salary: $30,000-$40,000 (Richmond)

**45. Retail Manager**
O*NET: 41-1011.00
Critical Subskills: Verbal Communication (80), Collaboration (78), Organization (80), Initiative (78), Empathy (72), Adaptability (78), Assertiveness (75), Emotional Regulation (75)
Salary: $38,000-$58,000 (Richmond)

**46. Restaurant Manager**
O*NET: 11-9051.00
Critical Subskills: Emotional Regulation (82), Adaptability (85), Organization (82), Collaboration (80), Initiative (80), Verbal Communication (78), Resilience (82), Assertiveness (75)
Salary: $42,000-$62,000 (Richmond)

**47. Dental Hygienist**
O*NET: 29-2021.00
Critical Subskills: Empathy (80), Verbal Communication (75), Self-discipline (82), Organization (80), Trust (82), Formality (78), Responsibility (82), Collaboration (68)
Salary: $62,000-$85,000 (Richmond)

**48. Real Estate Agent**
O*NET: 41-9022.00
Critical Subskills: Persuasion (88), Verbal Communication (85), Initiative (88), Confidence (85), Adaptability (80), Resilience (82), Organization (72), Empathy (72)
Salary: $35,000-$120,000+ (Richmond, commission-based)

**49. Fitness Trainer**
O*NET: 39-9031.00
Critical Subskills: Empathy (80), Verbal Communication (82), Confidence (82), Initiative (80), Adaptability (78), Delivery (80), Resilience (75), Collaboration (68)
Salary: $30,000-$55,000 (Richmond)

**50. Graphic Designer**
O*NET: 27-1024.00
Critical Subskills: Creativity (90), Aesthetic Appreciation (88), Self-discipline (78), Curiosity (78), Collaboration (70), Organization (72), Adaptability (72), Initiative (70)
Salary: $42,000-$68,000 (Richmond)

---

## SALARY DATA NOTES

- Richmond salary ranges are estimated based on BLS Occupational Employment and Wage Statistics for the Richmond, VA MSA (May 2024 data, most recent available)
- Ranges represent approximately 25th to 75th percentile
- National data used as fallback when Richmond-specific data unavailable
- Commission-based roles show wider ranges with notation

## RICHMOND-AREA ZIP CODES

Trigger enhanced local results when user ZIP starts with:
- 230xx (Richmond city, Henrico, Chesterfield)
- 231xx (Petersburg, Colonial Heights, Hopewell, Powhatan, Goochland)
- 232xx (Fredericksburg area -- include but note distance)
- 228xx (Charlottesville area -- include but note distance)

## RICHMOND MAJOR EMPLOYERS (Reference)

Healthcare: VCU Health, HCA (Chippenham, Henrico Doctors, Johnston-Willis), Bon Secours Mercy Health
Financial/Tech: Capital One, CarMax, Markel, Genworth, Federal Reserve Bank of Richmond, Owens & Minor
Government: Virginia state government, City of Richmond, Henrico County, Chesterfield County, Fort Gregg-Adams
Energy: Dominion Energy, Altria
Education: VCU, University of Richmond, Reynolds Community College, J. Sargeant Reynolds, Virginia Union, Virginia State University, Randolph-Macon
Data Centers: QTS Data Centers, Iron Mountain, Aligned Data Centers
Other: CoStar, Hims & Hers, WestRock, Performance Food Group
