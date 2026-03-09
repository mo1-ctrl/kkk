# منصة الامتحانات الطبية 🩺

## تعليمات استخدام المنصة

منصة امتحانات طبية متكاملة مصممة لطلاب الطب. تعمل كملف HTML واحد ويمكن رفعها على GitHub Pages أو أي استضافة ثابتة.

---

## خطوات تشغيل المشروع:

### 1. على GitHub Pages
1. ارفع المجلد كاملاً على repository جديد
2. اذهب إلى Settings > Pages
3. اختر Branch: main وFolder: / (root)
4. انتظر دقائق وافتح الرابط

### 2. للتجربة المحلية
- **الطريقة الأولى:** استخدم VS Code + Live Server Extension
  - افتح المجلد في VS Code
  - ثبت extension اسمه "Live Server"
  - اضغط Right Click على `index.html` > Open with Live Server

- **الطريقة الثانية:** Python HTTP Server
  ```bash
  cd exam
  python3 -m http.server 8000
  # ثم افتح http://localhost:8000
  ```

- **الطريقة الثالثة:** Node.js
  ```bash
  npx serve exam
  ```

---

## هيكل المجلدات:

```
exam/
├── index.html              ← الملف الرئيسي
├── questions.json          ← ملف الأسئلة
├── assets/
│   ├── images/             ← صور الأسئلة
│   │   ├── 1.jpg           ← صورة السؤال رقم 1
│   │   ├── 2.jpg           ← صورة السؤال رقم 2
│   │   └── ...
│   ├── audio/              ← ملفات الصوت
│   │   ├── login_success.mp3
│   │   ├── answer_correct.mp3
│   │   ├── answer_wrong.mp3
│   │   ├── submit_pass.mp3
│   │   └── submit_fail.mp3
│   └── video/              ← فيديو المحاضرة
│       └── lecture1.mp4
└── README.md
```

---

## لتغيير الامتحان:

### استبدال ملف الأسئلة:
1. افتح `questions.json`
2. استبدل المحتوى بملف الأسئلة الجديد
3. تأكد من الحفاظ على نفس الهيكل

### هيكل ملف الأسئلة:
```json
{
  "exam_name": "اسم الامتحان",
  "exam_password": "كلمة_المرور",
  "video_filename": "lecture1.mp4",
  "created_by": "Mohamed Salah",
  "questions": [
    {
      "id": 1,
      "question_id": "q1",
      "sequential_number": 1,
      "question_type": "mcq",
      "question_text": "نص السؤال",
      "highlight_terms": ["كلمة", "مهمة"],
      "options": {
        "A": "الخيار الأول",
        "B": "الخيار الثاني",
        "C": "الخيار الثالث",
        "D": "الخيار الرابع"
      },
      "correct_answer": "B",
      "explanation_egyptian": "شرح الإجابة بالعامية المصرية",
      "lecture_name": "المحاضرة الأولى",
      "page_number": "12"
    }
  ]
}
```

---

## إضافة صور للأسئلة:

- ضع صورة لكل سؤال باسم رقم السؤال في `assets/images/`
- الامتدادات المدعومة: `.jpg`, `.jpeg`, `.png`, `.gif`, `.webp`
- مثال: السؤال رقم 5 → صورة `5.jpg` أو `5.png`

---

## ملفات الصوت المطلوبة:

ضع هذه الملفات في `assets/audio/`:

| الملف | الوصف |
|-------|-------|
| `login_success.mp3` | يُشغل عند الدخول الصحيح |
| `answer_correct.mp3` | يُشغل عند الإجابة الصحيحة |
| `answer_wrong.mp3` | يُشغل عند الإجابة الخاطئة |
| `submit_pass.mp3` | يُشغل عند النجاح (≥50%) |
| `submit_fail.mp3` | يُشغل عند الرسوب (<50%) |

---

## إضافة فيديو المحاضرة:

1. ضع ملف الفيديو في `assets/video/`
2. اكتب اسم الملف في `questions.json` في حقل `video_filename`
3. الصيغ المدعومة: MP4, WebM, OGG

---

## نظام كلمة المرور:

- كلمة المرور تُقرأ من `questions.json` من حقل `exam_password`
- لتغييرها: عدّل القيمة في الملف

---

## Google Sheets Integration:

النتائج تُرسل تلقائياً إلى Google Sheet. للتعطيل:

1. افتح `index.html`
2. ابحث عن دالة `submitToSheets`
3. أضف `return;` في أول الدالة

### لتغيير رابط Google Sheets:
1. أنشئ Google Apps Script جديد
2. استخدم الكود التالي:
```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = JSON.parse(e.postData.contents);
  
  sheet.appendRow([
    data.name,
    data.id,
    data.exam,
    data.score,
    data.total,
    data.percent + '%',
    data.mcq,
    data.essay,
    data.wrong,
    data.duration,
    data.date
  ]);
  
  return ContentService.createTextOutput('OK');
}
```

---

## المميزات:

### ✅ شاشة تسجيل الدخول:
- تصميم Dark glassmorphism احترافي
- تحقق من الاسم والرقم الجامعي (9 أرقام) وكلمة المرور
- رسائل خطأ واضحة مع تأثيرات بصرية

### ✅ شاشة الامتحان:
- تصميم نظيف وواضح
- تصحيح فوري لكل سؤال (أخضر/أحمر)
- عرض صور الأسئلة (قابلة للطي)
- شرح كل سؤال بعد الإجابة
- مؤقت تلقائي (دقيقة لكل سؤال)
- تنقل سهل بين الأسئلة

### ✅ شاشة التحليلات:
- تصميم Dark احترافي
- دائرة النسبة المئوية متحركة
- إحصائيات مفصلة
- فيديو مراجعة المحاضرة
- تقرير الأسئلة الخاطئة
- إعادة الامتحان أو امتحان الغلط فقط

---

## ملاحظات تقنية:

1. **ملف واحد:** كل CSS و JavaScript داخل `index.html`
2. **Responsive:** يعمل على الموبايل والتابلت والكمبيوتر
3. **RTL:** دعم كامل للعربية من اليمين لليسار
4. **Offline:** يعمل بدون إنترنت (بعد التحميل الأول)
5. **صوت:** تأثيرات صوتية للتفاعل

---

## المتطلبات:

- متصفح حديث (Chrome, Firefox, Safari, Edge)
- لا يحتاج تثبيت أي برامج

---

## الدعم الفني:

للمساعدة أو الإبلاغ عن مشاكل:
- أنشئ Issue في GitHub Repository
- أو تواصل مع Mohamed Salah

---

**بالتوفيق في امتحاناتك! 🎓**

*by Mohamed Salah*
