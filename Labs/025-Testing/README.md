# Lab 025 - Testing & QA Automation

!!! hint "Overview" - Generate comprehensive test cases using Claude Code and AI - Automate browser testing with Playwright and MCP servers - Build health checks and smoke tests for supplier management workflows - Implement API testing, database testing, and performance benchmarks - Integrate test automation into CI/CD pipelines for continuous validation

## Prerequisites

- Completion of Lab 020 (Claude Code Testing)
- Node.js 18+ with Jest and Playwright installed
- Basic understanding of test automation frameworks
- Supabase setup with sample data
- Familiarity with CI/CD concepts and GitHub Actions
- Docker for containerized test environments
- API testing experience (curl, Postman, or similar)

## What You Will Learn

By completing this lab, you will understand:

- AI-driven test case generation with Claude Code
- Automated test execution and reporting
- Playwright MCP for headless browser automation
- Writing test specifications for AI implementation
- Test coverage analysis and improvement strategies
- Database testing with fixtures and migrations
- API testing with validation
- Accessibility testing automation
- Security testing patterns (OWASP, dependency scanning)
- Performance and load testing
- Regression testing automation
- Health checks and smoke tests for production
- CI/CD integration with automated reporting

---

## Background

## QA Automation Architecture

AI-driven QA combines test generation, execution, reporting, and continuous improvement. Claude Code can generate test cases from requirements, execute them, analyze failures, and suggest fixes.

```mermaid
graph LR
    A["Requirements"] --> B["Generate Tests<br/>via Claude Code"]
    B --> C["Test Implementation"]
    C --> D["Execute Tests"]
    D --> E["Parse Results"]
    E --> F{Pass?}
    F -->|Yes| G["Report & Deploy"]
    F -->|No| H["Diagnose & Fix"]
    H --> I["Claude Code<br/>Analyzes Failure"]
    I --> C
    style A fill:#e3f2fd
    style G fill:#c8e6c9
    style H fill:#ffccbc
```

## Testing Framework Comparison

| Framework      | Type             | Coverage    | CI/CD     | Best For           |
| -------------- | ---------------- | ----------- | --------- | ------------------ |
| **Jest**       | Unit/Integration | 95%+        | Excellent | JavaScript/Node.js |
| **Playwright** | E2E/Browser      | UI testing  | Great     | Web applications   |
| **Vitest**     | Unit             | Near Jest   | Good      | Modern tooling     |
| **Cypress**    | E2E              | UI-focused  | Good      | Single-page apps   |
| **Artillery**  | Load testing     | Performance | Good      | Stress testing     |

## Test Hierarchy

```mermaid
graph TD
    A["Test Pyramid"] --> B["Unit Tests<br/>70%"]
    A --> C["Integration Tests<br/>20%"]
    A --> D["E2E Tests<br/>10%"]
    B --> E["Fast, isolated,<br/>local execution"]
    C --> F["Component interaction,<br/>database"]
    D --> G["Full user workflows,<br/>production-like"]
    style B fill:#bbdefb
    style C fill:#ffe0b2
    style D fill:#ffccbc
```

---

## Lab Steps

## Step 1 - Generate Test Cases with Claude Code

Create `.claude/test-generation-prompt.md`:

```markdown
# Test Case Generation Specification

## System Context

You are generating comprehensive test cases for Elcon's Supplier Management System.

## Generate Tests For:

- Supplier CRUD operations
- Supplier tier classification (T1, T2, T3)
- Payment status tracking
- Compliance verification
- Contract generation from templates

## Test Structure

Each test must include:

1. Test name describing the scenario
2. Setup/fixtures required
3. Expected inputs
4. Expected outputs
5. Edge cases and error scenarios

## Output Format

Return valid Jest test code with:

- describe() blocks for test suites
- it() blocks for individual tests
- beforeEach() for setup
- expect() assertions
- Mock data for suppliers and documents

## Coverage Requirements

Aim for:

- Happy path (success scenarios)
- Error paths (validation failures)
- Edge cases (boundary conditions)
- Negative tests (invalid inputs)
```

## Step 2 - Jest Unit Test Suite

Create `tests/supplier.test.mjs`:

```javascript
import { describe, it, expect, beforeEach, afterEach } from "@jest/globals";
import {
  createSupplier,
  updateSupplierTier,
  checkCompliance,
  generateSupplierReport,
} from "../src/suppliers.mjs";

describe("Supplier Management", () => {
  let mockSupplier;

  beforeEach(() => {
    mockSupplier = {
      id: "S001",
      name: "ABC Manufacturing Ltd",
      tier: "T1",
      region: "APAC",
      email: "contact@abcmfg.com",
      paymentTerms: 30,
    };
  });

  describe("createSupplier", () => {
    it("should create a new supplier with valid data", async () => {
      const result = await createSupplier(mockSupplier);
      expect(result.id).toBeDefined();
      expect(result.name).toBe("ABC Manufacturing Ltd");
      expect(result.createdAt).toBeDefined();
    });

    it("should reject supplier without required fields", async () => {
      const invalid = { name: "Test Supplier" };
      await expect(createSupplier(invalid)).rejects.toThrow(
        "Missing required fields",
      );
    });

    it("should validate email format", async () => {
      const invalidEmail = { ...mockSupplier, email: "invalid-email" };
      await expect(createSupplier(invalidEmail)).rejects.toThrow(
        "Invalid email",
      );
    });

    it("should enforce unique supplier names", async () => {
      await createSupplier(mockSupplier);
      await expect(createSupplier(mockSupplier)).rejects.toThrow(
        "Supplier name already exists",
      );
    });
  });

  describe("updateSupplierTier", () => {
    it("should promote supplier to higher tier", async () => {
      const result = await updateSupplierTier("S001", "T2");
      expect(result.tier).toBe("T2");
      expect(result.updatedAt).toBeDefined();
    });

    it("should validate tier values", async () => {
      await expect(updateSupplierTier("S001", "T99")).rejects.toThrow(
        "Invalid tier",
      );
    });

    it("should track tier history", async () => {
      await updateSupplierTier("S001", "T2");
      const history = await getSupplierTierHistory("S001");
      expect(history).toHaveLength(2);
      expect(history[0].tier).toBe("T1");
      expect(history[1].tier).toBe("T2");
    });
  });

  describe("checkCompliance", () => {
    it("should verify supplier compliance status", async () => {
      const result = await checkCompliance("S001");
      expect(result.compliant).toBeDefined();
      expect(result.issues).toBeInstanceOf(Array);
    });

    it("should flag suppliers with expired certifications", async () => {
      const expiredSupplier = {
        ...mockSupplier,
        iso9001_expiry: "2020-01-01",
      };
      const result = await checkCompliance(expiredSupplier);
      expect(result.compliant).toBe(false);
      expect(result.issues).toContain("ISO 9001 certification expired");
    });

    it("should check regional compliance rules", async () => {
      const apacSupplier = { ...mockSupplier, region: "APAC" };
      const result = await checkCompliance(apacSupplier);
      expect(result.regionalChecks).toContain("APAC_LABOR_STANDARDS");
    });
  });

  describe("generateSupplierReport", () => {
    it("should generate comprehensive supplier report", async () => {
      const report = await generateSupplierReport("S001");
      expect(report).toHaveProperty("supplier");
      expect(report).toHaveProperty("metrics");
      expect(report).toHaveProperty("compliance");
    });

    it("should include payment history in report", async () => {
      const report = await generateSupplierReport("S001");
      expect(report.metrics).toHaveProperty("onTimePaymentRate");
      expect(report.metrics).toHaveProperty("averagePaymentDays");
    });

    it("should calculate performance score", async () => {
      const report = await generateSupplierReport("S001");
      expect(report.metrics.performanceScore).toBeGreaterThanOrEqual(0);
      expect(report.metrics.performanceScore).toBeLessThanOrEqual(100);
    });
  });
});
```

## Step 3 - Playwright E2E Test Suite

Create `tests/supplier-ui.spec.mjs`:

```javascript
import { test, expect } from "@playwright/test";

test.describe("Supplier Management UI", () => {
  test.beforeEach(async ({ page }) => {
    // Login and navigate to supplier dashboard
    await page.goto("http://localhost:3000/suppliers");
    await page.fill('input[name="email"]', "admin@elcon.com");
    await page.fill('input[name="password"]', "test_password_123");
    await page.click('button[type="submit"]');
    await page.waitForNavigation();
  });

  test("should display supplier list", async ({ page }) => {
    const supplierTable = page.locator("table");
    await expect(supplierTable).toBeVisible();

    const rows = await page.locator("table tbody tr").count();
    expect(rows).toBeGreaterThan(0);
  });

  test("should create new supplier", async ({ page }) => {
    await page.click('button:has-text("Add Supplier")');

    await page.fill('input[name="name"]', "New Supplier Corp");
    await page.fill('input[name="email"]', "contact@newsupplier.com");
    await page.selectOption('select[name="tier"]', "T2");
    await page.selectOption('select[name="region"]', "EMEA");

    await page.click('button[type="submit"]');

    const successMessage = page.locator(".success");
    await expect(successMessage).toContainText("Supplier created");

    // Verify in table
    const newSupplier = page.locator("text=New Supplier Corp");
    await expect(newSupplier).toBeVisible();
  });

  test("should filter suppliers by tier", async ({ page }) => {
    await page.selectOption('select[name="tierFilter"]', "T1");

    const rows = await page.locator("table tbody tr").all();
    for (const row of rows) {
      const tierCell = row.locator("td:nth-child(3)");
      await expect(tierCell).toContainText("T1");
    }
  });

  test("should open supplier detail page", async ({ page }) => {
    const firstSupplier = page.locator("table tbody tr:first-child");
    await firstSupplier.click();

    await page.waitForNavigation();

    const supplierName = page.locator("h1");
    await expect(supplierName).toBeDefined();

    const complianceSection = page.locator("text=Compliance Status");
    await expect(complianceSection).toBeVisible();
  });

  test("should update supplier tier with confirmation", async ({ page }) => {
    const firstSupplier = page.locator("table tbody tr:first-child");
    await firstSupplier.click();

    await page.click('button:has-text("Change Tier")');
    await page.selectOption('select[name="newTier"]', "T3");
    await page.click('button:has-text("Confirm")');

    const confirmation = page.locator(".success");
    await expect(confirmation).toContainText("Tier updated");
  });

  test("should export supplier report as PDF", async ({ page }) => {
    const firstSupplier = page.locator("table tbody tr:first-child");
    await firstSupplier.click();

    const downloadPromise = page.waitForEvent("popup");
    await page.click('button:has-text("Export PDF")');
    const newPage = await downloadPromise;

    expect(newPage.url()).toContain(".pdf");
    await newPage.close();
  });
});
```

## Step 4 - API Testing with Health Checks

Create `tests/api.test.mjs`:

```javascript
import { describe, it, expect, beforeAll, afterAll } from "@jest/globals";
import axios from "axios";

const API_BASE = "http://localhost:3000/api";

describe("Supplier API", () => {
  let authToken;
  let supplierId;

  beforeAll(async () => {
    // Get authentication token
    const loginResponse = await axios.post(`${API_BASE}/auth/login`, {
      email: "admin@elcon.com",
      password: "test_password_123",
    });
    authToken = loginResponse.data.token;
  });

  describe("GET /suppliers", () => {
    it("should return list of suppliers", async () => {
      const response = await axios.get(`${API_BASE}/suppliers`, {
        headers: { Authorization: `Bearer ${authToken}` },
      });

      expect(response.status).toBe(200);
      expect(Array.isArray(response.data.suppliers)).toBe(true);
      expect(response.data.total).toBeGreaterThan(0);
    });

    it("should support pagination", async () => {
      const response = await axios.get(`${API_BASE}/suppliers`, {
        params: { page: 1, limit: 10 },
        headers: { Authorization: `Bearer ${authToken}` },
      });

      expect(response.data).toHaveProperty("page", 1);
      expect(response.data).toHaveProperty("limit", 10);
    });

    it("should filter by tier", async () => {
      const response = await axios.get(`${API_BASE}/suppliers`, {
        params: { tier: "T1" },
        headers: { Authorization: `Bearer ${authToken}` },
      });

      response.data.suppliers.forEach((supplier) => {
        expect(supplier.tier).toBe("T1");
      });
    });
  });

  describe("POST /suppliers", () => {
    it("should create supplier and return 201", async () => {
      const supplierData = {
        name: "Test Supplier API",
        email: "test@api.com",
        tier: "T2",
        region: "APAC",
      };

      const response = await axios.post(`${API_BASE}/suppliers`, supplierData, {
        headers: { Authorization: `Bearer ${authToken}` },
      });

      expect(response.status).toBe(201);
      expect(response.data).toHaveProperty("id");
      expect(response.data.name).toBe(supplierData.name);

      supplierId = response.data.id;
    });

    it("should validate required fields", async () => {
      try {
        await axios.post(
          `${API_BASE}/suppliers`,
          { name: "Incomplete" },
          {
            headers: { Authorization: `Bearer ${authToken}` },
          },
        );
      } catch (error) {
        expect(error.response.status).toBe(400);
        expect(error.response.data.message).toContain("email");
      }
    });
  });

  describe("Health Checks", () => {
    it("should return 200 from health endpoint", async () => {
      const response = await axios.get(`${API_BASE}/health`);
      expect(response.status).toBe(200);
      expect(response.data).toHaveProperty("status", "ok");
    });

    it("should check database connectivity", async () => {
      const response = await axios.get(`${API_BASE}/health/db`);
      expect(response.status).toBe(200);
      expect(response.data.database).toBe("connected");
    });

    it("should return service dependencies status", async () => {
      const response = await axios.get(`${API_BASE}/health/deep`);
      expect(response.status).toBe(200);
      expect(response.data).toHaveProperty("database");
      expect(response.data).toHaveProperty("cache");
      expect(response.data).toHaveProperty("queue");
    });
  });
});
```

## Step 5 - Performance Testing Script

Create `tests/performance.test.mjs`:

```javascript
import { performance } from "perf_hooks";
import axios from "axios";

class PerformanceTest {
  constructor(baseUrl = "http://localhost:3000/api") {
    this.baseUrl = baseUrl;
    this.results = [];
  }

  async measureEndpoint(method, endpoint, data = null, iterations = 10) {
    const times = [];

    for (let i = 0; i < iterations; i++) {
      const start = performance.now();

      try {
        if (method === "GET") {
          await axios.get(`${this.baseUrl}${endpoint}`);
        } else if (method === "POST") {
          await axios.post(`${this.baseUrl}${endpoint}`, data);
        }
      } catch (error) {
        console.error(`Request failed: ${error.message}`);
        continue;
      }

      const end = performance.now();
      times.push(end - start);
    }

    const avg = times.reduce((a, b) => a + b, 0) / times.length;
    const min = Math.min(...times);
    const max = Math.max(...times);

    return {
      endpoint,
      method,
      iterations,
      average: avg.toFixed(2),
      min: min.toFixed(2),
      max: max.toFixed(2),
      unit: "ms",
    };
  }

  async runSuite() {
    const endpoints = [
      { method: "GET", path: "/suppliers" },
      { method: "GET", path: "/suppliers/S001" },
      { method: "POST", path: "/suppliers", data: { name: "Test" } },
    ];

    for (const endpoint of endpoints) {
      const result = await this.measureEndpoint(
        endpoint.method,
        endpoint.path,
        endpoint.data,
        20,
      );
      this.results.push(result);
      console.log(result);
    }

    return this.results;
  }
}

const perf = new PerformanceTest();
await perf.runSuite();
```

## Step 6 - CI/CD Integration

Create `.github/workflows/test.yml`:

```yaml
name: Test & QA

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:15-alpine
        env:
          POSTGRES_USER: test_user
          POSTGRES_PASSWORD: test_password
          POSTGRES_DB: test_db
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

    steps:
      - uses: actions/checkout@v3

      - uses: actions/setup-node@v3
        with:
          node-version: "18"
          cache: "npm"

      - name: Install dependencies
        run: npm ci

      - name: Run unit tests
        run: npm run test:unit -- --coverage

      - name: Run API tests
        run: npm run test:api
        env:
          DATABASE_URL: postgresql://test_user:test_password@localhost:5432/test_db

      - name: Run E2E tests
        run: npm run test:e2e

      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info

      - name: Performance baseline
        run: npm run test:perf

      - name: Publish test results
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Test Results
          path: "test-results/*.xml"
          reporter: "java-junit"
```

---

## Tasks

1. **Create comprehensive test suite**: Generate unit tests for all supplier operations (CRUD, tier updates, compliance checks). Implement Playwright E2E tests for the supplier management UI. Aim for 80%+ code coverage. Run the test suite and verify all tests pass.

2. **Build health check endpoints**: Create `/health`, `/health/db`, and `/health/deep` endpoints that verify system status. Test them with the provided health check test suite. Set up monitoring for these endpoints in a simple dashboard or logging system.

3. **Set up CI/CD testing pipeline**: Create a GitHub Actions workflow that runs all test suites on every push and pull request. Configure code coverage reporting. Generate performance baselines for key API endpoints. Document the test results and coverage metrics.

---

## Summary

- [x] Understand AI-driven test generation and execution
- [x] Create comprehensive Jest unit test suite
- [x] Build Playwright E2E tests for UI workflows
- [x] Implement API testing with validation
- [x] Create health check endpoints and monitoring
- [x] Set up performance testing and benchmarking
- [x] Configure CI/CD testing pipeline with GitHub Actions
- [x] Enable code coverage reporting and tracking
- [x] Test database operations and migrations
- [x] Implement accessibility and security testing
- [x] Document test results and failure analysis
- [x] Create test data fixtures and factories
