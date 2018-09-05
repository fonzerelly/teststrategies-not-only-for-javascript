## Did I do enough tests?
<img src="images/did-i-do-enough-tests.jpg" width="50%">
Note: In the beginning of this talk, I described the situation that we need to take care to prevent the broken window syndrome and we need trust in our test? So until now I showed you a lot of testing techniques and if you apply them, I suppose you will get a lot of good tests. But how can you get trust? Let's say you apply those techniques but your collegues are not so strict. This would mean, your code is properly tested but the rest of the application is not tested properly. This would mean that your team would loose confidence into the outcome of the test. So we need some kinds of quality measures to assure our trust.

??HORIZONTAL
## CodeCoverage
<img src="images/schattenspiele.png" width="50%">
Note: One option to gain that trust is Code Coverage. The basic idea is, that each line in your Code gets instrumentalized, so that meanwhile test we can track which line of code has been passed. The standard Code Coverage tool for JavaScript is Istanbul. In many cases it is already installed. If you setup your Angular Project for example with Angular-cli or if you work with react and you use Jest for your tests it is also already part of the installation. Let's have a look what code coverage data looks like: 

??HORIZONTAL
### Console output
<img src="images/code-coverage-console.png" width="200%">
Note: Here we see the short coverage report for all tested files. It shows three diffrent columns:
* The Statements-Column displays the proportion of all executed Statements compared to the whole of statements.
* The Branches-Column means the same for all if and else statements in your code.
* Similar is true Functions-Column and the Lines-Column

So besides our already known caesar-ciphers, we see also the file calc.js which seems to have some trouble. 

??HORIZONTAL
### calc.js
```JavaScript
module.exports = {
    add : function (a, b) {
        return a + b
    },
    sub : function (a, b) {
        return a - b
    }
}
```
Note: So calc is just a little dummy module with only two methods, add and sub. And where is the problem?

??HORIZONTAL
### calc.spec.js
```JavaScript
const calc = require('./calc.js')
describe('calc', function () {
    describe('add', function () {
        it('should add two numbers', function () {
            expect(calc.add(2,3)).toBe(5)
        })
    })
})
```
Note: The Problem is, that we are missing any test for sub. How would Istanbul have showed that to us?

??HORIZONTAL
### coverage report for calc.js
<img src="images/code-coverage-browser.png" width="80%">
Note: Exactly that: The code of the method sub has never been called.

So far so good. If you work with a decent elve, she or he would write at least one test for sub and we are done. But sometimes this efford seems to be too much for my fellow elves. So Evil Elfe could think of this:

??HORIZONTAL
### cheat on coverage in calc.js
```JavaScript
module.exports = {
    add : function (a   , b) { ... },
    
    sub : function (a, b) { ... },
    massivlyCheatCodeCoverage: function () {
        let i = 0
        i++
        i++
        i++
        ...
        i++
        i++
        i++
        return true;
    }
}
```
Note: A very long dummy method...

??HORIZONTAL
### cheat on code coverage calc.spec.js
```JavaScript
const calc = require('./calc.js')
describe('calc', function () {
    describe('add', function () { ... })
    it('should increase code coverage', () => {
        const result = calc.massivlyCheatCodeCoverage();
        expect(result).toBeTruthy();
    })
})
```
Note: ... and a meaningless test can trick code coverage very simple:

??HORIZONTAL
### Result on cheating code coverage
<img src="images/cheated-coverage-console.png" width="200%">

Note: Now Statements and Lines have a coverage of about 98%. I think in a more complex example you are very likly to overlook this issue and again have a hole in your window of trust.

??HORIZONTAL
## Mutation based Testing
<img src="images/mutation-based-testing.jpg" width="50%">
Note: A better option would be to work with mutation based testing. For JavaScript we use Stryker here. Instead of instrumenting your code it generates Mutants from it. This means it can replace + with - for example, or rename variables and many more. 
When Stryker runs it trys to kill each of the Mutants by running your test suite. And it might fail one or more Mutants, if you did not test for this specific issue.
In a configuration file, you can also decide which of those 30 Mutants must not be applied, if you think this aspect needs no attention.

??HORIZONTAL
### Stryker installation
<img src="images/stryker-install.gif" width="200%">
Note: Installing Stryker is fairly simple. After installing it with npm you run it with the command init. This allows you to specify the testframework, testrunnr and the genrated reports. 

??HORIZONTAL
### Stryker run
<img src="images/mutation-run.png" width="200%">
Note: In our example of calc, we see that Stryker
* initially runs the tests
* generates Mutants
* reruns the tests to kill them

??HORIZONTAL
### Stryker output
<img src="images/mutation-result.png">

Note: We can see here a very similar output like for code coverage. But it can not be tricked. The only drawback would be, that it takes more time to run since it has to run the test suite for each mutant one time. So maybe it should only be done on build server.

??HORIZONTAL
## Summary
* use TDD <!-- .element: class="fragment" -->
* use PBT <!-- .element: class="fragment" -->
* use dependency injection <!-- .element: class="fragment" -->
* get a first impression by CodeCoverage <!-- .element: class="fragment" -->
* use Mutation based testing on the CI-server <!-- .element: class="fragment" -->

??HORIZONTAL
<img src="images/sleep-well.jpg" width="50%">
Note:
Now you can sleep well, Santa. Now our UnitTests are fine. And next year we talk about Integration Tests with Cucumber and the like.