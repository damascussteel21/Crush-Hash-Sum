# Crush Hash Sum

### Problem Description

Your friend reveals to you the classmate in your class they are secretly admiring, but they do not reveal the person by name. Instead, they give you a ***large unsigned integer***, which represents the *sum of **each hash** of **every permutation** of their crush's name*, where the hash function being used is the ***polynomial rolling hash***. Given the large unsigned integer `crush` and a list of classmates `classmates` of size `k`, find the classmate who is your friend's crush.

#### Spoilers Ahead, Click to View

<details>
    <summary>Hint 1</summary>
    <br>
    <hr>
    <p>
        A brute force approach would be to sum the hash results of each permutation, but that would take O(n * n!) time. Searching through a long list of names could take hours. Is there a way to cheat the process?
    </p>
    <hr>
</details>

<details>
    <summary>Hint 2</summary>
    <br>
    <hr>
    <p>
        Think about the hash function being used, which is the <b><i>polynomial rolling hash</i></b>. What properties of this hash function can be exploited to skip redundant calculations?
    </p>
    <hr>
</details>