---
title: "What is Context? — It's Everywhere."
description: 'In this article, I will explain what the Context class, commonly used in Android applications, is and how it is used.'
summary: 'Context provides the environment and features of our application. Preferences of the application are stored here, and resources and functions specific to the application are accessed through this interface.'
image: https://miro.medium.com/v2/resize:fit:4800/format:webp/1*ka8-i-vA0oJA6dPvStcsDw.jpeg

date: 2021-04-03T12:00:00+03:00
draft: false

tags: ['android', 'context', 'application', 'activity', 'service']
showTags: true

readTime: true
math: false
hideBackToTop: true
---

![Photo by Sam Kolder on Pexels](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*ka8-i-vA0oJA6dPvStcsDw.jpeg#full "[Photo by Sam Kolder on Pexels](https://www.pexels.com/photo/three-men-standing-beside-waterfalls-2387873/)")

# "Context… It… Is Everywhere"

The concept of Context appears everywhere. Whether we want to **retrieve images** embedded in our application, **create a user interface element**, or **launch a new activity**, the Android system requires a context from us.

# "Context is the Essence of the Android System"

Context is an **abstract class** that globally contains the **contextual** information of the application. This class is provided to applications by the Android system and allows them to perform many tasks such as **starting intents**, **running certain services**, and **accessing resources**. We can think of Context as a **gateway** or an **intermediary** that enables these operations.

Almost all classes in the Android system are built upon this class, either directly or indirectly, and benefit from its features. The three most commonly used classes built upon Context are **Activity, Application, and Service.**

![Relationship between Activity-Context Classes](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*h8xRIQmweLBQt_ktA6U0fw.jpeg)
![Relationship between Application-Context Classes](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*5V32sRKOSiFcQOgRZC4DIg.jpeg)
![Relationship between Service-Context Classes](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*15LQI6yqyhFHKnPFEDMn2A.jpeg)

# Where/How Do We Use Context?

The Context class provides many functions. When we want to retrieve an image, if we are within a class that extends Context (e.g., Activity, Application, Service, etc.), we can directly use `getString(int)` or `getDrawable(int)`. However, if we are in a class that does not extend Context (e.g., Adapter, ViewHolder, etc.), we need to find a Context object and use `context.getString(int)` or `context.getDrawable(int)`. Regardless of how we use it, I will demonstrate it as `Context#getString(int)`. Here are three primary uses:

## 1- Starting an Intent

As mentioned above, depending on where we are, we can use `Context#startActivity(Intent)`. This command **starts a new Activity** and adds it to the top of the activity stack.

## 2- Using a Resource

When we want to retrieve an **array or image** embedded in our application, we can directly use `Context#getString(int)` or `Context#getDrawable(int)`.

## 3- Accessing a System Service

For example, if we want to create a notification, we need to use a service provided by the **Android system**. We need to access this service through Context. We use it as follows: `Context#getSystemService(Class)`. Creating a notification is just one example. If you want to discover more services, [click here](https://developer.android.com/reference/android/content/Context#getSystemService(java.lang.Class%3CT%3E)).

# What Are My Practices?

I don't do anything drastically different from other developers. Here are a few unique practices:

First, let's talk about unnecessary Context parameters. When creating a UI element within the `Adapter#onCreateViewHolder(ViewGroup, int)` function inside RecyclerView, I use the **Context within the parent** provided in the first parameter instead of sending it again when creating the adapter, thus saving memory. In ViewHolder, I use the **Context within the view** provided to perform any actions. This way, I don't have to pass Context to all my adapter structures each time.

Second, and finally, a suggestion for making our code look cleaner. Using some functions under Context can make the code harder to read. To prevent this, I try to use the **ContextCompat** library as much as possible. For example, instead of writing `mContext.getColor(int)`, I write `ContextCompat.getColor(Context, int)`. This makes the code much more readable.

---
Read more:
* [Android — Concepts of Context and Application](https://medium.com/@tugcekolcu/android-context-ve-application-kavramlar%C4%B1-7f33d0b0bcc6)
* [Context - Android Developers](https://developer.android.com/reference/android/content/Context)