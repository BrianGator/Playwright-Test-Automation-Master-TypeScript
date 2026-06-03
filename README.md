# Playwright Test Automation with TypeScript

A practical end-to-end test automation project using **Playwright**, **TypeScript**, and **Node.js**. This repository demonstrates modern QA automation concepts including UI testing, API testing, Page Object Model design, custom fixtures, authentication storage state, visual testing, browser projects, mobile emulation, screenshots, videos, traces, reporters, Docker execution, GitHub Actions, and CI-ready Playwright configuration.

**Written by Brian McCarthy**

---

## Table of Contents

- [Project Overview](#project-overview)
- [Languages Used](#languages-used)
- [Technologies Used](#technologies-used)
- [Methodologies Used](#methodologies-used)
- [File Structure](#file-structure)
- [Project File Links](#project-file-links)
- [How the Framework Works](#how-the-framework-works)
- [Important Scripts](#important-scripts)
- [Playwright Configuration Summary](#playwright-configuration-summary)
- [Core Functions and Patterns](#core-functions-and-patterns)
- [Master Test Automation with Playwright](#master-test-automation-with-playwright)
  - [Learning Playwright](#learning-playwright)
  - [1. Playwright Installation and Configuration](#1-playwright-installation-and-configuration)
  - [2. Creating Tests with Playwright](#2-creating-tests-with-playwright)
  - [3. Maintaining Playwright Tests](#3-maintaining-playwright-tests)
  - [Playwright Essential Training: Abstractions, Fixtures, and Complex Scenarios](#playwright-essential-training-abstractions-fixtures-and-complex-scenarios)
  - [4. Abstractions in Playwright](#4-abstractions-in-playwright)
  - [5. Fixtures in Playwright](#5-fixtures-in-playwright)
  - [6. Mocking and Emulation in Playwright](#6-mocking-and-emulation-in-playwright)
  - [7. Customizing the Playwright Config](#7-customizing-the-playwright-config)
  - [8. Handling Complex Scenarios and Interactions in Playwright](#8-handling-complex-scenarios-and-interactions-in-playwright)
  - [Playwright: Design Patterns](#playwright-design-patterns)
  - [9. Fixtures as a Design Pattern](#9-fixtures-as-a-design-pattern)
  - [10. Page Object Model](#10-page-object-model)
  - [11. Behavior Driven Development BDD](#11-behavior-driven-development-bdd)
  - [12. Data-Driven Testing](#12-data-driven-testing)
  - [Advanced Playwright Techniques](#advanced-playwright-techniques)
  - [13. Optimising Test Speed in Playwright](#13-optimising-test-speed-in-playwright)
  - [14. Reducing Test Flakiness in Playwright](#14-reducing-test-flakiness-in-playwright)
  - [15. Screenshot and Snapshot Testing Best Practices](#15-screenshot-and-snapshot-testing-best-practices)
  - [16. Running Tests on Microsoft Playwright Testing Service](#16-running-tests-on-microsoft-playwright-testing-service)
  - [17. Complete Supplemental Playwright Topics](#17-complete-supplemental-playwright-topics)
- [Sample Code from This Project](#sample-code-from-this-project)
  - [Basic UI Test](#basic-ui-test)
  - [API Test](#api-test)
  - [Page Object Model Login Page](#page-object-model-login-page)
  - [Authentication Storage State Setup](#authentication-storage-state-setup)
  - [Custom Page Fixtures](#custom-page-fixtures)
  - [Console Error Fixture and Custom Matcher](#console-error-fixture-and-custom-matcher)
  - [Merged Fixtures](#merged-fixtures)
  - [Checkout End-to-End Test](#checkout-end-to-end-test)
  - [Visual Screenshot Assertion](#visual-screenshot-assertion)
- [Tutorial: How to Run This Project](#tutorial-how-to-run-this-project)
- [Best Practices and Tips](#best-practices-and-tips)
- [Common Interview Questions](#common-interview-questions)
- [Troubleshooting](#troubleshooting)
- [Suggested Improvements](#suggested-improvements)
- [Author](#author)

---

## Project Overview

This project is a TypeScript Playwright automation framework built around a sample bagel/shop and practice testing application structure. It includes browser-based tests, API tests, reusable page objects, custom fixtures, storage-state authentication, test data helpers, visual assertions, custom matchers, and Playwright reporting/debugging features.

The project is useful for demonstrating practical QA automation skills such as:

- Creating browser automation tests with Playwright Test
- Writing readable TypeScript test specs
- Using Playwright locators such as `getByRole()`, `getByTestId()`, `getByText()`, and `getByLabel()`
- Testing APIs with Playwright's built-in `request` fixture
- Saving authenticated browser sessions with `storageState`
- Using Page Object Model classes for maintainability
- Extending Playwright with custom fixtures
- Creating custom assertions/matchers
- Running tests against desktop and mobile browser projects
- Capturing screenshots, videos, and traces for debugging
- Running tests from CLI, UI Mode, VS Code extension, Docker, and CI/CD

---

## Languages Used

| Language | Purpose |
|---|---|
| TypeScript | Main test automation language for specs, fixtures, page objects, helpers, and configuration |
| JavaScript / Node.js | Runtime environment and npm script execution |
| HTML/CSS | Application UI under test and generated Playwright reports |
| JSON | Package metadata, dependency management, auth storage state, and test data structure |
| dotenv / environment variables | Runtime configuration for URLs and secrets |

---

## Technologies Used

| Technology | Purpose |
|---|---|
| Playwright Test | End-to-end browser automation, API testing, assertions, fixtures, projects, and reports |
| TypeScript | Strongly typed automation code |
| Node.js | JavaScript runtime for Playwright and npm scripts |
| npm | Package installation and script execution |
| dotenv | Loads environment variables from `.env` |
| Playwright HTML Reporter | Generates local HTML test reports |
| Playwright Trace Viewer | Debugs failed or flaky tests step-by-step |
| Playwright Video/Screenshot Artifacts | Captures evidence for failed tests |
| Docker | Runs tests in a consistent containerized environment |
| GitHub Actions | Runs tests automatically for pull requests and commits |

---

## Methodologies Used

### 1. End-to-End UI Automation

The UI tests automate real user workflows such as opening pages, clicking buttons, completing checkout, selecting payment methods, validating popups, testing mobile navigation, and checking visible messages.

### 2. API Testing

The API specs use Playwright's `request` fixture to validate backend endpoints directly with GET, POST, PUT, PATCH, and DELETE-style workflows.

### 3. Page Object Model

The framework uses page object classes such as `LoginPage` to encapsulate locators and page actions. This reduces duplicate locator code and keeps specs focused on business behavior.

### 4. Fixture-Based Architecture

Custom fixtures provide reusable page objects and console monitoring. This allows tests to inject objects such as `loginPage`, `accountPage`, `messagesPage`, and `contactPage` instead of manually creating them in every spec.

### 5. Authentication State Reuse

The `auth.setup.ts` file logs in once and saves the authenticated session to `.auth/customer01.json`. Other tests can reuse this state instead of logging in repeatedly.

### 6. Data-Driven and Helper-Based Testing

Helper functions such as `randomState()` support dynamic data entry during checkout flows. Data factories support randomized user, message, product, and checkout data.

### 7. Visual Testing

The checkout test includes screenshot comparison logic using `toHaveScreenshot()` when the test runs in headless mode.

### 8. Cross-Project Browser Testing

The Playwright config defines a Chromium project and a Mobile Safari project using device emulation.

### 9. CI-Ready Execution

The configuration includes CI-friendly settings such as `forbidOnly`, retries, worker control, trace capture, videos, screenshots, deterministic test directories, and command-line execution.

---

## File Structure

```text
Playwright-Test-Automation-w-TypeScript/
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

## Project File Links

### Main Files

- [README.md](README.md)
- [package.json](package.json)
- [playwright.config.ts](playwright.config.ts)
- [tsconfig.json](tsconfig.json)

### Test Folders

- [API Tests](tests/api/)
- [Bagel Shop Tests](tests/bagel-shop/)
- [Checkout Tests](tests/checkout/)
- [Homepage Tests](tests/homepage/)
- [Authentication Setup](tests/auth.setup.ts)

### Framework Folders

- [Fixtures](lib/fixtures/)
- [Page Objects](lib/pages/)
- [Data Factory](lib/datafactory/)
- [Helpers](lib/helpers/)

### Key Project Files

- [Base Fixture](lib/fixtures/base.fixture.ts)
- [Pages Fixture](lib/fixtures/pages.fixture.ts)
- [Console Fixture](lib/fixtures/console.fixture.ts)
- [Login Page Object](lib/pages/login/login.page.ts)
- [API Spec](tests/api/api.spec.ts)
- [Bagel Shop Home Spec](tests/bagel-shop/home.spec.ts)
- [Checkout Challenge Spec](tests/checkout/checkoutChallenge.spec.ts)
- [Auth Setup](tests/auth.setup.ts)

---

## How the Framework Works

1. **npm scripts start and run Playwright commands.**
   The `package.json` file defines scripts for local execution, Chromium-only execution, UI mode, reports, and local server testing.

2. **Playwright reads `playwright.config.ts`.**
   The config defines the test directory, reporters, browser projects, retries, traces, screenshots, video settings, web server startup, and environment variable loading.

3. **The web server starts automatically.**
   The config starts the app with `npm run start` on port `5173`, then runs tests after the app is available.

4. **Setup tests run first.**
   The `setup` project runs files matching `*.setup.ts`, including `tests/auth.setup.ts`.

5. **Authentication state is saved.**
   The auth setup logs in as a customer and saves session state to `.auth/customer01.json`.

6. **Specs use Playwright fixtures.**
   Tests use built-in fixtures such as `page`, `request`, `context`, `headless`, and `isMobile`.

7. **Custom fixtures provide page objects and console validation.**
   The `lib/fixtures` folder extends Playwright's base test object with reusable page objects and console-message tracking.

8. **Tests execute actions and assertions.**
   Specs use locators, actions, and Playwright `expect()` assertions to validate UI and API behavior.

9. **Artifacts are captured for debugging.**
   Traces, screenshots, and videos are configured to help diagnose failures.

---

## Important Scripts

These scripts are defined in `package.json`:

| Script | Command | Purpose |
|---|---|---|
| `npm run start` | `cd bagel-shop && npm run start` | Starts the local bagel-shop application |
| `npm run install:bagel-shop` | `cd bagel-shop && npm install` | Installs dependencies for the sample app |
| `npm test` | `npx playwright test` | Runs the full Playwright test suite |
| `npm run test:chromium` | `npx playwright test --project chromium` | Runs tests only in the Chromium project |
| `npm run test:first` | `npx playwright test --grep @first` | Runs tests matching `@first` |
| `npm run test:local` | `BASE_URL=http://localhost:4200 npx playwright test` | Runs tests against a local base URL |
| `npm run test:report` | `npx playwright test && npx playwright show-report` | Runs tests and opens the HTML report |
| `npm run test:ui` | `npx playwright test --ui` | Opens Playwright UI mode |

---

## Playwright Configuration Summary

| Setting | Value / Purpose |
|---|---|
| `testDir` | Runs tests from `./tests` |
| `webServer` | Starts the app with `npm run start` on port `5173` |
| `timeout` | Sets each test timeout to 30 seconds |
| `globalTimeout` | Limits total run time to 10 minutes |
| `fullyParallel` | Allows tests to run in parallel |
| `forbidOnly` | Fails CI if `test.only` is committed |
| `retries` | Retries twice on CI and once locally |
| `workers` | Uses one worker on CI |
| `reporter` | Uses HTML and list reporters |
| `baseURL` | Reads from `process.env.URL` |
| `testIdAttribute` | Uses `data-test` for `getByTestId()` |
| `trace` | Captures traces with `on` |
| `video` | Retains video on failure |
| `screenshot` | Captures screenshots only on failure |
| `headless` | Runs headless by default |
| `projects` | Includes setup, Chromium, and Mobile Safari |

The config also extends Playwright `expect` with a custom `toBeNumber()` matcher.

---

## Core Functions and Patterns

| Function / Pattern | Purpose |
|---|---|
| `test()` | Defines a Playwright test case |
| `test.describe()` | Groups related tests |
| `test.beforeEach()` | Runs setup before each test in a group |
| `test.use()` | Overrides fixture options such as `storageState` |
| `page.goto()` | Navigates to a URL or relative route |
| `page.getByRole()` | Finds elements by accessible role and name |
| `page.getByTestId()` | Finds elements by `data-test` attribute |
| `page.getByText()` | Finds visible text on the page |
| `page.waitForEvent()` | Waits for browser events such as popup creation |
| `expect(locator).toBeVisible()` | Validates visibility |
| `expect(locator).toHaveText()` | Validates exact text |
| `expect(locator).toContainText()` | Validates partial text |
| `expect(locator).toBeDisabled()` | Validates disabled state |
| `expect(page).toHaveScreenshot()` | Performs visual screenshot comparison |
| `request.get()` | Sends an API GET request |
| `request.post()` | Sends an API POST request |
| `context.storageState()` | Saves browser authentication/session state |
| `baseTest.extend()` | Creates custom fixtures |
| `expect.extend()` | Creates custom matchers |
| `mergeTests()` | Combines fixture test objects |
| `mergeExpects()` | Combines custom expect objects |

---

# Master Test Automation with Playwright

## Learning Playwright

### Module Details

The Playwright learning path begins with understanding why Playwright is used for modern automation. Playwright supports Chromium, Firefox, and WebKit, includes a powerful test runner, supports web-first assertions, has built-in API testing, supports tracing, screenshots, videos, mobile emulation, fixtures, parallel execution, and CI/CD integration.

The learning path also compares Playwright with other tools such as Cypress. Playwright is especially strong when teams need cross-browser coverage, auto-waiting, multi-tab workflows, API and UI testing in one framework, browser contexts, storage state reuse, and flexible CI execution.

### Code Example: Minimal First Test

```ts
import { test, expect } from '@playwright/test';

test('home page loads', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveTitle(/Practice Software Testing|Bagel/i);
});
```

### Expected Result

Playwright launches a browser, opens the configured base URL, waits for the page to load, and verifies that the page title matches the expected pattern.

### Best Practices

- Start with small, focused tests.
- Use Playwright's auto-waiting locators instead of hard waits.
- Use `baseURL` in `playwright.config.ts` so tests can navigate with relative paths.
- Use TypeScript to catch mistakes earlier.

### Common Interview Questions

1. **What is Playwright?**
   Playwright is a browser automation and testing framework that supports Chromium, Firefox, and WebKit with a built-in test runner, fixtures, assertions, tracing, and API testing.

2. **Why choose Playwright over Selenium or Cypress?**
   Playwright provides strong cross-browser support, reliable auto-waiting, browser contexts, API testing, tracing, multi-tab support, and first-class TypeScript integration.

---

## 1. Playwright Installation and Configuration

### Module Details

This module covers installing Playwright from scratch, adding Playwright to an existing front-end application, configuring browsers/projects, using the test runner CLI, understanding `package.json`, using the VS Code extension, and running tests in UI Mode.

### Install Playwright as a New Independent Project

```bash
mkdir playwright-typescript-demo
cd playwright-typescript-demo
npm init -y
npm init playwright@latest
npx playwright install
npx playwright test
npx playwright show-report
```

### Add Playwright to an Existing Front-End Project

```bash
npm install -D @playwright/test
npx playwright install
mkdir tests
```

```ts
// tests/home.spec.ts
import { test, expect } from '@playwright/test';

test('existing app home page loads', async ({ page }) => {
  await page.goto('http://localhost:5173');
  await expect(page.locator('body')).toBeVisible();
});
```

### Playwright Config with Browsers and Projects

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

### Useful CLI Commands

```bash
npx playwright test
npx playwright test --headed
npx playwright test --debug
npx playwright test --ui
npx playwright test tests/homepage/home.spec.ts
npx playwright test -g "home page loads"
npx playwright test --project chromium
npx playwright show-report
```

### Expected Result

Playwright installs successfully, browser binaries are available, the config controls browser projects, and tests can be launched from CLI, UI Mode, or the VS Code extension.

### Best Practices

- Commit `playwright.config.ts`, `package.json`, and test specs.
- Do not commit generated reports, videos, traces, screenshots, or local auth files unless intentionally using sanitized examples.
- Use npm scripts to standardize commands for the team.
- Use project names that clearly describe browser/device coverage.

### Common Interview Questions

1. **What command installs Playwright?**
   `npm init playwright@latest` for a new setup or `npm install -D @playwright/test` for an existing project.

2. **What is the purpose of `playwright.config.ts`?**
   It centralizes test directories, timeouts, browser projects, reporters, base URL, retries, artifacts, and web server behavior.

---

## 2. Creating Tests with Playwright

### Module Details

This module covers the website under test, generating tests with Codegen, locator strategies, assertions, structuring Playwright tests, cookie authentication, visual testing, API testing, and deciding what should be automated.

### Test Structure

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

### Locator Strategy Examples

```ts
await page.getByRole('button', { name: 'Login' }).click();
await page.getByLabel('Email').fill('customer@example.com');
await page.getByPlaceholder('Search products').fill('hammer');
await page.getByText('Payment was successful').isVisible();
await page.getByTestId('checkout-submit').click();
await page.locator('css=.product-card').first().click();
await page.locator('//button[contains(., "Submit")]').click();
```

### Assertion Types

```ts
await expect(page.getByRole('heading', { name: 'Products' })).toBeVisible();
await expect(page.getByTestId('cart-quantity')).toHaveText('1');
await expect(page).toHaveURL(/checkout/);
expect(200).toBe(200);
expect.soft(await page.getByTestId('message').textContent()).toContain('success');
```

### Cookie Authentication Example

```ts
test('uses cookie authentication', async ({ context, page }) => {
  await context.addCookies([
    {
      name: 'session',
      value: 'demo-session-token',
      domain: 'localhost',
      path: '/'
    }
  ]);

  await page.goto('/account');
  await expect(page.getByTestId('account-dashboard')).toBeVisible();
});
```

### API Test Example

```ts
test('validates products API', async ({ request }) => {
  const response = await request.get(`${process.env.API_URL}/products`);
  expect(response.status()).toBe(200);

  const body = await response.json();
  expect(body.data.length).toBeGreaterThan(0);
});
```

### Expected Result

The tests are grouped, readable, and cover UI, API, authentication, assertions, and locator best practices.

### Best Practices

- Use user-facing locators first: role, label, text, placeholder.
- Use `data-test` attributes for stable automation-specific hooks.
- Avoid automating implementation details when user-facing behavior can be validated.
- Use `test.describe()` to group related workflows.
- Use `test.beforeEach()` for repeated setup that must happen before every test.

### Common Interview Questions

1. **What is auto-waiting in Playwright?**
   Playwright waits for elements to be actionable before interacting and waits for assertions to pass until timeout.

2. **What should you automate?**
   Prioritize high-value, repeatable, business-critical flows such as login, checkout, forms, API workflows, and smoke/regression checks.

---

## 3. Maintaining Playwright Tests

### Module Details

This module covers test maintenance through screenshots, videos, reporters, trace viewer, scaling strategies, and debugging. Maintaining tests means making failures actionable, reducing false failures, keeping tests readable, and designing framework code that can evolve with the application.

### Screenshots, Videos, and Reporter Config

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

### Trace Viewer Commands

```bash
npx playwright test --trace on
npx playwright show-report
npx playwright show-trace test-results/path-to-trace.zip
```

### Scaling Tests

```ts
export default defineConfig({
  fullyParallel: true,
  workers: process.env.CI ? 2 : undefined,
  retries: process.env.CI ? 2 : 0,
  shard: undefined
});
```

### Expected Result

Failed tests produce enough evidence to diagnose problems through HTML reports, traces, screenshots, and videos.

### Best Practices

- Review traces before changing test code.
- Keep tests independent and parallel-safe.
- Use retries to reveal flakiness, not hide defects.
- Keep setup code reusable through fixtures and page objects.

### Common Interview Questions

1. **What is the Playwright trace viewer?**
   A debugging tool that shows actions, DOM snapshots, console logs, network calls, screenshots, and timing information.

2. **How do you reduce maintenance in Playwright tests?**
   Use stable locators, page objects, fixtures, test data factories, and meaningful assertions.

---

## Playwright Essential Training: Abstractions, Fixtures, and Complex Scenarios

This section expands Playwright into maintainable framework engineering: abstractions, page objects, helpers, fixtures, API mocking, browser emulation, configuration customization, and complex interactions such as popups, alerts, uploads, downloads, and challenging UI elements.

---

## 4. Abstractions in Playwright

### Module Details

Abstractions reduce duplication but can also hide behavior if overused. The goal is to create helpful abstractions such as page objects, data factories, helpers, and custom assertions while keeping tests readable.

### Page Object Example

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

### Data Factory Example

```ts
import { faker } from '@faker-js/faker';

export function createContactMessage() {
  return {
    name: faker.person.fullName(),
    email: faker.internet.email().toLowerCase(),
    message: faker.lorem.sentence()
  };
}
```

### Helper Function Example

```ts
export async function clearAndFill(locator: Locator, value: string) {
  await locator.clear();
  await locator.fill(value);
}
```

### Custom Assertion Example

```ts
import { expect as baseExpect } from '@playwright/test';

export const expect = baseExpect.extend({
  async toBeNumber(received: unknown) {
    const pass = typeof received === 'number' && !Number.isNaN(received);
    return {
      pass,
      message: () => `expected ${received} to be a valid number`
    };
  }
});
```

### Expected Result

Tests become shorter and easier to maintain because common actions, test data, and checks are centralized.

### Best Practices

- Abstract repeated behavior, not one-off actions.
- Keep page object methods user-focused.
- Keep assertions in tests when they communicate the test purpose.
- Use TypeScript types for fixtures, data factories, and helpers.

### Common Interview Questions

1. **What are the risks of abstraction?**
   Over-abstraction can make tests harder to read, hide important behavior, and make debugging more difficult.

2. **What belongs in a page object?**
   Page-specific locators and user actions such as login, navigate, add to cart, submit form, or verify page state.

---

## 5. Fixtures in Playwright

### Module Details

Fixtures provide dependency injection for tests. Playwright includes built-in fixtures such as `page`, `context`, `browser`, `request`, `browserName`, `isMobile`, and `headless`. Custom fixtures can provide page objects, data factories, API clients, authenticated states, and console tracking.

### Custom Fixture Example

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

### Advanced Fixture with Setup and Teardown

```ts
type TestData = {
  userEmail: string;
};

export const test = base.extend<TestData>({
  userEmail: async ({}, use) => {
    const email = `user-${Date.now()}@example.com`;
    await use(email);
    // teardown cleanup could happen here
  }
});
```

### Expected Result

Tests receive reusable, typed dependencies directly in the test argument list.

### Best Practices

- Use fixtures instead of repeated `beforeEach()` setup when the setup represents a reusable dependency.
- Use worker-scoped fixtures for expensive setup shared by tests in a worker.
- Use test-scoped fixtures for isolated data or page objects.
- Name fixtures clearly.

### Common Interview Questions

1. **What is a fixture in Playwright?**
   A reusable dependency that Playwright sets up and tears down for tests.

2. **What is the difference between test-scoped and worker-scoped fixtures?**
   Test-scoped fixtures run per test; worker-scoped fixtures run once per worker process.

---

## 6. Mocking and Emulation in Playwright

### Module Details

This module covers network routing, request interception, response mocking, browser emulation, localization, geolocation, and JavaScript injection. Mocking is useful when external dependencies are slow, unstable, costly, or difficult to configure for every test.

### Intercept HTTP Requests

```ts
test('intercepts product API request', async ({ page }) => {
  await page.route('**/api/products', async route => {
    console.log('Intercepted:', route.request().url());
    await route.continue();
  });

  await page.goto('/products');
});
```

### Mock HTTP Response

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

### Modify API Response

```ts
test('modifies API response', async ({ page }) => {
  await page.route('**/api/products', async route => {
    const response = await route.fetch();
    const json = await response.json();
    json.data[0].name = 'Modified Product';

    await route.fulfill({ response, json });
  });

  await page.goto('/products');
  await expect(page.getByText('Modified Product')).toBeVisible();
});
```

### Browser Emulation, Localization, and Geolocation

```ts
test.use({
  locale: 'en-US',
  timezoneId: 'America/New_York',
  geolocation: { latitude: 27.9506, longitude: -82.4572 },
  permissions: ['geolocation']
});

test('uses geolocation', async ({ page }) => {
  await page.goto('/stores');
  await page.getByRole('button', { name: 'Use my location' }).click();
  await expect(page.getByText(/nearest store/i)).toBeVisible();
});
```

### Inject JavaScript

```ts
test('injects feature flag before page load', async ({ page }) => {
  await page.addInitScript(() => {
    window.localStorage.setItem('feature.checkoutV2', 'true');
  });

  await page.goto('/checkout');
  await expect(page.getByText('New checkout')).toBeVisible();
});
```

### Expected Result

Tests can isolate frontend behavior from unstable dependencies, simulate locations/locales/devices, and prepare browser state before the app loads.

### Best Practices

- Mock only when the real dependency is not the purpose of the test.
- Keep mock payloads realistic.
- Store large mock JSON files separately.
- Use API tests to validate backend behavior separately from mocked UI tests.

### Common Interview Questions

1. **What is `page.route()` used for?**
   It intercepts network requests so tests can continue, abort, modify, or fulfill them with mock responses.

2. **When should you mock APIs?**
   When testing frontend behavior independently from backend availability, when third-party services are unstable, or when edge cases are hard to create with real data.

---

## 7. Customizing the Playwright Config

### Module Details

This module covers running a web server during tests, optimizing workers and sharding, retrying tests, and configuring timeouts. The Playwright config should encode project-wide defaults while allowing test-level overrides when necessary.

### Web Server Config

```ts
export default defineConfig({
  webServer: {
    command: 'npm run start',
    url: 'http://localhost:5173',
    reuseExistingServer: !process.env.CI,
    timeout: 120_000
  }
});
```

### Workers, Sharding, Retries, and Timeouts

```ts
export default defineConfig({
  workers: process.env.CI ? 2 : undefined,
  retries: process.env.CI ? 2 : 0,
  timeout: 30_000,
  globalTimeout: 10 * 60 * 1000,
  expect: { timeout: 5_000 },
  use: {
    actionTimeout: 10_000,
    navigationTimeout: 20_000
  }
});
```

### Test-Level Timeout Override

```ts
test('slow report page loads', async ({ page }) => {
  test.setTimeout(60_000);
  await page.goto('/reports/monthly');
  await expect(page.getByRole('heading', { name: 'Monthly Report' })).toBeVisible();
});
```

### Expected Result

The framework runs consistently locally and in CI, starts the app automatically, handles expected slowness through controlled timeouts, and reruns flaky failures based on retry policy.

### Best Practices

- Keep global timeouts reasonable.
- Prefer fixing slow tests over increasing all timeouts.
- Use retries only in CI or for known temporary instability.
- Use sharding for large suites.

### Common Interview Questions

1. **What is `webServer` in Playwright config?**
   It starts a local application server before tests run and waits until the configured URL is available.

2. **What is sharding?**
   Splitting a test suite into separate parts so multiple CI jobs can run different portions in parallel.

---

## 8. Handling Complex Scenarios and Interactions in Playwright

### Module Details

This module covers multiple windows, popups, alerts, dialogs, uploads, downloads, difficult controls, web tables, date pickers, sliders, drag and drop, and iframes.

### Multiple Windows and Popups

```ts
test('opens promo popup', async ({ page }) => {
  await page.goto('/');

  const popupPromise = page.waitForEvent('popup');
  await page.getByRole('button', { name: 'Get Promo Code' }).click();
  const popup = await popupPromise;

  await expect(popup.getByText('The promo code is:')).toBeVisible();
});
```

### Alerts and Dialogs

```ts
test('accepts browser dialog', async ({ page }) => {
  page.on('dialog', async dialog => {
    expect(dialog.message()).toContain('Are you sure');
    await dialog.accept();
  });

  await page.goto('/account');
  await page.getByRole('button', { name: 'Delete' }).click();
});
```

### Uploads and Downloads

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

### Web Table Example

```ts
test('finds table row by cell text', async ({ page }) => {
  await page.goto('/orders');

  const row = page.getByRole('row').filter({ hasText: 'Order #1001' });
  await expect(row.getByRole('cell').nth(2)).toHaveText('Paid');
  await row.getByRole('button', { name: 'View' }).click();
});
```

### Date Picker Example

```ts
test('selects future date', async ({ page }) => {
  await page.goto('/schedule');

  const futureDate = new Date();
  futureDate.setDate(futureDate.getDate() + 7);
  const day = String(futureDate.getDate());

  await page.getByLabel('Date').click();
  await page.getByRole('button', { name: day, exact: true }).click();
  await expect(page.getByLabel('Date')).not.toHaveValue('');
});
```

### Slider Example

```ts
test('moves slider', async ({ page }) => {
  await page.goto('/filters');

  const slider = page.getByRole('slider', { name: 'Price' });
  await slider.focus();
  await page.keyboard.press('ArrowRight');
  await page.keyboard.press('ArrowRight');

  await expect(slider).toHaveAttribute('aria-valuenow', /\d+/);
});
```

### Drag and Drop with iframe

```ts
test('drag and drop inside iframe', async ({ page }) => {
  await page.goto('/drag-drop');

  const frame = page.frameLocator('iframe[data-test="demo-frame"]');
  await frame.getByText('Drag me').dragTo(frame.getByText('Drop here'));

  await expect(frame.getByText('Dropped')).toBeVisible();
});
```

### Expected Result

Complex UI workflows are automated reliably by waiting for browser events before triggering actions, using correct frame locators, and asserting results after interaction.

### Best Practices

- Start waiting for popups/downloads before clicking the trigger.
- Use `frameLocator()` for iframe content.
- Prefer keyboard interactions for accessible sliders when possible.
- Validate final UI state after complex interactions.

### Common Interview Questions

1. **How do you handle a popup in Playwright?**
   Start `page.waitForEvent('popup')` before clicking the element that opens the popup, then interact with the returned popup page.

2. **How do you interact with an iframe?**
   Use `page.frameLocator()` and locate elements within the frame.

---

## Playwright: Design Patterns

Design patterns make Playwright frameworks easier to maintain, scale, and explain in interviews. This section covers fixtures, Page Object Model, BDD, and data-driven testing.

---

## 9. Fixtures as a Design Pattern

### Module Details

Fixtures organize shared setup and dependencies. They can replace repetitive hooks and provide page objects, API clients, authenticated pages, generated data, and console monitoring.

### Automatic Fixture Example

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

Every test automatically captures console logs and attaches them to the report.

### Best Practices

- Use automatic fixtures for cross-cutting concerns such as logs.
- Keep fixtures strongly typed.
- Avoid putting unrelated setup into one large fixture.

### Common Interview Question

**Why use fixtures instead of hooks?**
Fixtures are reusable, typed, composable, and can provide dependencies directly to tests. Hooks are better for simple setup inside a test suite.

---

## 10. Page Object Model

### Module Details

The Page Object Model encapsulates page-specific locators and actions into classes. A good Playwright POM should expose business-level actions and keep tests readable.

### Recommended Architecture

```text
lib/pages/
├── base.page.ts
├── login/login.page.ts
├── account/account.page.ts
├── contact/contact.page.ts
└── checkout/checkout.page.ts
```

### Base Page Example

```ts
import { Page } from '@playwright/test';

export class BasePage {
  constructor(protected readonly page: Page) {}

  async goto(path = '/') {
    await this.page.goto(path);
  }

  async waitForAppReady() {
    await this.page.locator('body').waitFor({ state: 'visible' });
  }
}
```

### Page Manager Example

```ts
export class PageManager {
  readonly loginPage: LoginPage;
  readonly contactPage: ContactPage;

  constructor(page: Page) {
    this.loginPage = new LoginPage(page);
    this.contactPage = new ContactPage(page);
  }
}
```

### Expected Result

Tests import page objects or receive them through fixtures, reducing duplicate locator and workflow code.

### Best Practices

- Keep locators close to the page they belong to.
- Prefer method names based on user behavior.
- Do not turn page objects into assertion dumping grounds.
- Use helper base classes for common navigation and utility methods.

### Common Interview Questions

1. **What is Page Object Model?**
   A design pattern that encapsulates page locators and actions in reusable classes.

2. **Why combine POM with fixtures?**
   Fixtures inject ready-to-use page objects into tests, reducing setup repetition.

---

## 11. Behavior Driven Development BDD

### Module Details

BDD expresses behavior in business-readable scenarios. Playwright can be combined with Cucumber to map Gherkin steps to TypeScript step definitions. BDD is useful when non-technical stakeholders review acceptance criteria, but it adds overhead if the team does not use the scenarios actively.

### Gherkin Scenario

```gherkin
Feature: Login

  Scenario: Valid customer can log in
    Given the customer is on the login page
    When the customer signs in with valid credentials
    Then the account dashboard should be displayed
```

### Step Definition Example

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

A readable BDD scenario maps to executable Playwright automation.

### Best Practices

- Use BDD when scenarios are truly shared with business stakeholders.
- Keep step definitions reusable but not overly generic.
- Combine BDD with Page Objects to avoid step definition duplication.

### Common Interview Question

**When would you avoid BDD?**
Avoid BDD when it adds ceremony without stakeholder collaboration or when normal Playwright specs are clearer and faster to maintain.

---

## 12. Data-Driven Testing

### Module Details

Data-driven testing runs the same test logic against multiple inputs. This is useful for form validation, role testing, product filters, checkout variations, and API status checks.

### Inline Data Example

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

The same test behavior executes for each data row and validates the matching expected message.

### Best Practices

- Keep data close to the test when small.
- Move large data sets into JSON, factories, or test data builders.
- Use descriptive test names that include the input scenario.

### Common Interview Question

**How do you implement data-driven testing in Playwright?**
Use arrays, JSON files, factories, or fixtures, then loop over the cases and create tests dynamically.

---

## Advanced Playwright Techniques

This section covers speed, stability, screenshots, snapshots, cloud testing, Docker, GitHub Actions, and visual validation.

---

## 13. Optimising Test Speed in Playwright

### Module Details

Optimizing speed includes avoiding repeated UI logins, using `storageState`, running tests in parallel where safe, splitting suites by tags, reducing unnecessary waits, and diagnosing slow tests with traces and timing information.

### Storage State to Avoid Repeated Logins

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

### Project Dependency for Setup

```ts
export default defineConfig({
  projects: [
    { name: 'setup', testMatch: /.*\.setup\.ts/ },
    {
      name: 'chromium',
      dependencies: ['setup'],
      use: { storageState: '.auth/customer01.json' }
    }
  ]
});
```

### Expected Result

Login runs once in the setup project, then tests reuse the stored session, reducing suite execution time.

### Best Practices

- Use API login or storage state for repeated authenticated flows.
- Keep tests independent before enabling full parallelization.
- Use tags to run only the needed suite.
- Do not optimize by removing important assertions.

### Common Interview Question

**How do you speed up Playwright tests?**
Reuse authentication state, run independent tests in parallel, reduce unnecessary UI setup, use API setup for data, avoid hard waits, and shard large suites in CI.

---

## 14. Reducing Test Flakiness in Playwright

### Module Details

Flaky tests pass and fail inconsistently. Common causes include unstable locators, hard waits, external dependencies, animations, network timing, hydration issues, shared state, and non-isolated test data.

### Stable Locator Example

```ts
// Brittle
await page.locator('.btn:nth-child(3)').click();

// Better
await page.getByRole('button', { name: 'Checkout' }).click();

// Also good for stable app-specific hooks
await page.getByTestId('checkout-submit').click();
```

### Wait for Hydration / App Ready

```ts
await page.goto('/');
await expect(page.getByTestId('app-ready')).toHaveAttribute('data-ready', 'true');
await page.getByRole('button', { name: 'Submit' }).click();
```

### Detect Flakiness by Repetition

```bash
npx playwright test tests/checkout/checkoutChallenge.spec.ts --repeat-each=10
```

### Expected Result

Flaky tests become diagnosable and can be stabilized through better locators, isolated data, proper waits, and mock control of external dependencies.

### Best Practices

- Avoid `waitForTimeout()`.
- Use web-first assertions.
- Mock unstable external services when not under test.
- Run suspect tests repeatedly to confirm flakiness.

### Common Interview Question

**What causes flaky Playwright tests?**
Unstable locators, race conditions, hard waits, shared data, asynchronous rendering, external service instability, and environment differences.

---

## 15. Screenshot and Snapshot Testing Best Practices

### Module Details

Screenshot and snapshot testing detect visual and structural regressions. Screenshot tests compare image output. Snapshot tests can compare text, accessibility snapshots, API payloads, or other serializable output.

### Page Screenshot

```ts
test('homepage screenshot', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveScreenshot('home-page.png', {
    fullPage: true,
    maxDiffPixelRatio: 0.01
  });
});
```

### Component Screenshot

```ts
test('cart item screenshot', async ({ page }) => {
  await page.goto('/cart');
  await expect(page.getByTestId('cart-summary')).toHaveScreenshot('cart-summary.png');
});
```

### Text Snapshot

```ts
test('api response snapshot', async ({ request }) => {
  const response = await request.get(`${process.env.API_URL}/products`);
  const body = await response.json();
  expect(JSON.stringify(body.data[0], null, 2)).toMatchSnapshot('first-product.json');
});
```

### Update Snapshots

```bash
npx playwright test --update-snapshots
```

### Expected Result

The test fails when the visual or textual output differs from the approved baseline.

### Best Practices

- Review diffs before updating snapshots.
- Mask dynamic values.
- Use consistent viewport and fonts.
- Prefer component-level screenshots for stability.

### Common Interview Question

**When should visual tests be used?**
Use them for important UI components, layout-critical pages, checkout flows, marketing pages, and areas where visual regressions are costly.

---

## 16. Running Tests on Microsoft Playwright Testing Service

### Module Details

Cloud testing services scale browser execution beyond a local machine. Microsoft Playwright Testing Service can run tests in cloud-hosted browsers and help teams scale parallel execution across environments.

### Example Cloud-Oriented Command Pattern

```bash
npx playwright test --config=playwright.service.config.ts
```

### Example Separate Cloud Config

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

### Expected Result

The same tests run against a scalable remote browser environment rather than only local browser engines.

### Best Practices

- Keep cloud config separate from local config when needed.
- Start with smoke tests before moving full regression to cloud.
- Upload reports and artifacts.
- Keep secrets in CI or cloud secret stores.

### Common Interview Question

**Why use a cloud testing service?**
To scale browser execution, improve cross-environment coverage, and reduce local infrastructure constraints.

---

## 17. Complete Supplemental Playwright Topics

### JavaScript Fundamentals for Beginners

Playwright TypeScript tests depend on JavaScript fundamentals: variables, constants, objects, arrays, conditions, loops, functions, classes, methods, imports, exports, async/await, and TypeScript types.

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

**Expected Result:** The console prints the product names and a formatted product string.

### HTML Terminology and Locator Syntax

HTML elements are made of tags, attributes, text, parent/child relationships, classes, IDs, and accessible names. Playwright locators should target user-visible behavior first.

```html
<button id="submit" class="primary" data-test="checkout-submit">Submit Order</button>
```

```ts
await page.getByRole('button', { name: 'Submit Order' }).click();
await page.getByTestId('checkout-submit').click();
await page.locator('#submit').click();
await page.locator('.primary').click();
```

### Auto-Waiting and Timeout Hierarchy

Playwright automatically waits for elements to be actionable. Timeout levels include global timeout, test timeout, action timeout, navigation timeout, and expect timeout.

```ts
export default defineConfig({
  globalTimeout: 10 * 60 * 1000,
  timeout: 30_000,
  expect: { timeout: 5_000 },
  use: {
    actionTimeout: 10_000,
    navigationTimeout: 20_000
  }
});
```

### UI Components

```ts
await page.getByLabel('First name').fill('Brian');
await page.getByLabel('Subscribe').check();
await page.getByLabel('Standard shipping').check();
await page.getByLabel('State').selectOption('FL');
await page.getByRole('button', { name: 'Save' }).click();
await expect(page.getByText('Saved')).toBeVisible();
```

### API Mocking, API Requests, and Shared Auth State

```ts
await page.route('**/api/cart', route => route.fulfill({
  status: 200,
  contentType: 'application/json',
  body: JSON.stringify({ items: [] })
}));

const login = await request.post(`${process.env.API_URL}/users/login`, {
  data: { email: process.env.CUSTOMER_EMAIL, password: process.env.CUSTOMER_PASSWORD }
});
expect(login.ok()).toBeTruthy();
```

### Global Setup and Teardown

```ts
// global-setup.ts
import { FullConfig } from '@playwright/test';

async function globalSetup(config: FullConfig) {
  console.log('Global setup before all projects');
}

export default globalSetup;
```

```ts
// playwright.config.ts
export default defineConfig({
  globalSetup: './global-setup.ts'
});
```

### Test Tags

```ts
test('checkout smoke @smoke', async ({ page }) => {
  await page.goto('/checkout');
  await expect(page.getByRole('heading', { name: 'Checkout' })).toBeVisible();
});
```

```bash
npx playwright test --grep @smoke
npx playwright test --grep-invert @slow
```

### Docker Container Execution

```dockerfile
FROM mcr.microsoft.com/playwright:v1.55.0-jammy
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npx playwright install --with-deps
CMD ["npx", "playwright", "test"]
```

```yaml
services:
  tests:
    build: .
    volumes:
      - ./playwright-report:/app/playwright-report
      - ./test-results:/app/test-results
```

### GitHub Actions and Visual CI

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

The supplemental topics complete the framework coverage by adding beginner JavaScript, HTML/DOM locator knowledge, timeout concepts, UI component automation, API control, global setup, tags, Docker, and CI execution.

---

## Sample Code from This Project

### Basic UI Test

```ts
import { test, expect } from "@playwright/test";

test("Validate promo code popup", async ({ page }) => {
  await page.goto("http://localhost:5173/");

  const popupPromise = page.waitForEvent("popup");
  await page.getByRole("button", { name: "Get Promo Code" }).click();
  const popup = await popupPromise;

  await expect(popup.getByText("The promo code is:")).toBeVisible();
});
```

**How it works:**

- Opens the local app.
- Starts waiting for a popup before clicking the button.
- Clicks the promo code button.
- Captures the popup page object.
- Verifies expected text is visible inside the popup.

---

### API Test

```ts
import { test, expect } from "@playwright/test";

test("GET /products", async ({ request }) => {
  const apiUrl = process.env.API_URL;
  const response = await request.get(apiUrl + "/products");

  expect(response.status()).toBe(200);
  const body = await response.json();
  expect(body.data.length).toBe(9);
  expect(body.total).toBe(50);
});

test("POST /users/login", async ({ request }) => {
  const apiUrl = process.env.API_URL;
  const response = await request.post(apiUrl + "/users/login", {
    data: {
      email: "customer@practicesoftwaretesting.com",
      password: "welcome01",
    },
  });

  expect(response.status()).toBe(200);
  const body = await response.json();
  expect(body.access_token).toBeTruthy();
});
```

**How it works:**

- Uses Playwright's built-in `request` fixture.
- Sends API calls without opening a browser page.
- Validates HTTP status codes.
- Parses JSON response bodies.
- Confirms expected response fields and values.

---

### Page Object Model Login Page

```ts
import { type Locator, type Page } from "@playwright/test";

export class LoginPage {
  readonly page: Page;
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly loginButton: Locator;

  constructor(page: Page) {
    this.page = page;
    this.emailInput = page.getByTestId("email");
    this.passwordInput = page.getByTestId("password");
    this.loginButton = page.getByTestId("login-submit");
  }

  async goto() {
    await this.page.goto("/auth/login");
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }
}
```

**How it works:**

- Stores all login page locators in one class.
- Uses `readonly` properties for stable page elements.
- Provides a `goto()` method for navigation.
- Provides a `login()` method for reusable login behavior.
- Keeps test specs cleaner and easier to maintain.

---

### Authentication Storage State Setup

```ts
import { test as setup, expect } from "@playwright/test";
import { LoginPage } from "../lib/pages/login/login.page";

setup("Create customer 01 auth", async ({ page, context }) => {
  const email = "customer@practicesoftwaretesting.com";
  const password = "welcome01";
  const customer01AuthFile = ".auth/customer01.json";

  const loginPage = new LoginPage(page);

  await loginPage.goto();
  await loginPage.login(email, password);

  await expect(page.getByTestId("nav-menu")).toContainText("Jane Doe");
  await context.storageState({ path: customer01AuthFile });
});
```

**How it works:**

- Logs in once during setup.
- Confirms the logged-in user is visible.
- Saves cookies/local storage/session state to `.auth/customer01.json`.
- Other tests can reuse the saved login state with `test.use()`.

---

### Custom Page Fixtures

```ts
import { AccountPage } from "@pages/account/account.page";
import { LoginPage } from "@pages/login/login.page";
import { MessagesPage } from "@pages/account/messages.page";
import { ContactPage } from "@pages/contact/contact.page";
import { test as baseTest } from "@playwright/test";

type MyPages = {
  loginPage: LoginPage;
  accountPage: AccountPage;
  messagesPage: MessagesPage;
  contactPage: ContactPage;
};

export const test = baseTest.extend<MyPages>({
  loginPage: async ({ page }, use) => {
    await use(new LoginPage(page));
  },
  accountPage: async ({ page }, use) => {
    await use(new AccountPage(page));
  },
  messagesPage: async ({ page }, use) => {
    await use(new MessagesPage(page));
  },
  contactPage: async ({ page }, use) => {
    await use(new ContactPage(page));
  },
});

export { expect } from "@playwright/test";
```

**How it works:**

- Extends the default Playwright `test` fixture.
- Creates typed page object fixtures.
- Allows specs to request page objects directly in the test arguments.
- Reduces manual setup inside individual specs.

Example usage:

```ts
test("login with page fixture", async ({ loginPage }) => {
  await loginPage.goto();
  await loginPage.login("customer@practicesoftwaretesting.com", "welcome01");
});
```

---

### Console Error Fixture and Custom Matcher

```ts
import {
  expect as baseExpect,
  test as baseTest,
  type Page,
  type ConsoleMessage,
} from "@playwright/test";

class PageConsole {
  readonly messages: ConsoleMessage[] = [];

  constructor(page: Page) {
    page.on("console", (message) => this.messages.push(message));
  }
}

export const test = baseTest.extend<{ pageConsole: PageConsole }>({
  pageConsole: async ({ page }, use) => {
    const pageConsole = new PageConsole(page);
    await use(pageConsole);
  },
});

export const expect = baseExpect.extend({
  async toHaveNoConsoleErrors(pageConsole: PageConsole) {
    const errors = pageConsole.messages.filter(
      (message) => message.type() === "error"
    );
    const pass = errors.length === 0;

    return {
      message: () => `Errors: ${this.utils.stringify(errors.map((e) => e.text()))}\n`,
      pass,
      name: "toHaveNoConsoleErrors",
      actual: errors,
    };
  },
});
```

**How it works:**

- Listens for browser console messages.
- Stores console messages during the test.
- Adds a custom matcher named `toHaveNoConsoleErrors()`.
- Helps catch frontend JavaScript errors that may not be visible in the UI.

---

### Merged Fixtures

```ts
import { mergeExpects, mergeTests } from "@playwright/test";
import { test as pageTest, expect as pageExpect } from "@fixtures/pages.fixture";
import { test as consoleTest, expect as consoleExpect } from "@fixtures/console.fixture";

export const test = mergeTests(pageTest, consoleTest);
export const expect = mergeExpects(pageExpect, consoleExpect);
```

**How it works:**

- Combines the page-object fixture and console fixture into one exported `test` object.
- Combines custom and base expectations into one exported `expect` object.
- Lets specs import from one base fixture file instead of many fixture files.

---

### Checkout End-to-End Test

```ts
test.describe("Checkout challenge", async () => {
  test.use({ storageState: ".auth/customer01.json" });

  test.beforeEach(async ({ page }) => {
    await page.goto("/");
  });

  test("buy now pay later", async ({ page, headless, isMobile }) => {
    await page.getByText("Claw Hammer with Shock Reduction Grip").click();
    await page.getByTestId("add-to-cart").click();
    await expect(page.getByTestId("cart-quantity")).toHaveText("1");

    if (isMobile === true) {
      await page.getByLabel("Toggle navigation").click();
    }

    await page.getByTestId("nav-cart").click();
    await page.getByTestId("proceed-1").click();
    await page.getByTestId("proceed-2").click();

    await page.getByTestId("street").fill("123 Testing Way");
    await page.getByTestId("city").fill("Sacramento");
    await page.getByTestId("country").fill("USA");
    await page.getByTestId("postal_code").fill("98765");

    await page.getByTestId("proceed-3").click();
    await expect(page.getByTestId("finish")).toBeDisabled();

    await page.getByTestId("payment-method").selectOption("Buy Now Pay Later");
    await page.getByTestId("monthly_installments").selectOption("6 Monthly Installments");
    await page.getByTestId("finish").click();

    await expect(page.locator(".help-block")).toHaveText("Payment was successful");
  });
});
```

**How it works:**

- Reuses saved customer authentication state.
- Starts each test at the home page.
- Adds a product to the cart.
- Handles mobile navigation differently when `isMobile` is true.
- Completes checkout fields.
- Selects a payment method.
- Validates successful payment text.

---

### Visual Screenshot Assertion

```ts
await test.step("visual test", async () => {
  await expect(page).toHaveScreenshot("checkout.png", {
    mask: [page.getByTitle("Practice Software Testing - Toolshop")],
  });
});
```

**How it works:**

- Captures a screenshot of the page.
- Compares it against the expected baseline.
- Masks dynamic content to reduce false failures.
- Helps catch unexpected visual regressions.

---

## Tutorial: How to Run This Project

### 1. Install Required Software

Install:

- Node.js 20 or higher
- VS Code
- VS Code Playwright extension
- Git

### 2. Clone the Repository

```bash
git clone https://github.com/BrianGator/Playwright-Test-Automation-w-TypeScript.git
cd Playwright-Test-Automation-w-TypeScript
```

### 3. Install Root Dependencies

```bash
npm install
```

### 4. Install Bagel Shop Dependencies

```bash
npm run install:bagel-shop
```

### 5. Install Playwright Browsers

```bash
npx playwright install
```

### 6. Create `.env` File

Create a `.env` file at the root if your tests require runtime URLs:

```env
URL=http://localhost:5173
API_URL=https://api.practicesoftwaretesting.com
```

Adjust values based on the target environment.

### 7. Run All Tests

```bash
npm test
```

### 8. Run Chromium Only

```bash
npm run test:chromium
```

### 9. Run in UI Mode

```bash
npm run test:ui
```

### 10. Show HTML Report

```bash
npm run test:report
```

---

## Best Practices and Tips

### Use `data-test` Attributes

This project configures Playwright to use `data-test` as the test ID attribute:

```ts
testIdAttribute: "data-test"
```

That means this locator:

```ts
page.getByTestId("login-submit")
```

matches this HTML:

```html
<button data-test="login-submit">Login</button>
```

### Avoid Hard Waits

Avoid:

```ts
await page.waitForTimeout(5000);
```

Prefer:

```ts
await expect(page.getByTestId("status")).toHaveText("Complete");
```

### Use `test.step()` for Readability

```ts
await test.step("Add product to cart", async () => {
  await page.getByTestId("add-to-cart").click();
  await expect(page.getByTestId("cart-quantity")).toHaveText("1");
});
```

### Keep Test Data Separate

Put reusable data in `lib/datafactory` or another data folder. Avoid hardcoding too many values directly in specs unless the values are part of the test purpose.

### Use Environment Variables

Do not hardcode environment-specific URLs or secrets. Use `.env` and `process.env`.

```ts
const apiUrl = process.env.API_URL;
```

### Use Mobile Conditions Carefully

The checkout test checks `isMobile` and opens the mobile menu only when needed. This is a good pattern for tests that run across desktop and mobile projects.

```ts
if (isMobile === true) {
  await page.getByLabel("Toggle navigation").click();
}
```

### Review Traces for Failures

When a test fails, open the trace to inspect:

- Actions
- Locators
- DOM snapshots
- Console logs
- Network calls
- Screenshots

```bash
npx playwright show-trace path/to/trace.zip
```

---

## Common Interview Questions

### What is Playwright?

Playwright is a modern browser automation framework that supports Chromium, Firefox, and WebKit. It includes a test runner, fixtures, assertions, trace viewer, screenshots, videos, API testing, mobile emulation, and parallel execution.

### What is the difference between `page` and `context`?

`page` represents a browser tab. `context` represents an isolated browser session with its own cookies, local storage, permissions, and pages.

### What are Playwright fixtures?

Fixtures are reusable dependencies injected into tests. Built-in fixtures include `page`, `context`, `browser`, and `request`. Custom fixtures can provide page objects, data, auth states, or helper utilities.

### What is storage state?

Storage state is a JSON snapshot of cookies and local/session storage that can be reused to start tests already authenticated.

### How do you handle flaky tests?

Use stable locators, avoid hard waits, isolate data, mock unstable dependencies when appropriate, use web-first assertions, inspect traces, and run suspect tests with `--repeat-each`.

### What is Page Object Model?

Page Object Model is a design pattern where page-specific locators and actions are encapsulated in classes, making tests cleaner and reducing duplicated locator code.

### How do you test APIs with Playwright?

Use the `request` fixture to send HTTP requests directly, validate status codes, parse JSON bodies, and set up data before UI tests.

### How do you run Playwright in CI?

Install dependencies, install browsers with `npx playwright install --with-deps`, run `npx playwright test`, and upload reports/artifacts.

---

## Troubleshooting

### Dependencies Are Missing

Run:

```bash
npm install
npm run install:bagel-shop
npx playwright install
```

### Local App Does Not Start

The Playwright config expects the web server to run on port `5173`. Confirm the app starts correctly:

```bash
npm run start
```

### `.env` Values Are Missing

If tests use `process.env.URL` or `process.env.API_URL`, create or update `.env`:

```env
URL=http://localhost:5173
API_URL=https://api.practicesoftwaretesting.com
```

### Authentication State Missing

If `.auth/customer01.json` is missing, run the setup project or the full test suite so `auth.setup.ts` can create the file.

```bash
npx playwright test --project setup
```

### Screenshot Test Fails

Visual tests can fail because of dynamic UI, font rendering, viewport differences, or changed application layout. Regenerate snapshots only after verifying the UI change is expected.

```bash
npx playwright test --update-snapshots
```

### Test Works Headed but Fails Headless

Check for timing, animations, viewport differences, and hidden elements. Use traces and screenshots to diagnose.

---

## Suggested Improvements

Recommended improvements for future versions:

1. Add a richer root-level framework diagram.
2. Add GitHub Actions workflow for CI execution.
3. Add badges for Playwright, TypeScript, Node, and CI status.
4. Add Allure reporting for portfolio-style reporting.
5. Add more negative checkout and API tests.
6. Add environment-specific config examples.
7. Add sample `.env.example` file.
8. Add more page object classes for each major feature.
9. Add reusable test data builders for checkout and account workflows.
10. Add accessibility testing with `@axe-core/playwright`.
11. Add Docker and docker-compose execution files.
12. Add BDD examples with Cucumber.
13. Add visual CI examples with Argos or similar tools.

---

## Author

**Written by Brian McCarthy**

This repository demonstrates practical TypeScript Playwright automation skills including UI testing, API testing, Page Object Model design, custom fixtures, authentication state reuse, browser/mobile projects, visual assertions, Docker execution, CI-ready configuration, and modern test automation design patterns.
