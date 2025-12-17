# 🧠 Question 9 – ANFIS Design and Approximation

> **إجابة سؤال 9 كاملة ومُنظمة**  
> شرح شامل + طريقة التفكيك + ملخص السؤال  
> مناسبة للمذاكرة ولموديل الإجابة في الامتحان 📚

---

## 📋 Problem Statement | ملخص السؤال

Design an **ANFIS model** using **MATLAB** to approximate the nonlinear function:

$$f(x) = -2x - x^2, \quad x \in [-10, 10]$$

### 📊 Model Specifications

| Parameter | Value |
|-----------|-------|
| Training Data Pairs | 60 |
| Membership Functions | 3 Triangular MFs |
| Learning Algorithm | Backpropagation (BP) |
| Training Epochs | 100 |
| Number of Rules | 3 |
| Output | Optimized parameters |

---

## 🔧 Implementation Steps

### 1️⃣ Step 1: Generate Training Data

```matlab
clc; clear; close all;

n = 60;                                % Number of training samples
x1 = -10 + 20.*rand(n,1);              % Input x in range [-10,10]
y  = -2.*x1 - x1.^2;                   % f(x) = -2x - x^2
data = [x1 y];                         % Training data set
```

#### 💡 Explanation

- ✅ Random values of `x` are generated in the range **[-10, 10]**
- ✅ The exact function output is calculated
- ✅ These input-output pairs are used to train the ANFIS model

### 2️⃣ Step 2: Define ANFIS Parameters

```matlab
numMFs    = 3;          % Number of membership functions
mfType    = 'trimf';    % Triangular membership function
numEpochs = 100;        % Number of training epochs
```

#### 💡 Explanation

- ✅ Each MF corresponds to **one fuzzy rule**
- ✅ **Triangular MF** is chosen as required in the question
- ✅ Training is performed for **100 iterations**

### 3️⃣ Step 3: Generate Initial ANFIS Structure

```matlab
fismat1 = genfis1(data, numMFs, mfType);
```

#### 💡 Explanation

The `genfis1` function generates:
- ✅ Membership functions
- ✅ Fuzzy rules
- ✅ Initial (untrained) parameters

### 4️⃣ Step 4: Train ANFIS Using BP Algorithm

```matlab
[fismat2, trn_mse, tst_mse] = anfis(data, fismat1, numEpochs, NaN, data, 0);
```

#### 💡 Explanation

- ✅ `0` selects **Backpropagation learning**
- ✅ `trn_mse` stores **Mean Square Error** per epoch
- ✅ `fismat2` is the **trained ANFIS model**

### 5️⃣ Step 5: Evaluate Trained ANFIS

```matlab
anfis_output = evalfis(x1, fismat2);
[x1 y anfis_output]
```

#### 💡 Explanation

- ✅ The trained ANFIS output is calculated
- ✅ Results are compared with the exact function output

---

### 6️⃣ Step 6: Error Analysis

```matlab
AMSE = mean(trn_mse);
```

#### 💡 Explanation

- ✅ **AMSE** = Average Mean Square Error over 100 epochs
- ✅ Lower AMSE indicates **better approximation**

---

### 7️⃣ Step 7: Plot Results

#### 📈 Training Error Curve

```matlab
figure;
plot(1:numEpochs, trn_mse, 'LineWidth', 2);
xlabel('Epochs'); 
ylabel('MSE');
title('Training Error over Epochs');
grid on;
```

**Purpose:** Visualize how the error decreases during training

---

#### 📊 Desired Output vs ANFIS Output

```matlab
figure;
plot(1:n, y, 'x', 1:n, anfis_output, 'o');
xlabel('Sample Number'); 
ylabel('Output');
legend('Desired Output', 'ANFIS Output');
title('Comparison: Desired vs ANFIS Output');
grid on;
```

**Purpose:** Compare the exact function output with ANFIS predictions

---

## 🎯 Optimized ANFIS Parameters | المعاملات المُحسَّنة

### 🔸 Premise Parameters

- **3 Triangular MFs**
- Each MF has **3 control parameters**

$$\text{Number of premise parameters} = 3 \times 3 = 9 \text{ parameters}$$

### 🔸 Consequent Parameters

- **Sugeno-type** consequent function:
  $$f = ax + b$$

$$\text{Number of consequent parameters} = (1 + 1) \times 3 = 6 \text{ parameters}$$

### 🔸 Total ANFIS Parameters

| Parameter Type | Count |
|----------------|-------|
| Premise Parameters | 9 |
| Consequent Parameters | 6 |
| **Total Parameters** | **15** |

#### 📌 Display Optimized Values

```matlab
getfis(fismat2)
```

---

## ✅ Final Conclusion | الخلاصة النهائية

> **The ANFIS model successfully approximates the nonlinear function**  
> $$f(x) = -2x - x^2$$  
> using **3 triangular membership functions** and the **BP learning algorithm** with acceptable approximation error.

### 🎓 Key Takeaways | النقاط المهمة

1. ✅ **60 training samples** were used to train the model
2. ✅ **3 triangular MFs** provide adequate coverage of the input space
3. ✅ **Backpropagation** effectively optimizes the ANFIS parameters
4. ✅ **100 epochs** ensure convergence to minimal error
5. ✅ **15 total parameters** (9 premise + 6 consequent) are optimized

---



