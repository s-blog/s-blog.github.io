---
date: 2021-04-02 14:00:00
tags: react reactnative typescript
serie: monorepo-web-and-native
title: Monorepo R+RN, Ch 3
display-title: Ch 3 - Automated Deployment
---
<u class="faded-highlight">Automated deployments with AWS and CircleCi<u>
<!--more-->
<p>Go on and log in to you AWS console.</p>

<p>Now we will have to,</p>

<ul>
    <li>create IAM users in AWS with S3 rights, </li>
    <li>note the access keys, </li>
    <li>create an S3 bucket for static web hosting, </li>
    <li>and later provide those keys to CircleCI as environment variables.</li>
</ul>

<p>If you have trouble with this section please take the time to check out the resources below</p>

<ul>
<li>The chapter “Deployment to AWS” in the <a href="https://medium.com/@eferhatg/create-react-app-continuous-integration-config-with-circleci-and-aws-2b0238cde169" target="_blank">this tutorial</a> by Eyup Ferhat Guducu,
</li>

<li>and <a href="https://techinscribed.com/deploy-frontend-application-to-aws-s3-using-circle-ci/">this tutorial</a> by Praveen to do this via Circle CI.
</li>

<li>You can also review CircleCi’s deployment examples for AWS and other providers.
<br/>- <a href="https://circleci.com/docs/2.0/deployment-examples/index.html#aws">https://circleci.com/docs/2.0/deployment-examples/index.html#aws</a>
<br/>- <a href="https://circleci.com/docs/2.0/env-vars/#setting-an-environment-variable-in-a-project">https://circleci.com/docs/2.0/env-vars/#setting-an-environment-variable-in-a-project</a>
</li>
</ul>

<p>We create the CircleCI config file seen below inside our root directory “.circleci/config.yml”.</p> 
<u class="faded-highlight no-underline">&lt;root>/.circleci/config.yml</u>
{% highlight yaml %}

    version: 2.1
    orbs:
     aws-cli: circleci/aws-cli@1.4.1
    jobs:
     build_web_client:
       docker:
         - image: circleci/node:10.16.3
       steps:
         - checkout
         - run:
             name: Install Dependencies
             command: cd web-client && npm install
         - run:
             name: Run build
             command: cd web-client && npm run build
         - persist_to_workspace:
             root: .
             paths:
               - .
     aws_deploy_web_client:
       executor: aws-cli/default
       steps:
         - attach_workspace:
             at: .
         - aws-cli/setup:
             profile-name: monorepo-admin
         - run:
             name: Upload file to S3
             command: cd web-client && aws s3 sync ./build/ s3://monorepo-web-client-production --acl public-read --delete
    workflows:
     version: 2
     build-deploy:
       jobs:
         - build_web_client:
             filters:
                 tags:
                   only: /.*/

         - aws_deploy_web_client:
             requires:
               - build_web_client # Only run deploy job once the build job has completed
             context: AWS_monorepo
             filters:
               branches:
                 only: main # Only deploy when the commit is on the main branch
               tags:
                 only: /.*/
{% endhighlight %}

<p>Now that we configured our project’s core, we give it a spin locally, and if all is looking good, time to push our changes.</p>

{% highlight console %}
$ git add . && git commit -m “Init React app w TS, Babel, Eslint, Webpack and CircleCI config”
{% endhighlight %}

{% highlight console %}
$ git push -u origin main
{% endhighlight %}
<p>Let’s behold the magic of automation.</p>
<ul>
    <li>Login to CircleCI using your version control system, </li>
    <li> Create a “Context” within your organization setting to set environment variables
      <br/>- AWS_ACCESS_KEY_ID
      <br/>- AWS_SECRET_ACCESS_KEY
      <br/>- AWS_DEFAULT_REGION
    </li>
    <li> find your repo listed underneath projects and click “Set Up Project”</li>
    <li> Build with your existing config.yml </li>
</ul>

<p>Hopefully shortly afterwards you will be greeted by a successful workflow in your pipeline. If you have some troubleshooting to do, don’t worry. It will surely be worth it once it’s up and running.</p>
