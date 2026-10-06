# تشغيل المشروع على Railway

## الملفات المطلوبة

- `main.py`: التطبيق الرئيسي.
- `requirements.txt`: المكتبات المطلوبة.
- `railway.toml` و`Procfile`: إعدادات البناء والتشغيل.

## خطوات النشر

1. ارفع هذه الملفات إلى مستودع GitHub.
2. في Railway اختر **New Project → Deploy from GitHub Repo**.
3. Railway سيقرأ `railway.toml` ويشغّل:

   ```bash
   python main.py
   ```

4. من **Variables** أضف:

   | المتغير | القيمة المقترحة |
   |---|---|
   | `RUN_BOT` | `0` إذا لم ترفع `bot.py` |
   | `BASE_PATH` | `/data/panel_data` إذا أضفت Volume مركبًا على `/data`، وإلا `/app/panel_data` |
   | `PANEL_URL` | رابط Railway العام بعد إنشائه، مثل `https://your-app.up.railway.app` |
   | `PANEL_BOT_API_TOKEN` | اختياري، إذا كان الكود الخارجي يستخدم API الخاص باللوحة |

5. أنشئ **Public Domain** من إعدادات الخدمة.
6. إذا أردت عدم فقدان المستخدمين والملفات بعد إعادة النشر، أضف **Volume** واربطه على `/data` ثم اجعل `BASE_PATH=/data/panel_data`.

## ملاحظات مهمة

- Railway يمرر المنفذ تلقائيًا في المتغير `PORT`، والملف يستخدمه بالفعل مع `0.0.0.0`.
- الملف المرفوع لا يحتوي على `bot.py`؛ لذلك تشغيل Telegram لن يبدأ بهذه الحزمة وحدها. ارفع `bot.py` وأضف مكتباته إلى `requirements.txt` إذا كان البوت مطلوبًا.
- نظام التشغيل داخل Railway لا يضمن وجود Docker أو `apt-get` أو صلاحيات النظام؛ وظائف Docker/apt داخل اللوحة قد لا تعمل، بينما لوحة Flask نفسها تعمل.
- ملفات البيانات محلية؛ بدون Volume قد تختفي عند إعادة التشغيل أو إعادة النشر.
