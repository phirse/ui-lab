# UI lab results

Tool parts that I chose: Part 3 (Robot Framework) and Part 4 (Cypress)

## Part 0
- Step 0.1, Chrome version: Google Chrome 157.0.8103.79 (Official Build) (64-bit)
- Step 0.3, ChromeDriver version: ChromeDriver 157.0.8103.79 (6dc9bc3bdaea5c35ed6d41fceab2f7a3a2227c4f-refs/branch-heads/8103@{#1574}), at C:\drivers\chromedriver-win64\chromedriver.exe
- Step 0.4, locator of Pizza, Please!: `button[name="pizza-please"]`
- Step 0.4, locator of Excluded tools: `select` (the only select on the page, it has no id and its label points to `countries_multiple` which does not exist)
- Step 0.4, locator of Must be vegetarian: `#default-checkbox`
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name` (`#pizza-name` finds 2 elements after Pizza, Please! because the h2 of the answer has the same id)
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261122-141223, tool Scissors, 400 calories per slice

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | The heading reads Looking to break out of your pizza routine? | Pass |
| 2    | Max Calories per Slice (holding 1000), Excluded tools, Must be vegetarian and Custom Pizza Name appeared | Pass |
| 3    | The fields showed 600, Knife selected, vegetarian ticked and Lab Pizza 20261122-141223 | Pass |
| 4    | Name: Lab Pizza 20261122-141223, Dough: Thin, Tool: Scissors, Calories per slice: 400 | Pass |
| 5    | Please log in first. appeared under the two buttons | Pass |
| 6    | Both boxes already held admin. After Sign in the page showed Latest pizza recommendations | Pass |
| 7    | Lab Pizza 20261122-141223 (tool=Scissors, ingredients_number=6) was the 5th line, four newer pizzas from other users were above it. Same tool as step 4 | Pass |

## Part 3: Robot Framework
- robot --version: Robot Framework 7.5.1 (Python 3.11.9 on win32)
- Step 3.2, failure message for the wrong title: Title should have been 'Web Form' but was 'Web form'. (1 test, 0 passed, 1 failed, and the log showed selenium-screenshot-1.png under the red step)
- Step 3.8, run 1, result, elapsed time and pizza name: PASS (1 test, 1 passed, 0 failed), 00:00:10.639, Lab Pizza 20261122-142253 (tool Scissors, 325 calories per slice). My first headless try before this one failed with `Element 'id:svelte-announcer' did not appear in 10 seconds.` because Go To on /admin already took about 6 seconds that time. I ran the same command again and it passed, that is the run1 folder.
- Step 3.8, run 2, result, elapsed time and pizza name: PASS (1 test, 1 passed, 0 failed), 00:00:09.749, Lab Pizza 20261122-142315 (tool Scissors, 450 calories per slice)

## Part 4: Cypress
- node --version, npm --version and npx cypress version: v22.20.0, 10.9.3, Cypress package version 16.2.1 and Cypress binary version 16.2.1 (Electron 41.7.0, bundled Node 24.15.0). npm 10 gave no install-scripts warning, and npx cypress info found Chrome 157.0.8103.79
- Step 4.3, failure message for the wrong title: `Timed out retrying after 10000ms` with `+ expected - actual`, `-'Web form'` and `+'Web Form'`. The run took 12 seconds and saved "Web form -- fills in the fields and sends the form (failed).png"
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
