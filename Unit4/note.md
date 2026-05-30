# CI/CD and Jenkins Notes

## What is CI/CD?

CI/CD stands for Continuous Integration and Continuous Delivery (or Deployment), and it describes a modern approach to software development where code changes are built, tested, and released in an automated, repeatable way. The goal is to catch bugs early — when they're cheap to fix — and to ship software faster with far less manual effort and stress.

**Continuous Integration (CI)** 
is the practice of merging code frequently into a shared repository. Every time a developer pushes a change, an automated pipeline kicks off to build the project and run tests. This means problems are caught immediately rather than accumulating over weeks.

**Continuous Delivery (CD)** 
takes CI a step further by ensuring the codebase is always in a deployable state. The pipeline automatically prepares the release, but a human still clicks a button to push it to production — giving teams control over timing.

**Continuous Deployment** 
goes all the way: any change that passes the automated tests is deployed to production without any manual intervention at all. This works well for teams with high test confidence and a culture of small, frequent releases.

The typical pipeline flow looks like this: a developer commits code, which triggers a build, then automated tests run, then the result is deployed to a staging environment. From there, it either goes to production automatically (Continuous Deployment) or waits for a manual approval (Continuous Delivery).

---

## Jenkins Architecture

Jenkins is an open-source automation server that powers many CI/CD pipelines. It uses a **Master/Agent** model to distribute work. The **Master** (also called the Controller) is the brain of the operation and it manages jobs, hosts the web UI on port 8080, and stores all configuration. Importantly, it does not do any building itself.

The actual build and test work is handled by **Agents** (also called Nodes). Agents are separate machines that connect to the Master and execute pipeline steps. Because agents can run on different operating systems, a single Jenkins Master can orchestrate builds across Linux, Windows, and macOS simultaneously, enabling true parallel builds.

---

## The Jenkinsfile

Jenkins pipelines are defined in a file called a `Jenkinsfile`, which lives alongside your code in version control. This is strongly preferred over the older "Freestyle" job style because it makes your pipeline auditable, version-controlled, and reproducible.

A Jenkinsfile is built from a handful of core keywords. The `agent` directive tells Jenkins where to run the pipeline — on any available node, inside a Docker container, or on a node with a specific label. The `stages` block wraps all the major phases of your pipeline, and each individual `stage` (like Build, Test, or Deploy) is a named section that groups related work. Inside each stage, the actual commands live in a `steps` block. Finally, the `post` section runs after the pipeline completes, regardless of whether it passed or failed — making it the right place for cleanup tasks, notifications, or publishing test reports.

---

## Plugins

Out of the box, Jenkins is essentially an empty shell. Its real power comes from its plugin ecosystem, which extends it with integrations for source control, build tools, notifications, and more.

Some plugins are effectively mandatory for any modern pipeline. The Git plugin allows Jenkins to pull source code from GitHub or GitLab. The Pipeline plugin is what enables Jenkinsfile support in the first place. **JUnit** reads test result files and renders pass/fail graphs in the Jenkins UI. Blue Ocean replaces Jenkins' dated default interface with a cleaner, more visual pipeline view. The **Docker Pipeline** plugin lets you build and push Docker images directly from your Jenkinsfile. If your project uses Node.js, the NodeJS plugin makes `npm` commands available in pipeline steps. Finally, the Slack or Email Extension plugins let you send notifications whenever a build breaks or recovers.

Installing plugins is straightforward: navigate to Manage Jenkins -> Plugins -> Available, search for the plugin by name, and install it.

---

## Key Commands

A few commands used while working with Jenkins and Docker:

```bash
# Open the Jenkins web UI
localhost:8080

# Build a Docker image
docker build -t myapp:latest .

# Push an image to Docker Hub
docker push myusername/myapp:latest

# Authenticate with Docker Hub
docker login
```

---

## Common Errors and Fixes

Several errors come up frequently when setting up Jenkins pipelines. A No such file or directory error usually means your pipeline is running in an unexpected working directory adding a pwd step helps you debug where Jenkins thinks it is. Permission denied errors typically require either adding sudo or correcting file permissions on the server. If Jenkins can't find a credential, re-add it through the Jenkins UI and make sure the ID in your Jenkinsfile matches exactly. A failed git clone is often caused by an expired Personal Access Token or selecting the wrong stored credential. And if Docker commands fail with permission denied, the fix is to add the Jenkins user to the docker group on the server.

---

## Key Takeaways

CI automatically builds and tests your code on every commit, while CD takes the output of that process and either prepares it for deployment or deploys it outright. In a Jenkins setup, the Master coordinates everything while Agents do the heavy lifting — and that separation is what enables parallel, cross-platform builds.

Always define pipelines with a Jenkinsfile rather than Freestyle jobs; it keeps your pipeline under version control and makes it far easier to maintain. Jenkins lives at port 8080, and plugins are not optional without them, Jenkins can't do much of anything useful. Credentials should always be stored in Jenkins' credential store and never hardcoded in your pipeline file. And post { always {} } is a reliable way to publish test reports or send notifications no matter how the pipeline ends.