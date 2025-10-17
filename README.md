# PHONE DETECTOR
A **production-grade phone number detection library** written in modern C++ with performance, security, and correctness as top priorities. It is designed to find various phone number formats including domestic US numbers, international numbers, toll-free numbers, and mobile numbers within text.

## ✨ Features

  * **Multi-Format Support** – Detects formatted domestic numbers, toll-free numbers, international numbers (with + prefix), plain digit sequences, and mobile numbers.
  * **High-Performance Scanning** – Utilizes lookup tables, branch prediction hints (`LIKELY`/`UNLIKELY` macros), and efficient scanning algorithms achieving **10M+ operations/second**.
  * **Thread-Safe** – Designed for safe concurrent usage in multi-threaded environments.
  * **Security Hardened** – Implements input size limits (10MB max) to prevent Denial of Service (DoS) attacks.
  * **SOLID Principles** – Code is structured using SOLID principles for maintainability and extensibility.
  * **Duplicate & Overlap-Free** – Extracts unique, non-overlapping phone numbers from text.
  * **Space-Separated Number Detection** – Intelligently handles space-separated formats (e.g., `99 88 77 66 55`).
  * **Comprehensive Test Suite** – Includes validation tests, scanning tests, and a multi-threaded performance benchmark.

## 📌 Supported Phone Formats

| Format Type | Example | Description |
|------------|---------|-------------|
| `FORMATTED_DOMESTIC` | `(123) 456-7890`<br>`123-456-7890`<br>`123.456.7890` | US/Canadian formatted numbers with parentheses, dashes, or dots |
| `FORMATTED_TOLL_FREE` | `1-800-555-1234`<br>`1.800.555.1234` | 11-digit toll-free numbers starting with 1 |
| `INTERNATIONAL_PLUS` | `+1 123-456-7890`<br>`+91-9876543210`<br>`+44 20 1234 5678`<br>`+1 (415) 555-0182` | International format with + prefix |
| `PLAIN_10_DIGIT` | `2345678901` | 10-digit plain numbers without separators |
| `PLAIN_11_DIGIT` | `12345678901` | 11-digit plain numbers starting with 1 |
| `MOBILE_10_DIGIT` | `9876543210`<br>`99887 76655`<br>`998 877 6655` | Mobile numbers starting with 1-9, with or without space separators |

## 📌 Use Cases

  * Scan documents, emails, or logs for phone numbers.
  * Extract contact information from unstructured text.
  * Data validation and normalization pipelines.
  * Customer data extraction and processing systems.
  * Compliance and data discovery (e.g., PII detection).
  * Lead generation and contact information mining.

## 🚀 Included Components

  * `PhoneScanner` – The core detection and extraction logic with optimized scanning algorithms.
  * `IPhoneValidator` Interfaces – Individual validators for each phone type (domestic, international, mobile, etc.).
  * `CharacterClassifier` – Ultra-fast character classification using lookup tables.
  * `PhoneDetectorFactory` – A factory for creating scanner and validator instances.
  * `PhoneMatch` – Structured result containing type, value, normalized digits, and position.
  * Example usage and a full test suite in `main()`.

## 🔧 Build Instructions

### Optimized Build (Recommended for Production)

For maximum performance with aggressive optimizations:

#### GCC 
**For development:**
```bash
g++ -O3 -march=native -std=c++17 -pthread PhoneDetector.cpp -o PhoneDetector
```

#### GCC 
**For production/benchmarking:**
```bash
g++ -O3 -march=native -flto=auto -DNDEBUG -std=c++17 -pthread PhoneDetector.cpp -o PhoneDetector
```

#### Clang
```bash
clang++ -O3 -march=native -std=c++17 -pthread PhoneDetector.cpp -o PhoneDetector
```

#### With Link-Time Optimization (even faster)
```bash
g++ -O3 -march=native -flto -std=c++17 -pthread PhoneDetector.cpp -o PhoneDetector
```

**Compiler Flags Explained:**
- `-O3` – Maximum optimization level (~20x speedup)
- `-march=native` – CPU-specific optimizations (SIMD, AVX)
- `-std=c++17` – C++17 standard support
- `-pthread` – POSIX threading support
- `-flto` / `-flto=auto` – Link-time optimization (optional, slower compile time but faster runtime)
- `-DNDEBUG` – Disables assertions for production builds

**Performance:** ~10-16M operations/second on modern hardware with 16 threads

-----

### Unoptimized Build (Debug Mode)

For development, debugging, and getting more detailed error messages:

```bash
g++ -g -std=c++17 -pthread PhoneDetector.cpp -o PhoneDetector
```

  * `-g` – Includes debugging information in the binary for use with tools like GDB.

-----

## ▶️ Running the Program

### Linux/macOS
```bash
./PhoneDetector
```

### Windows (PowerShell)
```powershell
./PhoneDetector.exe
```

### Windows (CMD)
```cmd
PhoneDetector.exe
```

---

## 📊 Expected Output

Running the program will execute the validation and scanning test suites, followed by a live demo and a high-throughput performance benchmark. The output will look similar to this:

```
====================================================================================================
=== PHONE VALIDATION TESTS ===
====================================================================================================

✓ Formatted with parentheses
✓ Formatted with dashes
✓ Formatted with dots
✓ Invalid area code (starts with 0)
✓ Plain 10 digits
✓ Plain 11 digits with 1
✓ Invalid area code
✓ International format
✓ International mobile format
✓ International format
✓ Mobile 10 digits
✓ Mobile with country code
✓ Invalid mobile (starts with 5)

Result: 13/13 passed (100%)


====================================================================================================
=== PHONE SCANNING TESTS ===
====================================================================================================

✓ Formatted in text
  Found 1 phone(s)
    [FORMATTED_DOMESTIC] (123) 456-7890 (normalized: 1234567890)

✓ Multiple formatted numbers
  Found 2 phone(s)
    [FORMATTED_DOMESTIC] 123-456-7890 (normalized: 1234567890)
    [FORMATTED_DOMESTIC] 987-654-3210 (normalized: 9876543210)

✓ International format
  Found 1 phone(s)
    [INTERNATIONAL_PLUS] +91 9876543210 (normalized: 919876543210)

✓ Mixed formats
  Found 2 phone(s)
    [INTERNATIONAL_PLUS] +1 234-567-8900 (normalized: 12345678900)
    [MOBILE_10_DIGIT] 9876543210 (normalized: 9876543210)

✓ Space-separated mobile
  Found 1 phone(s)
    [MOBILE_10_DIGIT] 99887 76655 (normalized: 9988776655)

✓ International and spaced mobile
  Found 2 phone(s)
    [INTERNATIONAL_PLUS] +44 20 7946 0123 (normalized: 442079460123)
    [MOBILE_10_DIGIT] 98765 43210 (normalized: 9876543210)

✓ International and toll-free
  Found 2 phone(s)
    [INTERNATIONAL_PLUS] +1 (415) 555-0182 (normalized: 14155550182)
    [FORMATTED_TOLL_FREE] 1-800-555-0199 (normalized: 18005550199)

Result: 20/20 passed (100%)


====================================================================================================
=== PHONE DETECTION DEMO ===
====================================================================================================

Found 6 phone numbers:

  [FORMATTED_DOMESTIC] at pos 14
  Value: (234) 567-8900
  Normalized: 2345678900

  [INTERNATIONAL_PLUS] at pos 32
  Value: +91-9876543210
  Normalized: 919876543210

  [FORMATTED_DOMESTIC] at pos 56
  Value: 345-678-9012
  Normalized: 3456789012

  [MOBILE_10_DIGIT] at pos 78
  Value: 9123456789
  Normalized: 9123456789

  [MOBILE_10_DIGIT] at pos 95
  Value: 99887 76655
  Normalized: 9988776655

  [INTERNATIONAL_PLUS] at pos 114
  Value: +1 (234) 567-8900
  Normalized: 12345678900


====================================================================================================
=== PERFORMANCE BENCHMARK ===
====================================================================================================
Threads: 16
Iterations per thread: 100000
Test cases: 14
Total operations: 22400000
Starting benchmark...

----------------------------------------------------------------------------------------------------
RESULTS:
----------------------------------------------------------------------------------------------------
Time: 1380 ms
Ops/sec: 16231884
Total phones found: 25600000
====================================================================================================


====================================================================================================
✓ SOLID Principles Applied
✓ Optimized for 1M+ ops/sec Performance
✓ Character Classification Lookup Tables
✓ Thread-Safe Implementation
✓ Multiple Phone Format Support
✓ Space-Separated Number Detection
✓ Generic Country-Independent Detection
====================================================================================================
```

-----

## 🧪 Testing

The program includes a robust, self-contained test suite that runs automatically:

  * **Validation Tests:** Verifies that each `IPhoneValidator` correctly identifies valid and invalid phone number formats based on rules like area code validation (no leading 0), exchange code validation (must be ≥2), and digit count requirements.
  * **Scanning Tests:** Ensures the `PhoneScanner` can accurately find and extract phone numbers from various text blocks, including edge cases like:
    - Mixed format documents
    - Space-separated mobile numbers with various spacing patterns
    - International numbers with parentheses
    - Multiple phone numbers in the same text
    - Stories and real-world text scenarios
  * **Performance Benchmark:** A multi-threaded stress test that measures the number of scan operations per second on your hardware, typically achieving **10M+ ops/sec** on modern CPUs.

-----

## 🎯 Key Technical Features

### Character Classification Optimization
Uses a 256-entry lookup table (`CharacterClassifier::charTable`) for O(1) character classification:
- Digits (0-9)
- Separators (space, dash, dot, parentheses)
- Plus sign (+)

### Branch Prediction Hints
Uses compiler-specific macros for optimization:
```cpp
#define LIKELY(x) __builtin_expect(!!(x), 1)
#define UNLIKELY(x) __builtin_expect(!!(x), 0)
#define FORCE_INLINE __attribute__((always_inline)) inline
```

### Intelligent Format Detection
The scanner uses a multi-pass approach:
1. **International numbers** (scanned first to handle `+1 (xxx)` patterns correctly)
2. **Formatted numbers** (parentheses, dashes, dots, spaces)
3. **Plain digit sequences** (fallback for unformatted numbers)

### Overlap Prevention
Extracted phone numbers are sorted by position and filtered to ensure no overlapping matches, returning only the first valid match at each position.

### Mobile Number Intelligence
Distinguishes between:
- **Standard formatted**: `987-654-3210` with dashes/dots → `FORMATTED_DOMESTIC`
- **Space-separated mobile**: `99887 76655` with spaces + starts with 1-9 → `MOBILE_10_DIGIT`

-----

## 📋 Requirements

  * **Compiler:** A C++17 compatible compiler (e.g., GCC 7+, Clang 6+, MSVC v19.14+).
  * **OS:** Linux, macOS, or Windows.
  * **Hardware:** Any modern CPU. The optimized build will take advantage of CPU-specific instructions if `-march=native` is used.
  * **RAM:** Minimal requirements; handles up to 10MB text inputs by default.

-----

## ⚙️ Configuration

### Input Size Limits
The scanner has built-in safety limits to prevent DoS attacks:
```cpp
static constexpr size_t MAX_INPUT_SIZE = 10 * 1024 * 1024;  // 10MB
static constexpr size_t MAX_PHONE_LENGTH = 30;              // Max phone string length
```

These can be adjusted in the `PhoneScanner` class if needed for your use case.

### Validation Rules
The detector implements North American Numbering Plan (NANP) rules:
- Area code (NXX): First digit 2-9 (N), last two digits any 0-9 (XX)
- Exchange code (NXX): First digit 2-9 (N), last two digits any 0-9 (XX)
- Subscriber number (XXXX): Any four digits 0-9

These rules can be customized in the individual validator classes.

-----

## ⚠️ Important Notes

### Portability and `-march=native`

The `-march=native` flag produces a binary that is highly optimized for the machine you compile it on. This binary may fail to run on a machine with an older or different CPU architecture. If you need a portable binary that can run on multiple systems, compile without this flag:

```bash
g++ -O3 -std=c++17 -pthread PhoneDetector.cpp -o PhoneDetector
```

### Windows and `-pthread`

On some Windows environments (like MinGW), the `-pthread` flag may not be necessary or available. If you encounter errors related to it, you can safely omit it for single-threaded builds or use the native Windows threading libraries if needed.

```bash
g++ -O3 -march=native -std=c++17 PhoneDetector.cpp -o PhoneDetector.exe
```

### Character Encoding

The detector assumes UTF-8/ASCII input. Phone numbers with Unicode characters or RTL (right-to-left) text may not be detected correctly without preprocessing.

### False Positives

The detector is designed to be permissive and may occasionally detect numbers that aren't actually phone numbers (e.g., serial numbers, IDs). For production use, consider:
- Adding context-aware filtering
- Implementing allowlists/denylists
- Validating extracted numbers against phone number databases

-----

## 🔮 Future Enhancements

Potential improvements for future versions:
- [ ] Country-specific phone number validation
- [ ] E.164 format normalization
- [ ] Phone number carrier/region lookup
- [ ] Extension detection (e.g., x123, ext. 456)
- [ ] Vanity number support (1-800-FLOWERS)
- [ ] Configurable validation rules via JSON/YAML
- [ ] REST API wrapper for web services

-----

## 📄 License

This code is provided as-is for educational and commercial use. Feel free to modify and distribute according to your needs.

---

**Built with ❤️ and C++17 for maximum performance and reliability.**