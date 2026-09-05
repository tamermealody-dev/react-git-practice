# 🎓 Git & GitHub — Senior Dev Cheat Sheet

الدليل ده هيخليك تتكلم وتشتغل زي أي senior engineer. اتبع الخطوات بالترتيب، وطبّق كل حاجة فعليًا على الـ project ده.

---

## 1. المفاهيم الأساسية (Terminology)

| المصطلح | المعنى |
|---|---|
| **Repository (Repo)** | المجلد اللي فيه المشروع + كل الـ history بتاعته اللي Git بيتابعها |
| **Commit** | "لقطة" (snapshot) للتغييرات اللي عملتها، مع رسالة توضح إيه اللي اتغير |
| **Branch** | نسخة منفصلة من الكود بتشتغل عليها من غير ما تأثر على النسخة الأساسية |
| **Merge** | دمج التغييرات من branch لـ branch تاني |
| **Pull Request (PR)** | طلب رسمي على GitHub إنك عايز تدمج (merge) الـ branch بتاعك مع branch تاني، وبيتراجع (review) قبل ما يتوافق عليه |
| **Clone** | تنزيل نسخة من الـ repo على جهازك |
| **Push** | رفع الـ commits بتاعتك من جهازك لـ GitHub |
| **Pull** | تنزيل آخر تحديثات من GitHub لجهازك |
| **Remote** | الـ repo البعيد (على GitHub مثلاً) اللي بتتواصل معاه |
| **HEAD** | مؤشر بيقول "أنا واقف فين دلوقتي" في الـ history |
| **Staging Area** | منطقة وسطية بتحط فيها الملفات اللي عايز تعملها commit (باستخدام `git add`) |

---

## 2. أنواع الـ Branches (Branching Strategy)

السينيورز مش بيشتغلوا على `main` مباشرة أبدًا. بيستخدموا استراتيجية زي **Git Flow** أو **GitHub Flow**:

- **`main` (أو `master`)** → النسخة المستقرة (production-ready)، دايمًا شغالة 100%
- **`develop`** → الفرع اللي بيتجمع فيه كل الفيتشرز قبل ما تنزل production
- **`feature/*`** → لأي فيتشر جديدة، مثال: `feature/add-login`, `feature/dark-mode`
- **`bugfix/*`** → لإصلاح باگ مش عاجل، مثال: `bugfix/fix-input-validation`
- **`hotfix/*`** → إصلاح عاجل لمشكلة في الـ production، بيتعمل مباشرة من `main`
- **`release/*`** → لما تكون مستعد تطلع نسخة جديدة، بتعمل branch للتجهيز والاختبار

> 💡 **Agile concept**: كل فيتشر أو مهمة (task/story) بتاخد الـ branch بتاعها، وده بيسهل الـ **Sprint** والـ **code review** لأن كل حاجة منفصلة وواضحة.

---

## 3. خطوة بخطوة: من الصفر لأول Repo

```bash
# 1. روح لمجلد المشروع
cd react-git-practice

# 2. اعمل git init (لو لسه مش repo)
git init

# 3. شوف حالة الملفات
git status

# 4. ضيف الملفات للـ staging area
git add .

# 5. اعمل أول commit
git commit -m "chore: initial project scaffold"

# 6. اربط المشروع بـ GitHub (بعد ما تعمل repo فاضي على GitHub.com)
git remote add origin https://github.com/USERNAME/REPO_NAME.git

# 7. ارفع أول push
git branch -M main
git push -u origin main
```

---

## 4. تمرين عملي: هتعمل إيه بالظبط

### الخطوة أ: اعمل branch لفيتشر جديدة
```bash
git checkout -b feature/task-counter
```
ده هيخليك تنقل لـ branch جديد اسمه `feature/task-counter`.

### الخطوة ب: عدّل في الكود
افتح `src/App.jsx` وضيف feature بسيطة، مثلاً عداد يعرض عدد الـ tasks المتبقية.

### الخطوة ج: اعمل commit للتغيير
```bash
git add .
git commit -m "feat: add remaining tasks counter"
```

### الخطوة د: ارفع الـ branch على GitHub
```bash
git push -u origin feature/task-counter
```

### الخطوة هـ: افتح Pull Request
روح على GitHub → هتلاقي زرار "Compare & pull request" ظاهر تلقائي → اكتب وصف واضح لإيه اللي عملته → اعمل **Merge**.

### الخطوة و: ارجع لـ main وحدّثه
```bash
git checkout main
git pull origin main
```

---

## 5. Commit Message Convention (زي ما الشركات بتستخدم)

استخدم **Conventional Commits**:

```
feat: ضيفت فيتشر جديدة
fix: صلحت باگ
chore: تعديلات مش متعلقة بالكود (زي تحديث ملفات إعداد)
docs: تعديل في التوثيق
refactor: إعادة تنظيم كود من غير ما تغير السلوك
style: تعديل شكل الكود (spacing, formatting)
test: إضافة أو تعديل tests
```

مثال: `git commit -m "fix: resolve task deletion bug"`

---

## 6. مشاكل هتقابلك وحلها (Merge Conflicts)

لو اتنين شغالين على نفس السطر في نفس الملف، هيحصل **Merge Conflict**. لما يحصل:

```bash
git status          # هيوريك الملفات المتعارضة
# افتح الملف، هتلاقي علامات زي:
# <<<<<<< HEAD
# الكود بتاعك
# =======
# كود التاني
# >>>>>>> feature/branch-name

# بعد ما تختار الكود الصح وتمسح العلامات:
git add .
git commit -m "fix: resolve merge conflict"
```

---

## 7. أوامر يوميًا هتستخدمها كتير

```bash
git log --oneline --graph --all   # شوف تاريخ الـ commits بشكل مرئي
git branch                        # شوف كل الـ branches المحلية
git branch -a                     # شوف كل الـ branches (محلي + remote)
git diff                          # شوف الفرق قبل الـ commit
git stash                         # احفظ تعديلاتك مؤقتًا من غير commit
git stash pop                     # رجّع التعديلات المحفوظة
git checkout <branch>             # انتقل لـ branch تاني
git merge <branch>                # ادمج branch جانبي في اللي انت فيه
git reset --soft HEAD~1           # ألغي آخر commit بس سيب التعديلات
```

---

## 8. خطة اقتراحية لتتمرن (5 features = 5 branches = 5 PRs)

1. `feature/task-counter` → عداد للمهام المتبقية
2. `feature/delete-task` → زرار حذف مهمة
3. `feature/dark-mode` → تبديل بين dark/light theme
4. `bugfix/empty-input` → منع إضافة مهمة فاضية
5. `feature/local-storage` → حفظ المهام في localStorage

اعمل كل واحدة منهم في branch منفصل، بـ commits واضحة، وافتح PR لكل واحدة، وادمجها في `main`. بكده هتكون عملت فعليًا دورة حياة كاملة زي أي فريق حقيقي.

---

## 9. تشغيل المشروع محليًا

```bash
npm install
npm run dev
```

هيفتح على `http://localhost:5173` تقريبًا.

بالتوفيق 🚀
