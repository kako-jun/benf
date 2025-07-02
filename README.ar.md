# benf

أداة CLI لاكتشاف الشذوذ باستخدام قانون بنفورد مع دعم الأرقام الدولية (اليابانية، الصينية، الهندية، العربية).

## نظرة عامة

`benf` يحلل البيانات الرقمية للتحقق من اتباعها لقانون بنفورد، الذي ينص على أنه في العديد من مجموعات البيانات الطبيعية، يظهر الرقم 1 كـ**الرقم الأول (الرقم الرائد)** حوالي 30.1% من الوقت، و2 يظهر 17.6% من الوقت، وهكذا. الانحرافات عن هذا القانون قد تشير إلى تلاعب في البيانات أو احتيال.

**ملاحظة**: تحلل هذه الأداة فقط **الرقم الأول** من كل عدد، وليس تسلسل الأرقام بالكامل.

**الميزات الفريدة:**
- 🌍 **دعم الأرقام الدولية**: الإنجليزية، اليابانية (全角・漢数字)، الصينية (中文数字)، الهندية (हिन्दी अंक)، العربية (الأرقام العربية)
- 📊 تنسيقات إدخال متعددة (Microsoft Excel، Word، PowerPoint، PDF، إلخ)
- 🌐 تحليل URL مباشر مع تحليل HTML
- 🔍 التركيز على اكتشاف الاحتيال مع مؤشرات مستوى المخاطر

## دعم الأرقام الدولية

### تنسيقات الأرقام المدعومة

#### 1. الأرقام كاملة العرض
```bash
echo "１２３４５６ ７８９０１２" | benf
```

#### 2. أرقام الكانجي (أساسية)
```bash
echo "一二三四五六七八九" | benf
```

#### 3. أرقام الكانجي (موضعية)
```bash
echo "一千二百三十四 五千六百七十八 九万一千二百" | benf
```

#### 4. أنماط مختلطة
```bash
echo "مبيعات123万円 مصاريف45万6千円 ربح78万９千円" | benf
```

### قواعد التحويل

| كانجي | رقم | ملاحظات |
|-------|-----|---------|
| 一 | 1 | رقم أساسي |
| 十 | 10 | مكان العشرات |
| 百 | 100 | مكان المئات |
| 千 | 1000 | مكان الآلاف |
| 万 | 10000 | مكان عشرات الآلاف |
| 一千二百三十四 | 1234 | التدوين الموضعي |

#### الأرقام العشرية
```bash
# يتم تحليل الأرقام ≥ 1 فقط
echo "12.34 0.567 123.45" | benf
# النتيجة: 1، (مستثناة)، 1 (الأرقام < 1 مستثناة)
```

#### الأرقام السالبة
```bash
# يستخدم الرقم الأول للقيمة المطلقة
echo "-123 -456 -789" | benf
# النتيجة: 1، 4، 7
```

### توافق الأرقام الصينية

التنفيذ الحالي يدعم الأرقام الصينية الأساسية المتطابقة مع الكانجي الياباني:

#### مدعوم (الأشكال الأساسية)
- 一二三四五六七八九 (1-9) - نفس اليابانية
- 十百千 (10، 100، 1000) - علامات موضعية

#### الدعم المخطط
- **الأشكال المالية**: 壹貳參肆伍陸柒捌玖 (متغيرات مضادة للاحتيال)
- **التقليدية**: 萬 (10,000) مقابل اليابانية 万
- **المتغيرات الإقليمية**: الصينية التقليدية مقابل المبسطة

### الأرقام الهندية (हिन्दी अंक)
```bash
# أرقام ديوناغاري
echo "१२३४५६ ७८९०१२" | benf --lang hi
```

### الأرقام العربية (الأرقام العربية)
```bash  
# الأرقام العربية-الهندية الشرقية
echo "١٢٣٤٥٦ ٧٨٩٠١٢" | benf --lang ar
```

### أنظمة الأرقام الأخرى (الدعم المستقبلي)

#### النصوص الإضافية (مخططة)
- **الفارسية**: ۰۱۲۳۴۵۶۷۸۹ (إيران، أفغانستان)
- **البنغالية**: ০১২৩৪৫৬৭৮৯ (بنغلاديش)
- **التاميلية**: ௦௧௨௩௪௫௬௭௮௯ (تاميل نادو)
- **التايلاندية**: ๐๑๒๓๔๕๖๗๘๙ (تايلاند)
- **الميانمارية**: ၀၁၂၃၄၅၆၇၈၉ (ميانمار)

> **ملاحظة**: دعم الأرقام الدولية يستمر في التوسع بناءً على طلب المستخدمين. الأولوية الحالية: تحليل الوثائق المالية اليابانية/الصينية/الهندية/العربية.

## التثبيت

### البناء من المصدر
```bash
git clone https://github.com/kako-jun/benf
cd benf
cargo build --release
cp target/release/benf /usr/local/bin/
```

### إصدارات ثنائية
تحميل من [صفحة الإصدارات](https://github.com/kako-jun/benf/releases)

## البداية السريعة

```bash
# تحليل ملف CSV
benf data.csv

# تحليل بيانات الموقع
benf --url https://example.com/financial-report

# بيانات الأنبوب
echo "123 456 789 101112" | benf

# إخراج JSON للأتمتة
benf data.csv --format json
```

## الاستخدام

### الصيغة الأساسية
```bash
benf [OPTIONS] [INPUT]
```

### طرق الإدخال
1. **مسار الملف**: `benf financial_data.xlsx`
2. **URL**: `benf --url https://api.example.com/data`
3. **نص**: `benf "123 456 789 101112"`
4. **أنبوب**: `cat data.txt | benf`

الأولوية: URL > ملف > نص > أنبوب

### الخيارات

| خيار | وصف |
|------|------|
| `--url <URL>` | جلب البيانات من URL |
| `--format <FORMAT>` | تنسيق الإخراج: text, csv, json, yaml, toml, xml |
| `--quiet` | إخراج أدنى (أرقام فقط) |
| `--verbose` | إحصائيات مفصلة |
| `--lang <LANGUAGE>` | لغة الإخراج: en, ja, zh, hi, ar (افتراضي: auto) |
| `--filter <RANGE>` | تصفية الأرقام (مثال: `--filter ">=100"`) |
| `--threshold <LEVEL>` | عتبة التنبيه: low, medium, high, critical |
| `--proxy <URL>` | خادم بروكسي HTTP |
| `--insecure` | تخطي التحقق من شهادة SSL |
| `--timeout <SECONDS>` | مهلة الطلب (افتراضي: 30) |
| `--log-level <LEVEL>` | مستوى السجل: debug, info, warn, error |
| `--help, -h` | عرض المساعدة |
| `--version, -V` | عرض الإصدار |

### تنسيقات الملفات المدعومة

| تنسيق | امتدادات | ملاحظات |
|--------|----------|---------|
| Microsoft Excel | .xlsx, .xls | بيانات جداول البيانات |
| Microsoft Word | .docx, .doc | تحليل الوثائق |
| Microsoft PowerPoint | .pptx, .ppt | بيانات العروض التقديمية |
| OpenDocument | ods, .odt | ملفات OpenOffice/LibreOffice |
| PDF | .pdf | استخراج النص |
| CSV/TSV | .csv, .tsv | بيانات منظمة |
| JSON/XML | .json, .xml | استجابات API |
| YAML/TOML | .yaml, .toml | ملفات التكوين |
| HTML | .html | صفحات الويب |
| نص | .txt | نص عادي |

## الإخراج

### الإخراج الافتراضي
```
نتائج تحليل قانون بنفورد

مجموعة البيانات: financial_data.csv
الأرقام المحللة: 1,247
مستوى المخاطر: عالي ⚠️

توزيع الرقم الأول:
1: ████████████████████████████ 28.3% (متوقع: 30.1%)
2: ████████████████████ 20.1% (متوقع: 17.6%)
3: ██████████ 9.8% (متوقع: 12.5%)
...

الاختبارات الإحصائية:
كاي تربيع: 23.45 (القيمة الاحتمالية: 0.003)
متوسط الانحراف المطلق: 2.1%

الحكم: تم اكتشاف انحراف كبير
```

### إخراج JSON
```json
{
  "dataset": "financial_data.csv",
  "numbers_analyzed": 1247,
  "risk_level": "HIGH",
  "digits": {
    "1": {"observed": 28.3, "expected": 30.1, "deviation": -1.8},
    "2": {"observed": 20.1, "expected": 17.6, "deviation": 2.5}
  },
  "statistics": {
    "chi_square": 23.45,
    "p_value": 0.003,
    "mad": 2.1
  },
  "verdict": "SIGNIFICANT_DEVIATION"
}
```

## أمثلة الاستخدام الواقعي

Benf يتبع فلسفة Unix ويعمل بامتياز مع أدوات Unix القياسية لمعالجة ملفات متعددة:

### سير عمل التدقيق المالي

```bash
# التدقيق المالي الفصلي - فحص جميع تقارير Excel
find ./الربع-الرابع-2024 -name "*.xlsx" | while read file; do
    echo "تدقيق: $file"
    benf "$file" --filter ">=1000" --threshold critical --verbose
    echo "---"
done

# التحقق من تقارير المصروفات الشهرية
for dept in المحاسبة التسويق المبيعات; do
    echo "القسم: $dept"
    find "./المصروفات/$dept" -name "*.xlsx" -exec benf {} --format json \; | \
    jq '.risk_level' | sort | uniq -c
done

# التحقق من الوثائق الضريبية (تحليل عالي الدقة)
find ./الإقرارات-الضريبية -name "*.pdf" | parallel benf {} --min-count 50 --format csv | \
awk -F, '$3=="Critical" {print "🚨 حرج:", $1}'
```

### المراقبة التلقائية والتنبيهات

```bash
# سكريبت المراقبة اليومية لصادرات نظام المحاسبة
#!/bin/bash
ALERT_EMAIL="audit@company.com"
find /exports/daily -name "*.csv" -newer /var/log/last-benf-check | while read file; do
    benf "$file" --format json | jq -r 'select(.risk_level=="Critical" or .risk_level=="High") | "\(.dataset): \(.risk_level)"'
done | mail -s "تنبيه بنفورد اليومي" $ALERT_EMAIL

# اكتشاف الاحتيال بالتكامل المستمر
find ./التقارير-المرفوعة -name "*.xlsx" -mtime -1 | \
xargs -I {} sh -c 'benf "$1" || echo "تنبيه احتيال: $1" >> /var/log/fraud-alerts.log' _ {}

# مراقبة المجلد في الوقت الفعلي باستخدام inotify
inotifywait -m ./الرفع-المالي -e create --format '%f' | while read file; do
    if [[ "$file" =~ \.(xlsx|csv|pdf)$ ]]; then
        echo "$(date): تحليل $file" >> /var/log/benf-monitor.log
        benf "./الرفع-المالي/$file" --threshold high || \
        echo "$(date): تنبيه - ملف مشبوه: $file" >> /var/log/fraud-alerts.log
    fi
done
```

### معالجة البيانات واسعة النطاق

```bash
# معالجة نظام الملفات المؤسسي بالكامل لتدقيق الامتثال
find /corporate-data -type f \( -name "*.xlsx" -o -name "*.csv" -o -name "*.pdf" \) | \
parallel -j 16 'echo "{}: $(benf {} --format json 2>/dev/null | jq -r .risk_level // "خطأ")"' | \
tee compliance-audit-$(date +%Y%m%d).log

# تحليل الأرشيف - معالجة البيانات التاريخية بكفاءة
find ./الأرشيف/2020-2024 -name "*.xlsx" -print0 | \
xargs -0 -n 100 -P 8 -I {} benf {} --filter ">=10000" --format csv | \
awk -F, 'BEGIN{OFS=","} NR>1 && $3~/High|Critical/ {sum++} END{print "ملفات عالية المخاطر:", sum}'

# مسح التخزين الشبكي مع تتبع التقدم
total_files=$(find /nfs/financial-data -name "*.xlsx" | wc -l)
find /nfs/financial-data -name "*.xlsx" | nl | while read num file; do
    echo "[$num/$total_files] معالجة: $(basename "$file")"
    benf "$file" --format json | jq -r '"ملف: \(.dataset), مخاطر: \(.risk_level), أرقام: \(.numbers_analyzed)"'
done | tee network-scan-report.txt
```

### التقارير والتحليلات المتقدمة

```bash
# تحليل توزيع المخاطر عبر الأقسام
for dept in */; do
    echo "=== القسم: $dept ==="
    find "$dept" -name "*.xlsx" | xargs -I {} benf {} --format json 2>/dev/null | \
    jq -r '.risk_level' | sort | uniq -c | awk '{print $2": "$1" ملفات"}'
    echo
done

# تحليل مخاطر السلاسل الزمنية (يتطلب ملفات مرتبة حسب التاريخ)
find ./التقارير-الشهرية -name "202[0-4]-*.xlsx" | sort | while read file; do
    month=$(basename "$file" .xlsx)
    risk=$(benf "$file" --format json 2>/dev/null | jq -r '.risk_level // "غير متاح"')
    echo "$month,$risk"
done > risk-timeline.csv

# إنشاء ملخص إحصائي
{
    echo "ملف,مستوى_المخاطر,عدد_الأرقام,كاي_مربع,قيمة_p"
    find ./عينة-التدقيق -name "*.xlsx" | while read file; do
        benf "$file" --format json 2>/dev/null | \
        jq -r '"\(.dataset),\(.risk_level),\(.numbers_analyzed),\(.statistics.chi_square),\(.statistics.p_value)"'
    done
} > التحليل-الإحصائي.csv

# التحليل المقارن بين الفترات
echo "مقارنة مستويات المخاطر الربع الثالث مقابل الرابع..."
q3_high=$(find ./الربع-الثالث-2024 -name "*.xlsx" | xargs -I {} benf {} --format json 2>/dev/null | jq -r 'select(.risk_level=="High" or .risk_level=="Critical")' | wc -l)
q4_high=$(find ./الربع-الرابع-2024 -name "*.xlsx" | xargs -I {} benf {} --format json 2>/dev/null | jq -r 'select(.risk_level=="High" or .risk_level=="Critical")' | wc -l)
echo "ملفات عالية المخاطر الربع الثالث: $q3_high"
echo "ملفات عالية المخاطر الربع الرابع: $q4_high"
echo "التغيير: $((q4_high - q3_high))"
```

### التكامل مع أدوات أخرى

```bash
# خطاف Git ما قبل الالتزام للتحقق من البيانات
#!/bin/bash
# .git/hooks/pre-commit
changed_files=$(git diff --cached --name-only --diff-filter=A | grep -E '\.(xlsx|csv|pdf)$')
for file in $changed_files; do
    if ! benf "$file" --min-count 10 >/dev/null 2>&1; then
        echo "⚠️  تحذير: $file قد يحتوي على أنماط بيانات مشبوهة"
        benf "$file" --format json | jq '.risk_level'
    fi
done

# التحقق من استيراد قاعدة البيانات
psql -c "COPY suspicious_files FROM STDIN CSV HEADER" <<< $(
    echo "اسم_الملف,مستوى_المخاطر,كاي_مربع,قيمة_p"
    find ./بيانات-الاستيراد -name "*.csv" | while read file; do
        benf "$file" --format json 2>/dev/null | \
        jq -r '"\(.dataset),\(.risk_level),\(.statistics.chi_square),\(.statistics.p_value)"'
    done
)

# تكامل Slack/Teams webhook
high_risk_files=$(find ./الرفع-اليومي -name "*.xlsx" -mtime -1 | \
    xargs -I {} benf {} --format json 2>/dev/null | \
    jq -r 'select(.risk_level=="High" or .risk_level=="Critical") | .dataset')

if [ -n "$high_risk_files" ]; then
    curl -X POST -H 'Content-type: application/json' \
    --data "{\"text\":\"🚨 تم اكتشاف ملفات عالية المخاطر:\n$high_risk_files\"}" \
    $SLACK_WEBHOOK_URL
fi
```

### حالات الاستخدام المتخصصة

```bash
# تدقيق الانتخابات (فحص أعداد الأصوات)
find ./بيانات-الانتخابات -name "*.csv" | parallel benf {} --min-count 100 --threshold low | \
grep -E "(HIGH|CRITICAL)" > شذوذ-الانتخابات.txt

# التحقق من البيانات العلمية
find ./بيانات-البحث -name "*.xlsx" | while read file; do
    lab=$(dirname "$file" | xargs basename)
    result=$(benf "$file" --format json | jq -r '.risk_level')
    echo "$lab,$file,$result"
done | grep -E "(High|Critical)" > مشاكل-سلامة-البيانات.csv

# التحقق من فواتير سلسلة التوريد
find ./الفواتير/2024 -name "*.pdf" | parallel 'vendor=$(dirname {} | xargs basename); benf {} --format json | jq --arg v "$vendor" '"'"'{vendor: $v, file: .dataset, risk: .risk_level}'"'"' > تحليل-الفواتير.jsonl

# تحليل مطالبات التأمين
find ./المطالبات -name "*.xlsx" | while read file; do
    claim_id=$(basename "$file" .xlsx)
    benf "$file" --filter ">=1000" --format json | \
    jq --arg id "$claim_id" '{معرف_المطالبة: $id, تقييم_المخاطر: .risk_level, إجمالي_الأرقام: .numbers_analyzed}'
done | jq -s '.' > تقييم-مخاطر-المطالبات.json
```

## أمثلة

### اكتشاف الاحتيال
```bash
# مراقبة بيانات المبيعات
benf sales_report.xlsx --threshold high

# مراقبة السجلات في الوقت الفعلي
tail -f transactions.log | benf --format json | jq 'select(.risk_level == "HIGH")'

# تحليل دفعي
find . -name "*.csv" -exec benf {} \; | grep "HIGH"
```

### الأرقام العربية
```bash
# الأرقام العربية-الهندية
echo "١٢٣ ٤٥٦ ٧٨٩" | benf

# أنماط مختلطة
benf arabic_financial_report.pdf
```

### تحليل الويب
```bash
# موقع مالي
benf --url https://company.com/earnings --format json

# نقطة نهاية API
benf --url https://api.finance.com/data --proxy http://proxy:8080
```

### الأتمتة
```bash
# فحص الاحتيال اليومي
#!/bin/bash
RESULT=$(benf daily_sales.csv --format json)
RISK=$(echo $RESULT | jq -r '.risk_level')
if [ "$RISK" = "HIGH" ]; then
    echo $RESULT | mail -s "تنبيه احتيال" admin@company.com
fi
```

## مستويات المخاطر

| مستوى | قيمة كاي تربيع p | التفسير |
|-------|------------------|----------|
| منخفض | p > 0.1 | توزيع طبيعي |
| متوسط | 0.05 < p ≤ 0.1 | انحراف طفيف |
| عالي | 0.01 < p ≤ 0.05 | انحراف كبير |
| حرج | p ≤ 0.01 | دليل قوي على التلاعب |

## حالات الاستخدام الشائعة

- **تدقيق المحاسبة**: كشف السجلات المالية المُلاعب بها
- **التحقيقات الضريبية**: تحديد إقرارات الدخل المشبوهة
- **مراقبة الانتخابات**: التحقق من صحة عد الأصوات
- **مطالبات التأمين**: اكتشاف أنماط المطالبات الاحتيالية
- **البيانات العلمية**: التحقق من نتائج البحث
- **مراقبة الجودة**: مراقبة بيانات التصنيع

## ⚠️ قيود مهمة

**قانون بنفورد لا ينطبق على:**
- **النطاقات المقيدة**: طول البالغين (1.5-2.0م)، العمر (0-100)، درجة الحرارة
- **البيانات المتسلسلة**: أرقام الفواتير، معرفات الموظفين، الرموز البريدية
- **الأرقام المُعيَّنة**: أرقام الهاتف، أرقام الضمان الاجتماعي، أرقام اليانصيب
- **مجموعات البيانات الصغيرة**: أقل من 30-50 رقم (غير كافية للتحليل الإحصائي)
- **بيانات مصدر واحد**: من نفس العملية/الآلة بأحجام متشابهة
- **البيانات المدورة**: مبالغ مدورة بشدة (مثل، تنتهي جميعها بـ 00)

**الأنسب لـ:**
- **بيانات طبيعية متعددة المقاييس**: المعاملات المالية، السكان، القياسات الفيزيائية
- **مصادر متنوعة**: بيانات مختلطة من عمليات/إطارات زمنية مختلفة
- **مجموعات بيانات كبيرة**: 100+ رقم للتحليل الموثوق
- **بيانات غير مُلاعب بها**: طبيعية، غير مقيدة اصطناعياً

## الخلفية التاريخية

**الاكتشاف والتطوير:**
- **1881**: سيمون نيوكومب لاحظ الظاهرة لأول مرة أثناء دراسة جداول اللوغاريتم
- **1938**: الفيزيائي فرانك بنفورد أعاد اكتشاف وإضفاء الطابع الرسمي على القانون من خلال بحث شامل
- **1972**: أول تطبيق في الأدبيات الأكاديمية للمحاسبة واكتشاف الاحتيال
- **الثمانينيات**: بدأت شركات المحاسبة الكبرى في استخدام قانون بنفورد كأداة تدقيق قياسية
- **التسعينيات**: مارك نيغريني شعبى استخدامه في المحاسبة الجنائية واكتشاف الاحتيال الضريبي
- **الألفية الثانية+**: توسع إلى مراقبة الانتخابات، التحقق من البيانات العلمية، والتحقيق في الجرائم المالية

**التطبيقات الحديثة:**
- تستخدمه مصلحة الضرائب الأمريكية لفرز التدقيق الضريبي
- أداة قياسية في شركات المحاسبة الأربع الكبرى
- تطبق في اكتشاف احتيال الانتخابات (خاصة تحليل انتخابات إيران 2009)
- توظف في تحقيقات مكافحة غسل الأموال

## رموز الخروج

| رمز | المعنى |
|-----|-------|
| 0 | نجح |
| 1 | خطأ عام |
| 2 | معاملات غير صحيحة |
| 3 | خطأ ملف/شبكة |
| 10 | تم اكتشاف مخاطر عالية |
| 11 | تم اكتشاف مخاطر حرجة |

## التكوين

Benf يحترم متغيرات البيئة القياسية:
- `HTTP_PROXY` / `HTTPS_PROXY`: إعدادات البروكسي
- `NO_PROXY`: قائمة تجاوز البروكسي

السجلات تُكتب إلى:
- Linux: `~/.local/state/benf/`
- macOS: `~/Library/Logs/benf/`
- Windows: `%APPDATA%\benf\Logs\`

## المساهمة

انظر [CONTRIBUTING.md](CONTRIBUTING.md) لإرشادات التطوير.

## الترخيص

ترخيص MIT - انظر ملف [LICENSE](LICENSE).

## المراجع

- [قانون بنفورد - ويكيبيديا](https://ar.wikipedia.org/wiki/قانون_بنفورد)
- [استخدام قانون بنفورد لاكتشاف الاحتيال](https://example.com/benford-fraud)