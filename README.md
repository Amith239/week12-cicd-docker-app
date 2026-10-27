# Week 12 AWS CI/CD Docker App

**Suggested GitHub repository name:** `week12-cicd-docker-app`

A small Flask application for the Week-12 lab. AWS CodePipeline pulls this repository, AWS CodeBuild builds a Docker image, and the buildspec pushes the image to Amazon ECR.

## Files

- `app.py` — Flask web app with `/` and `/health` endpoints
- `requirements.txt` — Python dependencies
- `Dockerfile` — packages the app into a container
- `buildspec.yml` — CodeBuild build and ECR push instructions
- `.gitignore` — excludes local Python files

## Upload to GitHub

1. Sign in to GitHub and click **New repository**.
2. Repository name: `week12-cicd-docker-app`.
3. Choose Public or Private according to your lab instructions.
4. Create the repository. If uploading files through the browser, use **Add file → Upload files**.
5. Upload all files in this folder, including `buildspec.yml` and `Dockerfile`, then commit to the `main` branch.

## AWS setup needed before the build will succeed

1. Create a **private ECR repository** named `week12-docker-app` in `ap-south-1`.
2. In CodeBuild, use an environment that supports Docker builds and enable **Privileged** mode.
3. Configure these CodeBuild environment variables:
   - `AWS_DEFAULT_REGION` = `ap-south-1`
   - `IMAGE_REPO_NAME` = `week12-docker-app`
   - `AWS_ACCOUNT_ID` = your 12-digit AWS account ID (optional; the buildspec can discover it)
4. The CodeBuild service role needs permission to get an ECR login token and push image layers/manifests to this repository. It also needs the normal CloudWatch Logs permissions.
5. Create the CodePipeline source stage using this GitHub repository and branch `main`, then use this CodeBuild project for the build stage.

## Test the app locally (optional)

```bash
pip install -r requirements.txt
python app.py
```

Open `http://127.0.0.1:5000/` or `http://127.0.0.1:5000/health`.

## Notes

- The ECR repository name in AWS must match `IMAGE_REPO_NAME`.
- This pipeline builds and pushes a Docker image. It does **not** automatically deploy to ECS unless you add and configure an ECS deployment stage, task definition, and service.
- Do not upload AWS access keys, passwords, tokens, or `.env` files to GitHub.
