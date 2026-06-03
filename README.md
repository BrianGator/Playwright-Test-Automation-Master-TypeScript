# Playwright Test Automation with TypeScript

A practical end-to-end test automation project using **Playwright**, **TypeScript**, and **Node.js**. This README is structured as a master Playwright automation guide with a module-based table of contents, detailed explanations, TypeScript code samples, expected results, framework-building guidance, and preserved sample code from this project.

**Written by Brian McCarthy**

---

## Table of Contents

Here is the revised Table of Contents format with a second column describing each module's contents:

| Module | Contents Covered |
|---|---|
| [Learning Playwright](#learning-playwright) | Introduction to Playwright, why it is used for modern test automation, Playwright vs Cypress comparison, exercise files, and the overall learning path. |
| [1. Playwright Installation and Configuration](#1-playwright-installation-and-configuration) | Installing Playwright, configuring `playwright.config.ts`, browser projects, CLI commands, `package.json`, VS Code extension, and UI Mode. |
| [2. Creating Tests with Playwright](#2-creating-tests-with-playwright) | Website overview, Codegen, locator strategies, assertions, test structure, cookie authentication, visual testing, API testing, and choosing the right tests to automate. |
| [3. Maintaining Playwright Tests](#3-maintaining-playwright-tests) | Screenshots, videos, reporters, trace viewer, debugging failed tests, scaling tests, and keeping tests stable over time. |
| [Playwright Essential Training: Abstractions, Fixtures, and Complex Scenarios](#playwright-essential-training-abstractions-fixtures-and-complex-scenarios) | Advanced framework organization topics including page objects, data factories, helpers, custom assertions, fixtures, mocking, emulation, config customization, and complex UI interactions. |
| [4. Abstractions in Playwright](#4-abstractions-in-playwright) | Pros and cons of abstractions, creating and using page objects, managing test data, data factories, helpers, custom assertions, and TypeScript config management. |
| [5. Fixtures in Playwright](#5-fixtures-in-playwright) | Built-in fixtures, custom fixtures, fixture scope, fixture lifecycle, advanced fixtures, replacing repeated setup hooks, and reusable test dependencies. |
| [6. Mocking and Emulation in Playwright](#6-mocking-and-emulation-in-playwright) | Network routing, intercepting API calls, mocking HTTP responses, modifying API responses, browser emulation, localization, geolocation, and JavaScript injection. |
| [7. Customizing the Playwright Config](#7-customizing-the-playwright-config) | Running a web server during tests, optimizing workers and sharding, retries, global/test/action/navigation/expect timeouts, and project-level configuration. |
| [8. Handling Complex Scenarios and Interactions in Playwright](#8-handling-complex-scenarios-and-interactions-in-playwright) | Multiple windows, popups, browser alerts, dialogs, uploads, downloads, challenging elements, iframes, drag-and-drop, sliders, date pickers, and web tables. |
| [Playwright: Design Patterns](#playwright-design-patterns) | Framework design concepts including fixtures, Page Object Model, BDD, and data-driven testing. |
| [9. Fixtures as a Design Pattern](#9-fixtures-as-a-design-pattern) | Automatic fixtures, custom fixtures, fixture scope/isolation, fixture best practices, and combining fixtures into reusable framework layers. |
| [10. Page Object Model](#10-page-object-model) | What POM is, creating basic page objects, using POM in tests, combining POM with fixtures, reusable page models, and recommended Playwright POM architecture. |
| [11. Behavior Driven Development BDD](#11-behavior-driven-development-bdd) | BDD concepts, Cucumber setup, Gherkin scenarios, step definitions, centralized setup, and combining BDD with Page Object Model. |
| [12. Data-Driven Testing](#12-data-driven-testing) | Setting up test data, looping through test data sets, using factories, validating multiple scenarios, and reducing duplicated test logic. |
| [Advanced Playwright Techniques](#advanced-playwright-techniques) | Speed optimization, flakiness reduction, visual testing, cloud execution, Docker, CI/CD, and large-suite execution strategies. |
| [13. Optimising Test Speed in Playwright](#13-optimising-test-speed-in-playwright) | Green testing, diagnosing bottlenecks, using `storageState`, project dependencies for setup, parallelization, and improving slow tests. |
| [14. Reducing Test Flakiness in Playwright](#14-reducing-test-flakiness-in-playwright) | Stable locators, hydration issues, external dependency control, repeated test runs, fixing flaky tests, and avoiding timing-based failures. |
| [15. Screenshot and Snapshot Testing Best Practices](#15-screenshot-and-snapshot-testing-best-practices) | Capturing screenshots, component screenshots, snapshots, visual comparisons, updating baselines, and reviewing visual diffs. |
| [16. Running Tests on Microsoft Playwright Testing Service](#16-running-tests-on-microsoft-playwright-testing-service) | Cloud browser execution, Azure resource setup, cloud config, CLI execution, and running tests against local apps through cloud infrastructure. |
| [17. Complete Supplemental Playwright Topics](#17-complete-supplemental-playwright-topics) | JavaScript fundamentals, DOM terminology, locator syntax, user-facing locators, assertions, auto-waiting, timeouts, UI components, API mocking, API requests, shared authentication state, global setup/teardown, tags, mobile emulation, reporters, Docker, GitHub Actions, and visual CI. |
| [Build a Playwright Framework from Scratch](#build-a-playwright-framework-from-scratch) | Steps, files, requirements, and architecture decisions for creating a Playwright framework from scratch or adding one to an existing app. |
| [Required Framework Files](#required-framework-files) | Required and recommended framework files with purpose descriptions. |
| [Custom Framework vs Out-of-the-Box Playwright](#custom-framework-vs-out-of-the-box-playwright) | When to keep Playwright simple and when to add custom framework layers. |
| [Best and Most Popular Playwright Framework Patterns](#best-and-most-popular-playwright-framework-patterns) | Recommended framework styles for UI, API, POM, fixtures, BDD, visual, accessibility, mobile, Docker, cloud, and CI scenarios. |
| [Sample Code from This Project](#sample-code-from-this-project) | Basic UI Test, API Test, Page Object Model Login Page, Authentication Storage State Setup, Custom Page Fixtures, Console Error Fixture and Custom Matcher, Merged Fixtures, Checkout End-to-End Test, and Visual Screenshot Assertion. |

---

## Project Overview

This project is a TypeScript Playwright automation framework built around a sample bagel/shop and practice testing application structure. It includes browser-based tests, API tests, reusable page objects, custom fixtures, storage-state authentication, test data helpers, visual assertions, custom matchers, and Playwright reporting/debugging features.

The repository demonstrates professional test automation concepts: clean test structure, stable locators, test hooks, fixtures, Page Object Model, API testing, authentication state reuse, screenshot assertions, mobile-aware flows, custom matchers, and CI-ready execution.

---

## Project Structure

```text
Playwright-Test-Automation-Master-TypeScript/
├── bagel-shop/                         # Local sample app used by the tests
├── lib/
│   ├── datafactory/                    # Test data helpers and factories
│   ├── fixtures/                       # Custom and merged Playwright fixtures
│   │   ├── base.fixture.ts
│   │   ├── console.fixture.ts
│   │   └── pages.fixture.ts
│   ├── helpers/                        # Shared utility functions
│   └── pages/                          # Page Object Model classes
│       ├── account/
│       ├── contact/
│       └── login/
├── tests/
│   ├── api/                            # API tests with Playwright request fixture
│   ├── bagel-shop/                     # UI tests
│   ├── checkout/                       # Checkout workflow tests
│   ├── homepage/                       # Homepage tests
│   └── auth.setup.ts                   # Authentication storage state setup
├── package.json
├── playwright.config.ts
├── tsconfig.json
└── README.md
```

---

# Master Test Automation with Playwright

## Learning Playwright

### Detailed Explanation

Playwright is a modern browser automation framework for end-to-end testing, API testing, visual testing, and cross-browser validation. It supports Chromium, Firefox, and WebKit with one API. It is popular because it provides built-in auto-waiting, reliable locators, browser contexts, mobile emulation, storage state, tracing, videos, screenshots, fixtures, reporters, parallel execution, and API requests.

Compared with Cypress, Playwright is often preferred when teams need true multi-browser support including WebKit, multiple browser contexts, multi-tab workflows, stronger built-in API testing, and flexible CI/browser project configuration. Cypress is still popular for developer-friendly component and frontend testing, but Playwright is a strong choice for cross-browser enterprise automation.

### Code Sample

```ts
import { test, expect } from '@playwright/test';

test('home page loads successfully', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveTitle(/Practice Software Testing|Bagel/i);
  await expect(page.locator('body')).toBeVisible();
});
```

### Expected Result

Playwright launches a browser, opens the configured base URL, waits for the page to become ready, validates the title, and confirms the body is visible.

### Best Practices

- Learn Playwright's test runner before adding too many custom framework layers.
- Prefer built-in Playwright features before adding third-party packages.
- Use TypeScript for maintainability and compile-time feedback.

---

## 1. Playwright Installation and Configuration

### Detailed Explanation

This module covers installing Playwright from scratch or adding it to an existing application. Configuration includes browser projects, base URL, reporters, retries, workers, timeouts, test directory, artifacts, web server startup, and test ID attributes.

### Code Sample: New Project Setup

```bash
mkdir playwright-typescript-demo
cd playwright-typescript-demo
npm init -y
npm init playwright@latest
npx playwright install
npx playwright test
npx playwright show-report
```

### Code Sample: Add Playwright to Existing App

```bash
npm install -D @playwright/test typescript
npx playwright install
mkdir tests
```

### Code Sample: `playwright.config.ts`

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  timeout: 30_000,
  expect: { timeout: 5_000 },
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  reporter: [['list'], ['html', { open: 'never' }]],
  use: {
    baseURL: process.env.BASE_URL ?? 'http://localhost:5173',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    testIdAttribute: 'data-test'
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
    { name: 'webkit', use: { ...devices['Desktop Safari'] } },
    { name: 'mobile-safari', use: { ...devices['iPhone 14'] } }
  ]
});
```

### Expected Result

Playwright installs, browser engines are downloaded, tests can run from the command line, and the framework is configured for cross-browser execution.

### Best Practices

- Use `baseURL` so tests can call `page.goto('/')`.
- Keep CI retries separate from local retries.
- Use `trace: 'on-first-retry'` to avoid oversized artifacts.
- Add npm scripts for common commands.

---

## 2. Creating Tests with Playwright

### Detailed Explanation

Creating tests includes understanding the app under test, writing specs, generating starter code with Codegen, selecting reliable locators, adding assertions, using hooks, handling cookies, testing APIs, and deciding which flows are worth automating.

### Code Sample: Test Structure and Hooks

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

### Code Sample: Locators and Assertions

```ts
await page.getByRole('button', { name: 'Login' }).click();
await page.getByLabel('Email').fill('customer@example.com');
await page.getByPlaceholder('Search products').fill('hammer');
await page.getByTestId('checkout-submit').click();

await expect(page.getByRole('heading', { name: 'Products' })).toBeVisible();
await expect(page.getByTestId('cart-quantity')).toHaveText('1');
await expect(page).toHaveURL(/checkout/);
```

### Code Sample: API Test

```ts
import { test, expect } from '@playwright/test';

test('GET products API', async ({ request }) => {
  const response = await request.get(`${process.env.API_URL}/products`);
  expect(response.status()).toBe(200);

  const body = await response.json();
  expect(body.data.length).toBeGreaterThan(0);
});
```

### Expected Result

Tests are organized, readable, and validate real user-visible outcomes. API tests can validate backend behavior without opening a browser.

### Best Practices

- Prefer `getByRole`, `getByLabel`, and `getByTestId`.
- Avoid brittle CSS/XPath unless needed.
- Use Codegen for learning and discovery, then refactor generated code.
- Automate high-value workflows such as login, checkout, search, forms, and API contracts.

---

## 3. Maintaining Playwright Tests

### Detailed Explanation

Maintaining tests means making failures easy to debug and reducing false failures. Playwright supports HTML reports, list reports, screenshots, videos, traces, UI Mode, Inspector, and repeat runs. Stable maintenance also requires good locators, isolated data, reliable setup, and clear assertions.

### Code Sample: Reporter and Artifacts

```ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  reporter: [['list'], ['html', { open: 'never' }]],
  use: {
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    trace: 'on-first-retry'
  }
});
```

### Code Sample: Debug Commands

```bash
npx playwright test --debug
npx playwright test --ui
npx playwright show-report
npx playwright show-trace test-results/path-to-trace.zip
npx playwright test --repeat-each=10
```

### Expected Result

Failures include enough evidence to diagnose the issue. Engineers can inspect action steps, DOM snapshots, screenshots, console logs, and network activity through the trace viewer.

### Best Practices

- Review traces before changing code.
- Use `--repeat-each` to confirm flaky behavior.
- Keep tests independent and parallel-safe.
- Prefer web-first assertions over hard waits.

---

## Playwright Essential Training: Abstractions, Fixtures, and Complex Scenarios

### Detailed Explanation

This section focuses on framework organization beyond basic tests. It covers abstractions, Page Object Model, data factories, helpers, custom assertions, custom fixtures, mocking, emulation, configuration tuning, and complex UI interactions.

### Code Sample: Framework Folder Layout

```text
lib/
├── pages/
├── fixtures/
├── helpers/
├── datafactory/
└── api/
```

### Expected Result

The framework becomes easier to scale because repeated actions and setup are moved into reusable components.

---

## 4. Abstractions in Playwright

### Detailed Explanation

Abstractions reduce duplication but can make code harder to understand if overused. Good abstractions include page objects for page behavior, helpers for shared utilities, data factories for test data, and custom assertions for repeated validations.

### Code Sample: Page Object

```ts
import { Page, expect } from '@playwright/test';

export class ContactPage {
  constructor(private readonly page: Page) {}

  async goto() {
    await this.page.goto('/contact');
    await expect(this.page.getByRole('heading', { name: 'Contact' })).toBeVisible();
  }

  async submitMessage(name: string, email: string, message: string) {
    await this.page.getByLabel('Name').fill(name);
    await this.page.getByLabel('Email').fill(email);
    await this.page.getByLabel('Message').fill(message);
    await this.page.getByRole('button', { name: 'Submit' }).click();
  }
}
```

### Code Sample: Data Factory

```ts
export function createContactMessage() {
  return {
    name: 'Brian McCarthy',
    email: `brian-${Date.now()}@example.com`,
    message: 'Testing Playwright contact form automation.'
  };
}
```

### Expected Result

Tests become shorter because reusable page behavior and test data generation are centralized.

### Best Practices

- Abstract repeated behavior, not every single click.
- Keep page object methods focused on user actions.
- Keep assertions in tests when they describe the business expectation.

---

## 5. Fixtures in Playwright

### Detailed Explanation

Fixtures are Playwright's dependency injection system. Built-in fixtures include `page`, `context`, `browser`, `request`, `browserName`, `isMobile`, and `headless`. Custom fixtures can inject page objects, data factories, API clients, and console tracking.

### Code Sample: Custom Page Fixture

```ts
import { test as base, expect } from '@playwright/test';
import { LoginPage } from '../lib/pages/login/login.page';

type Fixtures = {
  loginPage: LoginPage;
};

export const test = base.extend<Fixtures>({
  loginPage: async ({ page }, use) => {
    await use(new LoginPage(page));
  }
});

export { expect };
```

### Code Sample: Worker-Scoped Fixture

```ts
export const test = base.extend<{}, { apiToken: string }>({
  apiToken: [async ({}, use) => {
    const token = process.env.API_TOKEN ?? 'demo-token';
    await use(token);
  }, { scope: 'worker' }]
});
```

### Expected Result

Tests can request typed dependencies directly in the test callback, reducing repeated setup and improving readability.

### Best Practices

- Use test-scoped fixtures for isolated page objects.
- Use worker-scoped fixtures for expensive shared setup.
- Keep fixtures small and composable.

---

## 6. Mocking and Emulation in Playwright

### Detailed Explanation

Mocking and emulation help isolate frontend behavior from unstable services, slow APIs, third-party dependencies, and hard-to-create data scenarios. Playwright can intercept requests, mock responses, modify responses, emulate mobile devices, set geolocation, set locale/timezone, and inject JavaScript before page load.

### Code Sample: Mock HTTP Response

```ts
import { test, expect } from '@playwright/test';

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

### Code Sample: Geolocation

```ts
test.use({
  geolocation: { latitude: 27.9506, longitude: -82.4572 },
  permissions: ['geolocation'],
  locale: 'en-US',
  timezoneId: 'America/New_York'
});

test('uses geolocation', async ({ page }) => {
  await page.goto('/stores');
  await page.getByRole('button', { name: 'Use my location' }).click();
  await expect(page.getByText(/nearest store/i)).toBeVisible();
});
```

### Expected Result

The UI behaves as if backend data, device location, locale, and browser conditions are controlled by the test.

### Best Practices

- Mock only dependencies that are not the purpose of the test.
- Keep mock payloads realistic.
- Use API tests to validate the backend separately.

---

## 7. Customizing the Playwright Config

### Detailed Explanation

The Playwright config controls how the suite runs. Important settings include `webServer`, `workers`, `retries`, `timeout`, `expect.timeout`, `actionTimeout`, `navigationTimeout`, `projects`, `reporter`, and artifact settings. Good configuration makes local and CI execution predictable.

### Code Sample

```ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  webServer: {
    command: 'npm run start',
    url: 'http://localhost:5173',
    reuseExistingServer: !process.env.CI,
    timeout: 120_000
  },
  workers: process.env.CI ? 2 : undefined,
  retries: process.env.CI ? 2 : 0,
  timeout: 30_000,
  expect: { timeout: 5_000 },
  use: {
    actionTimeout: 10_000,
    navigationTimeout: 20_000
  }
});
```

### Expected Result

The application starts before tests run, CI uses controlled parallelism/retries, and test/action/navigation assertions have consistent timeout behavior.

### Best Practices

- Keep global timeouts reasonable.
- Use project dependencies for auth setup.
- Use `forbidOnly` in CI.
- Use trace/video/screenshot artifacts strategically.

---

## 8. Handling Complex Scenarios and Interactions in Playwright

### Detailed Explanation

Complex scenarios include popups, multiple tabs, alerts, dialogs, uploads, downloads, custom controls, iframes, drag-and-drop, sliders, date pickers, and web tables. The key rule is to start waiting for events before triggering them.

### Code Sample: Popup

```ts
test('opens promo popup', async ({ page }) => {
  await page.goto('/');

  const popupPromise = page.waitForEvent('popup');
  await page.getByRole('button', { name: 'Get Promo Code' }).click();
  const popup = await popupPromise;

  await expect(popup.getByText('The promo code is:')).toBeVisible();
});
```

### Code Sample: Upload and Download

```ts
test('uploads and downloads a file', async ({ page }) => {
  await page.goto('/files');
  await page.getByLabel('Upload file').setInputFiles('test-data/sample.txt');
  await expect(page.getByText('sample.txt')).toBeVisible();

  const downloadPromise = page.waitForEvent('download');
  await page.getByRole('link', { name: 'Download receipt' }).click();
  const download = await downloadPromise;
  await download.saveAs(`test-results/${download.suggestedFilename()}`);
});
```

### Code Sample: Iframe and Drag-and-Drop

```ts
test('drag and drop inside iframe', async ({ page }) => {
  await page.goto('/drag-drop');

  const frame = page.frameLocator('iframe[data-test="demo-frame"]');
  await frame.getByText('Drag me').dragTo(frame.getByText('Drop here'));

  await expect(frame.getByText('Dropped')).toBeVisible();
});
```

### Expected Result

The test handles event-driven browser behavior reliably and validates the final user-visible result.

### Best Practices

- Start `waitForEvent()` before the click that triggers the event.
- Use `frameLocator()` for iframes.
- Validate the final state after every complex interaction.

---

## Playwright: Design Patterns

### Detailed Explanation

Playwright design patterns are framework-level approaches that make automation easier to scale. The most common are fixtures, Page Object Model, BDD, and data-driven testing.

### Code Sample: Recommended Architecture

```text
tests/
lib/pages/
lib/fixtures/
lib/datafactory/
lib/helpers/
lib/api/
```

### Expected Result

The project separates test intent from framework support code.

---

## 9. Fixtures as a Design Pattern

### Detailed Explanation

Fixtures can be used as a framework design pattern for reusable setup. Automatic fixtures can capture console logs, attach artifacts, or enforce cleanup. Merged fixtures combine multiple custom fixture sets into one test import.

### Code Sample: Automatic Console Fixture

```ts
import { test as base } from '@playwright/test';

export const test = base.extend<{ saveLogs: void }>({
  saveLogs: [async ({ page }, use, testInfo) => {
    const logs: string[] = [];
    page.on('console', msg => logs.push(`${msg.type()}: ${msg.text()}`));
    await use();
    await testInfo.attach('console-logs', {
      body: logs.join('\n'),
      contentType: 'text/plain'
    });
  }, { auto: true }]
});
```

### Expected Result

Every test automatically captures browser console logs and attaches them to the Playwright report.

### Best Practices

- Use automatic fixtures for cross-cutting concerns.
- Use merged fixtures when multiple fixture modules are needed.
- Avoid large fixtures that do too many unrelated tasks.

---

## 10. Page Object Model

### Detailed Explanation

Page Object Model stores page-specific locators and user actions in classes. It keeps test files focused on business workflow instead of selector details. In Playwright, POM works especially well when combined with fixtures.

### Code Sample: Base Page and Login Page

```ts
import { Page, Locator, expect } from '@playwright/test';

export class BasePage {
  constructor(protected readonly page: Page) {}

  async goto(path = '/') {
    await this.page.goto(path);
  }
}

export class LoginPage extends BasePage {
  readonly email: Locator;
  readonly password: Locator;
  readonly submit: Locator;

  constructor(page: Page) {
    super(page);
    this.email = page.getByTestId('email');
    this.password = page.getByTestId('password');
    this.submit = page.getByTestId('login-submit');
  }

  async login(email: string, password: string) {
    await this.goto('/auth/login');
    await this.email.fill(email);
    await this.password.fill(password);
    await this.submit.click();
    await expect(this.page.getByTestId('nav-menu')).toContainText('Jane Doe');
  }
}
```

### Expected Result

Login behavior is reusable, typed, and centralized in one class.

### Best Practices

- Page objects should model user actions.
- Do not put all assertions into page objects.
- Keep locators private or readonly where practical.

---

## 11. Behavior Driven Development BDD

### Detailed Explanation

BDD uses Gherkin syntax to describe behavior in business-readable scenarios. It can be useful when Product Owners, Business Analysts, QA, and developers collaborate on acceptance criteria. It adds overhead, so it should be used when the team gains value from shared scenarios.

### Code Sample: Gherkin Scenario

```gherkin
Feature: Login

  Scenario: Valid customer can log in
    Given the customer is on the login page
    When the customer signs in with valid credentials
    Then the account dashboard should be displayed
```

### Code Sample: Step Definition

```ts
import { Given, When, Then } from '@cucumber/cucumber';
import { expect } from '@playwright/test';

Given('the customer is on the login page', async function () {
  await this.page.goto('/auth/login');
});

When('the customer signs in with valid credentials', async function () {
  await this.page.getByTestId('email').fill(process.env.CUSTOMER_EMAIL!);
  await this.page.getByTestId('password').fill(process.env.CUSTOMER_PASSWORD!);
  await this.page.getByTestId('login-submit').click();
});

Then('the account dashboard should be displayed', async function () {
  await expect(this.page.getByTestId('nav-menu')).toContainText('Jane Doe');
});
```

### Expected Result

A human-readable scenario maps to executable Playwright automation.

### Best Practices

- Use BDD when stakeholders actually review scenarios.
- Keep step definitions reusable but not overly generic.
- Combine BDD with POM to avoid duplicated step code.

---

## 12. Data-Driven Testing

### Detailed Explanation

Data-driven testing runs the same test logic with multiple input sets. It is useful for login validation, forms, filters, checkout variations, user roles, API statuses, and negative testing.

### Code Sample

```ts
const loginCases = [
  { email: 'bad@example.com', password: 'wrong', message: 'Invalid email or password' },
  { email: '', password: 'welcome01', message: 'Email is required' },
  { email: 'customer@example.com', password: '', message: 'Password is required' }
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

### Expected Result

The same validation workflow runs for each data row and verifies the expected error message.

### Best Practices

- Keep small data sets inline.
- Move larger data sets into JSON, factories, or fixture data.
- Include the scenario name in the test title.

---

## Advanced Playwright Techniques

### Detailed Explanation

Advanced Playwright work focuses on speed, stability, CI/CD, screenshots, snapshots, Docker, cloud execution, and large-suite strategies.

### Code Sample: Tagged Execution

```ts
test('checkout smoke @smoke', async ({ page }) => {
  await page.goto('/checkout');
  await expect(page.getByRole('heading', { name: 'Checkout' })).toBeVisible();
});
```

```bash
npx playwright test --grep @smoke
```

### Expected Result

The suite can run targeted groups such as smoke, regression, mobile, or visual tests.

---

## 13. Optimising Test Speed in Playwright

### Detailed Explanation

Speed optimization includes avoiding repeated UI login, using `storageState`, using API setup, running tests in parallel, sharding large suites, reducing unnecessary waits, and controlling worker count in CI.

### Code Sample: Storage State Setup

```ts
import { test as setup, expect } from '@playwright/test';

setup('create customer auth state', async ({ page }) => {
  await page.goto('/auth/login');
  await page.getByTestId('email').fill(process.env.CUSTOMER_EMAIL!);
  await page.getByTestId('password').fill(process.env.CUSTOMER_PASSWORD!);
  await page.getByTestId('login-submit').click();
  await expect(page.getByTestId('nav-menu')).toContainText('Jane Doe');
  await page.context().storageState({ path: '.auth/customer01.json' });
});
```

### Code Sample: Project Dependency

```ts
projects: [
  { name: 'setup', testMatch: /.*\.setup\.ts/ },
  {
    name: 'chromium',
    dependencies: ['setup'],
    use: { storageState: '.auth/customer01.json' }
  }
]
```

### Expected Result

Login runs once in setup, then authenticated tests reuse the saved session, reducing total execution time.

### Best Practices

- Use API setup for expensive data creation.
- Use `storageState` for repeated login flows.
- Use parallelism only when tests are isolated.

---

## 14. Reducing Test Flakiness in Playwright

### Detailed Explanation

Flaky tests pass and fail inconsistently. Causes include brittle selectors, hard waits, hydration timing, animations, shared state, unstable test data, environment differences, and external dependencies.

### Code Sample: Stable Locators

```ts
// Brittle
await page.locator('.btn:nth-child(3)').click();

// Better
await page.getByRole('button', { name: 'Checkout' }).click();

// Also stable when the app exposes test ids
await page.getByTestId('checkout-submit').click();
```

### Code Sample: Flakiness Detection

```bash
npx playwright test tests/checkout/checkout.spec.ts --repeat-each=10
```

### Expected Result

The same test runs repeatedly, making intermittent instability easier to reproduce.

### Best Practices

- Avoid `waitForTimeout()`.
- Use user-facing locators and `data-test` hooks.
- Mock unstable external services.
- Keep data isolated for parallel execution.

---

## 15. Screenshot and Snapshot Testing Best Practices

### Detailed Explanation

Screenshot testing compares UI output to approved image baselines. Snapshot testing compares text, JSON, accessibility snapshots, or other serializable output. Visual tests catch layout changes, styling regressions, missing images, and unintended UI differences.

### Code Sample: Page Screenshot

```ts
test('homepage screenshot', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveScreenshot('home-page.png', {
    fullPage: true,
    maxDiffPixelRatio: 0.01
  });
});
```

### Code Sample: Component Screenshot

```ts
test('cart summary screenshot', async ({ page }) => {
  await page.goto('/cart');
  await expect(page.getByTestId('cart-summary')).toHaveScreenshot('cart-summary.png');
});
```

### Expected Result

The test fails when visual output differs from the approved baseline beyond the configured threshold.

### Best Practices

- Prefer component screenshots for stability.
- Mask dynamic content.
- Review diffs before updating snapshots.
- Keep viewport and test data deterministic.

---

## 16. Running Tests on Microsoft Playwright Testing Service

### Detailed Explanation

Cloud browser execution helps teams scale test runs beyond local infrastructure. Microsoft Playwright Testing Service and similar cloud services can provide remote browsers, higher parallelism, and more consistent environments for large suites.

### Code Sample: Cloud-Oriented Config

```ts
import { defineConfig } from '@playwright/test';
import baseConfig from './playwright.config';

export default defineConfig({
  ...baseConfig,
  workers: 8,
  retries: 2,
  reporter: [['list'], ['html']]
});
```

### Code Sample: CLI Command

```bash
npx playwright test --config=playwright.service.config.ts
```

### Expected Result

The same test suite runs in a scalable cloud browser environment rather than only on a local or single CI runner.

### Best Practices

- Start with smoke tests before moving the full suite to cloud.
- Store cloud credentials in CI secrets.
- Upload reports, traces, screenshots, and videos as artifacts.

---

## 17. Complete Supplemental Playwright Topics

### Detailed Explanation

This section fills in supporting knowledge needed for a complete Playwright framework: JavaScript fundamentals, DOM terminology, locator syntax, auto-waiting, timeout hierarchy, UI component handling, API mocking, API requests, storage state, global setup/teardown, test tags, mobile emulation, reporters, Docker, GitHub Actions, and visual CI.

### Code Sample: JavaScript Fundamentals

```ts
const product = { name: 'Hammer', price: 19.99 };
const products = ['Hammer', 'Saw', 'Drill'];

function formatProduct(name: string, price: number): string {
  return `${name}: $${price}`;
}

for (const item of products) {
  console.log(item);
}

console.log(formatProduct(product.name, product.price));
```

### Code Sample: UI Components

```ts
await page.getByLabel('First name').fill('Brian');
await page.getByLabel('Subscribe').check();
await page.getByLabel('Standard shipping').check();
await page.getByLabel('State').selectOption('FL');
await page.getByRole('button', { name: 'Save' }).click();
await expect(page.getByText('Saved')).toBeVisible();
```

### Code Sample: Dockerfile

```dockerfile
FROM mcr.microsoft.com/playwright:v1.55.0-jammy
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npx playwright install --with-deps
CMD ["npx", "playwright", "test"]
```

### Code Sample: GitHub Actions

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

### Expected Result

The framework has complete operational coverage: local execution, CI execution, Docker execution, UI component handling, API support, storage state, and reporting.

### Best Practices

- Keep `.env` out of Git.
- Commit `.env.example` instead.
- Use Docker for consistent CI dependencies.
- Use GitHub Actions artifacts for Playwright reports.

---

# Build a Playwright Framework from Scratch

## Steps

1. Install Node.js LTS.
2. Initialize the project with `npm init -y`.
3. Install Playwright with `npm init playwright@latest` or `npm install -D @playwright/test`.
4. Install browsers with `npx playwright install`.
5. Create `playwright.config.ts`.
6. Create `tsconfig.json` with path aliases.
7. Create `tests/` for specs.
8. Create `lib/pages/` for Page Object Model classes.
9. Create `lib/fixtures/` for custom fixtures.
10. Create `lib/datafactory/` for test data generation.
11. Create `lib/helpers/` or `lib/utils/` for shared utilities.
12. Add `auth.setup.ts` for storage state when login is repeated.
13. Add API helpers for setup and cleanup.
14. Add npm scripts.
15. Add `.env.example` and `.gitignore`.
16. Add GitHub Actions or another CI workflow.

## Required Setup Commands

```bash
npm init -y
npm install -D @playwright/test typescript dotenv
npx playwright install
mkdir -p tests lib/pages lib/fixtures lib/datafactory lib/helpers
```

## Expected Result

A clean Playwright TypeScript framework exists with separate layers for tests, page objects, fixtures, data, helpers, configuration, and CI execution.

---

# Required Framework Files

| File / Folder | Required? | Purpose |
|---|---:|---|
| `package.json` | Yes | npm scripts and dependencies. |
| `playwright.config.ts` | Yes | Main Playwright execution configuration. |
| `tsconfig.json` | Yes for TypeScript | TypeScript compiler settings and path aliases. |
| `tests/` | Yes | Test specs. |
| `tests/auth.setup.ts` | Recommended | Saves login/session state. |
| `lib/pages/` | Recommended | Page Object Model classes. |
| `lib/fixtures/` | Recommended | Custom fixtures for page objects and shared setup. |
| `lib/datafactory/` | Recommended | Test data builders and factories. |
| `lib/helpers/` | Recommended | Reusable helper utilities. |
| `.env` | Local only | Runtime values and secrets. |
| `.env.example` | Recommended | Safe template for expected environment variables. |
| `.gitignore` | Yes | Excludes reports, videos, traces, `node_modules`, `.auth`, and `.env`. |
| `.github/workflows/playwright.yml` | Recommended | CI execution. |
| `Dockerfile` | Optional | Containerized execution. |
| `README.md` | Yes | Framework documentation. |

---

# Custom Framework vs Out-of-the-Box Playwright

| Scenario | Use Out-of-the-Box Playwright | Build Custom Framework Layers |
|---|---|---|
| Small proof of concept | Yes | No |
| 5-20 smoke tests | Yes | Minimal fixtures only |
| 50+ regression tests | No | Yes |
| Multiple user roles | No | Yes, use storage states and role fixtures |
| Many repeated workflows | No | Yes, use POM and helpers |
| Complex data setup | No | Use API helpers and data factories |
| Multiple environments | Minimal config may work | Add environment config strategy |
| BDD requirement | No | Add Cucumber/Gherkin layer |
| Visual regression | Built-in screenshots may be enough | Add baselines, masking, and review workflow |
| Enterprise suite | No | Yes, structured framework required |

**Recommendation:** Start with Playwright Test out of the box. Add framework layers only when duplication, repeated setup, role management, CI reporting, or maintenance cost justifies it.

---

# Best and Most Popular Playwright Framework Patterns

| Framework Pattern | Best For | Why It Is Popular | Recommended Structure |
|---|---|---|---|
| Plain Playwright Test | Learning, POCs, small smoke suites | Fastest setup and least abstraction | `tests/*.spec.ts`, `playwright.config.ts` |
| Playwright + Page Object Model | Medium/large UI suites | Encapsulates locators and page workflows | `tests/`, `lib/pages/` |
| Playwright + Fixtures | Reusable setup, auth state, API clients | Native Playwright pattern; composable and typed | `fixtures/`, `pages/`, `tests/` |
| POM + Fixtures Hybrid | Professional TypeScript frameworks | Clean tests, reusable pages, typed injection | `lib/pages`, `lib/fixtures`, `tests` |
| API-First Playwright Framework | Backend/API validation and UI setup | Uses built-in `request` fixture | `tests/api`, `lib/api`, `datafactory` |
| BDD Playwright + Cucumber | BA/Product collaboration | Gherkin scenarios are business-readable | `features/`, `steps/`, `pages/` |
| Visual Regression Framework | UI-heavy apps and design systems | Built-in screenshot comparisons | `tests/visual`, baselines, masking helpers |
| Accessibility Framework | WCAG checks and compliance teams | Integrates with `@axe-core/playwright` | `tests/accessibility`, `utils/a11y.ts` |
| Mobile Web Emulation Framework | Responsive web workflows | Playwright device profiles are built in | mobile projects in config |
| Dockerized Playwright Framework | Consistent CI execution | Official Playwright images simplify browser dependencies | `Dockerfile`, `docker-compose.yml` |
| Cloud Browser Framework | Enterprise scaling | Useful for large suites and remote browser capacity | service config + CI secrets |

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

At minimum: `package.json`, `playwright.config.ts`, `tests/`, and test specs. For a TypeScript framework, also use `tsconfig.json`. For maintainability, add `pages/`, `fixtures/`, `datafactory/`, `helpers/`, `.env.example`, `.gitignore`, and CI workflow files.

### When do you build a custom framework instead of using Playwright out of the box?

Build custom layers when tests become repetitive, multiple user roles are needed, authentication must be reused, test data setup is complex, the team needs CI artifacts, or the suite will be maintained by multiple engineers.

### What is the most popular Playwright framework style?

The most common professional pattern is **Playwright Test + TypeScript + Page Object Model + custom fixtures + storage state + CI/CD**.

### What is storage state?

Storage state is a JSON file containing cookies and local/session storage. It allows tests to start already authenticated.

### How do you reduce flaky tests?

Use stable locators, avoid hard waits, isolate test data, mock unstable dependencies, use web-first assertions, review traces, and run suspected tests with `--repeat-each`.

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

## Author

**Written by Brian McCarthy**

This repository demonstrates practical TypeScript Playwright automation skills including UI testing, API testing, Page Object Model design, custom fixtures, authentication state reuse, browser/mobile projects, visual assertions, Docker execution, CI-ready configuration, and modern test automation design patterns.
