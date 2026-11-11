37. Testing in CI/CD (GitHub Actions, Jenkins)

# Testing in CI/CD (GitHub Actions, Jenkins)

> **Goal:** every time you push code, a robot runs your tests automatically. If a test fails, the code cannot be merged. By the end of this topic you will have a working pipeline in both GitHub Actions and Jenkins.

---

## 1. What Is CI/CD? (in 1 minute)

| Term | Simple meaning |
|---|---|
| **CI** (Continuous Integration) | On every push or pull request: build the code and run the tests automatically |
| **CD** (Continuous Delivery/Deployment) | If tests pass, deploy the app automatically (or with one click) |

**The flow you are building:**

```
git push  -->  pipeline starts  -->  build  -->  unit tests  -->  integration tests  -->  report  -->  green or red
```

**Why it matters:** "It works on my machine" stops being an excuse. Broken code is caught before it reaches `main`.

---

## 2. Before You Start (Checklist)

- [ ] Your project has tests that run from one command:
  - Java: `mvn verify`
  - Node.js: `npm test`
  - Python: `pytest`
- [ ] The project is on GitHub
- [ ] Tests do not depend on anything installed only on your laptop

---

## 3. GitHub Actions

### 3.1 Key words (only 4 to remember)

| Word | Meaning |
|---|---|
| **Workflow** | A YAML file in `.github/workflows/` |
| **Trigger (`on`)** | When it runs: push, pull request, schedule |
| **Job** | A group of steps that runs on one machine (runner) |
| **Step** | One command (`run`) or one ready-made action (`uses`) |

### 3.2 Your first pipeline: Java + Maven + JUnit

Create the file `.github/workflows/ci.yml`:

```yaml
name: Java CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get the code
        uses: actions/checkout@v4

      - name: Set up Java 21
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '21'
          cache: maven          # caches dependencies = faster builds

      - name: Build and run all tests
        run: mvn -B verify

      - name: Save test reports
        if: always()            # upload even when tests fail
        uses: actions/upload-artifact@v4
        with:
          name: test-reports
          path: |
            target/surefire-reports
            target/failsafe-reports
            target/site/jacoco
```

**Try it:** commit this file, push, and open the **Actions** tab on GitHub. A yellow dot means running, green means passed, red means failed. Click a failed run to read the logs.

### 3.3 Node.js (Jest or Vitest)

```yaml
name: Node CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [20, 22]          # runs the job once per Node version
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
          cache: npm

      - run: npm ci             # clean, repeatable install
      - run: npm test -- --coverage
```

### 3.4 Python (pytest)

```yaml
name: Python CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
          cache: pip

      - run: pip install -r requirements.txt
      - run: pytest --cov=app --cov-fail-under=80
```

Change `--cov=app` to your package folder name.

### 3.5 Testcontainers in GitHub Actions

Good news: `ubuntu-latest` already has Docker, so Testcontainers tests work with **no extra setup**. Just run `mvn -B verify` (Java) or `npm test` (Node).

### 3.6 Need a real database? Use a service container

If you are not using Testcontainers, GitHub can start a database next to your job:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: test
          POSTGRES_DB: testdb
        ports: ['5432:5432']
        options: >-
          --health-cmd "pg_isready -U postgres"
          --health-interval 5s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v4
      # ... your setup and test steps
```

Your app connects to `localhost:5432`.

### 3.7 Secrets (never put passwords in YAML)

1. GitHub repo, then **Settings**, then **Secrets and variables**, then **Actions**, then **New repository secret**
2. Use it in the workflow:

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

### 3.8 Add a status badge to your README

```markdown
![CI](https://github.com/YOUR-USER/YOUR-REPO/actions/workflows/ci.yml/badge.svg)
```

### 3.9 Block bad code from `main` (branch protection)

1. Repo **Settings**, then **Branches**, then **Add branch protection rule** for `main`
2. Tick **Require status checks to pass before merging**
3. Select your `test` job

Now nobody can merge a pull request with failing tests.

---

## 4. Make Your Pipeline Faster and Smarter

### 4.1 Separate fast tests from slow tests

| Test type | Java naming | When it runs |
|---|---|---|
| Unit | `OrderServiceTest` | Every push, finishes in seconds |
| Integration | `OrderRepositoryIT` | Every pull request |

Java setup: add the **maven-failsafe-plugin** (it runs `*IT` classes during `verify`).

```
mvn test      # unit tests only (fast)
mvn verify    # unit + integration tests
```

Run them as two jobs so you get quick feedback first:

```yaml
jobs:
  unit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with: { distribution: temurin, java-version: '21', cache: maven }
      - run: mvn -B test

  integration:
    needs: unit               # only starts if unit tests passed
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with: { distribution: temurin, java-version: '21', cache: maven }
      - run: mvn -B verify
```

### 4.2 Fail the build if coverage is too low

**Java (JaCoCo) in `pom.xml`** (use the latest plugin version):

```xml
<plugin>
  <groupId>org.jacoco</groupId>
  <artifactId>jacoco-maven-plugin</artifactId>
  <executions>
    <execution>
      <goals><goal>prepare-agent</goal></goals>
    </execution>
    <execution>
      <id>report</id>
      <phase>verify</phase>
      <goals><goal>report</goal></goals>
    </execution>
    <execution>
      <id>check</id>
      <phase>verify</phase>
      <goals><goal>check</goal></goals>
      <configuration>
        <rules>
          <rule>
            <element>BUNDLE</element>
            <limits>
              <limit>
                <counter>LINE</counter>
                <value>COVEREDRATIO</value>
                <minimum>0.70</minimum>
              </limit>
            </limits>
          </rule>
        </rules>
      </configuration>
    </execution>
  </executions>
</plugin>
```

**Jest** in `package.json`:

```json
"jest": {
  "coverageThreshold": { "global": { "lines": 80 } }
}
```

**Vitest** in `vitest.config.js`:

```js
export default {
  test: { coverage: { thresholds: { lines: 80 } } }
};
```

**Python:** `pytest --cov=app --cov-fail-under=80` (already in the workflow above)

### 4.3 Cancel old runs when you push again

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

### 4.4 Quick speed tips

- Always use caching (`cache: maven`, `cache: npm`, `cache: pip`)
- Use `npm ci`, not `npm install`
- Add a time limit so stuck jobs do not run for hours: `timeout-minutes: 15` under the job
- Keep unit tests free of Spring context, network and database

---

## 5. Jenkins

### 5.1 Key words

| Word | Meaning |
|---|---|
| **Jenkinsfile** | A text file in your repo that describes the pipeline (like the GitHub YAML) |
| **Pipeline** | The whole automated process |
| **Stage** | One phase (Build, Test, Deploy) |
| **Step** | One command inside a stage |
| **Agent** | The machine or container where it runs |
| **Post** | What to do after: always, success, failure |

### 5.2 One-time setup

1. Install Jenkins (the Docker image is the easiest way)
2. Install plugins: **Pipeline**, **Git**, **JUnit**, **JaCoCo** (and **Docker Pipeline** if you use Docker agents)
3. **Manage Jenkins, then Tools:** add a JDK named `JDK21` and Maven named `Maven3`

### 5.3 Your first Jenkinsfile (Java + Maven)

Create `Jenkinsfile` in the root of your repo:

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK21'
    }

    options {
        timeout(time: 15, unit: 'MINUTES')
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn -B test'
            }
        }

        stage('Integration Tests') {
            steps {
                sh 'mvn -B verify'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
        success {
            echo 'All tests passed'
        }
        failure {
            echo 'Build failed, check the test report'
        }
    }
}
```

> Using Testcontainers in Jenkins? The Jenkins agent must be able to run Docker (the Jenkins user needs access to the Docker daemon).

### 5.4 Run the pipeline

1. Jenkins dashboard, then **New Item**, enter a name, choose **Pipeline** (or **Multibranch Pipeline**), then **OK**
2. Under **Pipeline**, choose **Pipeline script from SCM**, select **Git**, and paste your repo URL
3. Set the script path to `Jenkinsfile`, then **Save**
4. Click **Build Now**, then open the build and click **Console Output**
5. Failed tests show up under **Test Result**

### 5.5 Run test stages in parallel

```groovy
stage('Tests') {
    parallel {
        stage('Unit') {
            steps { sh 'mvn -B test' }
        }
        stage('Lint') {
            steps { sh 'mvn -B checkstyle:check' }
        }
    }
}
```

### 5.6 Node.js in Jenkins (using Docker)

```groovy
pipeline {
    agent {
        docker { image 'node:22' }
    }
    stages {
        stage('Install') { steps { sh 'npm ci' } }
        stage('Test')    { steps { sh 'npm test -- --coverage' } }
    }
}
```

### 5.7 Auto-trigger on every push

- **Easiest:** poll the repo with `triggers { pollSCM('H/5 * * * *') }` in the Jenkinsfile
- **Better:** add a GitHub webhook pointing to `http://YOUR-JENKINS/github-webhook/` (Jenkins must be reachable from the internet)

### 5.8 Credentials

1. **Manage Jenkins, then Credentials,** add a secret
2. Use it in the pipeline:

```groovy
withCredentials([string(credentialsId: 'api-key', variable: 'API_KEY')]) {
    sh 'npm test'
}
```

---

## 6. GitHub Actions vs Jenkins

| | GitHub Actions | Jenkins |
|---|---|---|
| Setup | None, it is built into GitHub | You install and maintain a server |
| Config file | `.github/workflows/*.yml` | `Jenkinsfile` (Groovy) |
| Best for | Open source, personal projects, startups | Companies with existing Jenkins, custom or on-premise setups |
| Cost | Free tier for public repos, limited minutes for private | Free software, you pay for the server |
| Learning tip | **Start here** | Learn it second, because many companies still use it |

---

## 7. Common Problems and Fixes

| Problem | Likely cause | Fix |
|---|---|---|
| Passes locally, fails in CI | Different Java/Node/Python version, or depends on local files | Match versions in the workflow; avoid absolute paths |
| Tests hang forever | Waiting on network or a container | Add timeouts; check Docker is available |
| Testcontainers fails in Jenkins | Agent cannot reach Docker | Give the agent Docker access |
| Flaky test (random failures) | Sleeps, shared data, real time | Fix with fake timers or clocks; see the Testing Fundamentals topic |
| Slow builds | No caching, everything runs every time | Use caching; split unit and integration jobs |
| `Permission denied` on `mvnw` or scripts | File is not executable | `git update-index --chmod=+x mvnw` |
| Secret shows as empty | Wrong secret name, or running on a fork PR | Check the exact name; secrets are not shared with forks |

---

## 8. Practice Tasks (do them in order)

1. **Task 1 (15 min):** Add the Java, Node or Python workflow to one of your repos. Push and see it go green.
2. **Task 2:** Break a test on purpose, push, and watch the pipeline go red. Fix it and see it go green again.
3. **Task 3:** Add the status badge to your README.
4. **Task 4:** Turn on branch protection so `main` needs a passing check.
5. **Task 5:** Add a coverage threshold and confirm the build fails when coverage drops.
6. **Task 6:** Split unit and integration tests into two jobs using `needs`.
7. **Task 7:** Install Jenkins with Docker, create a Pipeline job and run the Jenkinsfile from section 5.3.
8. **Task 8 (capstone):** Add CI to your URL Shortener project with unit tests, Testcontainers integration tests, a coverage check and uploaded reports.

---

## 9. Key Takeaways

- CI runs your tests automatically on every push and pull request.
- Start with GitHub Actions: one YAML file is enough.
- Cache dependencies, add timeouts, and run fast tests first.
- Use branch protection so failing code cannot be merged.
- Coverage thresholds stop quality from silently dropping.
- Learn Jenkins too: a `Jenkinsfile` with stages and `post` blocks covers most real-world use.

---

## 10. Learn More

**GeeksforGeeks**
- [Introduction to GitHub Actions](https://www.geeksforgeeks.org/git/introduction-to-github-actions)
- [GitHub Actions (features and workflow components)](https://www.geeksforgeeks.org/git/github-actions)
- [How to Create a Basic CI Workflow Using GitHub Actions](https://www.geeksforgeeks.org/devops/how-to-create-a-basic-ci-workflow-using-github-actions/)
- [Jenkins Tutorial (Jenkinsfile, CI/CD pipeline, plugins)](https://www.geeksforgeeks.org/jenkins-tutorial)

**Official docs**
- [GitHub Actions documentation](https://docs.github.com/en/actions)
- [Jenkins Pipeline guide](https://www.jenkins.io/doc/book/pipeline/)
- [Jenkinsfile syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)