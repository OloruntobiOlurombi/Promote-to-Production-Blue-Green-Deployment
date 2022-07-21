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

### STESPS

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

