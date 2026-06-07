# Causal AI Explorations — Part 1: Does High Blood Sugar Cause Heart Disease?

> This is Part 1 of my Causal AI Explorations series, where I work through real datasets to build intuition for causal reasoning. Each entry picks a question that correlation alone can't answer.

---

## What I Wanted to Learn

I'd been reading about causal inference and kept hitting the same idea — correlation is not causation, and standard ML models can't tell the difference. I wanted to see this play out concretely on a real dataset.

Specifically, I wanted to understand:
- How to encode causal assumptions as a DAG
- What the backdoor criterion actually does in practice
- Whether a "significant" correlation survives causal adjustment

---

## The Question

**Does high fasting blood sugar (FBS) causally increase the risk of heart disease?**

The naive answer from the data: yes, strongly. The high FBS group had a 17% higher heart disease rate. But older patients are both more likely to have high blood sugar and more likely to have heart disease — so is FBS actually doing anything, or is age doing all the work?

---

## What I Did

**Dataset:** Heart Disease UCI (Kaggle) — 745 records after preprocessing

**Setup:**
- Treatment: Fasting blood sugar > 120 mg/dl
- Outcome: Presence of heart disease (binarized)
- Confounders: Age, sex, cholesterol, resting blood pressure

**Method:**
1. Built a causal graph (DAG) encoding how confounders relate to both treatment and outcome
2. Used DoWhy to formally identify the causal effect via the backdoor criterion
3. Estimated the Average Treatment Effect (ATE) using regression adjustment
4. Ran refutation tests to validate the estimate

---

## What I Found

| | Effect Size |
|---|---|
| Naive correlation | 0.165 |
| Causal ATE (after adjustment) | 0.043 |
| p-value | 0.37 (not significant) |

After controlling for age, sex, cholesterol, and blood pressure — the effect of high fasting blood sugar on heart disease dropped from 17% to 4%, and became statistically insignificant.

The confounders were doing most of the work. The correlation was real. The causation wasn't.

---

## What Surprised Me

I expected the causal effect to be smaller than the naive estimate — that's the whole point of the exercise. What I didn't expect was for it to essentially vanish. A 17% gap collapsing to a non-significant 4% is a stark result.

It made the value of causal analysis feel very concrete: if you were designing a clinical intervention based on the naive correlation, you'd be targeting the wrong variable.

---

## Tools Used
- Python, Jupyter Notebook
- DoWhy, pandas, matplotlib, seaborn

---

## What's Next

Part 2 takes causal reasoning into a fairness context — asking whether a workplace policy causally discriminates by gender.

→ [Part 2: Counterfactual Fairness in Employee Attrition](https://github.com/yourusername/causal-ai-explorations-02-hiring-fairness)
