# Crush Hash Sum

### Problem Description

Your friend reveals to you the classmate in your class they are secretly admiring, but they do not reveal the person by name. Instead, they give you a ***large unsigned integer***, which represents the *sum of **each hash** of **every permutation** of their crush's name*, where the hash function being used is the ***polynomial rolling hash***. Given the large unsigned integer `crush` and a list of classmates `classmates` of size `k`, find the classmate who is your friend's crush.

### Test Cases

```
Case 1:

crush = 5934982400453757329280
classmates = ["natalia", "karina", "benedetta", "lancelot"]

result = "benedetta"
```


```
Case 2:

crush = 10948127567160
classmates = ["akali", "katarina", "miss fortune", "evelynn", "fiora", "ahri", "qiyana", "seraphine"]

result = "qiyana"
```

```
Case 3:

crush = 120246460145260320860584479059307088269312000
classmates = ["palm siberia", "biscuit krueger", "neon nostrade", "morena prudo", "camilla hui guo rou", "shizuku murasaki", "machi komacine", "cheadle yorkshire", "kikyo zoldyck"]

result = "shizuku murasaki"
```


#### Spoilers Ahead, Click to View

### Hints

<details>
    <summary>Hint 1</summary>
    <br>
    <hr>
    <p>
        A brute force approach would be to sum the hash results of each permutation, but that would take <code>O(n * n!)</code> time. Searching through a long list of names could take hours. Is there a way to cheat the process?
    </p>
    <hr>
</details>

<details>
    <summary>Hint 2</summary>
    <br>
    <hr>
    <p>
        Checking the shortest names among the classmates first is a sensible approach from the brute force method. Is it really enough, though?
    </p>
    <hr>
</details>

<details>
    <summary>Hint 3</summary>
    <br>
    <hr>
    <p>
        Think about the hash function being used, which is the <b><i>polynomial rolling hash</i></b>. What properties of this hash function can be exploited to skip redundant calculations?
    </p>
    <hr>
</details>

### Solution

<details>
    <summary>Solution</summary>
    <br>
    <hr>
    <h4>Intuition</h4>
    <p>
        A brute force approach using the given formula is a sure way to solve the problem, but takes too much time and computations. In a list of <code>k</code> classmates, searching through the whole thing would take around <code>O(k * n * n!)</code> time, where <code>n</code> is the average string length of all the names.
    </p>
    <p>
        In order to skip the expensive calculations of finding all permutations of each name, we can simply manipulate a property of the polynomial rolling hash function in order to optimize the process. Let's take a look at the actual function implementation.
    </p>
    <pre>
function PolynomialRollingHash(str) {
    sum = 0
    for (index, character) in str {
        sum += ascii(character).pow(index) 
    }
    return sum
}
    </pre>
    <p>
        And then actually getting the sum of each hash of all permutations of a string by brute force would look like this.
    </p>
    <pre>
function HashSumOfPermutations(str) {
    sum = 0
    for p in permutations(str) {
        sum += PolynomialRollingHash(p)
    }
    return sum
}
    </pre>
    <p>
        The hash function here utilizes each character's position in the string in order to generate a uniqe value, and this property can help us simplify the calculation. Rather than painstakingly iterating through every permutation of a string, we can simply calculate <i>how many times</i> each character will occur at <i>each position of the string</i> across <i>all possible permutations</i>. 
    </p>
    <p>
        Let's look at an example string <code>"abc"</code>. This string generates the following possible permutations:
        <ul>
            <li><code>"abc"</code></li>
            <li><code>"acb"</code></li>
            <li><code>"bac"</code></li>
            <li><code>"bca"</code></li>
            <li><code>"cab"</code></li>
            <li><code>"cba"</code></li>
        </ul>
        The given string has a length <code>n</code> of <code>3</code>, which generates <code>n! -> 3! = 6</code> possible permutations. If we observe the same property in different strings, we notice a pattern where <i>each (non-unique) character</i> occurs <code>(n - 1)!</code> times <i>at every position in the string</i> across all permutations.
    </p>
    <p>
        As a result, we can skip the brute-force permutation calculation and get the sum of each hash of every permutation of a string by getting the <i>sum of <b>each character's hash at all positions</b> multiplied by the <b>number of times it will occur at each position</b></i>.
    </p>
    <pre>
function CheatSum(str) {
    sum = 0
    for character in str {
        csum = 0
        for index in str {
            csum += ascii(character).pow(index)
        }
        sum += csum * (n - 1)!
    }
    return sum
}
    </pre>
    <p>
        Through this process, we drastically reduce the time complexity of the computation for getting the sum of each hash of all permutations of a string from <code>O(n * n!)</code> to <code>O(n²)</code>, and searching through the entire list of classmates to find your friend's crush will be reduced from <code>O(k * n * n!)</code> to worst case <code>O(k * n²)</code>.
    </p>
    <hr>
</details>