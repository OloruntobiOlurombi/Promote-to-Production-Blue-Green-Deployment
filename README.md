# Topic: Promote-to-Production-Blue-Green-Deployment

### Overview

> In this project we will write a set of jobs that promotes a new environment to production and decommissions the old environment in an automated process without any downtime.

#### Prerequisites

- You should have finished the previous project below:

> Project: Remote Control Using Ansible,
> Project: Infrastructure Creation,
> Project: Config and Deployment, and
> Project: Rollback

#### Instructions

> There are a few manual steps we need to take to "prime the pump". These manual steps enable steps that will be automated later.

##### Let The Fun Begins!!!

### STEPS

##### Step 1: Create an S3 bucket (Manually)

> Create a public S3 Bucket (e.g., mybucket644752792305) manually. If you need help with this, follow the instructions found in the documentation.
> Create a simple HTML page (version 1) and name it index.html. It could be as simple as:

```
<!DOCTYPE html>
<html>
  <head>
      <title>Version 1</title>
  </head>

  <body>
      <h1>Hello World - version 1</h1>
  </body>
</html>
```

> Upload the index.html page to your bucket. Follow these steps if you need help. Make sure you can browse to the home page.
> Enable the Static website hosting in that bucket and set index.html as default page of the website. See the snapshot below.

<img width="1141" alt="Screen Shot 2022-07-21 at 10 59 18 AM" src="https://user-images.githubusercontent.com/40290711/180282460-5d43afe4-a12b-495a-84f6-127c7ee1e870.png">


