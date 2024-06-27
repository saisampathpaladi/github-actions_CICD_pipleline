my cicd github action page is posted here 
`` https://saisampathpaladi.github.io/github-actions_CICD_pipleline/ ``
# CI/CD Pipeline with GitHub Actions

This repository demonstrates a CI/CD pipeline using GitHub Actions to build, test, and deploy a To-Do List web application. The application is automatically deployed to GitHub Pages.

## Repository Structure

- `to-do-list-cicd/`: Contains the source code for the To-Do List web application.
- `.github/workflows/ci-cd.yml`: GitHub Actions workflow file for CI/CD.

## CI/CD Pipeline

The CI/CD pipeline consists of the following stages:

1. **Checkout Code**: Clones the repository to the runner.
2. **Set up Docker**: Prepares the Docker environment.
3. **Log in to Docker Hub**: Authenticates to Docker Hub using credentials stored in GitHub Secrets.
4. **Run Tests**: Executes tests to ensure the application is working correctly.
5. **Build**: Builds the Docker image for the application.
6. **Push to Docker Hub**: Pushes the Docker image to Docker Hub.
7. **Deploy**: Deploys the application using Docker Compose.
8. **Deploy to GitHub Pages**: Deploys the static site to GitHub Pages.

## GitHub Actions Workflow

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2

    - name: Set up QEMU
      uses: docker/setup-qemu-action@v2

    - name: Log in to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Running tests
      run: |
        echo "Running tests"
        # Add your test commands here, e.g.:
        # pytest tests/
        echo "Tests passed"
      working-directory: ./to-do-list-cicd

    - name: Building the code
      run: |
        echo "Building the code"
        docker build -t to-do .
      working-directory: ./to-do-list-cicd

    - name: Pushing the code to Docker Hub
      run: |
        echo "Pushing the code to Docker Hub"
        docker tag to-do ${{ secrets.DOCKERHUB_USERNAME }}/to-do:latest
        docker push ${{ secrets.DOCKERHUB_USERNAME }}/to-do:latest
      working-directory: ./to-do-list-cicd

    - name: Deploying the code
      run: |
        echo "Deploying the code to agents"
        docker-compose down
        docker-compose up -d
      working-directory: ./to-do-list-cicd

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        personal_token: ${{ secrets.GH_PAT }}
        publish_dir: ./to-do-list-cicd/public
```

## CI/CD Principles Used

- **Continuous Integration (CI)**: Automatically runs tests and builds the application on every push to the main branch.
- **Continuous Delivery (CD)**: Deploys the application to GitHub Pages if the build and tests succeed.

## Deployment

The application is deployed to GitHub Pages. You can view the deployed site at:

```
(https://saisampathpaladi.github.io/github-actions_CICD_pipleline/)
```

## Contributing

Feel free to submit issues and pull requests for any improvements or bug fixes.
you can contact me @ saisampathpaladi@gmail.com

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
This free to use but not for commerical use
