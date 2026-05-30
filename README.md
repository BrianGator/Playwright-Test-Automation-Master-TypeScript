# Playwright Test Automation with TypeScript

A practical end-to-end test automation project using **Playwright**, **TypeScript**, and **Node.js**. This repository demonstrates modern QA automation concepts including UI testing, API testing, Page Object Model design, custom fixtures, authentication storage state, visual testing, browser projects, mobile emulation, screenshots, videos, traces, reporters, and CI-ready Playwright configuration.

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
- [Guide: Building Playwright Automation in TypeScript](#guide-building-playwright-automation-in-typescript)
- [Best Practices and Tips](#best-practices-and-tips)
- [Troubleshooting](#troubleshooting)
- [Suggested Improvements](#suggested-improvements)
- [Author](#author)

---

## Project Overview

This project is a TypeScript Playwright automation framework built around a sample bagel/shop and practice testing application structure. It includes browser-based tests, API tests, reusable page objects, custom fixtures, storage-state authentication, test data helpers, and Playwright reporting/debugging features.

The project is useful for demonstrating practical QA automation skills such as:

- Creating browser automation tests with Playwright Test
- Writing readable TypeScript test specs
- Using Playwright locators such as `getByRole()`, `getByTestId()`, and `getByText()`
- Testing APIs with Playwright's built-in `request` fixture
- Saving authenticated browser sessions with `storageState`
- Using Page Object Model classes for maintainability
- Extending Playwright with custom fixtures
- Creating custom assertions/matchers
- Running tests against desktop and mobile browser projects
- Capturing screenshots, videos, and traces for debugging
- Using npm scripts for repeatable local execution

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

---

## Methodologies Used

### 1. End-to-End UI Automation

The UI tests automate real user workflows such as opening pages, clicking buttons, completing checkout, selecting payment methods, validating popups, and checking visible messages.

### 2. API Testing

The API specs use Playwright's `request` fixture to validate backend endpoints directly with GET and POST requests.

### 3. Page Object Model

The framework uses page object classes such as `LoginPage` to encapsulate locators and page actions. This reduces duplicate locator code and keeps specs focused on test behavior.

### 4. Fixture-Based Architecture

Custom fixtures provide reusable page objects and console monitoring. This allows tests to inject objects such as `loginPage`, `accountPage`, `messagesPage`, and `contactPage` instead of manually creating them in every spec.

### 5. Authentication State Reuse

The `auth.setup.ts` file logs in once and saves the authenticated session to `.auth/customer01.json`. Other tests can reuse this state instead of logging in repeatedly.

### 6. Data-Driven and Helper-Based Testing

Helper functions such as `randomState()` support dynamic data entry during checkout flows.

### 7. Visual Testing

The checkout test includes screenshot comparison logic using `toHaveScreenshot()` when the test runs in headless mode.

### 8. Cross-Project Browser Testing

The Playwright config defines a Chromium project and a Mobile Safari project using device emulation.

### 9. CI-Ready Execution

The configuration includes CI-friendly settings such as `forbidOnly`, retries, worker control, trace capture, videos, screenshots, and deterministic test directories.

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

The `playwright.config.ts` file includes several important framework settings:

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

## Guide: Building Playwright Automation in TypeScript

### Step 1: Write Small, Focused Tests

Good tests should follow Arrange, Act, Assert:

```ts
test("user can open contact page", async ({ page }) => {
  // Arrange
  await page.goto("/");

  // Act
  await page.getByRole("link", { name: "Contact" }).click();

  // Assert
  await expect(page).toHaveURL(/contact/);
});
```

### Step 2: Prefer Stable Locators

Recommended locator order:

1. `getByRole()` for accessible UI elements
2. `getByTestId()` for stable automation-specific attributes
3. `getByLabel()` for form fields
4. `getByText()` for visible copy
5. CSS locators only when needed

Example:

```ts
await page.getByRole("button", { name: "Submit" }).click();
await page.getByTestId("email").fill("customer@test.com");
```

### Step 3: Move Repeated Behavior into Page Objects

```ts
export class ContactPage {
  constructor(private page: Page) {}

  async goto() {
    await this.page.goto("/contact");
  }

  async submitMessage(message: string) {
    await this.page.getByTestId("message").fill(message);
    await this.page.getByRole("button", { name: "Send" }).click();
  }
}
```

### Step 4: Use Fixtures for Shared Objects

```ts
export const test = base.extend<{ contactPage: ContactPage }>({
  contactPage: async ({ page }, use) => {
    await use(new ContactPage(page));
  },
});
```

### Step 5: Use Storage State for Login

```ts
test.use({ storageState: ".auth/customer01.json" });
```

This keeps tests fast because they do not need to log in through the UI every time.

### Step 6: Add API Tests for Faster Coverage

```ts
test("API health check", async ({ request }) => {
  const response = await request.get(process.env.API_URL + "/products");
  expect(response.ok()).toBeTruthy();
});
```

### Step 7: Capture Debugging Evidence

Use Playwright artifacts:

```bash
npx playwright test --trace on --video retain-on-failure --screenshot only-on-failure
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

---

## Author

**Written by Brian McCarthy**

This repository demonstrates practical TypeScript Playwright automation skills including UI testing, API testing, Page Object Model design, custom fixtures, authentication state reuse, browser/mobile projects, visual assertions, and CI-ready test execution patterns.
