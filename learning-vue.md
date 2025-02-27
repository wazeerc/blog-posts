---
title: I Built an App With a JS Library I Just Learned
description: Learning Vue 3, building and deploying an app using it
tags: ["Software Development", "Vue"]
pubDate: Feb 16 2025
readTime: 8 min
heroImage: /thumbnails/learning-vue.png
---

As I was exploring more facets of the frontend world, I realized it might be a good time to learn a new framework. While I’ve mostly been using React for my projects, as mentioned in my [previous blog post](/blog/react-good-and-bad), I wasn’t sick of it, I simply felt it was time to expand my horizons and dive into something new.

## The Learning Process

So, I decided to learn Vue.js. I signed up for a [fundamentals course](https://frontendmasters.com/courses/vue-fundamentals/) on Frontend Masters by Ben Hong. I picked up the basics pretty quickly. Some concepts felt a bit strange at first, but most of them made sense. As a React developer, I was surprised by a few things. For example, the fact that you can bind an attribute to a variable by simply doing `:id` instead of `:id="id"`, as `const id = "Vue"` already exists was an interesting little feature. It may seem trivial, but it’s little things like this that really improve the DX. However, I did find it a bit confusing at first to use variables within " ", and they aren’t actually treated as strings.

I was introduced to the Options and Composition API, honestly I did not like the former at all. However, I was totally sold on the latter. The composition API felt very familiar. Learning Vue was a very refreshing experience; I find it very enlightening to discover new ways to solve problems. Yes, we're still talking about JavaScript, but React and Vue have their own unique approaches and quirks. Vue, being a more opinionated framework compared to React, can be a major advantage to some.

This also turned out to be the perfect opportunity to put into practice many of the other skills I’ve been learning throughout 2024; TailwindCSS, Unit Testing, Docker, Cloud, DevOps, and more.

Like many of us in 2024, when I felt a bit stuck for inspiration, I turned to ChatGPT for some project ideas. I already had a few in mind but wanted something more “original.” One suggestion was a markdown editor. Ironically, back then I had been getting frustrated with the one on GitHub, and had been using [readme.so](https://readme.so/) by Katherine Oelsner, which heavily inspired my first Vue project.

So, I decided to build [PrevueMD](https://prevuemd-67440579388.us-central1.run.app/)--a real-time markdown editor.

## The Idea

I wanted to build a Markdown editor with live preview. Whenever I learn a new skill, I like to build something alongside or shortly after grasping the fundamentals. This approach helps me put my learning into practice, rather than just reading notes or watching tutorials. Plus, it’s a great way to create a new project to add to my portfolio and document my journey.

The plan was simple: I wanted to build a lightweight, accessible app with a nice UI using Vue 3.

## The Design

I kicked things off by opening Excalidraw to plan the application’s infrastructure. I quickly drafted some user flow diagrams and made adjustments as I went along. For state management, I chose Pinia, which is the recommended solution for Vue. Then, I turned my attention to finding the right tool for markdown parsing. There were several ready-made options like `vue-markdown-preview` and `marked`, but I wanted to dive a bit deeper and create my own parsing layer. After analysing and going through the source code of several parsers, I found that most were using a particular package—`remark`. So, I decided to go with that. Remark offers a suite of plugins that work with Markdown as structured data and ASTs.

Next, I moved on to designing the user interface. I opened Figma and created low-fidelity mock-ups. I kept the layouts simple, and since I planned to use TailwindCSS, I knew implementing these designs would be straightforward.

## The Coding

### Coding the Design

While I was still going through the Vue course, I began implementing the designs. At that point, I didn’t have much experience with TailwindCSS, so it was a great opportunity to learn the framework and make some nice progress.

### Coding the Functionality

A few weeks later, I began writing the functionality. The idea was simple: an editor on the left that captures the markdown code input, parses the markdown into markup, and renders it on the right inside the previewer. The user should also be able to copy the markdown code to their clipboard or download it as a `.md` file.

I set up my store to handle data flow and wrote unit tests simultaneously. The project structure evolved a bit over time, but the core arrangement remained consistent—generic, independent components with their functionality tested appropriately. I also went through a few rounds of refactoring to ensure my code was clean, maintainable, and that the interface remained accessible.

## The Optimisations

Things got interesting at this point. I wanted to make the app available as a PWA—something I had never done before. So, naturally, I did some research and watched a Fireship video on it. After running my first Lighthouse test, I realized there were a few issues with my app: poor SEO and performance results. Thankfully, the accessibility (a11y) and best practices scores were decent. Since the plan was to deploy the project, I needed to address the underlying gaps.

I dove into tooling and researched how to optimize web applications. I was using Vite so, I reviewed the documentation and watched a few videos by Evan You discussing the tool. I learned a lot about bundling, the build process, and more.

Next, I applied some optimization techniques. I began with SEO, putting into practice what I had recently learned in a Web Analytics course for my postgraduate studies. I created a `robots.txt` file and made other minor improvements. After that, I tackled performance issues, which, though not critical, still had room for enhancement. The first thing I did was modularize my markdown parsing function, cache a couple of instances, and dynamically load dependencies while keeping them minimal. I was satisfied with the results: they were all greatly improved, (98 for performance, 100 for a11y, 96 for best practices and 100 for SEO).

## The Testing

I didn’t technically follow a traditional TDD approach, but I thoroughly tested my code as I wrote it. I tried Vitest, and found it relatively easy to get the hang of. I see many aspects where Vitest outperforms Jest. Later, I decided to try Playwright to write a few End-to-End tests—once again, a tool that was new to me. I grasped the basics quickly and got tests running in the background.

## The Deployment

### Containerisation

I’ve used Docker in the past, taken a few courses, and I am comfortable with the tool. So, again, this was another first. It was a great time to containerize my application. The process was straightforward—I found a Dockerfile for containerising a Vue app online and used that as a reference for PrevueMD. While I wasn’t fluent in YAML syntax, I understood the concepts and could figure things out as I went along.

### DevOps

Initially, I had planned to host my container on ACR and deploy it on Azure. However, I ran into some issues with my billing account, so I decided to go with Google Cloud Platform instead. A quick ChatGPT search gave me a condensed list of steps to deploy a container on GCP, and the process was pretty direct.

I had to do some additional research to set up my pipelines/workflows. The Docker container deployment itself was straightforward, and my app was deployed within minutes. However, I encountered some issues with authentication during the build process. To make it short, the google cloud workflow available on GitHub Actions was using a specific auth method which was not working on my GCP account as it required an organization, which I did not have. After switching to a different auth method, everything worked as expected.

Now, every time I push changes to my main branch, the CI and CD [workflow](https://github.com/wazeerc/PrevueMD/blob/main/.github/workflows/ci-cd.yml) is triggered;

1. My Unit and E2E tests are run
2. My Test Coverage results are uploaded to Coveralls
3. My Docker image is built
4. The Container is pushed to my artifact registry
5. Cloud Run deploys my app

These pipelines were very interesting to set up.

## Conclusion

I believe the best way to learn coding is to sit down and write code. I personally have been sticking to this practice for the past 5 years or so. I learn a concept, then apply it by building something. Recently, I’ve been focusing on using my skills to solve real-life problems. A great example is this PrevueMD project: I needed a better markdown editor, so I built one and made it available for everyone to use. Not only did I learn a new JavaScript library, but I also got to work with a range of tools and technologies along the way.

Did I build something revolutionary or the best app ever? Probably not, but I solved a problem I was facing and developed a solution I would personally use. I came across a post on LinkedIn the other day that said something like, “If you can't turn your ideas into reality in this day and age, where GenAI is at your fingertips, you might never be able to do it.” This resonated with me.

Throughout this project, I kept a "I'll figure it out" mentality. I faced many challenges—being new to Vue, struggling with testing, encountering performance issues, having trouble deploying on GCP, and dealing with other minor setbacks—but I figured them out. Whether by watching tutorials, asking ChatGPT for help, or reading through documentation, I figured it out.

Going back to the gist of this article - Vue.js has been a great experience for me so far. I really like its approach and simplicity. I am looking forward to exploring the ecosystem and trying out Nuxt. Overall, Vue has definitely earned a spot in my toolkit.
