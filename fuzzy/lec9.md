# Lecture 9: Differential Evolution (DE) Algorithm

أهلاً بك يا دكتور، أنا متخصص في خوارزميات التحسين والـ Fuzzy Control. خوارزمية **Differential Evolution (DE)** من أهم الخوارزميات اللي بتستخدم في تدريب الأنظمة الضبابية (زي ANFIS)، وهي مرشحة بقوة في الامتحان.

هنشرحها بالتفصيل خطوة بخطوة باللغة العربية البسيطة.

---

## 1. أساسيات خوارزمية التطور التفاضلي (Differential Evolution - DE)

### ما هي خوارزمية DE؟

- **الفكرة الأساسية (بالعربية)**:
  DE هي خوارزمية تحسين **تطورية (Evolutionary)** تعتمد على **المجموعات (Population-based)**، تم تقديمها بواسطة Storn و Price في عام 1997. تم تطويرها خصيصاً عشان تحسن الدوال الرياضية اللي بتحتوي على معاملات حقيقية (Real Parameter, Real Valued Functions).

  *DE is an evolutionary, population-based optimization algorithm introduced in 1997.*

- **لماذا هي خوارزمية تطورية (Evolutionary Algorithm)?**:
  لأنها تعمل في دورة مستمرة من المراحل الأربعة الأساسية للتطور: التوليد المبدئي (Initialization)، الطفرة (Mutation)، التهجين (Crossover)، والاختيار (Selection).
  
  *DE is structured around a cycle of mutation, crossover, and selection stages.*

### لماذا تُسمى "تفاضلية" (Differential)؟

- **الفكرة الأساسية (بالعربية)**:
  الاسم "تفاضلي" (Differential) بيجي من قلب عملية الـ **Mutation (الطفرة)**. الـ DE بتستخدم الفرق المرجح بين متجهين عشوائيين ($\mathbf{x}_{r2,G} - \mathbf{x}_{r3,G}$) عشان تخلق متجه جديد. هذا الفرق التفاضلي هو اللي بيوجه البحث وبيحدد حجم خطوة الطفرة.
  
  *The name 'Differential' originates from using the weighted difference between two random vectors to create the new mutant vector.*

### المصطلحات الأساسية في DE

1. **حجم المجموعة (Population Size - NP)**:
   عدد المتجهات (الحلول المرشحة) الموجودة في كل جيل. هذا العدد يجب أن يكون **أكبر من أو يساوي 4** ($\text{NP} \geq 4$).
   
   *NP is the population size, representing the number of candidate solutions (vectors).*

2. **الأبعاد (Dimension - D)**:
   عدد المعاملات الحقيقية (Real Parameters) في المشكلة التي نحاول تحسينها. كل متجه ($x_i$) يحتوي على $D$ من هذه المعاملات.
   
   *D is the dimensionality, or the total number of parameters to be optimized.*

3. **الجيل (Generation - G)**:
   رقم التكرار (Iteration) الحالي للخوارزمية. العملية بتستمر حتى الوصول للحد الأقصى للجيل ($G_{max}$).
   
   *G is the current iteration number.*

---

## 2. المراحل الأساسية لخوارزمية DE

دورة عمل خوارزمية DE تتكون من أربع مراحل تحدث لكل متجه في المجموعة في كل جيل:

### 1) Initialization (التوليد المبدئي)

- **الفكرة الأساسية (بالعربية)**:
  في الجيل الأول ($G=1$)، يتم إعطاء قيم عشوائية لجميع المتجهات في المجموعة.

- **آلية التنفيذ**: يجب أولاً تحديد الحدود الدنيا ($x_j^L$) والعليا ($x_j^U$) لكل معامل (Constraint Bounds). بعد ذلك، يتم اختيار القيم بشكل عشوائي وموحد (Uniformly) بين هذه الحدود.
  
  *Initialization sets random, uniformly distributed values for all parameter vectors within defined bounds in the first generation.*

### 2) Mutation (الطفرة)

- **الفكرة الأساسية (بالعربية)**:
  الطفرة هي أهم خطوة في DE، وهي المسؤولة عن خلق متجه مرشح جديد اسمه **Mutant Vector ($\mathbf{v}_{i,G+1}$)**.

- **آلية التنفيذ (لـ DE/rand/1/bin)**:
  - لكل متجه حالي ($\mathbf{x}_{i,G}$) (يسمى **Target Vector**)، يتم اختيار 3 متجهات عشوائية أخرى ($\mathbf{x}_{r1,G}, \mathbf{x}_{r2,G}, \mathbf{x}_{r3,G}$) من المجموعة، بشرط أن تكون فهارسهم ($i, r1, r2, r3$) مختلفة.
  - يتم إنشاء المتجه المتحور ($\mathbf{v}_{i,G+1}$) عن طريق إضافة الفرق المرجح بين متجهين ($\mathbf{x}_{r2,G} - \mathbf{x}_{r3,G}$) إلى متجه أساسي ($\mathbf{x}_{r1,G}$).
  - يتم التحكم في حجم هذا الفرق بواسطة معامل **Mutation Factor (F)**.
  
  *The mutation stage generates a mutant vector by adding the weighted differential of two random vectors to a third base vector.*

- **دور معامل الطفرة (Mutation Factor - F)**:
  - $F$ هو ثابت يحدده المستخدم ويتراوح بين.
  - $F$ يتحكم في تضخيم (Amplification) الفرق التفاضلي، وبالتالي يتحكم في **حجم الخطوة** وسرعة التقارب.
  
  *F controls the amplification of the differential variation and affects the convergence speed.*

### 3) Crossover (التهجين)

- **الفكرة الأساسية (بالعربية)**:
  الـ Crossover بيخلط المعاملات بين المتجه الهدف ($\mathbf{x}_{i,G}$) والمتجه المتحور ($\mathbf{v}_{i,G+1}$) لإنشاء متجه مرشح نهائي يسمى **Trial Vector ($\mathbf{u}_{i,G+1}$)**.
  
  *Crossover combines parameters from the target and mutant vectors to form the trial vector.*

- **آلية التنفيذ**:
  - يتم استخدام معامل التحكم **Crossover Rate (CR)**، وهو ثابت بين يحدده المستخدم.
  - لكل معامل (Component $j$) في المتجه، يتم عمل مقارنة: لو كان رقم عشوائي (Rand) أقل من أو يساوي CR، يتم أخذ المعامل من **Mutant Vector ($\mathbf{v}_{i,G+1}$)**. وإلا، يتم أخذ المعامل من **Target Vector ($\mathbf{x}_{i,G}$)**.
  - يتم ضمان أن الـ Trial Vector يحصل على الأقل على معامل واحد من الـ Mutant Vector.
  - بعد التهجين، يتم فحص الـ Trial Vector للتأكد من أنه لم يخرج خارج نطاق البحث المحدد في الـ Initialization، وفي حالة الخروج، يتم تعديله ليعود للحدود المسموح بها.
  
  *CR controls the number of components transferred; a parameter is taken from the mutant if a random number is less than or equal to CR.*

- **دور معدل التهجين (Crossover Rate - CR)**:
  - $CR$ يتحكم في عدد المعاملات التي سيتم استبدالها في المتجه.
  - قيمة $CR$ كبيرة (قرب 1) تؤدي إلى نقل معظم المكونات من المتجه المتحور، وهذا يسرع التقارب ومناسب للدوال **غير القابلة للفصل (Non-separable)**.
  
  *CR controls the degree of mixing between the target and mutant solutions.*

### 4) Selection (الاختيار)

- **الفكرة الأساسية (بالعربية)**:
  الاختيار هو مرحلة البقاء للأصلح. هنا يتم المقارنة بين جودة الحل الحالي (Target Vector $\mathbf{x}_{i,G}$) والحل الجديد (Trial Vector $\mathbf{u}_{i,G+1}$).
  
  *Selection determines which vector proceeds to the next generation based on performance.*

- **آلية التنفيذ**:
  - يتم مقارنة قيمة دالة الهدف ($f$) للمتجهين.
  - إذا كانت قيمة دالة الهدف للـ Trial Vector ($\mathbf{u}_{i,G+1}$) أقل أو تساوي قيمة دالة الهدف للـ Target Vector ($\mathbf{x}_{i,G}$)، فهذا يعني أن الحل الجديد أفضل، ويتم اختيار الـ Trial Vector ليكون مكان المتجه في الجيل التالي ($\mathbf{x}_{i,G+1}$).
  - وإذا لم يكن الحل الجديد أفضل، يتم الاحتفاظ بالـ Target Vector الأصلي للمجموعة التالية.

---

## 3. إيقاف الخوارزمية والمتغيرات (Stopping Criteria and Variants)

### معايير التوقف (Stopping Criteria)

- **الفكرة الأساسية (بالعربية)**:
  عملية التحسين بتتوقف لما نلبي شرط من الشروط المحددة مسبقاً.

- **الشروط الشائعة**:
  1. الوصول للحد الأقصى لعدد التكرارات/الأجيال ($G_{max}$).
  2. الوصول إلى قيمة خطأ (Error Value) مقبولة أو مُرضية.
  
  *The algorithm continues until the maximum number of iterations is reached or an acceptable error value is achieved.*

### شرح التسمية القياسية لـ DE

الخوارزمية القياسية تسمى **DE/rand/1/bin**:

- **DE**: اختصار لـ Differential Evolution.
- **rand**: يشير إلى أن المتجه الأساسي ($\mathbf{x}_{r1,G}$) المستخدم في الـ Mutation يتم اختياره **عشوائياً** من المجموعة.
- **1**: يشير إلى أنه تم استخدام **متجه فرق واحد** فقط في عملية الـ Mutation.
- **bin**: يشير إلى أن عملية الـ Crossover المستخدمة هي **ثنائية (Binomial)**.

### متغيرات (Variants) خوارزمية DE (مفاهيمياً)

الفرق بين المتغيرات بيكون في طريقة إنشاء المتجه المتحور ($\mathbf{v}_{i,G+1}$):

- **DE/rand/1/bin**: (كما شرحنا سابقاً) المتجه الأساسي عشوائي.
- **DE/best/1/bin**: المتجه الأساسي المستخدم في الـ Mutation هو **أفضل متجه** تم إيجاده حتى الآن في الجيل الحالي ($\mathbf{x}_{best,G}$). هذا النوع يعزز **الاستغلال (Exploitation)** بشكل كبير لأنه يوجه البحث نحو أفضل حل.
- **DE/target-to-best/1/bin**: هذا النوع يستخدم كلاً من المتجه الهدف والمتجه الأفضل لتوجيه البحث بشكل مباشر نحو المنطقة الأفضل، مما يزيد من سرعة الاستغلال.
- **DE/rand/2/bin**: يستخدم **متجهي فرق** بدل متجه واحد.

---

## 4. التحكم في الاستكشاف والاستغلال (Exploration vs Exploitation)

تعتمد كفاءة DE على ضبط المعاملات الثلاثة (NP, F, CR) لتحقيق توازن بين الاستكشاف (البحث الواسع) والاستغلال (تحسين الحلول الموجودة).

### تأثير المعاملات على الأداء

1. **حجم المجموعة (NP)**:
   - $NP$ له علاقة مباشرة بقدرة **الاستكشاف (Exploration)**.
   - **NP صغيرة** (اقل من 30): تقلل الاستكشاف وتزيد من خطر **التقارب المبكر (Premature Convergence)**.
   - **NP كبيرة**: تزيد من الاستكشاف، لكن تبطئ التقارب وتزيد الحسابات.

2. **معامل الطفرة (F)**:
   - $F$ يتحكم في حجم الخطوة.
   - **F صغيرة** (مثل 0.5): تحسن الاستغلال ولكن قد تؤدي إلى التقارب المبكر.
   - **F كبيرة** (أكبر من 1): تأخذ خطوات كبيرة، مما يزيد **الاستكشاف** ويبطئ التقارب.

3. **معدل التهجين (CR)**:
   - **CR صغيرة** (قرب 0): تؤدي إلى تغييرات قليلة في المتجه، وهذا مفيد للدوال **القابلة للفصل (Separable Functions)**.
   - **CR كبيرة** (قرب 1): تؤدي إلى تغييرات كثيرة (مما يسرع التقارب) وهذا مناسب للدوال **غير القابلة للفصل (Non-separable Functions)**.

### المشاكل الشائعة (Common Problems)

- **التقارب المبكر (Premature Convergence)**:
  يحدث عندما تقع الخوارزمية في حد أمثل محلي (Local Optimum) وتتوقف عن البحث (Stagnation). يزداد خطر هذه المشكلة عندما يكون معدل **الاستكشاف (Exploration)** قليلاً (مثل عندما يكون F أو NP صغير).
  
  *Premature convergence occurs when the algorithm gets stuck in a local optimum due to insufficient exploration.*

---

## 5. الخلاصة والمراجعة الامتحانية

### (1) تدفق خوارزمية DE في خطوات بسيطة

1. **Initialization**: إنشاء مجموعة عشوائية من المتجهات ($x_{i,1}$) في الجيل الأول $G=1$ ضمن الحدود المسموح بها.
2. **Repeat (Cycle)**: ابدأ التكرار من $G$ إلى $G+1$ لكل متجه في المجموعة.
3. **Mutation**: اختر 3 متجهات عشوائية ($r1, r2, r3$) وقم بإنشاء المتجه المتحور ($\mathbf{v}_{i,G+1}$) باستخدام عامل الطفرة $F$.
4. **Crossover**: ادمج المتجه الهدف ($\mathbf{x}_{i,G}$) مع المتجه المتحور ($\mathbf{v}_{i,G+1}$) باستخدام معدل التهجين $CR$ لإنشاء المتجه المرشح ($\mathbf{u}_{i,G+1}$).
5. **Selection**: قارن دالة الهدف للـ $\mathbf{x}_{i,G}$ والـ $\mathbf{u}_{i,G+1}$. يتم اختيار المتجه الأفضل (الأقل قيمة) للجيل التالي $\mathbf{x}_{i,G+1}$.
6. **Check Stop**: هل تم الوصول إلى $G_{max}$ أو الخطأ المطلوب؟ إذا نعم توقف، وإلا عد للخطوة 2.

### (2) ملاحظات مهمة للامتحان (Exam Notes)

- **الـ F و الـ CR ثوابت**: المعاملين F و CR يتم تحديدهما بواسطة المستخدم وهما ثوابت في الخوارزمية القياسية.
- **شرط NP**: يجب أن يكون حجم المجموعة NP $\geq 4$ حتى نتمكن من اختيار 3 متجهات عشوائية ومتجه هدف واحد (بفهارس مختلفة) في مرحلة الـ Mutation.
- **التقارب المبكر**: هو نتيجة ضعف الـ **Exploration** بسبب صغر قيمة NP أو F.
- **العلاقة بين CR والدوال**: CR الكبير (قرب 1) مناسب للدوال **غير القابلة للفصل (Non-separable)**، بينما CR الصغير مناسب للدوال **القابلة للفصل (Separable)**.

### (3) أخطاء شائعة للطلاب

1. **خلط الأدوار**: الخلط بين الـ Target Vector ($\mathbf{x}_{i,G}$) والـ Mutant Vector ($\mathbf{v}_{i,G+1}$) والـ Trial Vector ($\mathbf{u}_{i,G+1}$). (الـ Target هو الأصل، الـ Mutant هو المولود بالطفرة، والـ Trial هو الناتج النهائي للتهجين).

2. **التعميم على F و CR**: اعتبار أن F و CR الصغيرين دائماً سيئين؛ لا، هما يزيدان من **الاستغلال (Exploitation)**، وقد يكونا فعالين إذا كان الاستكشاف تم بنجاح في البداية.

3. **فهم الـ '1' في التسمية**: نسيان أن الرقم '1' في **DE/rand/1/bin** يشير إلى استخدام **متجه فرق واحد** فقط في الطفرة.

### (4) ورقة غش للمراجعة السريعة (Last-Minute Revision Cheat Sheet)

| المصطلح | دوره ووظيفته | التأثير على الأداء |
| :--- | :--- | :--- |
| **NP** (Population Size) | عدد الحلول المرشحة. | يتحكم في **Exploration**. NP صغير = خطر التقارب المبكر. |
| **F** (Mutation Factor) | يضخم الفرق التفاضلي (حجم الخطوة). | يتحكم في **Step Size** و **Convergence Speed**. |
| **CR** (Crossover Rate) | احتمالية أخذ المعامل من المتجه المتحور. | CR كبير جيد للـ Non-separable ويحسن التقارب. |
| **Mutation** | يخلق **Mutant Vector** ($\mathbf{v}$) باستخدام الفرق المرجح. | مصدر الـ Differential. |
| **Crossover** | يدمج الـ Target مع الـ Mutant لإنشاء **Trial Vector** ($\mathbf{u}$). | يتم التحكم فيه بواسطة CR. |
| **Selection** | يختار الأفضل بين $\mathbf{x}$ و $\mathbf{u}$ للجيل القادم. | مبدأ البقاء للأصلح. |
