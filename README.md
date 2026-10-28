# Landing Optimizer Demo

Standalone copy of the Launchly mock landing page. Plain HTML, CSS and JavaScript, served by Nginx. No npm install, backend or sibling repositories required.

## Publish through GitHub + Google Cloud

1. Create an **empty GitHub repository** named `landing-optimizer-demo` (skip GitHub's README/gitignore initialization). From this directory, push the local repository:

   ```powershell
   git add .
   git commit -m "Add standalone demo website"
   git remote add origin https://github.com/YOUR_USERNAME/landing-optimizer-demo.git
   git push -u origin main
   ```

2. In [Google Cloud Console](https://console.cloud.google.com/run), select a project with billing enabled. Open **Cloud Run → Connect repository**. Select **Cloud Build**, connect GitHub and authorize this repository. Enable the requested APIs and grant the required build/deploy permissions when prompted.
3. Configure branch `^main$`, build type **Dockerfile**, and Dockerfile location `/Dockerfile` at the repository root.
4. Name the service `landing-optimizer-demo`, choose a nearby region, select **Allow public access** (unauthenticated), and set the container port to **8080**. Use request-based billing and minimum instances **0**; maximum instances **1** is sufficient for this demo.
5. Click **Create**. After the build/deployment completes, open the service's HTTPS `run.app` URL.

Future pushes to `main` automatically build and deploy. Google manages HTTPS; no GitHub Actions workflow or Google credentials in this repository are needed. Cloud Run, Cloud Build and image storage can incur charges.

See Google's [continuous deployment instructions](https://docs.cloud.google.com/run/docs/continuous-deployment) for the current console flow and required permissions.

## Preview locally

With Python installed, run `python -m http.server 8080` here and open `http://localhost:8080`.

To test the deployment container with Docker:

```powershell
docker build -t landing-optimizer-demo .
docker run --rm -p 8080:8080 landing-optimizer-demo
```

## Editing and tracking

Edit `index.html`, commit and push. CSS and JavaScript are inside that file. This copy is independent of the original snippet demo; changes do not synchronize.

The signup form is a mock: it displays a message but sends no email and stores no submission.

The original embedded tracking snippet pointed at localhost and an existing site's keys, so it was removed. To enable Landing Optimizer tracking, paste a newly generated site snippet before `</head>`, use publicly reachable **HTTPS** SDK/API URLs, and allow the deployed website's exact origin in both the site's origin list and the API's CORS configuration. Deploying this repository only hosts the mock website.
