---
name: automated-test-generator
description: Dedicated skill to design, write, and execute robust automated test suites (unit, integration, and E2E) utilizing Vitest, Jest, Playwright, or Cypress. Emphasizes clean mocking and resilience against flakes.
---
# Automated Test Generator Specialist

This skill directs the agent on how to write comprehensive unit, integration, and end-to-end tests that verify functionality, prevent regressions, and run reliably in CI.

## Testing Strategy & Best Practices

### 1. Structure Tests with AAA (Arrange-Act-Assert)
- Formulate all test blocks following the AAA pattern to ensure readability and maintainability:
  - **Arrange:** Set up variables, mocks, and initial states.
  - **Act:** Execute the target function or trigger user interactions.
  - **Assert:** Validate that the actual outcome matches the expected outcome.
  ```javascript
  test('increments counter value when button is clicked', async () => {
    // Arrange
    render(<Counter />);
    const button = screen.getByRole('button', { name: /increment/i });
    
    // Act
    await userEvent.click(button);
    
    // Assert
    expect(screen.getByTestId('counter-value')).toHaveTextContent('1');
  });
  ```

### 2. Clean Mocking & Isolating Side Effects
- **Mock Service Worker (MSW):** Prefer network-level mocking via MSW rather than intercepting fetch libraries globally.
- **Isolate Module Mocks:** Mock external systems/third-party APIs cleanly at the top of the test file.
  ```javascript
  vi.mock('axios'); // For Vitest
  ```
- **Teardown Mocks:** Always clean up or reset mock histories in `afterEach` hooks to prevent leak-through between individual tests:
  ```javascript
  afterEach(() => {
    vi.clearAllMocks(); // Or jest.clearAllMocks()
  });
  ```

### 3. Resilient End-to-End (E2E) Tests (Playwright / Cypress)
- **Do Not Use Static Timeouts:** Avoid `page.waitForTimeout(3000)`. Instead, use dynamic wait selectors (e.g., `page.waitForSelector('.success-message')` or `expect(locator).toBeVisible()`).
- **Query via Accessible Roles:** Target interactive elements by their accessible role names rather than highly fragile CSS class selectors.
  ```javascript
  // Good:
  await page.getByRole('button', { name: 'Submit form' }).click();
  // Bad:
  await page.click('.btn-primary-active-state');
  ```
- **Visual & State Verification:** Leverage visual regression testing or capture screenshots/videos on failure to debug failures easily.

---

## Execution & Debugging Workflow

1. **Verify Local Executability:** Locate the workspace test runner scripts in `package.json` (e.g., `npm run test`, `npx vitest`, or `npx playwright test`).
2. **Execute Tests:** Run specific test suites or targets using the `run_command` tool.
3. **Parse Failure Logs:** Inspect compiler or runner stack traces. If an assertion fails, compare the diff (expected vs actual) to pinpoint code bugs or stale assertions.
4. **Fix Flakiness:** Watch out for asynchronous code executing outside test lifecycles, race conditions, or shared state modifications. Resolve by adding proper async/await boundaries or scoping variables.
5. **Coverage Auditing:** Run coverage reporting commands to locate untested branch conditions or logic paths.
