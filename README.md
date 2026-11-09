# Diamonds Price Prediction

A beginner data-analysis and machine-learning project on the seaborn diamonds dataset (53,940 rows), built in Python (pandas, scikit-learn) on Google Colab. Goal: predict a diamond's price.

## Data cleaning

- `describe` revealed impossible values: `x`, `y` or `z` equal to 0 in 20 rows (removed).
- Three more rows had `y` or `z` above 20 mm (58.9 and 31.8) for diamonds weighing 0.51 to 2.00 carat, which are clearly recording errors (removed instead of guessing).
- 23 rows removed in total (about 0.04 percent), leaving 53,917.

## Findings

- Price is right-skewed: mean 3,933 vs median 2,401 (before cleaning).
- `carat` and `price` have a correlation of 0.92, but the scatter plot shows a curve, not a straight line.
- A confounder: the "Ideal" cut has the lowest average price (3,457) because its diamonds are the lightest (average 0.70 carat). The "Fair" cut averages 1.05 carat. Weight drives price, so comparing average price by cut alone is misleading.

## Model

Random Forest regressor (100 trees), 80/20 train/test split, random_state=42. The `cut`, `color` and `clarity` columns were converted to 0/1 columns (one-hot encoding), giving 26 features.

| Model | MAE (USD) | vs. baseline |
|---|---|---|
| Baseline (always predict the mean) | 3,008 | - |
| Random Forest (6 numeric features) | 776 | about 74 percent better |
| Random Forest (+ cut, color, clarity) | 266 | about 91 percent better |

### Feature importance (Random Forest, 26 features)

`carat` 0.61 and `y` 0.28 together account for about 88 percent. The cut columns do not appear in the top 8.

## Limitations

- A single train/test split was used, so the numbers could shift with a different split.
- MAE is an average in dollars; errors on expensive diamonds are likely larger than on cheap ones (not checked yet).
- Feature importance is shared among correlated columns (`carat`, `x`, `y`, `z`) and across one-hot columns, so individual values should be read with care.
- No hyperparameter tuning and no cross-validation.

## Tools

Python, pandas, seaborn (dataset), matplotlib, scikit-learn, Google Colab.

---

## ملخص بالعربي

- الداتا فيها 53,940 ألماسة، والهدف توقع السعر.
- `describe` كشف قيم مستحيلة: 20 صف بأبعاد صفر و3 صفوف بأبعاد ضخمة غلط تسجيل، وشلنا 23 صف بس.
- الارتباط بين الوزن والسعر 0.92، لكن العلاقة منحنى مش خط مستقيم.
- عامل خفي: القطع `Ideal` أرخص متوسط سعر لأن ماسته أخف (0.70 قيراط)، فالمقارنة بالمتوسط وحده بتخدع.
- النموذج `Random Forest` وصل لمتوسط غلط 266 دولار، مقابل 3,008 دولار للنموذج الغبي (أحسن بحوالي 91 بالمئة).
- حدود النتيجة: تقسيم واحد فقط، والغلط على الألماس الغالي غالبًا أكبر.
