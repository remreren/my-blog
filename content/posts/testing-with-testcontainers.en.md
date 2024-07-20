---
title: 'Testing with Testcontainers'
description: "In this article, I will explain why Testcontainers should be used instead of H2 and what problems it solves."
summary: "Although we can write tests with tools like H2, mocks don't fully replace the real ones. This is where Testcontainers comes in."

preview:
  title: 'Testing with Testcontainers'
  description: "Although we can write tests with tools like H2, mocks don't fully replace the real ones. This is where Testcontainers comes in."
  image: 'https://miro.medium.com/v2/resize:fit:1400/format:webp/1*_foQAb0bTEbESsdKk8atnA.jpeg'

date: 2022-08-24T12:00:00+03:00
draft: false

tags: ['testing', 'testcontainers', 'integration test', 'acceptance test', 'unit test']
showTags: true

readTime: true
math: false
hideBackToTop: true
---

![Photo by Pixabay](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*_foQAb0bTEbESsdKk8atnA.jpeg#full "[Photo by Pixabay](https://www.pexels.com/photo/astronaut-graffiti-on-semi-trailers-163811/)")

# Introduction

[In our previous article](https://medium.com/emlakjet/unit-test-ve-test-türleri-3659574fec8d), we mentioned that automated tests are applied to test the features of an application after writing it. Some of these tests are unit, integration, and acceptance tests.

Since it takes a lot of resources, time, and effort to install, test, and then delete the necessary components one by one while applying these tests, solutions requiring fewer resources have been needed over time.

# Why existing solutions don't work?

One of the existing solutions is to find a different, lighter, and easier-to-use component that mimics the component and run all tests on this component. Although this seems like a sufficient solution, in large projects, there will inevitably be a feature of the component that works differently from the real one. This reduces the reliability of the tests.

For example, as you can see [here](https://stackoverflow.com/questions/71268596/h2-postgresql-dialect-not-working-with-using-keyword), the new features coming to the actual component are not synchronized with the component we use for testing. This prevents us from making improvements in our code thanks to the new features. Either we are forced to follow the technology behind or we have to choose a method that is less performant or does not provide the best performance to work in both environments.

# The emergence of Testcontainers

Recognizing such problems, Richard North developed a tool that allows us to run tests on real systems instead of mock systems, with the help of Docker. This tool is called Testcontainers.

What is Testcontainers? Testcontainers is a lightweight, disposable test platform used with JUnit. Depending on your purpose, it makes your integration and acceptance tests more reliable compared to mock methods.

The reason it makes it more reliable is, as mentioned above, the mock method does not fully replace the real component. Even if the component used for testing works 90% similar to the real one, it needs to work the same way for our tests to be reliable.

# How Testcontainers works

Testcontainers fundamentally uses Docker. Therefore, Docker must be installed on our computer. We define and start the Docker image of the component we want to test in the test class and apply our tests on it. By default, the container is closed and deleted when our tests are finished.

# Usage details

To set up Testcontainers in our project, we first need to add the Testcontainers library and, if officially supported, the components it supports to our project. If there is no official support for a component you will use, you can run your containers manually by giving parameters thanks to GenericContainer.

Since I am using PostgreSQL in my project and it is officially supported, I will add it to the dependencies. For this, we add the following code to our pom.xml file. Of course, don't forget to use the latest version. I added the sample project to the references section.

![Dependencies](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*3HVNETP0GX2i20UIznOStw.png)

To make our tests, we need to create a test class. But before doing that, I will create a repository as an example and perform the read-write test on this repository.

![Repository](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*TkAHAWukIceVi8T_uvWDOw.png)

After creating the repository, we create a test class for it. I will first do the tests with a version that has a separate database and then convert it to the Testcontainers version. After specifying the database information active in the test environment on the application.yml file, we will see the result when we run the following test.

![Test without Testcontainers](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*lqcpAol02FMhmMIxlW9jeQ.png)

So what will happen when the database in the test environment changes? For example, what will happen if we write a test based on a value in the database and someone goes and deletes that value? In such cases, a PostgreSQL container that we create with sample data will do the job perfectly.

Now let's switch this code to the Testcontainers version. We need to create a database container and overwrite its information with our settings. Of course, another option is to give a database name, login information, and user information to this new database we will create and request it to be opened with it, but the first option I mentioned is simpler with Testcontainers.

![Test with Testcontainers](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*s6-iDg-tYYL0ter5cF3gQw.png)

Thus, we wrote our first test using Testcontainers! We can improve this structure a bit more and avoid issues like containers closing and opening between tests, so we don't experience performance drops. Or we can create an SQL file for sample data and load it into the container when it opens, running our tests with the data inside.

---
# References
- [Unit Test and Test Types](https://medium.com/emlakjet/unit-test-ve-test-türleri-3659574fec8d)
- [Don't use In-Memory Databases (H2, Fongo) for Tests](https://phauer.com/2017/dont-use-in-memory-databases-tests-h2/)
- [Testcontainers](https://www.testcontainers.org/)
- [How to use Testcontainers with integration tests](https://proitcsolution.com.ve/how-to-use-testcontainers-with-integration-tests/)
- [GitHub - remreren/testcontainers-demo](https://github.com/remreren/testcontainers-demo)
