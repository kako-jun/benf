# benf

A CLI tool for detecting anomalies using Benford's Law with support for Japanese numerals.

## Overview

`benf` analyzes numerical data to check if it follows Benford's Law, which states that in many naturally occurring datasets, the digit 1 appears as the **first (leading) digit** about 30.1% of the time, 2 appears 17.6% of the time, and so on. Deviations from this law can indicate data manipulation or fraud.

**Note**: This tool analyzes only the **first digit** of each number, not the entire number sequence.

**Unique Features:**
- 🇯🇵 Japanese numeral support (full-width digits: ０１２, kanji numerals: 一二三)
- 📊 Multiple input formats (Microsoft Excel, Word, PowerPoint, PDF, etc.)
- 🌐 Direct URL analysis with HTML parsing
- 🔍 Fraud detection focus with risk level indicators

## International Numeral Support

### Supported Number Formats

#### 1. Full-width Digits
```bash
echo "１２３４５６ ７８９０１２" | benf
```

#### 2. Kanji Numerals (Basic)
```bash
echo "一二三四五六七八九" | benf
```

#### 3. Kanji Numerals (Positional)
```bash
echo "一千二百三十四 五千六百七十八 九万一千二百" | benf
```

#### 4. Mixed Patterns
```bash
echo "売上123万円 経費45万6千円 利益78万９千円" | benf
```

### Conversion Rules

| Kanji | Number | Notes |
|-------|--------|-------|
| 一 | 1 | Basic digit |
| 十 | 10 | Tens place |
| 百 | 100 | Hundreds place |
| 千 | 1000 | Thousands place |
| 万 | 10000 | Ten thousands place |
| 一千二百三十四 | 1234 | Positional notation |

#### Decimal Numbers
```bash
# Only numbers ≥ 1 are analyzed
echo "12.34 0.567 123.45" | benf
# Result: 1, (excluded), 1 (numbers < 1 are excluded)
```

#### Negative Numbers
```bash
# Uses absolute value's first digit
echo "-123 -456 -789" | benf
# Result: 1, 4, 7
```

### Chinese Numeral Compatibility

Current implementation supports basic Chinese numerals that are identical to Japanese kanji:

#### Supported (Basic Forms)
- 一二三四五六七八九 (1-9) - Same as Japanese
- 十百千 (10, 100, 1000) - Positional markers

#### Planned Support
- **Financial forms**: 壹貳參肆伍陸柒捌玖 (anti-fraud variants)
- **Traditional**: 萬 (10,000) vs Japanese 万
- **Regional variants**: Traditional vs Simplified Chinese

### Other Numeral Systems (Planned)

#### Arabic-Indic Numerals
- **Eastern Arabic**: ٠١٢٣٤٥٦٧٨٩ (Middle East)
- **Persian**: ۰۱۲۳۴۵۶۷۸۹ (Iran, Afghanistan)

#### South Asian Scripts
- **Hindi**: ०१२३४५६७८९ (India)
- **Bengali**: ০১২৩৪৫৬৭৮৯ (Bangladesh)
- **Tamil**: ௦௧௨௩௪௫௬௭௮௯ (Tamil Nadu)

#### Southeast Asian Scripts
- **Thai**: ๐๑๒๓๔๕๖๗๘๙ (Thailand)
- **Myanmar**: ၀၁၂၃၄၅၆၇၈၉ (Myanmar)

> **Note**: International numeral support is being expanded based on user demand. Current focus is on Japanese/Chinese financial document analysis.

## Installation

### From Source
```bash
git clone https://github.com/kako-jun/benf
cd benf
cargo build --release
cp target/release/benf /usr/local/bin/
```

### Binary Releases
Download from [releases page](https://github.com/kako-jun/benf/releases)

## Quick Start

```bash
# Analyze CSV file
benf data.csv

# Analyze website data
benf --url https://example.com/financial-report

# Pipe data
echo "123 456 789 101112" | benf

# JSON output for automation
benf data.csv --format json
```

## Usage

### Basic Syntax
```bash
benf [OPTIONS] [INPUT]
```

### Input Methods
1. **File path**: `benf financial_data.xlsx`
2. **URL**: `benf --url https://api.example.com/data`
3. **String**: `benf "123 456 789 101112"`
4. **Pipe**: `cat data.txt | benf`

Priority: URL > File > String > Pipe

### Options

| Option | Description |
|--------|-------------|
| `--url <URL>` | Fetch data from URL |
| `--format <FORMAT>` | Output format: text, csv, json, yaml, toml, xml |
| `--quiet` | Minimal output (numbers only) |
| `--verbose` | Detailed statistics |
| `--filter <RANGE>` | Filter numbers (e.g., `--filter ">=100"`) |
| `--threshold <LEVEL>` | Alert threshold: low, medium, high, critical |
| `--proxy <URL>` | HTTP proxy server |
| `--insecure` | Skip SSL certificate verification |
| `--timeout <SECONDS>` | Request timeout (default: 30) |
| `--log-level <LEVEL>` | Log level: debug, info, warn, error |
| `--help, -h` | Show help |
| `--version, -V` | Show version |

### Supported File Formats

| Format | Extensions | Notes |
|--------|------------|-------|
| Microsoft Excel | .xlsx, .xls | Spreadsheet data |
| Microsoft Word | .docx, .doc | Document analysis |
| Microsoft PowerPoint | .pptx, .ppt | Presentation data |
| OpenDocument | ods, .odt | OpenOffice/LibreOffice files |
| PDF | .pdf | Text extraction |
| CSV/TSV | .csv, .tsv | Structured data |
| JSON/XML | .json, .xml | API responses |
| YAML/TOML | .yaml, .toml | Configuration files |
| HTML | .html | Web pages |
| Text | .txt | Plain text |

## Output

### Default Output
```
Benford's Law Analysis Results

Dataset: financial_data.csv
Numbers analyzed: 1,247
Risk Level: HIGH ⚠️

First Digit Distribution:
1: ████████████████████████████ 28.3% (expected: 30.1%)
2: ████████████████████ 20.1% (expected: 17.6%)
3: ██████████ 9.8% (expected: 12.5%)
...

Statistical Tests:
Chi-square: 23.45 (p-value: 0.003)
Mean Absolute Deviation: 2.1%

Verdict: SIGNIFICANT DEVIATION DETECTED
```

### JSON Output
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

## Examples

### Fraud Detection
```bash
# Monitor sales data
benf sales_report.xlsx --threshold high

# Real-time log monitoring
tail -f transactions.log | benf --format json | jq 'select(.risk_level == "HIGH")'

# Batch analysis
find . -name "*.csv" -exec benf {} \; | grep "HIGH"
```

### Japanese Numerals
```bash
# Full-width digits
echo "１２３ ４５６ ７８９" | benf

# Kanji numerals
echo "一千二百三十四 五千六百七十八" | benf

# Mixed patterns
benf japanese_financial_report.pdf
```

### Web Analysis
```bash
# Financial website
benf --url https://company.com/earnings --format json

# API endpoint
benf --url https://api.finance.com/data --proxy http://proxy:8080
```

### Automation
```bash
# Daily fraud check
#!/bin/bash
RESULT=$(benf daily_sales.csv --format json)
RISK=$(echo $RESULT | jq -r '.risk_level')
if [ "$RISK" = "HIGH" ]; then
    echo $RESULT | mail -s "Fraud Alert" admin@company.com
fi
```

## Risk Levels

| Level | Chi-square p-value | Interpretation |
|-------|-------------------|----------------|
| LOW | p > 0.1 | Normal distribution |
| MEDIUM | 0.05 < p ≤ 0.1 | Slight deviation |
| HIGH | 0.01 < p ≤ 0.05 | Significant deviation |
| CRITICAL | p ≤ 0.01 | Strong evidence of manipulation |

## Common Use Cases

- **Accounting audits**: Detect manipulated financial records
- **Tax investigations**: Identify suspicious income declarations
- **Election monitoring**: Verify vote count authenticity
- **Insurance claims**: Spot fraudulent claim patterns
- **Scientific data**: Validate research results
- **Quality control**: Monitor manufacturing data

## ⚠️ Important Limitations

**Benford's Law does NOT apply to:**
- **Constrained ranges**: Adult heights (1.5-2.0m), ages (0-100), temperatures
- **Sequential data**: Invoice numbers, employee IDs, zip codes
- **Assigned numbers**: Phone numbers, social security numbers, lottery numbers
- **Small datasets**: Less than 30-50 numbers (insufficient for statistical analysis)
- **Single-source data**: All from same process/machine with similar magnitudes
- **Rounded data**: Heavily rounded amounts (e.g., all ending in 00)

**Best suited for:**
- **Multi-scale natural data**: Financial transactions, populations, physical measurements
- **Diverse sources**: Mixed data from different processes/timeframes
- **Large datasets**: 100+ numbers for reliable analysis
- **Unmanipulated data**: Naturally occurring, not artificially constrained

## Historical Background

**Discovery and Development:**
- **1881**: Simon Newcomb first observed the phenomenon while studying logarithm tables
- **1938**: Physicist Frank Benford rediscovered and formalized the law with extensive research
- **1972**: First application to accounting and fraud detection in academic literature
- **1980s**: Major accounting firms began using Benford's Law as a standard audit tool
- **1990s**: Mark Nigrini popularized its use in forensic accounting and tax fraud detection
- **2000s+**: Expanded to election monitoring, scientific data validation, and financial crime investigation

**Modern Applications:**
- Used by IRS for tax audit screening
- Standard tool in Big Four accounting firms
- Applied in election fraud detection (notably 2009 Iran election analysis)
- Employed in anti-money laundering investigations

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | General error |
| 2 | Invalid arguments |
| 3 | File/network error |
| 10 | High risk detected |
| 11 | Critical risk detected |

## Configuration

Benf respects standard environment variables:
- `HTTP_PROXY` / `HTTPS_PROXY`: Proxy settings
- `NO_PROXY`: Proxy bypass list

Logs are written to:
- Linux: `~/.local/state/benf/`
- macOS: `~/Library/Logs/benf/`
- Windows: `%APPDATA%\benf\Logs\`

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for development guidelines.

## License

MIT License - see [LICENSE](LICENSE) file.

## References

- [Benford's Law - Wikipedia](https://en.wikipedia.org/wiki/Benford%27s_law)
- [Using Benford's Law for Fraud Detection](https://example.com/benford-fraud)