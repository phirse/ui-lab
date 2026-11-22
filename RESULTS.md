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
- Step 4.6, tool and calories in the answer of POST /api/pizza: tool Scissors and calories 400 (run 2 below). The test waits for the request with status 200, checks that request.body.maxCaloriesPerSlice is 600, and checks that the page shows the same tool and calories as response.body, so qp01-step4-recommendation.png shows the values of the answer
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261122-143248 (tool=Scissors, ingredients_number=5) (newest)
- Step 4.8, run 1, result, duration and pizza name: All specs passed!, 00:14 (pizza-scenario.cy.js 00:10, web-form.cy.js 00:03), Lab Pizza 20261122-143201 (tool Pizza cutter, 325 calories per slice, first line in the admin list)
- Step 4.8, run 2, result, duration and pizza name: All specs passed!, 00:15 (pizza-scenario.cy.js 00:11, web-form.cy.js 00:03), Lab Pizza 20261122-143248 (tool Scissors, 400 calories per slice). The videos and screenshots in the repo are from this run
- Step 4.9 (optional), browser and result: not done

## Part B
1. The three number boxes on QuickPizza share the id floating_filled, and `$$("#floating_filled").length` gave 3 in the Console. A locator like `#floating_filled` only finds Max Calories per Slice because it happens to be the first of the three, so it would break as soon as the order of the boxes changes. That is why the tests start from the label text, for example `//label[normalize-space()='Max Calories per Slice']/preceding-sibling::input`. I would give each box its own id, like `id="max-calories"`, and point the label to it with `for="max-calories"`. Then a test can use `#max-calories`, which is short and does not depend on the label wording or on where the label sits in the HTML.

2. It is a consistency oracle, because it compares two outputs of QuickPizza with each other instead of comparing with a value I knew before the run. In Cypress run 2 the answer said Tool: Scissors and the admin list said Lab Pizza 20261122-143248 (tool=Scissors, ingredients_number=5), so the two parts agreed. The check would catch a fault where the server stores a different tool from the one it showed the customer, for example if it picks the tool again when it saves the pizza and ignores the Knife exclusion. It would also catch the admin page putting the tool of another pizza next to our name.

3. A check for vegetarian needs to know which ingredients are vegetarian, and the page only shows names, like Mozzarella fior di latte, Onions and Fresh basil in my Cypress run 1. In Cypress I could add `expect(response.body.vegetarian).to.equal(true)` inside the `cy.get('@pizza')` block, because the answer of POST /api/pizza has a vegetarian field next to calories. That only checks what the server says about its own pizza, so a stronger check would compare every ingredient on the page with a list of non-vegetarian ingredients such as pepperoni, ham or anchovies. That list has to come from another source, for example the ingredient data of QuickPizza or a list kept in our test data.

4. In Robot Framework I had to write every wait myself: Wait Until Element Is Visible for the heading, Wait Until Page Contains Element for the Knife option, Wait Until Element Contains for the answer and the rate message, and Wait Until The Page Is Ready on /admin, all with timeout=10s. Cypress retried every get and should for up to 10 seconds by itself, so the only waits I wrote were `cy.wait('@pizza')` and `should('exist')` on #svelte-announcer. QP-01 was faster to write in Cypress, but `.then` and the aliases needed more thinking, while the Robot keywords were more typing but read almost like the test case table. Both ran QP-01 in about 10 seconds (Robot 10.639 s and 9.749 s, Cypress 00:10 and 00:11 for the spec). Robot's log.html was the easiest to read, it shows each keyword with messages like `${tool} = Scissors` and the screenshot at its step, and when my first headless run timed out on /admin it showed the red step with a picture of the login form. The Cypress video shows the Command Log next to the page, but the page is scaled to 33% because of the 1280x1600 viewport, so the step screenshots were more useful to read.

5. I would choose Cypress for Project 2. First, the waiting is built in: both full Cypress runs passed the first time with no fixed pause and almost no waits written by me, while my first headless Robot run timed out on the admin page and I had to run it again. Second, it gives the evidence Project 2 asks for with very little work, `video: true` recorded both specs and `cy.screenshot` saved full page pictures of steps 4, 5 and 7. `cy.intercept` also let me check the answer of POST /api/pizza against the page, which I could not do in Robot without another library. The thing to plan for is that Cypress controls one browser per test, so a staff and customer test has to log out and log in again inside the same test.

6. Part 3: the locator of the calories box is only in one place, the variable `${MAX CALORIES}` in the Variables section of resources/quickpizza.resource. If QuickPizza changed the box, for example gave it the id max-calories, I would change only that line to `id:max-calories`. The keywords Turn On Advanced and Fill In The Pizza Request use the variable three times, so they pick up the new locator without other edits. pizza_scenario.robot only calls Fill In The Pizza Request with 600, Knife and the pizza name and never sees a locator, so the test file does not change at all.

7. Part 4: Cypress clears cookies, local storage and aliases before each test. QuickPizza runs on several servers, a cookie keeps one browser on one server, and the admin list only shows the pizzas of that server. If steps 6 and 7 were a second `it()`, that test would start without the cookie and could reach another server, so `cy.contains('li', pizzaName)` would not find the pizza and would time out after 10 seconds. It would also fail earlier at `cy.get('@tool')`, because the alias from step 4 is gone, and pizzaName only exists inside the first test. With QP-01 as one test, both of my runs found the pizza as the newest line of the list.
