# UI lab results

Tool parts that I chose: Part 3 (Robot Framework) and Part 4 (Cypress)

## Part 0
- Step 0.1, Chrome version: 157.0.8103.51 (Official Build) (64-bit)
- Step 0.3, ChromeDriver version: ChromeDriver 157.0.8103.51
- Step 0.4, locator of Pizza, Please!: `button[name='pizza-please']` (CSS), or `//button[normalize-space()='Pizza, Please!']` (XPath)
- Step 0.4, locator of Excluded tools: `select[multiple]` (CSS), or `//label[normalize-space()='Excluded tools']/following::select[1]` (XPath)
- Step 0.4, locator of Must be vegetarian: `#default-checkbox` (CSS)
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name` (CSS)
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261116-2031, Pizza cutter, 550 calories per slice

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | The home page opened and the heading read Looking to break out of your pizza routine? | Pass |
| 2    | Max Calories per Slice, Excluded tools, Must be vegetarian and Custom Pizza Name appeared after turning on Advanced | Pass |
| 3    | The fields showed 600, Knife selected, Must be vegetarian ticked, and the name Lab Pizza 20261116-2031 | Pass |
| 4    | The answer showed Name: Lab Pizza 20261116-2031, Tool: Pizza cutter (not Knife), Calories per slice: 550 (600 or fewer) | Pass |
| 5    | After clicking Love it! without logging in, the page said Please log in first. | Pass |
| 6    | After Sign in with admin / admin, the page showed Latest pizza recommendations | Pass |
| 7    | The list showed the line Lab Pizza 20261116-2031 (tool=Pizza cutter, ingredients_number=6), the same tool as in step 4 | Pass |

## Part 3: Robot Framework
- robot --version: Robot Framework 7.5.1 (Python 3.14.7 on win32)
- Step 3.2, failure message for the wrong title: `Title should have been 'Web Form' but was 'Web form'.`
- Step 3.8, run 1, result, elapsed time and pizza name: PASS, 16.5 s, Lab Pizza 20261116-204251
- Step 3.8, run 2, result, elapsed time and pizza name: PASS, 12.9 s, Lab Pizza 20261116-204307

## Part 4: Cypress
- node --version, npm --version and npx cypress version: v24.7.0, 11.5.1, Cypress package and binary version 16.2.1
- Step 4.3, failure message for the wrong title: `Timed out retrying after 10000ms`, `+ expected - actual`, `-'Web form'`, `+'Web Form'`
- Step 4.6, tool and calories in the answer of POST /api/pizza: tool Scissors, calories 475 (run 2 of Step 4.8; the test checked that the page showed the same values as the answer)
- Step 4.7, the line of the admin list that shows your pizza: `Lab Pizza 20261116-205820 (tool=Scissors, ingredients_number=6)`
- Step 4.8, run 1, result, duration and pizza name: All specs passed (2 of 2), 00:23 in total (pizza-scenario 00:19, web-form 00:03), Lab Pizza 20261116-205712
- Step 4.8, run 2, result, duration and pizza name: All specs passed (2 of 2), 00:19 in total (pizza-scenario 00:15, web-form 00:04), Lab Pizza 20261116-205820
- Step 4.9 (optional), browser and result:

## Part B
1. The three number boxes of the home page, Max Calories per Slice, Min Number of Toppings and Max Number of Toppings, share the id `floating_filled`. In Step 0.4 `$$("#floating_filled").length` answered 3, so the id cannot tell the calories box from the other two, and a test that used it would type into whichever box comes first. The label text is different for each box, so my tests find the label Max Calories per Slice and take the input next to it. The page could give each box an id of its own, for example `max-calories`, `min-toppings` and `max-toppings`, or a `data-testid` attribute. A tester could then write a short locator such as `#max-calories` that finds exactly one element and keeps working when the label text or the layout changes.

2. This check is a consistency oracle. The test case does not state in advance which tool the admin list must show, because QuickPizza picks the tool itself and it changes from run to run. The expected value comes from an earlier output of the same system, the answer of step 4, and the test checks that two views of the same pizza agree. In my Cypress runs the answer said Pizza cutter in run 1 and Scissors in run 2, and the admin line showed the same tool each time, for example `Lab Pizza 20261116-205820 (tool=Scissors, ingredients_number=6)`. The check would catch a fault where the server saves a different tool from the one it sent to the customer, or where the admin list shows the tool of another pizza next to my pizza name.

3. The answer of `POST /api/pizza` has a field `vegetarian`, so the Cypress test could add `expect(response.body.vegetarian).to.equal(true)` next to the checks of Step 4.6. That check only trusts what the server says about its own pizza. A stronger check reads every ingredient that the page lists and checks that none of them is meat or fish. The page shows only the names of the ingredients, for example Olive oil, Mozzarella and Pineapple in my run 2, so the test needs another source that says which ingredients are vegetarian, such as a list from the specification or from the ingredient data of the API.

4. In Robot Framework I wrote each wait as a keyword, such as `Wait Until Element Is Visible    css:h1` and `Wait Until Element Contains    id:recommendations    Name:`, with `timeout=10s` set once on the library. In Cypress I wrote no wait for elements, because every `cy.get` and `should` tries again for up to 10 seconds, and the only wait in the spec is `cy.wait('@pizza')` for the answer of the request. QP-01 needed more files in Robot Framework, eleven keywords in the resource file and a test file that calls them, while Cypress needed one spec and one custom command, but the Robot Framework test reads like the written test case. The full QP-01 took 16.5 s and 12.9 s in Robot Framework, and the QP-01 spec took 19 s and 15 s in Cypress. The Robot Framework log.html shows every keyword with its arguments, its message and the screenshot at the step that took it, which makes it easy to find the value of `${tool}` or `${calories}`. The Cypress video shows the Command Log next to the page while the test runs, which is quicker to follow, but I cannot search it for a value.

5. I would use Cypress for the automated UI tests of Project 2. First, Cypress waits by itself, so the spec has no wait keyword to forget, and when the title was wrong in Step 4.3 it retried for 10 seconds and then showed the expected and the actual text side by side. Second, one command, `npx cypress run --browser chrome`, runs every spec and records a video and the screenshots, which is the evidence of a full run that Project 2 asks for.

6. Part 3. If QuickPizza changed the locator of the calories box, I would change one line, the variable `${MAX CALORIES}` in the Variables section of `resources/quickpizza.resource`. The keywords Turn On Advanced and Fill In The Pizza Request use that variable three times, and none of those lines would need a change. The test file `pizza_scenario.robot` holds no locator of that box. It only calls the keywords by name, for example `Fill In The Pizza Request    600    Knife    ${name}`, so it stays the same, and every other test that imports the resource file is repaired by the same one-line change.

7. Part 4. Cypress clears the cookies before each test. QuickPizza runs on several servers, a cookie keeps one browser on one server, and the admin list shows only the pizzas of that server. If steps 6 and 7 were a second test, the browser would lose the cookie and could reach another server, whose admin list does not hold the pizza of step 4, so step 7 would fail on some runs and pass on others. The second test would also lose the alias `@tool`, because Cypress resets aliases between tests, and it would need the pizza name from the first test, so the two tests would depend on each other.
