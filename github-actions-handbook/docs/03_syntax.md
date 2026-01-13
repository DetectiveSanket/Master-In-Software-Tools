# 03: GitHub Actions YAML Syntax Reference

The foundation of every workflow is a YAML file. Here is the reference for the most common keys.

## 🔑 Core Keywords

| Keyword | Description | Required |
| :--- | :--- | :--- |
| `name` | The name of the workflow (visible in GitHub UI). | No |
| `on` | The event that triggers the workflow. | **Yes** |
| `jobs` | A group of tasks that run in the workflow. | **Yes** |
| `runs-on` | The type of machine to run the job on. | **Yes (per job)** |
| `steps` | A sequence of tasks to execute in a job. | **Yes (per job)** |

## 📦 Event Triggers (`on`)

```yaml
on:
  # Single event
  push:

  # Multiple events
  pull_request:
    branches: [ main, develop ]
  
  # Scheduled (UTC)
  schedule:
    - cron: '30 5 * * 1,3' # Every Mon and Wed at 05:30
```

## 🛠️ Step Configuration

```yaml
steps:
  - name: Check out code
    uses: actions/checkout@v4

  - name: Set up Node.js
    uses: actions/setup-node@v4
    with:
      node-version: '20'

  - name: Install dependencies
    run: npm install

  - name: Build
    run: npm run build
```

## 🧪 Expressions and Contexts

GitHub Actions uses `${{ <expression> }}` to evaluate variables and logic.

- **Contexts**: `${{ github.repository }}`, `${{ github.sha }}`, `${{ secrets.MY_SECRET }}`
- **Operators**: `==`, `!=`, `&&`, `||`, `!`
- **Functions**: `contains()`, `startsWith()`, `hashFiles()`, `success()`, `failure()`

Example:
```yaml
if: github.ref == 'refs/heads/main'
run: echo "I am on the main branch!"
```

## 🌍 Environment Variables

```yaml
env:
  GLOBAL_VAR: "I am available everywhere"

jobs:
  my_job:
    env:
      JOB_VAR: "I am only in this job"
    steps:
      - run: echo $GLOBAL_VAR
      - env:
          STEP_VAR: "I am only in this step"
        run: echo $STEP_VAR
```

> [!CAUTION]
> Never print sensitive data or secrets to the logs. GitHub will try to mask them, but it's best practice to be careful!
