# Generic Promotion Workflows

This repository provides a streamlined and automated approach to managing code promotions across development, uat, and production environments in a Node.js/npm ecosystem. It leverages GitHub Actions workflows to ensure consistency, quality, and efficiency in the software delivery pipeline.

## Key Features and Benefits

1. **Automated Workflows**:
   - Automates the process of code promotion, reducing manual intervention and human error.
   - Ensures that all necessary checks (e.g., tests, bake times) are performed before promoting code to the next environment.

2. **Consistency Across Environments**:
   - Standardizes the promotion process for `dev`, `uat`, and `main` branches.
   - Enforces branch naming conventions and validation to maintain repository hygiene.

3. **Quality Assurance**:
   - Runs unit tests and end-to-end tests at every stage to ensure code quality.
   - Includes bake times to monitor stability before further promotion.

4. **Flexibility and Customization**:
   - Supports manual triggers for workflows (e.g., UAT promotion).
   - Allows for scheduled promotions (e.g., bi-weekly UAT promotions).

5. **Collaboration and Transparency**:
   - Automatically notifies reviewers and creates pull requests for peer reviews.
   - Generates detailed reports and GitHub releases for production deployments.

6. **Scalability**:
   - Designed to be easily adaptable for other projects or teams with similar promotion needs.

By using this repository, teams can achieve faster, more reliable, and transparent software delivery, ultimately improving productivity and reducing time-to-market.

## Workflows

### 1. Pull Request Workflow

**File:** `.github/workflows/pull_request.yml`

- **Trigger:** On pull request creation targeting the `dev` branch.
- **Steps:**
  1. Checkout code.
  2. Validate branch name (must start with `feature-` or `issue-`).
  3. Set up Node.js (version 24).
  4. Install dependencies.
  5. Run tests.
  6. Notify reviewers.

### 2. Development Promotion Workflow

**File:** `.github/workflows/dev_promote.yml`

- **Trigger:** When a pull request is merged into the `dev` branch.
- **Steps:**
  1. Checkout code from the `dev` branch.
  2. Set up Node.js (version 24).
  3. Install dependencies.
  4. Run unit tests.
  5. Run end-to-end tests.
  6. Bake time (30 minutes).
  7. Trigger the uat promotion workflow.

### 3. UAT Promotion Workflow

**File:** `.github/workflows/uat_promote.yml`

- **Trigger:** Automated every 2 weeks (bi-weekly on Thursday at 9:00 AM UTC) or manually via `workflow_dispatch`.
- **Steps:**
  1. Checkout code from the `dev` branch.
  2. Set up Node.js (version 24).
  3. Install dependencies.
  4. Run unit tests.
  5. Run end-to-end tests.
  6. Bake time (6 hours).
  7. Create a pull request to promote changes from `dev` to `uat` (requires 2 peer reviews).

### 4. Production Release Workflow

**File:** `.github/workflows/prod_release.yml`

- **Trigger:** When a pull request is merged into the `main` branch.
- **Steps:**
  1. Checkout code.
  2. Set up Node.js (version 24).
  3. Start a 24-hour bake time.
  4. Deploy to production.
  5. Create a GitHub release.

## Repository Structure

```
.
├── .github/
│   └── workflows/
│       ├── pull_request.yml
│       ├── dev_promote.yml
│       ├── uat_promote.yml
│       └── prod_release.yml
└── README.md
```

## Prerequisites

- Node.js (version 24)
- npm
- GitHub repository with appropriate permissions for workflows

## Usage

1. **Pull Request Workflow:**
   - Create a pull request from a feature or issue branch to the `dev` branch.
   - The workflow will validate the branch name, run tests, and notify reviewers.

2. **Development Promotion Workflow:**
   - Merge the pull request into the `dev` branch.
   - The workflow will run tests, perform a bake time, and trigger the uat promotion workflow.

3. **UAT Promotion Workflow:**
   - Manually trigger the workflow via `workflow_dispatch`.
   - The workflow will run tests, perform a bake time, and create a pull request to promote changes to the `uat` branch (requires 2 peer reviews).

4. **Production Release Workflow:**
   - Merge the uat pull request into the `main` branch.
   - The workflow will perform a bake time, deploy to production, and create a GitHub release.

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make your changes and commit them with clear messages.
4. Open a pull request to the `main` branch.

## License

This project is licensed under the MIT License. See the LICENSE file for details.
