# سكلات Musa85

مجموعة سكلات (Skills) لوكلاء الذكاء الاصطناعي: Claude Code وClaude.ai وCodex وCursor وغيرها.

## السكلات

| السكل | الوظيفة |
|---|---|
| [**tadween**](skills/tadween/SKILL.md) | **تدوين المشروع:** يكتب ويحدّث `AGENTS.md` و`CLAUDE.md` ومجلد `docs/`، فتبدأ أي جلسة جديدة فاهمةً المشروع دون إعادة شرح. يعمل عند قولك: «دوّن»، «وثّق»، «حدّث التوثيق». |

## التركيب

### Claude Code (وأي وكيل آخر) — على أي جهاز
```bash
# عالميًّا: يعمل في كل المجلدات
npx skills add Musa85/skills --skill tadween -g

# أو داخل مشروع واحد فقط
npx skills add Musa85/skills --skill tadween
```
بعد التركيب أعد تشغيل جلسة Claude Code، فالسكلات تُقرأ عند بدء الجلسة فقط.

**التحديث لاحقًا:** `npx skills update tadween -g`

### Claude.ai (المتصفّح وتطبيق سطح المكتب والجوّال)
1. نزّل المجلد `skills/tadween` واضغطه ملفَّ zip باسم `tadween.zip`، بحيث يكون `tadween/SKILL.md` داخله.
2. في claude.ai: **Settings ← Capabilities ← Skills ← Upload skill**، ثم اختر الملف.

## الاستخدام
- تلقائي: قل «دوّن» أو «وثّق المشروع» أو «حدّث التوثيق».
- يدوي: `/tadween` مع تركيز اختياري، مثل `/tadween ركّز على القرارات`.

شغّله دائمًا من **جذر المشروع**، لأنه يكتب ملفّاته في المجلد الحالي إن لم يكن المشروع مستودع git.

## الترخيص
MIT
