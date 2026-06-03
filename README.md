# Playwright Test Automation with TypeScript

A practical end-to-end test automation project using **Playwright**, **TypeScript**, and **Node.js**. This repository demonstrates modern QA automation concepts including UI testing, API testing, Page Object Model design, custom fixtures, authentication storage state, visual testing, browser projects, mobile emulation, screenshots, videos, traces, reporters, Docker execution, GitHub Actions, and CI-ready Playwright configuration.

**Written by Brian McCarthy**

---

## Table of Contents

| Section | Contents Covered |
|---|---|
| [Project Overview](#project-overview) | Purpose of this Playwright TypeScript automation framework. |
| [Project Structure](#project-structure) | Recommended folder structure and file responsibilities. |
| [How the Framework Works](#how-the-framework-works) | How Playwright config, tests, fixtures, page objects, auth state, and reports work together. |
| [Master Test Automation with Playwright](#master-test-automation-with-playwright) | Installation, test creation, maintenance, fixtures, POM, mocking, config, BDD, data-driven testing, speed, flakiness, screenshots, and cloud testing. |
| [Build a Playwright Framework from Scratch](#build-a-playwright-framework-from-scratch) | Steps to build a Playwright framework from nothing or add it to an existing app. |
| [Required Framework Files](#required-framework-files) | Required and recommended files for a professional Playwright framework. |
| [Requirements and Prerequisites](#requirements-and-prerequisites) | Tools, skills, access, and environment setup needed before framework work. |
| [Custom Framework vs Out-of-the-Box Playwright](#custom-framework-vs-out-of-the-box-playwright) | When to use default Playwright and when to build framework layers. |
| [Best and Most Popular Playwright Framework Patterns](#best-and-most-popular-playwright-framework-patterns) | Best framework styles for UI, API, fixtures, POM, BDD, visual, accessibility, mobile, and CI scenarios. |
| [Sample Code from This Project](#sample-code-from-this-project) | Basic UI Test, API Test, POM Login Page, Auth Storage State, Custom Fixtures, Console Error Fixture, Merged Fixtures, Checkout E2E, and Visual Screenshot Assertion. |
| [How to Run This Project](#how-to-run-this-project) | Local setup and execution commands. |
| [Best Practices](#best-practices) | Practical rules for stable, maintainable Playwright automation. |
| [Common Interview Questions](#common-interview-questions) | Interview-ready answers about Playwright framework design. |
| [Troubleshooting](#troubleshooting) | Common framework problems and fixes. |

---

## Project Overview

This project is a TypeScript Playwright automation framework built around a sample bagel/shop and practice testing application structure. It includes browser-based tests, API tests, reusable page objects, custom fixtures, storage-state authentication, test data helpers, visual assertions, custom matchers, and Playwright reporting/debugging features.

The project demonstrates practical QA automation skills such as creating UI tests, validating APIs with Playwright's `request` fixture, reusing authentication state, building Page Object Model classes, extending Playwright fixtures, capturing traces/screenshots/videos, and running tests from CLI, UI Mode, VS Code, Docker, or CI/CD.

---

## Project Structure

```text
Playwright-Test-Automation-Master-TypeScript/
├── bagel-shop/                         # Local sample application used by the tests
├── lib/
│   ├── datafactory/                    # Test data generation and message data helpers
│   ├── fixtures/                       # Custom Playwright fixtures and merged test objects
│   │   ├── base.fixture.ts             # Merges page and console fixtures
│   │   ├── console.fixture.ts          # Captures browser console messages and custom matcher
│   │   └── pages.fixture.ts            # Creates reusable page object fixtures
│   ├── helpers/                        # Helper utilities such as random state selection
│   └── pages/                          # Page Object Model classes
│       ├── account/                    # Account and message page objects
│       ├── contact/                    # Contact page object
│       └── login/                      # Login page object
├── tests/
│   ├── api/                            # API tests using Playwright request fixture
│   ├── bagel-shop/                     # Bagel shop UI tests
│   ├── checkout/                       # Checkout workflow tests
│   ├── homepage/                       # Homepage tests
│   └── auth.setup.ts                   # Authentication setup and storage state creation
├── package.json                        # npm scripts and project dependencies
├── playwright.config.ts                # Main Playwright Test configuration
├── tsconfig.json                       # TypeScript path aliases
└── README.md                           # Project documentation
```

---

## How the Framework Works

1. `package.json` stores dependencies and test scripts.
2. `playwright.config.ts` controls test directories, browser projects, retries, reports, traces, screenshots, video, web server startup, and environment variables.
3. Setup tests such as `auth.setup.ts` run first when configured as project dependencies.
4. Authentication state is saved to a JSON file such as `.auth/customer01.json`.
5. Specs use built-in fixtures such as `page`, `context`, `browser`, `request`, `headless`, and `isMobile`.
6. Custom fixtures inject page objects and shared utilities into tests.
7. Page objects store page-specific locators and reusable page actions.
8. API helpers create, read, update, and clean up data through API calls.
9. Reports, traces, screenshots, and videos provide failure evidence.

---

# Master Test Automation with Playwright

## Learning Playwright

Playwright is a browser automation and test framework that supports Chromium, Firefox, and WebKit. It includes a test runner, fixtures, assertions, trace viewer, screenshots, videos, API testing, mobile emulation, parallel execution, and CI/CD integration.

```ts
import { test, expect } from '@playwright/test';

test('home page loads', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveTitle(/Practice Software Testing|Bagel/i);
});
```

**Expected Result:** Playwright opens the configured application URL and validates the page title.

## 1. Playwright Installation and Configuration

```bash
mkdir playwright-typescript-demo
cd playwright-typescript-demo
npm init -y
npm init playwright@latest
npx playwright install
npx playwright test
npx playwright show-report
```

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  timeout: 30_000,
  expect: { timeout: 5_000 },
  fullyParallel: true,
  reporter: [['list'], ['html']],
  use: {
    baseURL: process.env.URL ?? 'http://localhost:5173',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure'
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
    { name: 'webkit', use: { ...devices['Desktop Safari'] } },
    { name: 'mobile-safari', use: { ...devices['iPhone 14'] } }
  ]
});
```

**Expected Result:** The framework can run desktop and mobile browser projects through CLI, VS Code, or UI Mode.

## 2. Creating Tests with Playwright

```ts
import { test, expect } from '@playwright/test';

test.describe('Homepage', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/');
  });

  test('shows navigation menu', async ({ page }) => {
    await expect(page.getByTestId('nav-menu')).toBeVisible();
  });
});
```

**Expected Result:** Tests are grouped, readable, and reusable. `beforeEach()` handles common navigation setup.

## 3. Maintaining Playwright Tests

```ts
export default defineConfig({
  reporter: [['list'], ['html', { open: 'never' }]],
  use: {
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    trace: 'on-first-retry'
  }
});
```

**Expected Result:** Failed tests produce screenshots, videos, and traces for debugging.

## 4. Abstractions in Playwright

```ts
import { Page, expect } from '@playwright/test';

export class NavigationPage {
  constructor(private readonly page: Page) {}

  async openHome() {
    await this.page.goto('/');
  }

  async openContact() {
    await this.page.getByRole('link', { name: 'Contact' }).click();
    await expect(this.page).toHaveURL(/contact/);
  }
}
```

**Expected Result:** Repeated navigation behavior is moved into reusable framework code.

## 5. Fixtures in Playwright

```ts
import { test as base, expect } from '@playwright/test';
import { LoginPage } from '../lib/pages/login/login.page';

type Fixtures = { loginPage: LoginPage };

export const test = base.extend<Fixtures>({
  loginPage: async ({ page }, use) => {
    await use(new LoginPage(page));
  }
});

export { expect };
```

**Expected Result:** Tests can request `loginPage` directly as a typed fixture.

## 6. Mocking and Emulation in Playwright

```ts
test('mocks product response', async ({ page }) => {
  await page.route('**/api/products', async route => {
    await route.fulfill({
      status: 200,
      contentType: 'application/json',
      body: JSON.stringify({ data: [{ id: 1, name: 'Mock Hammer' }] })
    });
  });

  await page.goto('/products');
  await expect(page.getByText('Mock Hammer')).toBeVisible();
});
```

**Expected Result:** The UI test validates frontend behavior without depending on a real backend response.

## 7. Complex Interactions

```ts
test('opens promo popup', async ({ page }) => {
  await page.goto('/');

  const popupPromise = page.waitForEvent('popup');
  await page.getByRole('button', { name: 'Get Promo Code' }).click();
  const popup = await popupPromise;

  await expect(popup.getByText('The promo code is:')).toBeVisible();
});
```

**Expected Result:** The test waits for a popup before triggering the action, preventing race conditions.

## 8. Data-Driven Testing

```ts
const loginCases = [
  { email: 'bad@example.com', password: 'wrong', message: 'Invalid email or password' },
  { email: '', password: 'welcome01', message: 'Email is required' }
];

for (const data of loginCases) {
  test(`login validation: ${data.message}`, async ({ page }) => {
    await page.goto('/auth/login');
    await page.getByTestId('email').fill(data.email);
    await page.getByTestId('password').fill(data.password);
    await page.getByTestId('login-submit').click();
    await expect(page.getByText(data.message)).toBeVisible();
  });
}
```

**Expected Result:** One test pattern validates multiple input/error-message combinations.

## 9. Visual Testing

```ts
test('homepage screenshot', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveScreenshot('home-page.png', {
    fullPage: true,
    maxDiffPixelRatio: 0.01
  });
});
```

**Expected Result:** The page screenshot is compared against an approved baseline.

---

# Build a Playwright Framework from Scratch

## When to Build a Framework from Scratch

Build a custom Playwright framework when the project has repeated workflows, multiple test types, authentication reuse, multiple user roles, environment-specific URLs, CI/CD execution, data setup needs, API/UI integration, page object reuse, reporting needs, or a growing suite that several engineers will maintain.

Use Playwright out of the box when the project is small, the suite has fewer than 20–30 tests, the tests are mostly independent smoke checks, the app is still rapidly changing, or the team is still learning Playwright fundamentals.

## Step-by-Step Build Process

### Step 1: Create or Add Playwright

New project:

```bash
mkdir playwright-framework
cd playwright-framework
npm init -y
npm init playwright@latest
npx playwright install
```

Existing front-end app:

```bash
npm install -D @playwright/test typescript
npx playwright install
mkdir tests lib pages fixtures data utils
```

### Step 2: Add npm Scripts

```json
{
  "scripts": {
    "test": "playwright test",
    "test:ui": "playwright test --ui",
    "test:headed": "playwright test --headed",
    "test:debug": "playwright test --debug",
    "test:chromium": "playwright test --project=chromium",
    "report": "playwright show-report",
    "codegen": "playwright codegen"
  }
}
```

### Step 3: Create `playwright.config.ts`

```ts
import { defineConfig, devices } from '@playwright/test';
import dotenv from 'dotenv';

dotenv.config();

export default defineConfig({
  testDir: './tests',
  timeout: 30_000,
  expect: { timeout: 5_000 },
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 2 : undefined,
  reporter: [['list'], ['html', { open: 'never' }]],
  use: {
    baseURL: process.env.BASE_URL ?? 'http://localhost:5173',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    testIdAttribute: 'data-test'
  },
  projects: [
    { name: 'setup', testMatch: /.*\.setup\.ts/ },
    { name: 'chromium', use: { ...devices['Desktop Chrome'] }, dependencies: ['setup'] },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] }, dependencies: ['setup'] },
    { name: 'webkit', use: { ...devices['Desktop Safari'] }, dependencies: ['setup'] },
    { name: 'mobile-safari', use: { ...devices['iPhone 14'] }, dependencies: ['setup'] }
  ]
});
```

### Step 4: Create `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "CommonJS",
    "moduleResolution": "Node",
    "strict": true,
    "baseUrl": ".",
    "paths": {
      "@pages/*": ["lib/pages/*"],
      "@fixtures/*": ["lib/fixtures/*"],
      "@data/*": ["lib/data/*"],
      "@utils/*": ["lib/utils/*"]
    }
  }
}
```

### Step 5: Create Page Objects

```ts
import { Page, Locator, expect } from '@playwright/test';

export class LoginPage {
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly loginButton: Locator;

  constructor(private readonly page: Page) {
    this.emailInput = page.getByTestId('email');
    this.passwordInput = page.getByTestId('password');
    this.loginButton = page.getByTestId('login-submit');
  }

  async goto() {
    await this.page.goto('/auth/login');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }

  async expectSuccessfulLogin() {
    await expect(this.page.getByTestId('nav-menu')).toContainText('Jane Doe');
  }
}
```

### Step 6: Create Custom Fixtures

```ts
import { test as base, expect } from '@playwright/test';
import { LoginPage } from '@pages/login/login.page';

type PageFixtures = {
  loginPage: LoginPage;
};

export const test = base.extend<PageFixtures>({
  loginPage: async ({ page }, use) => {
    await use(new LoginPage(page));
  }
});

export { expect };
```

### Step 7: Add Authentication Setup

```ts
import { test as setup } from '@playwright/test';
import { LoginPage } from '@pages/login/login.page';

setup('create customer auth state', async ({ page, context }) => {
  const loginPage = new LoginPage(page);
  await loginPage.goto();
  await loginPage.login(process.env.CUSTOMER_EMAIL!, process.env.CUSTOMER_PASSWORD!);
  await loginPage.expectSuccessfulLogin();
  await context.storageState({ path: '.auth/customer01.json' });
});
```

### Step 8: Add API Helpers

```ts
import { APIRequestContext, expect } from '@playwright/test';

export async function loginByApi(request: APIRequestContext, email: string, password: string) {
  const response = await request.post(`${process.env.API_URL}/users/login`, {
    data: { email, password }
  });

  expect(response.status()).toBe(200);
  return await response.json();
}
```

### Step 9: Add Test Data Factories

```ts
export function createCheckoutAddress() {
  return {
    street: '123 Testing Way',
    city: 'Sacramento',
    country: 'USA',
    postalCode: '98765'
  };
}
```

### Step 10: Add CI Workflow

```yaml
name: Playwright Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npx playwright install --with-deps
      - run: npx playwright test
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: playwright-report
          path: playwright-report/
```

---

# Required Framework Files

| File / Folder | Required? | Purpose |
|---|---:|---|
| `package.json` | Yes | npm scripts and dependencies. |
| `playwright.config.ts` | Yes | Main Playwright execution configuration. |
| `tsconfig.json` | Yes for TypeScript | TypeScript compiler settings and aliases. |
| `tests/` | Yes | Test specs. |
| `tests/auth.setup.ts` | Recommended | Saves login/session state. |
| `lib/pages/` | Recommended | Page Object Model classes. |
| `lib/fixtures/` | Recommended | Custom fixtures for page objects and shared setup. |
| `lib/data/` or `lib/datafactory/` | Recommended | Test data builders and factories. |
| `lib/utils/` or `lib/helpers/` | Recommended | Reusable helper utilities. |
| `.env` | Optional local file | Local environment variables. |
| `.env.example` | Recommended | Safe template for required env vars. |
| `.gitignore` | Yes | Excludes reports, videos, traces, `node_modules`, and secret files. |
| `.github/workflows/playwright.yml` | Recommended | CI execution. |
| `Dockerfile` | Optional | Containerized execution. |
| `README.md` | Yes | Framework documentation. |

## Example `.gitignore`

```gitignore
node_modules/
playwright-report/
test-results/
.auth/
.env
*.log
```

## Example `.env.example`

```env
BASE_URL=http://localhost:5173
API_URL=https://api.example.com
CUSTOMER_EMAIL=customer@example.com
CUSTOMER_PASSWORD=Password123!
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=Password123!
```

---

# Requirements and Prerequisites

| Requirement | Details |
|---|---|
| Node.js | Use current LTS, commonly Node 20+. |
| npm | Required for dependency installation and scripts. |
| TypeScript | Strongly recommended for maintainable Playwright frameworks. |
| Playwright browsers | Installed with `npx playwright install`. |
| Application URL | Local, QA, staging, or production-safe target environment. |
| Test credentials | Use safe test users only; store in `.env` or CI secrets. |
| Stable locators | Prefer accessible locators and `data-test` attributes. |
| CI environment | GitHub Actions, Azure DevOps, Jenkins, or similar. |
| Test data strategy | API setup, factories, seeded test data, or database reset strategy. |

---

# Custom Framework vs Out-of-the-Box Playwright

| Scenario | Use Out-of-the-Box Playwright | Build Custom Framework Layers |
|---|---|---|
| Small proof of concept | Yes | No |
| 5–20 smoke tests | Yes | Minimal fixtures only |
| 50+ regression tests | No | Yes |
| Multiple user roles | No | Yes, use storage states and fixtures |
| Many repeated workflows | No | Yes, use POM/helpers |
| CI/CD execution required | Playwright config may be enough | Add CI, reports, artifacts, tags |
| Complex data setup | No | Use API helpers and data factories |
| Multiple environments | Minimal env config | Add environment config strategy |
| BDD requirement | No | Add Cucumber/Gherkin layer |
| Visual testing only | Playwright screenshots may be enough | Add baselines and masking strategy |
| Enterprise scale suite | No | Yes, structured framework required |

**Recommendation:** Start with Playwright Test out of the box. Add framework layers only when duplication or maintenance cost appears. The best Playwright framework is usually a thin framework: Playwright Test + TypeScript + fixtures + POM + API helpers + storage state + CI configuration.

---

# Best and Most Popular Playwright Framework Patterns

| Framework Pattern | Best For | Why It Is Popular | Recommended Structure |
|---|---|---|---|
| Plain Playwright Test | Learning, POCs, small smoke suites | Fastest setup and least abstraction | `tests/*.spec.ts`, `playwright.config.ts` |
| Playwright + Page Object Model | Medium/large UI suites | Encapsulates locators and page workflows | `tests/`, `lib/pages/` |
| Playwright + Fixtures | Reusable setup, page injection, auth state, API clients | Native Playwright pattern; composable and typed | `fixtures/`, `pages/`, `tests/` |
| POM + Fixtures Hybrid | Professional TypeScript frameworks | Clean tests, reusable pages, typed injection | `lib/pages`, `lib/fixtures`, `tests` |
| API-First Playwright Framework | Backend/API validation and UI setup | Uses built-in `request` fixture | `tests/api`, `lib/api`, `data` |
| BDD Playwright + Cucumber | Teams with BA/Product collaboration | Gherkin scenarios are business-readable | `features/`, `steps/`, `pages/` |
| Visual Regression Framework | UI-heavy apps and design systems | Built-in screenshot comparisons | `tests/visual`, baselines, masking helpers |
| Accessibility Framework | WCAG checks and compliance teams | Integrates with `@axe-core/playwright` | `tests/accessibility`, `utils/a11y.ts` |
| Mobile Web Emulation Framework | Responsive web workflows | Playwright device profiles are built in | mobile projects in config |
| Dockerized Playwright Framework | Consistent CI execution | Official Playwright images simplify browser dependencies | `Dockerfile`, `docker-compose.yml` |
| Cloud Browser Framework | Enterprise scaling | Useful for large suites and remote browser capacity | service config + CI secrets |

## Best Framework by Task

| Task / Scenario | Best Framework Choice |
|---|---|
| Learn Playwright basics | Plain Playwright Test |
| Build portfolio project | POM + fixtures + auth state + API tests |
| Enterprise UI regression | POM + fixtures + data factories + CI matrix |
| API automation | Playwright `request` fixture + API helpers |
| Login-heavy app | Storage state setup project + role fixtures |
| Multi-role app | Separate auth setup files per role |
| E-commerce site | POM + checkout workflow + API test data setup |
| Design-system visual checks | Visual regression framework |
| Accessibility compliance | Playwright + axe-core utility layer |
| BDD team with product owners | Cucumber + Playwright + POM |
| Flaky external dependencies | API mocking and network interception layer |
| Mobile/responsive coverage | Device projects and mobile-specific specs |
| Fast CI smoke tests | Tagged `@smoke` suite + Chromium only |
| Full release regression | Browser matrix + retries + trace/video artifacts |

---

## Sample Code from This Project

### Basic UI Test

```ts
import { test, expect } from '@playwright/test';

test('Validate promo code popup', async ({ page }) => {
  await page.goto('http://localhost:5173/');

  const popupPromise = page.waitForEvent('popup');
  await page.getByRole('button', { name: 'Get Promo Code' }).click();
  const popup = await popupPromise;

  await expect(popup.getByText('The promo code is:')).toBeVisible();
});
```

### API Test

```ts
import { test, expect } from '@playwright/test';

test('GET /products', async ({ request }) => {
  const apiUrl = process.env.API_URL;
  const response = await request.get(apiUrl + '/products');

  expect(response.status()).toBe(200);
  const body = await response.json();
  expect(body.data.length).toBeGreaterThan(0);
});
```

### Page Object Model Login Page

```ts
import { type Locator, type Page } from '@playwright/test';

export class LoginPage {
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly loginButton: Locator;

  constructor(private readonly page: Page) {
    this.emailInput = page.getByTestId('email');
    this.passwordInput = page.getByTestId('password');
    this.loginButton = page.getByTestId('login-submit');
  }

  async goto() {
    await this.page.goto('/auth/login');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }
}
```

### Authentication Storage State Setup

```ts
import { test as setup, expect } from '@playwright/test';
import { LoginPage } from '../lib/pages/login/login.page';

setup('Create customer 01 auth', async ({ page, context }) => {
  const loginPage = new LoginPage(page);

  await loginPage.goto();
  await loginPage.login('customer@practicesoftwaretesting.com', 'welcome01');

  await expect(page.getByTestId('nav-menu')).toContainText('Jane Doe');
  await context.storageState({ path: '.auth/customer01.json' });
});
```

### Custom Page Fixtures

```ts
import { LoginPage } from '@pages/login/login.page';
import { test as baseTest } from '@playwright/test';

type MyPages = {
  loginPage: LoginPage;
};

export const test = baseTest.extend<MyPages>({
  loginPage: async ({ page }, use) => {
    await use(new LoginPage(page));
  }
});

export { expect } from '@playwright/test';
```

### Console Error Fixture and Custom Matcher

```ts
import { expect as baseExpect, test as baseTest, type Page, type ConsoleMessage } from '@playwright/test';

class PageConsole {
  readonly messages: ConsoleMessage[] = [];

  constructor(page: Page) {
    page.on('console', message => this.messages.push(message));
  }
}

export const test = baseTest.extend<{ pageConsole: PageConsole }>({
  pageConsole: async ({ page }, use) => {
    const pageConsole = new PageConsole(page);
    await use(pageConsole);
  }
});

export const expect = baseExpect.extend({
  async toHaveNoConsoleErrors(pageConsole: PageConsole) {
    const errors = pageConsole.messages.filter(message => message.type() === 'error');
    return {
      pass: errors.length === 0,
      message: () => `Console errors: ${errors.map(error => error.text()).join('\n')}`
    };
  }
});
```

### Merged Fixtures

```ts
import { mergeExpects, mergeTests } from '@playwright/test';
import { test as pageTest, expect as pageExpect } from '@fixtures/pages.fixture';
import { test as consoleTest, expect as consoleExpect } from '@fixtures/console.fixture';

export const test = mergeTests(pageTest, consoleTest);
export const expect = mergeExpects(pageExpect, consoleExpect);
```

### Checkout End-to-End Test

```ts
test.describe('Checkout challenge', async () => {
  test.use({ storageState: '.auth/customer01.json' });

  test.beforeEach(async ({ page }) => {
    await page.goto('/');
  });

  test('buy now pay later', async ({ page, isMobile }) => {
    await page.getByText('Claw Hammer with Shock Reduction Grip').click();
    await page.getByTestId('add-to-cart').click();
    await expect(page.getByTestId('cart-quantity')).toHaveText('1');

    if (isMobile) {
      await page.getByLabel('Toggle navigation').click();
    }

    await page.getByTestId('nav-cart').click();
    await page.getByTestId('proceed-1').click();
    await page.getByTestId('proceed-2').click();
    await page.getByTestId('street').fill('123 Testing Way');
    await page.getByTestId('city').fill('Sacramento');
    await page.getByTestId('country').fill('USA');
    await page.getByTestId('postal_code').fill('98765');
    await page.getByTestId('proceed-3').click();
    await page.getByTestId('payment-method').selectOption('Buy Now Pay Later');
    await page.getByTestId('monthly_installments').selectOption('6 Monthly Installments');
    await page.getByTestId('finish').click();

    await expect(page.locator('.help-block')).toHaveText('Payment was successful');
  });
});
```

### Visual Screenshot Assertion

```ts
await test.step('visual test', async () => {
  await expect(page).toHaveScreenshot('checkout.png', {
    mask: [page.getByTitle('Practice Software Testing - Toolshop')]
  });
});
```

---

## How to Run This Project

```bash
git clone https://github.com/BrianGator/Playwright-Test-Automation-Master-TypeScript.git
cd Playwright-Test-Automation-Master-TypeScript
npm install
npm run install:bagel-shop
npx playwright install
npm test
npm run test:ui
npm run test:report
```

Create `.env` when needed:

```env
URL=http://localhost:5173
API_URL=https://api.practicesoftwaretesting.com
CUSTOMER_EMAIL=customer@practicesoftwaretesting.com
CUSTOMER_PASSWORD=welcome01
```

---

## Best Practices

- Prefer `getByRole()`, `getByLabel()`, and `getByTestId()` over brittle CSS/XPath.
- Avoid `waitForTimeout()` except for temporary debugging.
- Use `storageState` for repeated login-heavy tests.
- Use fixtures to inject reusable objects.
- Use Page Object Model for repeated page workflows.
- Use API helpers for data setup and backend validation.
- Keep tests independent and parallel-safe.
- Store secrets in environment variables or CI secrets.
- Review traces before changing test code.
- Use tags such as `@smoke`, `@regression`, and `@visual` to control execution.

---

## Common Interview Questions

### What files are required for a Playwright framework?

At minimum: `package.json`, `playwright.config.ts`, `tests/`, and test specs. For a TypeScript framework, also use `tsconfig.json`. For maintainability, add `pages/`, `fixtures/`, `data/`, `utils/`, `.env.example`, `.gitignore`, and CI workflow files.

### When do you build a custom framework instead of using Playwright out of the box?

Build custom layers when tests become repetitive, multiple user roles are needed, authentication must be reused, test data setup is complex, the team needs CI artifacts, or the suite will be maintained by multiple engineers.

### What is the most popular Playwright framework style?

The most common professional pattern is **Playwright Test + TypeScript + Page Object Model + custom fixtures + storage state + CI/CD**.

### What is the difference between a page object and a fixture?

A page object stores page-specific locators and actions. A fixture creates and injects reusable dependencies, such as page objects, authenticated pages, API clients, or test data.

### What is storage state?

Storage state is a JSON file containing cookies and local/session storage. It allows tests to start already authenticated.

### How do you reduce flaky tests?

Use stable locators, avoid hard waits, isolate test data, mock unstable dependencies, use web-first assertions, review traces, and run suspected tests with `--repeat-each`.

### How do you test APIs with Playwright?

Use the `request` fixture to send HTTP requests, validate status codes, parse JSON, create test data, clean up data, and support UI test setup.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Dependencies missing | Run `npm install` and `npx playwright install`. |
| App does not start | Run `npm run start` and verify the configured port. |
| `.env` values missing | Create `.env` from `.env.example`. |
| Auth state missing | Run `npx playwright test --project setup`. |
| Visual test fails | Review the diff before updating snapshots. |
| Test passes headed but fails headless | Check timing, viewport, animations, and traces. |
| CI fails but local passes | Compare environment variables, browser dependencies, workers, and timeouts. |

---

## Suggested Improvements

1. Add `.env.example`.
2. Add GitHub Actions workflow.
3. Add Dockerfile and docker-compose support.
4. Add Allure or enhanced reporting.
5. Add accessibility tests with `@axe-core/playwright`.
6. Add more API setup/teardown helpers.
7. Add multiple role-based storage states.
8. Add BDD Cucumber examples.
9. Add visual CI examples.
10. Add framework architecture diagram.

---

## Author

**Written by Brian McCarthy**

This repository demonstrates practical TypeScript Playwright automation skills including UI testing, API testing, Page Object Model design, custom fixtures, authentication state reuse, browser/mobile projects, visual assertions, Docker execution, CI-ready configuration, and modern test automation design patterns.
