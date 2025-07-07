# lawkit-js NPM Package Structure

This document outlines the complete structure of the lawkit-js NPM package.

## 📁 Directory Structure

```
lawkit-npm/
├── package.json                 # NPM package configuration
├── index.js                     # Main wrapper script & CLI entry point
├── README.md                    # Package documentation
├── .gitignore                   # Git ignore rules
├── .npmignore                   # NPM ignore rules
├── scripts/
│   └── download-binary.js       # Automatic binary download script
├── bin/                         # Binary directory (populated at install)
├── test.js                      # Package test script
├── examples.js                  # Usage examples
├── verify-package.js            # Package verification script
├── publish.md                   # Publishing instructions
└── STRUCTURE.md                 # This file
```

## 📋 File Descriptions

### Core Files

- **`package.json`** - NPM package configuration with metadata, dependencies, and scripts
- **`index.js`** - Main wrapper that spawns the lawkit binary and exports programmatic API
- **`README.md`** - Comprehensive documentation with installation, usage, and examples

### Scripts

- **`scripts/download-binary.js`** - Downloads appropriate binary for the user's platform
- **`test.js`** - Test script that verifies basic functionality
- **`examples.js`** - Comprehensive examples demonstrating all features
- **`verify-package.js`** - Verification script for package structure

### Configuration

- **`.gitignore`** - Git ignore rules (excludes binaries, temp files, etc.)
- **`.npmignore`** - NPM ignore rules (excludes dev files, includes only essentials)

### Documentation

- **`publish.md`** - Step-by-step publishing instructions
- **`STRUCTURE.md`** - This file, describing the package structure

## 🚀 Key Features

### 1. Automatic Binary Download
- Detects user's platform (Linux, macOS, Windows)
- Downloads appropriate binary from GitHub releases
- Handles architecture detection (x86_64, aarch64)
- Graceful fallback with helpful error messages

### 2. Dual Usage Pattern
- **CLI Tool**: Can be used directly as `lawkit` command
- **Node.js Library**: Can be imported and used programmatically

### 3. Platform Support
- **Linux**: x86_64, aarch64
- **macOS**: x86_64 (Intel), aarch64 (Apple Silicon)
- **Windows**: x86_64

### 4. Comprehensive Testing
- Basic functionality tests
- Full feature examples
- Package structure verification

## 🔧 Technical Implementation

### Binary Management
```javascript
// Platform detection
function getPlatform() {
  const platform = process.platform;
  const arch = process.arch;
  // Returns appropriate binary filename
}

// Download and extraction
async function downloadFile(url, dest) {
  // HTTPS download with redirect handling
}

async function extractArchive(archivePath, extractDir) {
  // Platform-specific extraction (tar.gz, zip)
}
```

### CLI Wrapper
```javascript
// Direct CLI usage
const child = spawn(binaryPath, process.argv.slice(2), {
  stdio: 'inherit',
});

// Programmatic usage
function runLawkit(args) {
  return new Promise((resolve, reject) => {
    // Returns { code, stdout, stderr }
  });
}
```

## 📊 Statistical Laws Supported

1. **Benford's Law** - Fraud detection and data quality
2. **Pareto Principle** - Business analysis and optimization
3. **Zipf's Law** - Text and language analysis
4. **Normal Distribution** - Quality control and validation
5. **Poisson Distribution** - Event prediction and analysis
6. **Comparative Analysis** - Multi-law validation

## 🎯 Usage Patterns

### Command Line
```bash
npm install -g lawkit-js
lawkit benf financial-data.csv
lawkit pareto sales-data.json --business-analysis
```

### Programmatic
```javascript
const { runLawkit } = require('lawkit-js');

const result = await runLawkit(['benf', 'data.csv', '--format', 'json']);
if (result.code === 0) {
  const analysis = JSON.parse(result.stdout);
  console.log('Analysis:', analysis);
}
```

## 🔍 Quality Assurance

### Pre-publish Checklist
- [ ] Version matches lawkit release
- [ ] All tests pass
- [ ] Examples work correctly
- [ ] Package structure verified
- [ ] Documentation updated
- [ ] Binary download URLs tested

### Testing Commands
```bash
npm test                 # Run basic tests
npm run examples         # Run comprehensive examples
npm run verify          # Verify CLI functionality
node verify-package.js  # Verify package structure
```

## 📦 Publishing Process

1. **Preparation**
   - Update version in `package.json`
   - Update `LAWKIT_VERSION` in download script
   - Ensure GitHub release exists

2. **Verification**
   - Run all tests
   - Test binary download
   - Verify package structure

3. **Publishing**
   - `npm publish`
   - Verify on npmjs.com
   - Test installation

## 🌟 Benefits

- **Zero Configuration**: Works out of the box
- **Cross-Platform**: Supports all major platforms
- **Automatic Updates**: Version synced with lawkit releases
- **Dual Interface**: CLI + programmatic usage
- **Comprehensive**: Full feature coverage
- **Reliable**: Robust error handling and fallbacks

This structure provides a complete, production-ready NPM package that makes lawkit easily accessible to the Node.js ecosystem while maintaining all the power and flexibility of the original CLI tool.