# Clean Code. A Myth?

<img src="https://github.com/user-attachments/assets/ce455872-1aa0-4c85-ae6e-8f3e698c753a" alt="clean and well formatted code illustration">

You might have seen the term "Clean Code" thrown around online. Maybe your favourite LinkedIn influencer wrote a post about it that went viral because, let’s face it, anyone who shares their opinions on software development will inevitably spark spirited debates. I've even seen job listings that mention clean code as a requirement.

In this article, I’ll go over some small but impactful quality-of-life improvements: what clean code is, why it matters, and how to achieve it. There’ll be no KISS, DRY, or other design patterns here.

However, it’s important to note that this is a highly subjective topic, and not everyone might agree with my perspective. Every developer has their own way of writing code, and I'm not here to say your code sucks—you probably already know that. Some argue that shipping features is all that matters and everything else is secondary. Maybe they’re right. But in a professional context, you'll likely spend more time reading code than writing it.

TL; DR - Clean Code is about writing readable, maintainable code. Make smart decisions.

## What is Clean Code?
The term was popularised by Robert C. Martin (aka Uncle Bob), who wrote a book titled Clean Code. In it, he defines clean code as "simple and direct." That’s it: clean code is code that’s simple and direct.

Sounds easy enough. But how does it actually work?

## How to Write Clean Code?
When I was doing my undergrad a few years back, we were always told to add comments to our code. Don’t get me wrong, comments can be useful, but they’re also a great way to turn your codebase into a mess.

If you have to rely on comments to make your code readable, something’s wrong.

- **Tip #1: Use comments to explain the why, not the what.**

Code should be self-explanatory. Anyone reading it should understand what's going on. However, why the code does something specific may not always be obvious, especially for new devs working on your codebase.

Instead of:

```js
// Calculate 10% of Total Sales Amt
let tax = totalSales * 0.10;
```

Do:

```js
let taxRate = 0.10; // Adjust this if tax policy changes
let tax = totalSales * taxRate;
```

In most editors, comments are hard to read because their font colour is often grey or very pale. Let’s say you have a function that calculates tax and you hardcode the tax percentage (e.g., 10%) with a comment like “Calculate 10% of Total Sales Amt.” During a PR review, your colleague points out that it should be 15%, not 10%. Now you need to change the value and commit again, and the chances of forgetting to update the comment from 10 to 15 are high. If you’re using a tool that checks for code smells, it won’t catch this discrepancy.

If you work with JavaScript, you might already be familiar with JSDoc. It’s a tool that generates documentation, which is different from comments. No, writing multi-line comments doesn’t turn your useless comments into documentation.

Here's how you can use it:

```js
/**
 * Calculates tax based on total sales and current tax rate.
 * @param {number} totalSales - The total sales amount.
 * @returns {number} - Tax amount.
 */
function calculateTax(totalSales) {
    const taxRate = 0.10;
    return totalSales * taxRate;
}
```

This approach is much easier to understand and read. You can quickly see a summary of what the function does and what arguments it takes just by hovering over it.

Great, now you know that making your code readable can often mean getting rid of comments.

- **Tip #2: Don’t be afraid to add whitespace and empty lines. They improve the visual hierarchy, making the code easier to read and clearly showing separation of concerns.**

Without whitespace:

```js
let tax = totalSales * taxRate;let discount = totalSales * discountRate;let total = totalSales - discount + tax;
```

With whitespace:

```js
let tax = totalSales * taxRate;

let discount = totalSales * discountRate;

let total = totalSales - discount + tax;
```

Whitespace here makes the logical grouping of steps in the calculation much clearer.

- **Tip #3: Declare variables where you’ll use them. Stop declaring all your variables at the top of functions or classes.**

Instead of:

```js
let tax;
let discount;
let total;

// Much later in the function
tax = totalSales * taxRate;
discount = totalSales * discountRate;
total = totalSales - discount + tax;
```

Do:

```js
let tax = totalSales * taxRate;
let discount = totalSales * discountRate;
let total = totalSales - discount + tax;
```

- **Tip #4: Avoid Magic Numbers.**

Instead of:

```js
let finalAmount = totalSales * 0.85; // What does 0.85 mean?
```

Do:

```js
const DISCOUNT_RATE = 0.15;
let finalAmount = totalSales * (1 - DISCOUNT_RATE);
```

- **Tip #5: Choose descriptive names for your variables and functions.**

Instead of:

```js
let x = 0.15;
function calc(t) {
    return t * x;
}
```

Do:

```js
let discountRate = 0.15;
function calculateDiscount(totalSales) {
    return totalSales * discountRate;
}
```

You can also use aliases for functions. I find this particularly useful in React projects.

```js
import {genRandomData as createRandomUsers} from './utils'
```

BTW: A function should only do one thing. I’ll stop here, or else this will turn into an article about SOLID.

- **Tip #6: Avoid double negation. Nobody has time to guess what !!notLoading means. If you’re going to use operators like this, it’s better to avoid them altogether. You’d be better off creating separate booleans for each state.**
Instead of:

```js
let !!notLoading = true; // This is confusing
```

Do:

```js
let isLoading = false;
let isProcessing = true;
```

> Good developers write clean code naturally—it’s not something you have to overthink. The goal is to make code readable, maintainable, and easy to work with.

Here are two questions I often see:

1. **What’s the cost of writing clean code?**
It's free.

1. **If I have only two hours to ship a feature, should I worry about clean code?**
Do you want to spend two hours developing a feature and four hours debugging it? Or do you want to make it harder for others to collaborate? If you answered yes to either, then don’t worry about clean code.

## Conclusion
Clean Code is a subjective matter. If you’re working in a team, your best bet is to develop a set of coding guidelines or standards and refer to that as the clean code.

So, is clean code a myth? If it is, those who sold the idea did a great job.

But honestly, code should be clean regardless. Programming languages are meant to be human-readable; otherwise, we’d all be writing machine code.

At the end of the day, your end-users will never get to see how clean your code is. Don't overthink it. Don't expect your code to be perfect. Code is liability, it might never be perfect. The least you can do is write code that is maintainable and makes collaboration easy.


### References:
Martin, R.C. (2010). Clean code a handbook of agile software craftmanship. Upper Saddle River [Etc.] Prentice Hall.

‌Don't Write Comments (2023). Don’t Write Comments. [online] YouTube. Available at: https://youtu.be/Bf7vDBBOBUA?si=kOpttDW8O11ptMIo [Accessed 28 Oct. 2024].
