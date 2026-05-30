# Unit 5: Advanced Pipeline

## Two Ways to Write a Pipeline

Jenkins gives you two distinct ways to write a pipeline, and the right choice depends on how much complexity your project actually needs.

The **Declarative** style is the recommended starting point for beginners. It works by providing a set of pre-built, structured blocks that you simply fill in. You don't need to know much about the underlying Groovy language — Jenkins handles the scaffolding and you focus on what each stage should do.

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
    }
}
```

The **Scripted** style, on the other hand, exposes the full Groovy programming language and gives you complete control over every aspect of the pipeline. There are no guardrails — you write everything from scratch. This is only worth the extra complexity when your pipeline has genuinely advanced logic that Declarative syntax can't express.

```groovy
node('any') {
    stage('Build') {
        sh 'npm install'
        if (result == 'SUCCESS') {
            echo 'Good'
        }
    }
}
```

In short: if you're just starting out, use Declarative. Only reach for Scripted when you've hit a real limitation and need custom logic that can't be handled any other way.

---

## Why Pipelines Are Written as Code

Storing your pipeline as a `Jenkinsfile` in version control isn't just a convenience — it's a best practice with real benefits. Because the file lives alongside your source code, every member of the team can read and understand exactly how the pipeline works. Changes to the pipeline go through the same code review process as any other change, which means mistakes get caught before they cause problems. And if something does go wrong, rolling back is as simple as reverting the file. On top of all that, Jenkins reads the `Jenkinsfile` automatically every time you push, so the pipeline is always in sync with your code without any manual reconfiguration.

---

## Useful Pipeline Tricks

### Running Things in Parallel

By default, pipeline stages run sequentially — one finishes before the next begins. The `parallel` directive changes this by letting multiple stages run at the same time. If you have several independent test suites, for example, running them in parallel can cut your total pipeline time significantly rather than waiting for each one to complete in turn.

### Controlling When a Stage Runs

Not every stage should run on every branch. The `when` directive lets you attach conditions to a stage so it only executes in certain situations. A common pattern is restricting the deploy stage to the `main` branch — that way, developers pushing to feature branches will still get their tests run, but the deployment step is skipped entirely until the code is actually ready for production.

### Cleaning Up with `post`

The `post` section runs after the rest of the pipeline finishes, and it runs regardless of whether the pipeline passed or failed. This makes it the right place to put anything that should always happen — publishing test reports, sending notifications, or cleaning up temporary files. This was used in Assignment 2 to ensure Jest test results were always published even when the build failed.

### Reusing Values with `environment`

The `environment` block lets you define a variable once at the top of your pipeline and reference it anywhere below. Instead of repeating a value like an app name or a registry URL in multiple places, you define it once and use it with `${VARIABLE_NAME}` syntax — for example, `echo "Building ${APP_NAME}"`. This makes pipelines easier to read and much easier to update.

### Keeping Secrets Safe with `credentials`

Passwords, API keys, and other sensitive values should never be written directly into a `Jenkinsfile`. Once that file is pushed to GitHub, anyone with access to the repository can see it. The correct approach is to store credentials inside Jenkins' credential store and reference them by ID in your pipeline.

```groovy
environment {
    DOCKER_CREDENTIALS = credentials('docker-creds')
}
```

When Jenkins resolves a credential like this, it automatically provides two safe environment variables — `DOCKER_CREDENTIALS_USR` for the username and `DOCKER_CREDENTIALS_PSW` for the password — without ever exposing the raw values in your code or logs.

---
