# benf

अंतर्राष्ट्रीय अंकों (जापानी, चीनी, हिंदी, अरबी) के समर्थन के साथ बेनफोर्ड के नियम का उपयोग करके विसंगतियों का पता लगाने वाला CLI उपकरण।

## अवलोकन

`benf` संख्यात्मक डेटा का विश्लेषण करता है यह जांचने के लिए कि यह बेनफोर्ड के नियम का पालन करता है या नहीं, जो बताता है कि कई प्राकृतिक रूप से घटित होने वाले डेटासेट में, अंक 1 **पहले अंक (अग्रणी अंक)** के रूप में लगभग 30.1% समय दिखाई देता है, 2 17.6% समय दिखाई देता है, और इसी तरह। इस नियम से विचलन डेटा हेराफेरी या धोखाधड़ी का संकेत दे सकता है।

**नोट**: यह उपकरण केवल प्रत्येक संख्या के **पहले अंक** का विश्लेषण करता है, पूरे संख्या अनुक्रम का नहीं।

**अनूठी विशेषताएं:**
- 🌍 **अंतर्राष्ट्रीय अंक समर्थन**: अंग्रेजी, जापानी (全角・漢数字), चीनी (中文数字), हिंदी (हिन्दी अंक), अरबी (الأرقام العربية)
- 📊 कई इनपुट प्रारूप (Microsoft Excel, Word, PowerPoint, PDF, आदि)
- 🌐 HTML पार्सिंग के साथ प्रत्यक्ष URL विश्लेषण
- 🔍 जोखिम स्तर संकेतकों के साथ धोखाधड़ी का पता लगाने पर फोकस

## अंतर्राष्ट्रीय अंक समर्थन

### समर्थित संख्या प्रारूप

#### 1. पूर्ण-चौड़ाई अंक
```bash
echo "１２３४५６ ７８９０１２" | benf
```

#### 2. कांजी अंक (बुनियादी)
```bash
echo "一二三四五六七八九" | benf
```

#### 3. कांजी अंक (स्थितीय)
```bash
echo "一千二百三十四 五千六百七十八 九万一千二百" | benf
```

#### 4. मिश्रित पैटर्न
```bash
echo "बिक्री123万円 खर्च45万6千円 लाभ78万９千円" | benf
```

### रूपांतरण नियम

| कांजी | संख्या | टिप्पणियां |
|-------|--------|----------|
| 一 | 1 | बुनियादी अंक |
| 十 | 10 | दहाई का स्थान |
| 百 | 100 | सैकड़े का स्थान |
| 千 | 1000 | हजार का स्थान |
| 万 | 10000 | दस हजार का स्थान |
| 一千二百三十四 | 1234 | स्थितीय संकेतन |

#### दशमलव संख्याएं
```bash
# केवल ≥ 1 संख्याओं का विश्लेषण किया जाता है
echo "12.34 0.567 123.45" | benf
# परिणाम: 1, (बाहर), 1 (1 से कम संख्याएं बाहर)
```

#### नकारात्मक संख्याएं
```bash
# निरपेक्ष मान के पहले अंक का उपयोग करता है
echo "-123 -456 -789" | benf
# परिणाम: 1, 4, 7
```

### चीनी अंक अनुकूलता

वर्तमान कार्यान्वयन जापानी कांजी के समान बुनियादी चीनी अंकों का समर्थन करता है:

#### समर्थित (बुनियादी रूप)
- 一二三四五六七八九 (1-9) - जापानी के समान
- 十百千 (10, 100, 1000) - स्थितीय मार्कर

#### योजनाबद्ध समर्थन
- **वित्तीय रूप**: 壹貳參肆伍陸柒捌玖 (धोखाधड़ी विरोधी रूप)
- **पारंपरिक**: 萬 (10,000) बनाम जापानी 万
- **क्षेत्रीय रूप**: पारंपरिक बनाम सरलीकृत चीनी

### हिंदी अंक (हिन्दी अंक)
```bash
# देवनागरी अंक
echo "१२३४५६ ७८९०१२" | benf --lang hi
```

### अरबी अंक (الأرقام العربية)
```bash  
# पूर्वी अरबी-भारतीय अंक
echo "١٢٣٤٥٦ ٧٨٩٠١٢" | benf --lang ar
```

### अन्य अंक प्रणालियां (भविष्य में समर्थन)

#### अतिरिक्त लिपियां (योजनाबद्ध)
- **फारसी**: ۰۱۲۳۴۵۶۷۸۹ (ईरान, अफगानिस्तान)
- **बंगाली**: ০১২৩৪৫৬৭৮৯ (बांग्लादेश)
- **तमिल**: ௦௧௨௩௪௫௬௭௮௯ (तमिलनाडु)
- **थाई**: ๐๑๒๓๔๕๖๗๘๙ (थाईलैंड)
- **म्यांमार**: ၀၁၂၃၄၅၆၇၈၉ (म्यांमार)

> **नोट**: अंतर्राष्ट्रीय अंक समर्थन उपयोगकर्ता मांग के आधार पर निरंतर विस्तृत हो रहा है। वर्तमान प्राथमिकता: जापानी/चीनी/हिंदी/अरबी वित्तीय दस्तावेज विश्लेषण।

## स्थापना

### स्रोत से निर्माण
```bash
git clone https://github.com/kako-jun/benf
cd benf
cargo build --release
cp target/release/benf /usr/local/bin/
```

### बाइनरी रिलीज
[रिलीज पृष्ठ](https://github.com/kako-jun/benf/releases) से डाउनलोड करें

## त्वरित प्रारंभ

```bash
# CSV फ़ाइल का विश्लेषण
benf data.csv

# वेबसाइट डेटा का विश्लेषण
benf --url https://example.com/financial-report

# पाइप डेटा
echo "123 456 789 101112" | benf

# स्वचालन के लिए JSON आउटपुट
benf data.csv --format json
```

## उपयोग

### बुनियादी वाक्यविन्यास
```bash
benf [OPTIONS] [INPUT]
```

### इनपुट विधियां
1. **फ़ाइल पथ**: `benf financial_data.xlsx`
2. **URL**: `benf --url https://api.example.com/data`
3. **स्ट्रिंग**: `benf "123 456 789 101112"`
4. **पाइप**: `cat data.txt | benf`

प्राथमिकता: URL > फ़ाइल > स्ट्रिंग > पाइप

### विकल्प

| विकल्प | विवरण |
|--------|--------|
| `--url <URL>` | URL से डेटा प्राप्त करें |
| `--format <FORMAT>` | आउटपुट प्रारूप: text, csv, json, yaml, toml, xml |
| `--quiet` | न्यूनतम आउटपुट (केवल संख्याएं) |
| `--verbose` | विस्तृत आंकड़े |
| `--lang <LANGUAGE>` | आउटपुट भाषा: en, ja, zh, hi, ar (डिफ़ॉल्ट: auto) |
| `--filter <RANGE>` | संख्याओं को फ़िल्टर करें (उदा.: `--filter ">=100"`) |
| `--threshold <LEVEL>` | अलर्ट थ्रेशोल्ड: low, medium, high, critical |
| `--proxy <URL>` | HTTP प्रॉक्सी सर्वर |
| `--insecure` | SSL प्रमाणपत्र सत्यापन छोड़ें |
| `--timeout <SECONDS>` | अनुरोध समय सीमा (डिफ़ॉल्ट: 30) |
| `--log-level <LEVEL>` | लॉग स्तर: debug, info, warn, error |
| `--help, -h` | सहायता दिखाएं |
| `--version, -V` | संस्करण दिखाएं |

### समर्थित फ़ाइल प्रारूप

| प्रारूप | एक्सटेंशन | टिप्पणियां |
|---------|------------|------------|
| Microsoft Excel | .xlsx, .xls | स्प्रेडशीट डेटा |
| Microsoft Word | .docx, .doc | दस्तावेज़ विश्लेषण |
| Microsoft PowerPoint | .pptx, .ppt | प्रस्तुति डेटा |
| OpenDocument | ods, .odt | OpenOffice/LibreOffice फ़ाइलें |
| PDF | .pdf | टेक्स्ट निष्कर्षण |
| CSV/TSV | .csv, .tsv | संरचित डेटा |
| JSON/XML | .json, .xml | API प्रतिक्रियाएं |
| YAML/TOML | .yaml, .toml | कॉन्फ़िगरेशन फ़ाइलें |
| HTML | .html | वेब पेज |
| टेक्स्ट | .txt | सादा टेक्स्ट |

## आउटपुट

### डिफ़ॉल्ट आउटपुट
```
बेनफोर्ड के नियम का विश्लेषण परिणाम

डेटासेट: financial_data.csv
विश्लेषित संख्याएँ: 1,247
जोखिम स्तर: उच्च ⚠️

पहले अंक का वितरण:
1: ████████████████████████████ 28.3% (अपेक्षित: 30.1%)
2: ████████████████████ 20.1% (अपेक्षित: 17.6%)
3: ██████████ 9.8% (अपेक्षित: 12.5%)
...

सांख्यिकीय परीक्षण:
काई-स्क्वायर: 23.45 (p-मान: 0.003)
औसत निरपेक्ष विचलन: 2.1%

फैसला: महत्वपूर्ण विचलन का पता चला
```

### JSON आउटपुट
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

## वास्तविक-विश्व उपयोग उदाहरण

Benf Unix दर्शन का पालन करता है और कई फ़ाइलों को संसाधित करने के लिए मानक Unix उपकरणों के साथ उत्कृष्ट रूप से काम करता है:

### वित्तीय ऑडिट वर्कफ़्लो

```bash
# त्रैमासिक वित्तीय ऑडिट - सभी Excel रिपोर्ट की जांच
find ./Q4-2024 -name "*.xlsx" | while read file; do
    echo "ऑडिटिंग: $file"
    benf "$file" --filter ">=1000" --threshold critical --verbose
    echo "---"
done

# मासिक व्यय रिपोर्ट सत्यापन
for dept in लेखा विपणन बिक्री; do
    echo "विभाग: $dept"
    find "./व्यय/$dept" -name "*.xlsx" -exec benf {} --format json \; | \
    jq '.risk_level' | sort | uniq -c
done

# कर दस्तावेज़ सत्यापन (उच्च-सटीकता विश्लेषण)
find ./कर-फाइलिंग -name "*.pdf" | parallel benf {} --min-count 50 --format csv | \
awk -F, '$3=="Critical" {print "🚨 गंभीर:", $1}'
```

### स्वचालित निगरानी और अलर्ट

```bash
# लेखांकन सिस्टम निर्यात के लिए दैनिक निगरानी स्क्रिप्ट
#!/bin/bash
ALERT_EMAIL="audit@company.com"
find /exports/daily -name "*.csv" -newer /var/log/last-benf-check | while read file; do
    benf "$file" --format json | jq -r 'select(.risk_level=="Critical" or .risk_level=="High") | "\(.dataset): \(.risk_level)"'
done | mail -s "दैनिक बेनफोर्ड अलर्ट" $ALERT_EMAIL

# निरंतर एकीकरण धोखाधड़ी का पता लगाना
find ./अपलोड-रिपोर्ट -name "*.xlsx" -mtime -1 | \
xargs -I {} sh -c 'benf "$1" || echo "धोखाधड़ी अलर्ट: $1" >> /var/log/fraud-alerts.log' _ {}

# inotify के साथ रियल-टाइम फ़ोल्डर निगरानी
inotifywait -m ./वित्तीय-अपलोड -e create --format '%f' | while read file; do
    if [[ "$file" =~ \.(xlsx|csv|pdf)$ ]]; then
        echo "$(date): विश्लेषण $file" >> /var/log/benf-monitor.log
        benf "./वित्तीय-अपलोड/$file" --threshold high || \
        echo "$(date): अलर्ट - संदिग्ध फ़ाइल: $file" >> /var/log/fraud-alerts.log
    fi
done
```

### बड़े पैमाने पर डेटा प्रसंस्करण

```bash
# अनुपालन ऑडिट के लिए संपूर्ण कॉर्पोरेट फ़ाइल सिस्टम प्रक्रिया
find /corporate-data -type f \( -name "*.xlsx" -o -name "*.csv" -o -name "*.pdf" \) | \
parallel -j 16 'echo "{}: $(benf {} --format json 2>/dev/null | jq -r .risk_level // "त्रुटि")"' | \
tee compliance-audit-$(date +%Y%m%d).log

# आर्काइव विश्लेषण - ऐतिहासिक डेटा कुशलतापूर्वक संसाधित करें
find ./अभिलेखागार/2020-2024 -name "*.xlsx" -print0 | \
xargs -0 -n 100 -P 8 -I {} benf {} --filter ">=10000" --format csv | \
awk -F, 'BEGIN{OFS=","} NR>1 && $3~/High|Critical/ {sum++} END{print "उच्च-जोखिम फ़ाइलें:", sum}'

# प्रगति ट्रैकिंग के साथ नेटवर्क स्टोरेज स्कैनिंग
total_files=$(find /nfs/financial-data -name "*.xlsx" | wc -l)
find /nfs/financial-data -name "*.xlsx" | nl | while read num file; do
    echo "[$num/$total_files] प्रसंस्करण: $(basename "$file")"
    benf "$file" --format json | jq -r '"फ़ाइल: \(.dataset), जोखिम: \(.risk_level), संख्याएं: \(.numbers_analyzed)"'
done | tee network-scan-report.txt
```

### उन्नत रिपोर्टिंग और विश्लेषण

```bash
# विभागों में जोखिम वितरण विश्लेषण
for dept in */; do
    echo "=== विभाग: $dept ==="
    find "$dept" -name "*.xlsx" | xargs -I {} benf {} --format json 2>/dev/null | \
    jq -r '.risk_level' | sort | uniq -c | awk '{print $2": "$1" फ़ाइलें"}'
    echo
done

# समय-श्रृंखला जोखिम विश्लेषण (तिथि-क्रमबद्ध फ़ाइलों की आवश्यकता)
find ./मासिक-रिपोर्ट -name "202[0-4]-*.xlsx" | sort | while read file; do
    month=$(basename "$file" .xlsx)
    risk=$(benf "$file" --format json 2>/dev/null | jq -r '.risk_level // "N/A"')
    echo "$month,$risk"
done > risk-timeline.csv

# सांख्यिकीय सारांश पीढ़ी
{
    echo "फ़ाइल,जोखिम_स्तर,संख्या_गिनती,काई_स्क्वायर,p_मान"
    find ./ऑडिट-नमूना -name "*.xlsx" | while read file; do
        benf "$file" --format json 2>/dev/null | \
        jq -r '"\(.dataset),\(.risk_level),\(.numbers_analyzed),\(.statistics.chi_square),\(.statistics.p_value)"'
    done
} > सांख्यिकीय-विश्लेषण.csv

# अवधियों के बीच तुलनात्मक विश्लेषण
echo "Q3 बनाम Q4 जोखिम स्तरों की तुलना..."
q3_high=$(find ./Q3-2024 -name "*.xlsx" | xargs -I {} benf {} --format json 2>/dev/null | jq -r 'select(.risk_level=="High" or .risk_level=="Critical")' | wc -l)
q4_high=$(find ./Q4-2024 -name "*.xlsx" | xargs -I {} benf {} --format json 2>/dev/null | jq -r 'select(.risk_level=="High" or .risk_level=="Critical")' | wc -l)
echo "Q3 उच्च-जोखिम फ़ाइलें: $q3_high"
echo "Q4 उच्च-जोखिम फ़ाइलें: $q4_high"
echo "परिवर्तन: $((q4_high - q3_high))"
```

### अन्य उपकरणों के साथ एकीकरण

```bash
# डेटा सत्यापन के लिए Git प्री-कमिट हुक
#!/bin/bash
# .git/hooks/pre-commit
changed_files=$(git diff --cached --name-only --diff-filter=A | grep -E '\.(xlsx|csv|pdf)$')
for file in $changed_files; do
    if ! benf "$file" --min-count 10 >/dev/null 2>&1; then
        echo "⚠️  चेतावनी: $file में संदिग्ध डेटा पैटर्न हो सकते हैं"
        benf "$file" --format json | jq '.risk_level'
    fi
done

# डेटाबेस आयात सत्यापन
psql -c "COPY suspicious_files FROM STDIN CSV HEADER" <<< $(
    echo "फ़ाइलनाम,जोखिम_स्तर,काई_स्क्वायर,p_मान"
    find ./आयात-डेटा -name "*.csv" | while read file; do
        benf "$file" --format json 2>/dev/null | \
        jq -r '"\(.dataset),\(.risk_level),\(.statistics.chi_square),\(.statistics.p_value)"'
    done
)

# Slack/Teams webhook एकीकरण
high_risk_files=$(find ./दैनिक-अपलोड -name "*.xlsx" -mtime -1 | \
    xargs -I {} benf {} --format json 2>/dev/null | \
    jq -r 'select(.risk_level=="High" or .risk_level=="Critical") | .dataset')

if [ -n "$high_risk_files" ]; then
    curl -X POST -H 'Content-type: application/json' \
    --data "{\"text\":\"🚨 उच्च-जोखिम फ़ाइलें खोजी गईं:\n$high_risk_files\"}" \
    $SLACK_WEBHOOK_URL
fi
```

### विशेष उपयोग के मामले

```bash
# चुनाव ऑडिट (वोट गिनती की जांच)
find ./चुनाव-डेटा -name "*.csv" | parallel benf {} --min-count 100 --threshold low | \
grep -E "(HIGH|CRITICAL)" > चुनाव-विसंगतियां.txt

# वैज्ञानिक डेटा सत्यापन
find ./अनुसंधान-डेटा -name "*.xlsx" | while read file; do
    lab=$(dirname "$file" | xargs basename)
    result=$(benf "$file" --format json | jq -r '.risk_level')
    echo "$lab,$file,$result"
done | grep -E "(High|Critical)" > डेटा-अखंडता-मुद्दे.csv

# आपूर्ति श्रृंखला चालान सत्यापन
find ./चालान/2024 -name "*.pdf" | parallel 'vendor=$(dirname {} | xargs basename); benf {} --format json | jq --arg v "$vendor" '"'"'{vendor: $v, file: .dataset, risk: .risk_level}'"'"' > चालान-विश्लेषण.jsonl

# बीमा दावा विश्लेषण
find ./दावे -name "*.xlsx" | while read file; do
    claim_id=$(basename "$file" .xlsx)
    benf "$file" --filter ">=1000" --format json | \
    jq --arg id "$claim_id" '{दावा_आईडी: $id, जोखिम_मूल्यांकन: .risk_level, कुल_संख्याएं: .numbers_analyzed}'
done | jq -s '.' > दावे-जोखिम-मूल्यांकन.json
```

## उदाहरण

### धोखाधड़ी का पता लगाना
```bash
# बिक्री डेटा की निगरानी
benf sales_report.xlsx --threshold high

# रियल-टाइम लॉग निगरानी
tail -f transactions.log | benf --format json | jq 'select(.risk_level == "HIGH")'

# बैच विश्लेषण
find . -name "*.csv" -exec benf {} \; | grep "HIGH"
```

### हिंदी अंक
```bash
# देवनागरी अंक
echo "१२३ ४५६ ७८९" | benf

# मिश्रित पैटर्न
benf hindi_financial_report.pdf
```

### वेब विश्लेषण
```bash
# वित्तीय वेबसाइट
benf --url https://company.com/earnings --format json

# API एंडपॉइंट
benf --url https://api.finance.com/data --proxy http://proxy:8080
```

### स्वचालन
```bash
# दैनिक धोखाधड़ी जांच
#!/bin/bash
RESULT=$(benf daily_sales.csv --format json)
RISK=$(echo $RESULT | jq -r '.risk_level')
if [ "$RISK" = "HIGH" ]; then
    echo $RESULT | mail -s "धोखाधड़ी अलर्ट" admin@company.com
fi
```

## जोखिम स्तर

| स्तर | काई-स्क्वायर p-मान | व्याख्या |
|------|---------------------|---------|
| कम | p > 0.1 | सामान्य वितरण |
| मध्यम | 0.05 < p ≤ 0.1 | हल्का विचलन |
| उच्च | 0.01 < p ≤ 0.05 | महत्वपूर्ण विचलन |
| गंभीर | p ≤ 0.01 | हेराफेरी का मजबूत प्रमाण |

## सामान्य उपयोग के मामले

- **लेखांकन ऑडिट**: हेराफेरी वाले वित्तीय रिकॉर्ड का पता लगाना
- **कर जांच**: संदिग्ध आय घोषणाओं की पहचान
- **चुनाव निगरानी**: वोट गिनती की प्रामाणिकता की जांच
- **बीमा दावे**: धोखाधड़ी के दावे के पैटर्न का पता लगाना
- **वैज्ञानिक डेटा**: अनुसंधान परिणामों की पुष्टि
- **गुणवत्ता नियंत्रण**: निर्माण डेटा की निगरानी

## ⚠️ महत्वपूर्ण सीमाएं

**बेनफोर्ड का नियम लागू नहीं होता:**
- **बाधित रेंज**: वयस्क ऊंचाई (1.5-2.0मी), आयु (0-100), तापमान
- **अनुक्रमिक डेटा**: इनवॉइस नंबर, कर्मचारी ID, पिन कोड
- **निर्दिष्ट संख्याएं**: फोन नंबर, सामाजिक सुरक्षा नंबर, लॉटरी नंबर
- **छोटे डेटासेट**: 30-50 से कम संख्याएं (सांख्यिकीय विश्लेषण के लिए अपर्याप्त)
- **एकल-स्रोत डेटा**: समान प्रक्रिया/मशीन से समान परिमाण के साथ
- **गोल डेटा**: भारी रूप से गोल राशि (जैसे, सभी 00 में समाप्त)

**सबसे उपयुक्त:**
- **बहु-पैमाने प्राकृतिक डेटा**: वित्तीय लेनदेन, जनसंख्या, भौतिक माप
- **विविध स्रोत**: विभिन्न प्रक्रियाओं/समयसीमा से मिश्रित डेटा
- **बड़े डेटासेट**: विश्वसनीय विश्लेषण के लिए 100+ संख्याएं
- **अहेराफेरी डेटा**: प्राकृतिक रूप से घटित, कृत्रिम रूप से बाधित नहीं

## ऐतिहासिक पृष्ठभूमि

**खोज और विकास:**
- **1881**: साइमन न्यूकॉम्ब ने लॉगरिदम तालिकाओं का अध्ययन करते समय पहली बार घटना देखी
- **1938**: भौतिकविद् फ्रैंक बेनफोर्ड ने व्यापक अनुसंधान के साथ नियम को फिर से खोजा और औपचारिक रूप दिया
- **1972**: अकादमिक साहित्य में लेखांकन और धोखाधड़ी का पता लगाने में पहला अनुप्रयोग
- **1980s**: प्रमुख लेखांकन फर्मों ने बेनफोर्ड के नियम को मानक ऑडिट उपकरण के रूप में उपयोग करना शुरू किया
- **1990s**: मार्क निग्रिनी ने फोरेंसिक अकाउंटिंग और कर धोखाधड़ी का पता लगाने में इसके उपयोग को लोकप्रिय बनाया
- **2000s+**: चुनाव निगरानी, वैज्ञानिक डेटा सत्यापन, और वित्तीय अपराध जांच में विस्तार

**आधुनिक अनुप्रयोग:**
- IRS द्वारा कर ऑडिट स्क्रीनिंग के लिए उपयोग
- बिग फोर अकाउंटिंग फर्मों में मानक उपकरण
- चुनाव धोखाधड़ी का पता लगाने में लागू (विशेष रूप से 2009 ईरान चुनाव विश्लेषण)
- मनी लॉन्ड्रिंग विरोधी जांच में कार्यरत

## निकास कोड

| कोड | अर्थ |
|-----|------|
| 0 | सफलता |
| 1 | सामान्य त्रुटि |
| 2 | अमान्य तर्क |
| 3 | फ़ाइल/नेटवर्क त्रुटि |
| 10 | उच्च जोखिम का पता चला |
| 11 | गंभीर जोखिम का पता चला |

## कॉन्फ़िगरेशन

Benf मानक पर्यावरण चर का सम्मान करता है:
- `HTTP_PROXY` / `HTTPS_PROXY`: प्रॉक्सी सेटिंग्स
- `NO_PROXY`: प्रॉक्सी बाईपास सूची

लॉग लिखे जाते हैं:
- Linux: `~/.local/state/benf/`
- macOS: `~/Library/Logs/benf/`
- Windows: `%APPDATA%\benf\Logs\`

## योगदान

विकास दिशानिर्देशों के लिए [CONTRIBUTING.md](CONTRIBUTING.md) देखें।

## लाइसेंस

MIT लाइसेंस - [LICENSE](LICENSE) फ़ाइल देखें।

## संदर्भ

- [बेनफोर्ड का नियम - विकिपीडिया](https://hi.wikipedia.org/wiki/बेनफोर्ड_का_नियम)
- [धोखाधड़ी का पता लगाने के लिए बेनफोर्ड के नियम का उपयोग](https://example.com/benford-fraud)