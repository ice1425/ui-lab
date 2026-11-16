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
- Step 4.3, failure message for the wrong title:
- Step 4.6, tool and calories in the answer of POST /api/pizza:
- Step 4.7, the line of the admin list that shows your pizza:
- Step 4.8, run 1, result, duration and pizza name:
- Step 4.8, run 2, result, duration and pizza name:
- Step 4.9 (optional), browser and result:

## Part B
1.
2.
3.
4.
5.
6.
7.
