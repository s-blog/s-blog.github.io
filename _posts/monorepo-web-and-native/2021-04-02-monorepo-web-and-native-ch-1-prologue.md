---
date: 2021-04-02 12:00:00
tags: react reactnative typescript
serie: monorepo-web-and-native
title: Monorepo R+RN, Ch 1
display-title: Ch 1 - Prologue
---
<p>The goal of this journal is to highlight my process and challenges to construct a production ready, monorepo web/mobile platform, using React and React Native.</p>
<!--more-->
<p>This is also meant to be a learning exercise for integrating and configuring many related popular libraries. I will be highlighting the steps to configure my preferred tools throughout this tutorial. However feel free to swap out any piece of technology as it suits your needs. I will provide links to the articles I’ve found useful as we go through.</p>
<p>In a brief breakdown, let’s review what we will be using.</p>

<u class="faded-highlight">core of our project</u>
<br/>React - for our web application
<br/>React Native - for our iOS and Android mobile applications
<br/>Webpack - the bundler of our choice 
<br/>Metro - the bundler without a choice
<br/>Babel and Typescript - for transpiling and type checking
<br/>ESlint and Prettier - for linting and formatting
<br/>Jest, Detox and Enzyme - for testing

<u class="faded-highlight">client end choices</u>
<br/>Redux via Redux Toolkit - for state management
<br/>React Router - for navigating this mess
<br/>Styled Components - for fancy shmancy components
<br/>Material UI - for boring components
<br/>Formik and Yup - for forms and their validation
<br/>Mapbox - for map and location interactions
<br/>I18n-js - for internationalization and dictionary locales

<u class="faded-highlight">infrastructure choices</u>
<br/>AWS Amplify - for authentication (swap with Auth provider of your choice)
<br/>GraphQL - for our queries (swap with DB of your choice)
<br/>Apollo - for caching query data (also to sync user input in lack of connectivity)
<br/>Bitbucket - for source code management
<br/>CircleCI - for continuous integration pipeline
<br/>Docker and AWS - for deploymentMixpanel - for analytics
<br/>Optimizely - for A/B testing
<br/>Segment - for metrics integration

<p>To keep things tidy we will be using a branching workflow with frequent commits. Maintaining and growing our test suite will be a priority. As in agile best practices, we will limit our work in progress to a single task, and adhere to a strict definition of done so we don’t need to backtrack.</p>
<u class="faded-highlight">Our “Definition of Done” checklist</u>
<br/>prepare tests (unit/integration/e2e)
<br/>prototype the featuredeploy for review
<br/>write release notes
<br/>refactor & push to production

<p>It’s already a nauseating list, we have some work to do.</p>
So let’s get to it.‍