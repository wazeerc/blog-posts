---
title: React Sucks. Here's Why You Should Learn It.
description: The Good and Bad Side of React
tags: ["Software Development", "React"]
pubDate: Dec 12 2024
readTime: 5 min
heroImage: /thumbnails/react-crown.png
---

React, loved by many and equally hated, is the UI library developed by Facebook (now Meta) in 2013. It’s arguably one of the most, if not the most popular JavaScript libraries out there. But let’s be honest: popularity isn’t the best metric to measure how good or bad something is. In this article, we’ll dive into the many quirks of React.js and explore whether it’s the right library for you.

I’ve been working with React on and off for the past 3–4 years, so there might occasionally be some biased ideas here and there.

TL; DR - Choose the right tool for the job. If you want to land an entry-level job quickly, React might be a solid choice.

## What is React?

React is a declarative, component-based JavaScript library for building user interfaces. It doesn’t dictate how you manage data or handle side effects, making it less opinionated compared to frameworks like Vue.js.

Its core concept revolves around components; reusable building blocks that represent parts of your UI. React uses a virtual DOM to optimize rendering performance and has a huge ecosystem with libraries like Redux, React Query, and Next.js complementing it.

## Does it actually suck?

### Hooks

React hooks were a game changer when they were introduced in 2019, but they can lead to messy implementations. I’ve seen codebases littered with unnecessary useEffect calls or an overuse of useMemo and useCallback. These hooks are supposed to optimize performance, but when overused or used incorrectly, they can actually make the app slower and harder to maintain.

### JSX: A Blessing and a Curse

JSX, React’s syntax extension for JavaScript, is powerful but divisive. On the one hand, it allows you to write HTML-like code within JavaScript, which is great for readability and modularity. On the other hand, it blurs the line between logic and UI, sometimes leading to overly complex components.

### Dirty React Code

React's flexibility can be a double-edged sword. It doesn’t enforce strict conventions, which means every developer can implement their own style. While this is great for creativity, it often results in inconsistent codebases. Many React developers, based on my experience, don’t grasp the library’s fundamentals, leading to chaotic code filled with unnecessary hooks and poorly managed state.

If you don't want to fall into this trap, check out my article on writing [clean code](/blog/clean-code).

### State Management

State management is another pain point. While React’s built-in Context API works fine for smaller components, scaling a project often requires additional libraries like Redux, MobX, or Zustand. This added complexity can be overwhelming, especially for beginners.

### The New Features

React keeps introducing new features, which is both exciting and frustrating. Staying up-to-date can feel like a full-time job. Just when you master one feature, another one drops, adding yet another layer of abstraction to learn. React 19, I'm talking to you.

## Why you should still learn it

### Beginner-Friendly (Sort of)

Compared to libraries like Vue.js, React is less opinionated. It feels closer to "vanilla" JavaScript. For instance, React’s map() approach to rendering lists is just JavaScript, while Vue’s v-for is a custom directive. This means that beginners working with React are more likely to pick up JavaScript fundamentals along the way, which is another very important factor. I've seen many devs jumping on the React hype-train without having a decent understanding of the basic concepts of JavaScript or the Web in general.

### The Job Market

The demand for React developers is massive. The Stack Overflow Developer Survey and others consistently shows React as one of the most loved and wanted libraries. Additionally, frameworks like Next.js, which extend React’s functionality, are gaining popularity, making React knowledge even more valuable.

Here are some numbers from the Developer Survey 2024 for React.

- Used by 39.5%, desired by 33.4% and admired by 62.2% of respondents

### Ecosystem

React’s ecosystem is unparalleled. From state management libraries (Redux, Zustand) to styling solutions (Tailwind CSS, Styled Components) to full-fledged frameworks like Next.js, React’s versatility means you can build virtually anything with it.

### React Native

React Native lets you extend your React skills to mobile app development. Meta is heavily investing in React Native for their VR and AR apps, so knowing React opens doors to those areas as well.

## Conclusion

React gets the job done, but at what cost? I’ll leave that for you to judge. Personally, I’ve been exploring libraries like Vue.js and going back to good old vanilla JavaScript with tools like Vite for my recent projects.

If you’d asked me about React a couple of months ago, I’d have told you it was the best thing ever; mainly because I’d been using it 90% of the time. It’s like living in a town with only one restaurant: of course, it has the best food ever, until you try something new. Now, based on my experience, I can confidently say React sucks. But that’s fine—programming in general sucks.

The real issue is that based on my experience over 50% of React developers write bad code, leading to terrible DX. So, while React has its quirks, learning it is still worthwhile, it’s just important to understand its trade-offs and be aware that it is only a tool hiding under several layers of abstraction which will eventually end up as a script in your bundle.
