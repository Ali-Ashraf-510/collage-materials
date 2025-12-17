
## خلّيك مع النسخة المنسّقة 👇

# 📘 Fuzzy Control

## 🔹 Fuzzy Inference Systems (Mamdani – Sugeno – Tsukamoto)

---

## 1️⃣ أساسيات الـ Fuzzy Controller

أي **Fuzzy Controller** بيحوّل:

* **Crisp Input** ➜ **Crisp Output**

> *A Fuzzy Controller translates a crisp input into a usable crisp output.*

---

### 🔹 1. Fuzzification

تحويل القيم الرقمية إلى درجات انتماء داخل مجموعات ضبابية.

> *Converts crisp inputs into membership grades.*

**مثال رياضي:**

[
e = 0.5 ;\Rightarrow; \mu_{\text{Positive}}(e) = 0.7
]

---

### 🔹 2. Inference Mechanism & Rules

تطبيق قواعد **IF–THEN**
وقوة كل قاعدة ( w ) تتحسب باستخدام:

* **Min**
* أو **Product**

[
w_i = \min \left( \mu_{A_i}(x_1), \mu_{B_i}(x_2) \right)
]

أو

[
w_i = \mu_{A_i}(x_1) \times \mu_{B_i}(x_2)
]

> *Uses IF–THEN rules to infer fuzzy outputs.*

---

### 🔹 3. Defuzzification

تحويل الناتج الضبابي النهائي إلى قيمة رقمية واضحة.

> *Converts fuzzy output into a crisp value.*

أشهر طريقة:

[
y = \frac{\int y , \mu(y), dy}{\int \mu(y), dy}
]

(**Center of Gravity – COG**)

---

## 2️⃣ أنواع أنظمة الاستنتاج الضبابي (FIS)

> *There are three main types of Fuzzy Inference Systems.*

1. **Mamdani**
2. **Sugeno / TSK**
3. **Tsukamoto**

---

## 3️⃣ شرح الأنواع بالتفصيل

---

## 🔹 (1) Mamdani Fuzzy Inference System

### 🔸 شكل القاعدة

[
\text{IF } x_1 \text{ is } A_i ;\text{AND}; x_2 \text{ is } B_i
;\text{THEN}; y \text{ is } C_i
]

* **THEN** = مجموعة ضبابية
* ناتج القاعدة = **Fuzzy Set**

---

### 🔸 Defuzzification (COG)

[
y_{\text{crisp}} =
\frac{\int y , \mu_{C}(y), dy}
{\int \mu_{C}(y), dy}
]

---

## 🔹 (2) Sugeno / TSK

### 🔸 شكل القاعدة

[
\text{IF } x_1 \text{ is } A_i ;\text{AND}; x_2 \text{ is } B_i
;\text{THEN};
y_i = p_i x_1 + q_i x_2 + r_i
]

* **THEN** = معادلة رياضية
* ناتج القاعدة = **Crisp**

---

### 🔸 Weighted Average

[
y_{\text{crisp}} =
\frac{\sum w_i , y_i}{\sum w_i}
]

---

### 🔸 مثال رقمي

[
y = 3x_1 + 2x_2 + 1
]

لو:

[
x_1 = 3,\quad x_2 = 7
]

[
y = 3(3) + 2(7) + 1 = 24
]

---

## 🔹 (3) Tsukamoto

### 🔸 شكل القاعدة

[
\text{IF } x_1 \text{ is } A_i
;\text{THEN}; y \text{ is } C_i
]

* ( C_i ) لازم تكون **Monotonic Fuzzy Set**
* كل قاعدة تطلع قيمة رقمية مباشرة

---

### 🔸 Weighted Average

[
y_{\text{crisp}} =
\frac{\sum w_i , y_i}{\sum w_i}
]

---

### 🔸 مثال

لو:

[
w = 0.7,\quad y = 80
]

➡ تدخل مباشرة في الـ **Weighted Average**

---

## 4️⃣ جدول مقارنة (منسق رياضياً)

| الخاصية         | Mamdani   | Sugeno (TSK) | Tsukamoto           |
| --------------- | --------- | ------------ | ------------------- |
| THEN Part       | Fuzzy Set | ( y = f(x) ) | Monotonic Fuzzy Set |
| ناتج القاعدة    | Fuzzy     | Crisp        | Crisp               |
| Defuzzification | COG       | Weighted Avg | Weighted Avg        |
| الحسابات        | تكامل     | جبر          | جبر                 |

---

## ✅ ملاحظة مهمة للامتحان

> أي **معادلة في THEN ⇒ TSK مباشرة**
> **COG ⇒ Mamdani فقط**

---


قولّي و أظبطهولك فورًا 🔥
