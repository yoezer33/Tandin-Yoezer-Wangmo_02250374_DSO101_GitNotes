# Self Exploration — Google Cloud Platform (GCP)

**Student:** Tandin Yoezer Wangmo
**Student ID:** 02250374
**Module:** DSO101 — Continuous Integration and Continuous Deployment
**Topic:** Cloud Platform Exploration — Google Cloud Platform (GCP)

---

## What is Google Cloud Platform?

Google Cloud Platform (GCP) is a cloud computing platform made by Google. Just like Render.com hosts your apps online, GCP does the same thing — but on a much larger, more powerful scale. GCP gives you access to the same infrastructure that Google uses to run YouTube, Gmail, and Google Search. Instead of buying your own servers, you rent computing power, storage, and networking from Google and pay only for what you use.

In simple terms: GCP is like a giant computer in the sky that you can use to run your applications, store files, manage databases, and automate deployments — all without owning any physical hardware.

---

## GCP vs Render.com — What is the Difference?

| Feature | Render.com | Google Cloud (GCP) |
|--------|------------|-------------------|
| Difficulty | Beginner-friendly | Intermediate to advanced |
| Setup | Very simple, few clicks | More configuration needed |
| Services | Web apps, databases, Docker | 200+ services (compute, AI, storage, networking…) |
| Free tier | Limited free plan | $300 free credits for new users |
| Best for | Small projects, students | Large-scale, production-grade apps |
| CI/CD support | Built-in auto-deploy | Cloud Build, Cloud Run, GKE |
| Control | Less control | Full control over everything |

**The short answer:** Render is like a simple ready-made kitchen where you just cook. GCP is like owning the entire restaurant — you control everything but you also have to set more things up yourself.

---

## Key GCP Services (What You Need to Know)

### 1. Google Cloud Run
Cloud Run is the GCP service most similar to Render. You give it a Docker image and it runs your container in the cloud. It scales automatically — if more people visit your app, Cloud Run starts more containers. If nobody is using it, it scales down to zero so you are not charged.

**This is what you would use instead of Render for deploying Docker containers.**

### 2. Google Cloud Build
Cloud Build is GCP's built-in CI/CD tool — similar to GitHub Actions or Jenkins. It automatically builds your Docker image, runs tests, and deploys your app whenever you push code to GitHub. You define the steps in a file called `cloudbuild.yaml`.

**This is what you would use instead of GitHub Actions for automating your pipeline.**

### 3. Google Container Registry (GCR) / Artifact Registry
This is GCP's version of Docker Hub. Instead of pushing your Docker images to Docker Hub, you push them to GCR. The images are stored privately inside your GCP project.

**This is what you would use instead of Docker Hub for storing images.**

### 4. Google Compute Engine (GCE)
A virtual machine (VM) in the cloud — like a real computer you can log into and control. You choose the operating system, CPU, and memory. You can install Jenkins, Node.js, or anything else on it. This is the most flexible option but also requires the most setup.

### 5. Google Kubernetes Engine (GKE)
Kubernetes is a system for managing many containers at once across multiple machines. GKE is Google's managed Kubernetes service. It is used for large applications that need high availability and can handle heavy traffic. This is advanced — not needed for beginner projects.

### 6. Cloud Storage
Like Google Drive but for your application. Used to store files, images, backups, and build artifacts. Very cheap and reliable.

### 7. Cloud SQL
GCP's managed database service — similar to Render's PostgreSQL. You can create a PostgreSQL or MySQL database and GCP manages the backups, updates, and scaling for you.

---

## How GCP Fits Into a CI/CD Pipeline

Here is how you would replace Render + GitHub Actions with a full GCP pipeline:

```
Push code to GitHub
        ↓
Cloud Build detects the push
        ↓
Cloud Build builds Docker image
        ↓
Image pushed to Artifact Registry (GCP's Docker Hub)
        ↓
Cloud Run pulls the new image
        ↓
App is live at https://your-app.run.app
```

Compare this to your Assignment 3 pipeline:
```
Push code to GitHub
        ↓
GitHub Actions detects the push
        ↓
GitHub Actions builds Docker image
        ↓
Image pushed to Docker Hub
        ↓
Render pulls the new image via deploy hook
        ↓
App is live at https://your-app.onrender.com
```

The concept is exactly the same — GCP just uses its own tools at each step.

---

## GCP Free Tier — What You Get for Free

Google gives every new user **$300 in free credits** that last 90 days. You can use these credits to try any GCP service. There is also an **Always Free** tier that never expires:

| Service | Free allowance |
|---------|---------------|
| Cloud Run | 2 million requests per month |
| Cloud Build | 120 build-minutes per day |
| Cloud Storage | 5 GB storage |
| Compute Engine | 1 small VM (e0-micro) per month |
| Cloud SQL | Not included in always-free |

---

## How to Get Started with GCP (Step by Step)

**Step 1 — Create a Google account**
Go to [cloud.google.com](https://cloud.google.com) and sign in with a Google account.

**Step 2 — Create a project**
GCP organises everything into projects. Click **New Project**, give it a name like `dso101-exploration`, and click Create.

**Step 3 — Enable billing**
You need to add a credit/debit card to unlock free credits. You will not be charged unless you manually upgrade and exceed the free tier.

**Step 4 — Open Cloud Shell**
GCP has a built-in terminal in the browser called Cloud Shell. Click the terminal icon at the top right. It gives you a Linux terminal with `gcloud` (GCP's command-line tool) already installed — no setup needed.

**Step 5 — Deploy a container to Cloud Run**
```bash
# Deploy a sample hello-world container
gcloud run deploy hello-world \
  --image gcr.io/cloudrun/hello \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated
```

GCP gives you a live URL instantly: `https://hello-world-xxxx.run.app`

---

## Key GCP Commands (gcloud CLI)

| Command | What it does |
|---------|-------------|
| `gcloud auth login` | Log in to your GCP account from the terminal |
| `gcloud config set project PROJECT_ID` | Set which GCP project you are working in |
| `gcloud run deploy` | Deploy a container to Cloud Run |
| `gcloud run services list` | List all your Cloud Run services |
| `gcloud builds submit` | Trigger a Cloud Build manually |
| `gcloud container images list` | List Docker images in your registry |
| `gcloud sql instances list` | List your Cloud SQL databases |

---

## Key Terms for the Viva

| Term | Simple explanation |
|------|-------------------|
| GCP | Google's cloud platform — rent servers, storage, and tools from Google |
| Cloud Run | Run Docker containers on GCP — scales automatically, similar to Render |
| Cloud Build | GCP's CI/CD tool — builds and deploys code automatically, similar to GitHub Actions |
| Artifact Registry | GCP's image registry — stores Docker images, similar to Docker Hub |
| Compute Engine | A full virtual machine (VM) on GCP — like renting a real computer |
| GKE | Google Kubernetes Engine — manages many containers at scale |
| Cloud SQL | Managed database on GCP — similar to Render's PostgreSQL |
| Region | The physical location of the data centre where your app runs (e.g. `us-central1`, `asia-east1`) |
| Project | A container in GCP that groups all your services, billing, and permissions together |
| gcloud | The command-line tool for controlling GCP from a terminal |
| IAM | Identity and Access Management — controls who has permission to do what in GCP |
| $300 credits | Free money GCP gives new users to try out services for 90 days |

---

## What I Learned from GCP Exploration

- **GCP and Render solve the same problem** — hosting apps in the cloud — but GCP gives you far more control and far more services to work with.
- **Cloud Run is the simplest GCP service for deploying containers** — it works very similarly to Render. You give it a Docker image and it runs it for you with auto-scaling.
- **Cloud Build replaces GitHub Actions** — it is GCP's own CI/CD tool. It reads a `cloudbuild.yaml` file and automatically builds, tests, and deploys your code.
- **Everything in GCP is a service** — storage, databases, networking, security, AI — each has its own product. You only enable and pay for what you need.
- **The free tier is generous for learning** — $300 credits plus the always-free tier means you can explore almost everything without paying anything.
- **GCP uses regions** — you choose which physical data centre your app runs in. Choosing a region close to your users makes the app faster.
- **IAM controls security** — every person or service that accesses GCP needs the right permissions. This is important for keeping your project secure.

---

## Comparison: GCP vs Render vs AWS vs Azure (Big Picture)

| Platform | Best known for | Difficulty | Free tier |
|----------|---------------|------------|-----------|
| Render.com | Simple app hosting, student projects | Very easy | Limited |
| Google Cloud (GCP) | Data, AI/ML, Kubernetes, global scale | Medium | $300 credits |
| AWS (Amazon) | Largest cloud, most services | Medium–Hard | 12 months free tier |
| Microsoft Azure | Enterprise, Windows, Microsoft tools | Medium–Hard | $200 credits |

All four platforms can host Docker containers and support CI/CD pipelines. GCP stands out for its AI and data tools, and its Kubernetes engine (GKE) is considered the best managed Kubernetes service available.

---


