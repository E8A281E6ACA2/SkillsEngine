---
name: auto-test
description: 自动生成测试用例，支持单元测试和集成测试
---

# /auto-test - 自动测试生成器

自动分析代码并生成完整的测试用例，支持多种测试框架和编程语言。

## 功能特性

- 🧪 自动生成单元测试
- 🔗 生成集成测试
- 📊 生成测试覆盖率报告
- 🎯 智能识别边界条件和异常情况
- 🔄 支持多种测试框架
  - JavaScript/TypeScript: Jest, Mocha, Vitest
  - Python: pytest, unittest
  - Java: JUnit, TestNG
  - Go: testing
  - Rust: cargo test
- 📝 生成测试文档和注释
- 🚀 自动运行测试并报告结果

## 使用方法

```bash
# 基本用法 - 为单个文件生成测试
/auto-test src/utils.ts

# 为整个目录生成测试
/auto-test src/

# 指定测试框架
/auto-test src/utils.ts --framework jest
/auto-test src/utils.py --framework pytest

# 生成并运行测试
/auto-test src/utils.ts --run

# 生成覆盖率报告
/auto-test src/ --coverage

# 只生成特定函数的测试
/auto-test src/utils.ts --function calculateTotal

# 包含边界测试和异常测试
/auto-test src/utils.ts --edge-cases --exceptions

# 生成集成测试
/auto-test src/api/ --integration
```

## What You Must Do When Invoked

当用户调用 `/auto-test` 时，按以下步骤执行：

### Step 1 - 分析目标代码

```bash
TARGET="$1"

# 检查是文件还是目录
if [[ -f "$TARGET" ]]; then
    echo "📄 Analyzing file: $TARGET"
    FILES=("$TARGET")
elif [[ -d "$TARGET" ]]; then
    echo "📁 Analyzing directory: $TARGET"
    # 查找所有源代码文件
    FILES=($(find "$TARGET" -type f \( -name "*.ts" -o -name "*.js" -o -name "*.py" -o -name "*.java" -o -name "*.go" -o -name "*.rs" \)))
else
    echo "❌ Target not found: $TARGET"
    exit 1
fi

echo "Found ${#FILES[@]} file(s) to test"
```

### Step 2 - 识别编程语言和测试框架

```bash
# 根据文件扩展名识别语言
case "$TARGET" in
*.ts|*.tsx)
        LANGUAGE="typescript"
      DEFAULT_FRAMEWORK="jest"
        TEST_DIR="__tests__"
        TEST_SUFFIX=".test.ts"
     ;;
    *.js|*.jsx)
        LANGUAGE="javascript"
DEFAULT_FRAMEWORK="jest"
        TEST_DIR="__tests__"
        TEST_SUFFIX=".test.js"
        ;;
  *.py)
   LANGUAGE="python"
DEFAULT_FRAMEWORK="pytest"
   TEST_DIR="tests"
        TEST_SUFFIX="_test.py"
 ;;
    *.java)
        LANGUAGE="java"
    DEFAULT_FRAMEWORK="junit"
     TEST_DIR="src/test/java"
        TEST_SUFFIX="Test.java"
        ;;
    *.go)
        LANGUAGE="go"
DEFAULT_FRAMEWORK="testing"
        TEST_DIR="."
        TEST_SUFFIX="_test.go"
        ;;
 *.rs)
    LANGUAGE="rust"
        DEFAULT_FRAMEWORK="cargo"
        TEST_DIR="tests"
        TEST_SUFFIX="_test.rs"
        ;;
esac

FRAMEWORK="${FRAMEWORK:-$DEFAULT_FRAMEWORK}"
echo "🔍 Language: $LANGUAGE, Framework: $FRAMEWORK"
```

### Step 3 - 读取并分析代码

使用 Read 工具读取源代码，分析：

1. **函数/方法列表**
   - 函数名
   - 参数类型
   - 返回值类型
   - 函数功能

2. **类/模块结构**
   - 类名
   - 方法列表
   - 依赖关系

3. **边界条件**
   - 空值处理
   - 边界值
   - 异常情况

4. **依赖项**
   - 外部模块
   - 需要 mock 的依赖

### Step 4 - 生成测试用例

根据分析结果生成测试代码：

#### TypeScript/Jest 示例

```typescript
import { calculateTotal, validateEmail } from './utils';

describe('utils', () => {
  describe('calculateTotal', () => {
    it('should calculate total for valid items', () => {
      const items = [
        { price: 10, quantity: 2 },
        { price: 5, quantity: 3 }
      ];
   expect(calculateTotal(items)).toBe(35);
    });

    it('should return 0 for empty array', () => {
      expect(calculateTotal([])).toBe(0);
});

    it('should handle negative prices', () => {
   const items = [{ price: -10, quantity: 2 }];
      expect(calculateTotal(items)).toBe(-20);
    });

    it('should throw error for invalid input', () => {
      expect(() => calculateTotal(null)).toThrow();
    });
  });

  describe('validateEmail', () => {
    it('should validate correct email', () => {
      expect(validateEmail('test@example.com')).toBe(true);
    });

    it('should reject invalid email', () => {
    expect(validateEmail('invalid-email')).toBe(false);
    });

    it('should handle empty string', () => {
      expect(validateEmail('')).toBe(false);
 });

    it('should handle null/undefined', () => {
      expect(validateEmail(null)).toBe(false);
      expect(validateEmail(undefined)).toBe(false);
    });
  });
});
```

#### Python/pytest 示例

```python
import pytest
from utils import calculate_total, validate_email

class TestCalculateTotal:
    def test_calculate_total_valid_items(self):
        items = [
 {'price': 10, 'quantity': 2},
       {'price': 5, 'quantity': 3}
        ]
  assert calculate_total(items) == 35

    def test_calculate_total_empty_list(self):
     assert calculate_total([]) == 0

  def test_calculate_total_negative_prices(self):
    items = [{'price': -10, 'quantity': 2}]
        assert calculate_total(items) == -20

    def test_calculate_total_invalid_input(self):
        with pytest.raises(TypeError):
            calculate_total(None)

class TestValidateEmail:
    def test_validate_email_correct(self):
        assert validate_email('test@example.com') is True

    def test_validate_email_invalid(self):
      assert validate_email('invalid-email') is False

    @pytest.mark.parametrize('email', ['', None, 123])
    def test_validate_email_edge_cases(self, email):
     assert validate_email(email) is False
```

### Step 5 - 创建测试文件

```bash
# 确定测试文件路径
SOURCE_FILE="$TARGET"
SOURCE_DIR=$(dirname "$SOURCE_FILE")
SOURCE_NAME=$(basename "$SOURCE_FILE" | sed 's/\.[^.]*$//')

# 创建测试目录
TEST_FILE_DIR="$SOURCE_DIR/$TEST_DIR"
mkdir -p "$TEST_FILE_DIR"

# 生成测试文件名
TEST_FILE="$TEST_FILE_DIR/${SOURCE_NAME}${TEST_SUFFIX}"

# 写入测试代码
echo "$TEST_CODE" > "$TEST_FILE"
echo "✅ Test file created: $TEST_FILE"
```

### Step 6 - 生成测试配置（如果需要）

检查是否存在测试配置文件，如果不存在则创建：

**Jest 配置 (jest.config.js):**
```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  testMatch: ['**/__tests__/**/*.test.ts'],
  collectCoverageFrom: ['src/**/*.ts'],
  coverageDirectory: 'coverage',
  coverageReporters: ['text', 'lcov', 'html']
};
```

**pytest 配置 (pytest.ini):**
```ini
[pytest]
testpaths = tests
python_files = *_test.py
python_classes = Test*
python_functions = test_*
addopts = -v --cov=src --cov-report=html
```

### Step 7 - 运行测试（如果指定 --run）

```bash
if [[ "$RUN_TESTS" == "true" ]]; then
    echo ""
    echo "🧪 Running tests..."
  
    case "$FRAMEWORK" in
        jest)
      npm test -- "$TEST_FILE"
;;
        pytest)
  pytest "$TEST_FILE" -v
         ;;
        junit)
            mvn test -Dtest="$(basename $TEST_FILE .java)"
        ;;
        testing)
     go test -v
            ;;
        cargo)
            cargo test
      ;;
    esac
 
    if [[ $? -eq 0 ]]; then
        echo "✅ All tests passed!"
    else
        echo "❌ Some tests failed. Please review."
    fi
fi
```

### Step 8 - 生成覆盖率报告（如果指定 --coverage）

```bash
if [[ "$COVERAGE" == "true" ]]; then
    echo ""
    echo "📊 Generating coverage report..."
    
    case "$FRAMEWORK" in
      jest)
            npm test -- --coverage
            echo "📄 Coverage report: coverage/index.html"
      ;;
     pytest)
     pytest --cov=src --cov-report=html
            echo "📄 Coverage report: htmlcov/index.html"
      ;;
    esac
fi
```

### Step 9 - 总结报告

```bash
echo ""
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "📋 Test Generation Summary"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "Source file: $SOURCE_FILE"
echo "Test file: $TEST_FILE"
echo "Framework: $FRAMEWORK"
echo "Test cases generated: $TEST_COUNT"
echo ""
echo "Next steps:"
echo "1. Review generated tests: $TEST_FILE"
echo "2. Run tests: npm test (or appropriate command)"
echo "3. Adjust tests as needed"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
```

## 示例

### 示例 1：为 TypeScript 文件生成测试

**源代码 (src/utils.ts):**
```typescript
export function calculateTotal(items: Array<{price: number, quantity: number}>): number {
  return items.reduce((sum, item) => sum + item.price * item.quantity, 0);
}

export function validateEmail(email: string): boolean {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}
```

**输入：**
```bash
/auto-test src/utils.ts --run
```

**输出：**
```
📄 Analyzing file: src/utils.ts
🔍 Language: typescript, Framework: jest
📝 Analyzing code structure...
   Found 2 functions: calculateTotal, validateEmail
🧪 Generating test cases...
   - calculateTotal: 4 test cases
   - validateEmail: 4 test cases
✅ Test file created: src/__tests__/utils.test.ts

🧪 Running tests...
 PASS  src/__tests__/utils.test.ts
  utils
    calculateTotal
      ✓ should calculate total for valid items (3 ms)
      ✓ should return 0 for empty array (1 ms)
      ✓ should handle negative prices (1 ms)
      ✓ should throw error for invalid input (2 ms)
    validateEmail
      ✓ should validate correct email (1 ms)
      ✓ should reject invalid email (1 ms)
      ✓ should handle empty string (1 ms)
      ✓ should handle null/undefined (1 ms)

Test Suites: 1 passed, 1 total
Tests:       8 passed, 8 total

✅ All tests passed!

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Test Generation Summary
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Source file: src/utils.ts
Test file: src/__tests__/utils.test.ts
Framework: jest
Test cases generated: 8

Next steps:
1. Review generated tests: src/__tests__/utils.test.ts
2. Run tests: npm test
3. Adjust tests as needed
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### 示例 2：生成覆盖率报告

**输入：**
```bash
/auto-test src/ --coverage
```

**输出：**
```
📁 Analyzing directory: src/
Found 5 file(s) to test
🔍 Generating tests for all files...

✅ Generated tests for 5 files
📊 Generating coverage report...

----------|---------|----------|---------|---------|-------------------
File   | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s 
----------|---------|----------|---------|---------|-------------------
All files |   95.23 |    88.46 |   100   |   95.23 |           
 utils.ts |   100   |    100   |   100   |   100   |     
 api.ts   |   92.5  |    80  |   100   |   92.5  | 45-47      
----------|---------|----------|---------|---------|-------------------

📄 Coverage report: coverage/index.html
```

### 示例 3：为 Python 生成测试

**输入：**
```bash
/auto-test src/calculator.py --framework pytest --run
```

**输出：**
```
📄 Analyzing file: src/calculator.py
🔍 Language: python, Framework: pytest
📝 Analyzing code structure...
   Found 4 functions: add, subtract, multiply, divide
🧪 Generating test cases...
✅ Test file created: tests/calculator_test.py

🧪 Running tests...
============================= test session starts ==============================
collected 12 items

tests/calculator_test.py ............     [100%]

============================== 12 passed in 0.05s ===============================
✅ All tests passed!
```

## 注意事项

- 生成的测试是基础模板，可能需要根据实际业务逻辑调整
- 对于复杂的业务逻辑，建议手动补充测试用例
- Mock 外部依赖需要手动配置
- 异步函数的测试需要特别注意
- 确保测试框架已安装

## 依赖

根据语言和框架不同：
- **JavaScript/TypeScript**: jest, @types/jest, ts-jest
- **Python**: pytest, pytest-cov
- **Java**: JUnit, Mockito
- **Go**: 内置 testing 包
- **Rust**: 内置 cargo test

## 配置

创建 `.auto-test-config.json`：

```json
{
  "default_framework": {
    "typescript": "jest",
    "python": "pytest",
    "java": "junit"
  },
  "test_dir": "__tests__",
  "coverage_threshold": 80,
  "generate_mocks": true,
  "edge_cases": true,
  "run_after_generate": false
}
```

## 相关 Skills

- `/auto-doc` - 自动生成文档
- `/simplify` - 代码简化和审查

## 相关资源

- [Jest Documentation](https://jestjs.io/)
- [pytest Documentation](https://docs.pytest.org/)
- [JUnit Documentation](https://junit.org/)
