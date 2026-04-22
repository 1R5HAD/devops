# DevOps Lab Guide — 23CSE1663
**B.N.M. Institute of Technology | VTU Autonomous | CSE Sem 6 | 2025–2026**

---

## Table of Contents

### Part A — Git, GitHub & Maven/Jenkins (Exp 1–10)
- [Experiment 1 — Introduction to Git and Local Repository Management](#experiment-1--introduction-to-git-and-local-repository-management)
- [Experiment 2 — Working with Git Branches](#experiment-2--working-with-git-branches)
- [Experiment 3 — GitHub Repository Creation and Push](#experiment-3--github-repository-creation-and-push)
- [Experiment 4 — GitHub Collaboration Using Pull Requests](#experiment-4--github-collaboration-using-pull-requests)
- [Experiment 5 — Git Tagging and Release Creation](#experiment-5--git-tagging-and-release-creation)
- [Experiment 6 — Creation and Build of a Java Project Using Apache Maven](#experiment-6--creation-and-build-of-a-java-project-using-apache-maven)
- [Experiment 7 — Jenkins Freestyle Job](#experiment-7--jenkins-freestyle-job)
- [Experiment 8 — Jenkins Build Triggers](#experiment-8--jenkins-build-triggers)
- [Experiment 9 — Jenkins Declarative Pipeline](#experiment-9--jenkins-declarative-pipeline)
- [Experiment 10 — Jenkins Pipeline Visualization and Logs](#experiment-10--jenkins-pipeline-visualization-and-logs)

### Part B — JUnit, Docker & CI/CD (Exp 11–20)
- [Experiment 11 — Writing Unit Tests using JUnit](#experiment-11--writing-unit-tests-using-junit)
- [Experiment 12 — Running JUnit Tests with Maven](#experiment-12--running-junit-tests-with-maven)
- [Experiment 13 — Integrating JUnit with Jenkins](#experiment-13--integrating-junit-with-jenkins)
- [Experiment 14 — Test Coverage using JaCoCo](#experiment-14--test-coverage-using-jacoco)
- [Experiment 15 — Introduction to Docker and Basic Commands](#experiment-15--introduction-to-docker-and-basic-commands)
- [Experiment 16 — Creating Docker Images using Dockerfile](#experiment-16--creating-docker-images-using-dockerfile)
- [Experiment 17 — Docker Volumes and Port Mapping](#experiment-17--docker-volumes-and-port-mapping)
- [Experiment 18 — Docker Image Push to Docker Hub](#experiment-18--docker-image-push-to-docker-hub)
- [Experiment 19 — Jenkins and Docker Integration](#experiment-19--jenkins-and-docker-integration)
- [Experiment 20 — Simple End-to-End CI/CD Pipeline](#experiment-20--simple-end-to-end-cicd-pipeline)

---

## Experiment 1 — Introduction to Git and Local Repository Management

**Tasks:** Create repository, add and commit files, view history.  
**Outcome:** Understand version control fundamentals.

### Step 1: Configure Git (one-time setup)

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --list          # verify settings
```

### Step 2: Create a local repository

```bash
mkdir my-project
cd my-project
git init
```

Expected output: `Initialized empty Git repository in .../my-project/.git/`

### Step 3: Create some files

```bash
echo "Hello, Git!" > hello.txt
echo "This is my DevOps project" > readme.txt
```

### Step 4: Check the status

```bash
git status
```

Files will show as **Untracked** — Git sees them but is not tracking them yet.

### Step 5: Stage the files

```bash
git add hello.txt          # add one specific file
git add .                  # add ALL files in current directory
```

Run `git status` again — files now show as **Changes to be committed** (green).

### Step 6: Commit the files

```bash
git commit -m "Initial commit: added hello.txt and readme.txt"
```

### Step 7: Make a change and commit again

```bash
echo "Second line added" >> hello.txt
git add hello.txt
git commit -m "Updated hello.txt with second line"
```

### Step 8: View commit history

```bash
git log                    # full detailed history
git log --oneline          # compact one-line view
git log --oneline --graph  # visual branch graph
```

### Step 9: Other useful commands

```bash
git diff                   # see unstaged changes
git diff --staged          # see staged changes before committing
git show                   # details of last commit
git rm hello.txt           # remove file and stage the deletion
git restore hello.txt      # undo changes to a file (not yet staged)
git reset HEAD hello.txt   # unstage a file (keep changes)
```

### Key Concept — The 3 Areas of Git

```
Working Directory → (git add) → Staging Area → (git commit) → Local Repository
```

### Reference Table

| Command | What it does |
|---------|-------------|
| `git init` | Initialize a new local repository |
| `git add .` | Stage all changes |
| `git commit -m "msg"` | Save staged changes as a snapshot |
| `git status` | Show state of working directory |
| `git log --oneline` | Compact commit history |
| `git diff` | Show unstaged changes |
| `git restore file` | Discard working directory changes |

### Viva Answer

> "Git is a distributed version control system. `git init` creates a `.git` folder that tracks all changes. `git add` moves files to the staging area where you prepare what goes into the next snapshot. `git commit` saves that snapshot permanently to the repository with a message. `git log` shows the history of all commits with their SHA hashes."

---

## Experiment 2 — Working with Git Branches

**Tasks:** Create feature branch, commit changes, merge branches, resolve conflicts.  
**Outcome:** Learn parallel development.

### Step 1: Check current branch

```bash
git branch                 # lists all branches, * = current
```

### Step 2: Create and switch to a new branch

```bash
git branch feature-login          # creates branch
git checkout feature-login        # switches to it

# OR do both in one command:
git checkout -b feature-login
```

### Step 3: Make changes on the feature branch

```bash
echo "Login feature code" > login.txt
git add login.txt
git commit -m "Added login feature"

echo "More login logic" >> login.txt
git add login.txt
git commit -m "Updated login feature"
```

### Step 4: Switch back to main and merge

```bash
git checkout main
git merge feature-login
```

If no conflicts: **Fast-forward merge** — Git just moves the pointer forward.

### Step 5: Delete the branch after merging

```bash
git branch -d feature-login       # delete merged branch
git branch                        # verify it's gone
```

### Step 6: Creating a merge conflict (for practice)

```bash
# On main branch — create a file
echo "main branch version" > conflict.txt
git add conflict.txt
git commit -m "main: added conflict.txt"

# Create new branch and edit the SAME file differently
git checkout -b feature-b
echo "feature branch version" > conflict.txt
git add conflict.txt
git commit -m "feature-b: added conflict.txt"

# Switch back to main and merge — conflict!
git checkout main
git merge feature-b
```

Output: `CONFLICT (content): Merge conflict in conflict.txt`

### Step 7: Resolve the conflict

Open `conflict.txt` — it looks like:

```
<<<<<<< HEAD
main branch version
=======
feature branch version
>>>>>>> feature-b
```

Edit the file to keep what you want, then:

```bash
echo "resolved version" > conflict.txt
git add conflict.txt
git commit -m "Resolved merge conflict in conflict.txt"
```

### Branch Commands Reference

```bash
git branch                    # list local branches
git branch -a                 # list all branches including remote
git checkout -b feature-x     # create + switch in one step
git merge feature-x           # merge into current branch
git branch -d feature-x       # delete branch (safe — merged only)
git branch -D feature-x       # force delete (even unmerged)
```

### Viva Answer

> "Branches allow parallel development without affecting the main codebase. A merge conflict occurs when two branches modify the same line of the same file — Git cannot auto-resolve it and marks the conflict with `<<<`, `===`, `>>>` markers. The developer manually resolves it, stages the file, and commits."

---

## Experiment 3 — GitHub Repository Creation and Push

**Tasks:** Create GitHub repo, push local code, clone repository.  
**Outcome:** Understand remote version control.

### Step 1: Create repository on GitHub

Go to [github.com](https://github.com) → Click **+** → **New repository** → Fill in:
- Repository name: `my-project`
- **Do NOT check** "Initialize with README" (since you have local code)
- Click **Create repository**

### Step 2: Connect local repo to GitHub

```bash
cd my-project
git remote add origin https://github.com/yourusername/my-project.git
git remote -v          # verify remote was added
```

### Step 3: Push local code to GitHub

```bash
git push -u origin main
```

`-u` sets upstream — after this first push, you can just type `git push`.

### Step 4: Make changes and push again

```bash
echo "New feature added" >> readme.txt
git add readme.txt
git commit -m "Updated readme with new feature info"
git push
```

### Step 5: Clone a repository

```bash
git clone https://github.com/yourusername/my-project.git
cd my-project
ls                     # all files are here
git log --oneline      # full history is preserved
```

### Step 6: Pull latest changes from remote

```bash
git pull               # fetch + merge latest changes from GitHub
git fetch              # only download, don't merge yet
```

### Remote Commands Reference

```bash
git remote add origin <url>     # connect local to remote
git remote -v                   # view remote URLs
git push -u origin main         # first push (sets upstream)
git push                        # push after first time
git pull                        # pull latest changes
git clone <url>                 # download repo from GitHub
```

### Viva Answer

> "GitHub is a remote hosting service for Git repositories. `git remote add origin` connects the local repo to the remote. `git push` uploads commits to GitHub. `git clone` downloads a full copy of a remote repository including all history and branches. `git pull` is shorthand for `git fetch` (download) + `git merge` (integrate changes)."

---

## Experiment 4 — GitHub Collaboration Using Pull Requests

**Tasks:** Fork repository, create pull request, review and merge.  
**Outcome:** Team-based development understanding.

### Step 1: Fork a repository

Go to any GitHub repo → Click **Fork** (top right) → Select your account.  
A copy appears at `github.com/yourusername/repo-name`.

### Step 2: Clone your fork locally

```bash
git clone https://github.com/yourusername/repo-name.git
cd repo-name
```

### Step 3: Add the original repo as upstream

```bash
git remote add upstream https://github.com/originaluser/repo-name.git
git remote -v          # should show both origin and upstream
```

### Step 4: Create a feature branch for your changes

```bash
git checkout -b fix-typo-in-readme
```

> Never make changes directly on `main` — always use a branch for PRs.

### Step 5: Make changes and commit

```bash
echo "Fixed the typo here" >> README.md
git add README.md
git commit -m "Fix: corrected typo in README"
```

### Step 6: Push the branch to your fork

```bash
git push origin fix-typo-in-readme
```

### Step 7: Open a Pull Request on GitHub

1. Go to `github.com/yourusername/repo-name`
2. Click **Compare & pull request** banner
3. Set base: `originaluser/repo-name → main`
4. Set compare: `yourusername/repo-name → fix-typo-in-readme`
5. Write title and description → Click **Create pull request**

### Step 8: Review and merge (as the repo owner)

- Click **Files changed** tab to review the diff
- Add a review comment if needed
- Click **Merge pull request** → **Confirm merge**
- Optionally delete the branch after merging

### Step 9: Sync your fork after merge

```bash
git checkout main
git pull upstream main         # pull merged changes from original
git push origin main           # update your fork on GitHub
```

### Viva Answer

> "Forking creates a personal copy of someone else's repository under your account. Pull Requests propose changes from your fork/branch to the original repository. The workflow is: fork → clone → branch → commit → push → open PR → review → merge. This is the standard open-source contribution model. PRs enable code review before changes enter the main branch."

---

## Experiment 5 — Git Tagging and Release Creation

**Tasks:** Create annotated tags, push tags, create GitHub release.  
**Outcome:** Understand software versioning.

### Step 1: Check existing tags

```bash
git tag                        # list all tags
```

### Step 2: Create a lightweight tag (simple pointer)

```bash
git tag v1.0
```

### Step 3: Create an annotated tag (recommended for releases)

```bash
git tag -a v1.0 -m "Release version 1.0 - initial stable release"
```

- `-a` = annotated (stores tagger name, date, message)
- `-m` = message (like a commit message for the tag)

### Step 4: View tag details

```bash
git show v1.0                  # shows tag message + commit it points to
```

### Step 5: Push a tag to GitHub

```bash
git push origin v1.0           # push one specific tag
git push origin --tags         # push ALL tags at once
```

### Step 6: Tag a specific old commit

```bash
git log --oneline              # find the commit hash
git tag -a v0.9 abc1234 -m "Version 0.9 - beta release"
git push origin v0.9
```

### Step 7: Create a GitHub Release

1. Go to your GitHub repo → Click **Releases** (right sidebar)
2. Click **Create a new release**
3. Click **Choose a tag** → select `v1.0`
4. Release title: `Version 1.0`
5. Write release notes → Click **Publish release**

### Step 8: Delete a tag if needed

```bash
git tag -d v1.0                # delete tag locally
git push origin --delete v1.0  # delete from GitHub
```

### Tags Reference

```bash
git tag                          # list all tags
git tag v1.0                     # lightweight tag
git tag -a v1.0 -m "msg"         # annotated tag
git tag -a v1.0 <hash> -m "msg"  # tag an old commit
git show v1.0                    # inspect a tag
git push origin --tags           # push all tags
git tag -d v1.0                  # delete locally
git push origin --delete v1.0    # delete from remote
```

### Viva Answer

> "Git tags mark specific commits as important milestones — typically release versions. A lightweight tag is just a pointer to a commit. An annotated tag stores additional metadata: tagger name, date, and message — making it suitable for releases. GitHub Releases are built on top of Git tags and allow attaching release notes and downloadable assets. Semantic versioning (`v1.0.0`) means Major.Minor.Patch."

---

## Experiment 6 — Creation and Build of a Java Project Using Apache Maven

**Tasks:** Creation of Maven Repository, Maven lifecycle.  
**Outcome:** `artifactId:sample-app version 1.0-SNAPSHOT` (jar file).

### Step 1: Verify Maven and Java are installed

```bash
mvn --version
java -version
```

Expected: `Apache Maven 3.x.x` and `Java 11` or higher.

### Step 2: Create a Maven project using archetype

```bash
mvn archetype:generate \
  -DgroupId=com.example \
  -DartifactId=sample-app \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DarchetypeVersion=1.4 \
  -DinteractiveMode=false
```

- `groupId` = your organization/package namespace
- `artifactId` = your project name → becomes `sample-app`

### Step 3: Navigate into the project

```bash
cd sample-app
```

### Step 4: Generated folder structure

```
sample-app/
├── pom.xml
└── src/
    ├── main/
    │   └── java/
    │       └── com/example/
    │           └── App.java
    └── test/
        └── java/
            └── com/example/
                └── AppTest.java
```

### Step 5: View the generated pom.xml

```xml
<groupId>com.example</groupId>
<artifactId>sample-app</artifactId>
<version>1.0-SNAPSHOT</version>
```

> **SNAPSHOT** means it is a development/in-progress version (not a final release).

### Step 6: Run the Maven lifecycle phases

```bash
mvn compile          # compile source code → target/classes/
mvn test             # compile + run tests
mvn package          # compile + test + create JAR file
mvn clean package    # delete target/ first, then full build
```

### Step 7: Verify the JAR was created

```bash
ls target/
# You should see: sample-app-1.0-SNAPSHOT.jar
```

### Step 8: Run the JAR

```bash
java -cp target/sample-app-1.0-SNAPSHOT.jar com.example.App
```

Output: `Hello World!`

### Maven Lifecycle Phases

| Phase | What it does |
|-------|-------------|
| `validate` | Check project is correct |
| `compile` | Compile source code |
| `test` | Run unit tests |
| `package` | Create JAR/WAR |
| `verify` | Run integration tests |
| `install` | Copy JAR to local `~/.m2` repo |
| `deploy` | Push to remote repo |

### Viva Answer

> "Maven is a build and dependency management tool. The `pom.xml` defines the project — `groupId` is like a package namespace, `artifactId` is the project name, and `version` is `1.0-SNAPSHOT` meaning it is in development. `mvn package` triggers the full lifecycle: validate → compile → test → package, producing a JAR in the `target/` directory. SNAPSHOT versions are mutable development builds; release versions like `1.0` are immutable."

---

## Experiment 7 — Jenkins Freestyle Job

**Tasks:** Pull code from GitHub, run build commands, archive artifacts.  
**Outcome:** Automated builds.

### Pre-requirement: Push Maven project to GitHub

```bash
cd sample-app
git init
git add .
git commit -m "initial maven project"
git remote add origin https://github.com/yourusername/sample-app.git
git push -u origin main
```

### Step 1: Open Jenkins

```
http://localhost:8080
```

### Step 2: Create a new Freestyle job

- Click **New Item**
- Name: `Maven-Build`
- Select **Freestyle project** → Click **OK**

### Step 3: Configure Source Code Management

- Select **Git**
- Repository URL: `https://github.com/yourusername/sample-app.git`
- Branch Specifier: `*/main`

### Step 4: Add Build Step

- **Build Steps** → **Add build step** → **Invoke top-level Maven targets**
- Goals:

```
clean package
```

### Step 5: Archive the artifact

- **Post-build Actions** → **Add post-build action** → **Archive the artifacts**
- Files to archive:

```
**/target/*.jar
```

### Step 6: Save and build

- Click **Save** → **Build Now**
- Click the build number → **Console Output** to watch live

### Step 7: Verify artifact

After a successful build:
- Click the build number → **Build Artifacts**
- You should see `sample-app-1.0-SNAPSHOT.jar` available for download

### Console Output — Key Lines to Look For

```
[INFO] Building sample-app 1.0-SNAPSHOT
[INFO] BUILD SUCCESS
Archiving artifacts
Finished: SUCCESS
```

### Viva Answer

> "A Jenkins Freestyle job is a simple configurable build job. It connects to a Git repository, pulls the latest code, runs the Maven build command, and archives the JAR as a build artifact. Archiving artifacts stores the output of each build so it can be downloaded or used in downstream jobs. This automates the entire build process — no manual compilation needed."

---

## Experiment 8 — Jenkins Build Triggers

**Tasks:** Configure SCM polling and GitHub webhook.  
**Outcome:** Automatic build triggering.

### Method A — SCM Polling (Jenkins checks GitHub periodically)

#### Step 1: Open your existing Jenkins job

Jenkins → `Maven-Build` → **Configure**

#### Step 2: Enable SCM Polling

- Scroll to **Build Triggers**
- Check **Poll SCM**
- Schedule field:

```
H/5 * * * *
```

This means: check every 5 minutes. If there's a new commit, trigger a build.

#### Cron Syntax Reference

```
* * * * *
│ │ │ │ └── Day of week (0=Sun, 7=Sat)
│ │ │ └──── Month (1-12)
│ │ └────── Day of month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)

H/5 * * * *   → every 5 minutes
H * * * *     → every hour
0 8 * * 1-5   → 8 AM every weekday
```

---

### Method B — GitHub Webhook (GitHub notifies Jenkins instantly)

#### Step 1: Add webhook in GitHub

1. Go to your GitHub repo → **Settings** → **Webhooks** → **Add webhook**
2. Payload URL: `http://your-jenkins-url/github-webhook/`
3. Content type: `application/json`
4. Trigger: **Just the push event**
5. Click **Add webhook**

#### Step 2: Configure Jenkins job

- Jenkins → your job → **Configure**
- Build Triggers → check **GitHub hook trigger for GITScm polling**
- Save

#### Step 3: Test it

```bash
echo "trigger build" >> readme.txt
git add readme.txt
git commit -m "test webhook trigger"
git push
```

Jenkins starts a new build within seconds.

### Viva Answer

> "Build triggers automate when Jenkins runs. SCM Polling uses cron-like syntax — Jenkins checks GitHub at set intervals and builds if new commits are found. GitHub Webhooks are push-based — GitHub sends an HTTP POST to Jenkins the moment a push happens, triggering an instant build. Webhooks are preferred in production as they are real-time and don't waste resources polling."

---

## Experiment 9 — Jenkins Declarative Pipeline

**Tasks:** Create Jenkinsfile, define build and test stages.  
**Outcome:** Pipeline execution understanding.

### Step 1: Create Jenkinsfile in project root

```bash
cd sample-app
touch Jenkinsfile
```

### Step 2: Write the Jenkinsfile

```groovy
pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/yourusername/sample-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Compiling the Maven project...'
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running JUnit tests...'
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging into JAR...'
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs above.'
        }
    }
}
```

### Step 3: Push the Jenkinsfile to GitHub

```bash
git add Jenkinsfile
git commit -m "Added Jenkinsfile for declarative pipeline"
git push
```

### Step 4: Create a Pipeline job in Jenkins

- Jenkins → **New Item** → Name: `Declarative-Pipeline`
- Select **Pipeline** → OK
- Pipeline section → Definition: **Pipeline script from SCM**
- SCM: **Git** → Repository URL → Script Path: `Jenkinsfile`
- Click **Save** → **Build Now**

### Declarative Pipeline Structure

```groovy
pipeline {          // required wrapper
    agent any       // run on any available Jenkins agent

    environment {   // optional: set environment variables
        APP_NAME = "sample-app"
    }

    stages {
        stage('Name') {
            steps {
                sh 'command'     // run shell command
                echo 'message'   // print message
                git 'url'        // clone repo
            }
        }
    }

    post {
        always { }  // always runs
        success { } // only on success
        failure { } // only on failure
    }
}
```

### Viva Answer

> "A Declarative Pipeline is Jenkins pipeline defined as code in a Jenkinsfile. It uses a structured syntax with `pipeline`, `agent`, `stages`, and `post` blocks. Each stage represents a logical step — Clone gets the code, Build compiles it, Test runs JUnit, Package creates the JAR. Defining the pipeline as code means it is version-controlled with the application and reproducible across environments."

---

## Experiment 10 — Jenkins Pipeline Visualization and Logs

**Tasks:** View pipeline stages, analyze logs, debug failures.  
**Outcome:** CI troubleshooting skills.

### Step 1: Access the Stage View

After a build runs on a Pipeline job, you see the **Stage View** table:
- Columns = stages, Rows = builds
- Green = passed, Red = failed, Grey = not run

### Step 2: View Console Output

- Click a build number (e.g. `#3`) in **Build History**
- Click **Console Output**
- Read top to bottom — Jenkins logs every command and its output

### Step 3: Key log patterns to recognize

```bash
# Successful build ends with:
[INFO] BUILD SUCCESS
Finished: SUCCESS

# Failed build ends with:
[INFO] BUILD FAILURE
[ERROR] Tests run: 3, Failures: 1, Errors: 0
Finished: FAILURE

# Compilation error:
[ERROR] COMPILATION ERROR
[ERROR] /path/to/File.java:[12,8] error: ';' expected

# Test failure:
Tests run: 3, Failures: 1
expected:<5> but was:<6>
```

### Step 4: View individual stage logs

- In Stage View, hover over any stage box
- Click **Logs** to see only that stage's output

### Step 5: View Test Results

- Click a build → **Test Result**
- Shows pass/fail breakdown per test class and method
- Click a failed test to see the exact assertion that failed

### Step 6: Intentionally fail a pipeline to practice debugging

```java
// In CalculatorTest.java — change expected value to wrong number
assertEquals(99, calc.add(2, 3));   // will fail — 5 ≠ 99
```

```bash
git add .
git commit -m "intentional test failure for debugging practice"
git push
```

Find the failure in Stage View → click Logs → read the error → fix it → rebuild.

### Step 7: Replay a pipeline

- Click a build → **Replay**
- Edit the Jenkinsfile directly in browser → Click **Run**
- Useful for quick debugging without pushing a new commit

### Common Jenkins Issues and Fixes

| Problem | What to look for in logs | Fix |
|---------|--------------------------|-----|
| Git clone fails | `remote: Repository not found` | Check repo URL in job config |
| Maven not found | `mvn: command not found` | Configure Maven in Global Tools |
| Test failure | `expected:<X> but was:<Y>` | Fix the assertion or the code |
| Port in use | `Address already in use` | `docker stop` old container |
| Permission denied | `Permission denied` | Add jenkins to docker group |

### Viva Answer

> "Jenkins Pipeline Visualization shows each stage as a colored block — green for pass, red for fail. When a build fails, you click the red stage → Logs to see the exact error. Console Output shows the complete sequential log of everything Jenkins ran. The Replay feature lets you re-run a pipeline with temporary Jenkinsfile edits for quick debugging without committing."

---

## Experiment 11 — Writing Unit Tests using JUnit

**Tasks:** Write Java program and JUnit test cases.  
**Outcome:** Understand automated testing.

### Step 1: Create the project folder structure

```bash
mkdir junit-demo
cd junit-demo
mkdir -p src/main/java src/test/java
```

### Step 2: Create Calculator.java

Save at `src/main/java/Calculator.java`

```java
public class Calculator {

    public int add(int a, int b) {
        return a + b;
    }

    public int subtract(int a, int b) {
        return a - b;
    }

    public int multiply(int a, int b) {
        return a * b;
    }

    public double divide(int a, int b) {
        return (double) a / b;
    }
}
```

### Step 3: Create CalculatorTest.java

Save at `src/test/java/CalculatorTest.java`

```java
import org.junit.Test;
import static org.junit.Assert.*;

public class CalculatorTest {

    Calculator calc = new Calculator();

    @Test
    public void testAdd() {
        assertEquals(5, calc.add(2, 3));
    }

    @Test
    public void testSubtract() {
        assertEquals(1, calc.subtract(3, 2));
    }

    @Test
    public void testMultiply() {
        assertEquals(6, calc.multiply(2, 3));
    }

    @Test
    public void testDivide() {
        // 3rd argument is delta (acceptable error margin for doubles)
        assertEquals(2.0, calc.divide(6, 3), 0.001);
    }
}
```

### JUnit Annotations and Assertions Reference

| Annotation / Method | Purpose |
|---------------------|---------|
| `@Test` | Marks a method as a test case |
| `assertEquals(exp, act)` | Passes if expected == actual |
| `assertTrue(condition)` | Passes if condition is true |
| `assertFalse(condition)` | Passes if condition is false |
| `assertNotNull(obj)` | Passes if object is not null |
| `assertNull(obj)` | Passes if object is null |
| `@Before` | Runs before each test method |
| `@After` | Runs after each test method |

### Viva Answer

> "JUnit is a unit testing framework for Java. Each method annotated with `@Test` is an independent test case. `assertEquals` checks if the method returns the expected value — if the assertion fails, the test fails. Unit testing allows us to verify individual components of code in isolation without running the full application."

---

## Experiment 12 — Running JUnit Tests with Maven

**Tasks:** Configure Maven and run tests.  
**Outcome:** Build tool familiarity.

### Step 1: Create pom.xml in the project root

Place inside `junit-demo/` — same level as the `src/` folder.

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>junit-demo</artifactId>
  <version>1.0</version>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

### Step 2: Verify folder structure

```
junit-demo/
├── pom.xml
└── src/
    ├── main/
    │   └── java/
    │       └── Calculator.java
    └── test/
        └── java/
            └── CalculatorTest.java
```

### Step 3: Run tests using Maven

```bash
mvn clean test
```

Expected output:

```
Tests run: 4, Failures: 0, Errors: 0, Skipped: 0
BUILD SUCCESS
```

### Step 4: Other Maven commands

```bash
mvn clean          # delete target/ folder
mvn compile        # compile source code only
mvn test           # run tests without clean
mvn clean test     # clean then run tests (recommended)
mvn package        # compile + test + create .jar
```

### Viva Answer

> "Maven is a build automation and dependency management tool. The `pom.xml` defines the project structure and lists dependencies. Maven automatically downloads JUnit from Maven Central. The `scope test` means JUnit is only on the classpath during testing, not included in the production build. The `maven-surefire-plugin` runs JUnit tests during the `test` phase."

---

## Experiment 13 — Integrating JUnit with Jenkins

**Tasks:** Run JUnit tests in Jenkins and view reports.  
**Outcome:** CI-based testing.

### Step 1: Push project to GitHub

```bash
cd junit-demo
git init
git add .
git commit -m "initial junit project"
git remote add origin https://github.com/yourusername/junit-demo.git
git push -u origin main
```

### Step 2: Open Jenkins

```
http://localhost:8080
```

- **New Item** → Name: `JUnit-Demo` → **Freestyle project** → OK

### Step 3: Configure Source Code Management

- Select **Git**
- Repository URL: your GitHub URL
- Branch Specifier: `*/main`

### Step 4: Add Build Step

- **Build Steps** → **Add build step** → **Invoke top-level Maven targets**
- Goals:

```
clean test
```

### Step 5: Add Post-build Action for Test Reports

- **Post-build Actions** → **Add post-build action** → **Publish JUnit test result report**
- Test report XMLs:

```
**/target/surefire-reports/*.xml
```

### Step 6: Save, Build, and view results

- Click **Save** → **Build Now**
- After build completes → click build number → **Test Result**
- See pass/fail breakdown with trend graphs

### Viva Answer

> "Jenkins CI ensures every code change is automatically tested. When code is pushed to GitHub, Jenkins pulls it, runs Maven tests, and publishes the JUnit XML report generated by the maven-surefire-plugin. If a test fails, the build fails and the team is notified — preventing broken code from being merged."

---

## Experiment 14 — Test Coverage using JaCoCo

**Tasks:** Generate and analyze coverage reports.  
**Outcome:** Quality metrics understanding.

### Step 1: Replace pom.xml with complete version including JaCoCo

```xml
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>junit-demo</artifactId>
  <version>1.0</version>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.8.11</version>
        <executions>
          <execution>
            <!-- Attaches JaCoCo agent before tests run -->
            <goals>
              <goal>prepare-agent</goal>
            </goals>
          </execution>
          <execution>
            <id>report</id>
            <!-- Generates report after test phase -->
            <phase>test</phase>
            <goals>
              <goal>report</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project>
```

### Step 2: Run tests to generate coverage report

```bash
mvn clean test
```

JaCoCo automatically generates the report after tests complete — no extra command needed.

### Step 3: Open the HTML coverage report

```
target/site/jacoco/index.html
```

Open this file in your browser. Green = covered by tests. Red = not covered.

### JaCoCo Coverage Metrics

| Metric | What it measures |
|--------|-----------------|
| Instructions | Individual bytecode instructions executed |
| Branches | if/else/switch branches covered |
| Lines | Source code lines executed during tests |
| Methods | How many methods were called |
| Classes | How many classes were exercised |

### Viva Answer

> "JaCoCo (Java Code Coverage) works by instrumenting bytecode at runtime using a JVM agent attached by the `prepare-agent` goal. During test execution, JaCoCo tracks which instructions, branches, and lines are executed. After the test phase, the `report` goal generates an HTML report with color-coded source code. High coverage does not guarantee bug-free code but shows how well tests exercise the codebase."

---

## Experiment 15 — Introduction to Docker and Basic Commands

**Tasks:** Install Docker, run sample containers.  
**Outcome:** Container fundamentals.

### Step 1: Verify Docker installation

```bash
docker --version
docker info
```

Expected: `Docker version 24.x.x, build ...`

### Step 2: Pull and run your first container

```bash
docker pull hello-world
docker run hello-world
```

Output: `Hello from Docker!` — confirms Docker is working correctly.

### Step 3: Run an interactive Ubuntu container

```bash
docker run -it ubuntu bash
```

- `-i` = interactive (keep stdin open)
- `-t` = allocate a pseudo-terminal
- You are now inside the container. Type `exit` to leave.

### Step 4: Essential Docker commands

```bash
docker ps                        # list running containers
docker ps -a                     # list ALL containers (running + stopped)
docker images                    # list all locally downloaded images
docker pull nginx                # pull nginx image from Docker Hub
docker stop <container_id>       # gracefully stop a running container
docker start <container_id>      # restart a stopped container
docker rm <container_id>         # permanently delete a container
docker rmi <image_name>          # delete an image from local storage
docker logs <container_id>       # see output/logs of a container
docker exec -it <id> bash        # enter a running container's shell
docker inspect <id>              # detailed info about a container
```

### Core Concepts

| Concept | Explanation |
|---------|-------------|
| Image | Read-only template — like a class in OOP |
| Container | Running instance of an image — like an object |
| Docker Hub | Public registry where images are stored |
| `docker pull` | Downloads image from Docker Hub |
| `docker run` | Creates and starts a container from an image |

### Viva Answer

> "Docker is a containerization platform that packages applications with all their dependencies into isolated containers. Unlike virtual machines, containers share the host OS kernel making them lightweight and fast. An image is an immutable blueprint; a container is a running instance. Docker Hub is the public registry where official and community images are stored."

---

## Experiment 16 — Creating Docker Images using Dockerfile

**Tasks:** Write Dockerfile, build and run image.  
**Outcome:** Application containerization.

### Step 1: Create project folder

```bash
mkdir myapp
cd myapp
```

### Step 2: Create app.py

```python
print("Hello from Docker container!")
print("Application is containerized successfully.")
```

### Step 3: Create Dockerfile

> Filename is exactly `Dockerfile` — capital D, no extension.

```dockerfile
# Step 1: Choose base image
FROM python:3.9-slim

# Step 2: Set working directory inside container
WORKDIR /app

# Step 3: Copy source files from host to container
COPY app.py .

# Step 4: Default command to run when container starts
CMD ["python", "app.py"]
```

### Step 4: Build the Docker image

```bash
docker build -t myapp:1.0 .
```

> The `.` (dot) at the end is mandatory — it means "use current directory as the build context".

### Step 5: Run the image and verify

```bash
docker run myapp:1.0
docker images            # verify myapp:1.0 appears in list
```

Expected output: `Hello from Docker container!`

### Dockerfile Instruction Reference

| Instruction | Purpose | Example |
|-------------|---------|---------|
| `FROM` | Base image to build on | `FROM python:3.9-slim` |
| `WORKDIR` | Sets working dir inside container | `WORKDIR /app` |
| `COPY src dest` | Copy files from host to container | `COPY . .` |
| `RUN cmd` | Execute command during build | `RUN pip install flask` |
| `EXPOSE port` | Document which port app uses | `EXPOSE 5000` |
| `CMD ["cmd"]` | Default run command | `CMD ["python","app.py"]` |
| `ENV KEY=val` | Set environment variable | `ENV DEBUG=false` |

### Viva Answer

> "A Dockerfile is a text script with instructions to build a Docker image. Each instruction creates a new read-only layer — Docker caches layers so unchanged layers don't need to be rebuilt. FROM sets the base, WORKDIR sets the directory, COPY brings source files in, RUN executes commands during build, and CMD specifies the default command when the container starts."

---

## Experiment 17 — Docker Volumes and Port Mapping

**Tasks:** Map ports and volumes.  
**Outcome:** Data persistence knowledge.

### Step 1: Create a Flask web app project

```bash
mkdir flask-app
cd flask-app
```

### Step 2: Create app.py

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Docker with Port Mapping!"

@app.route('/about')
def about():
    return "This app is running inside a Docker container."

if __name__ == '__main__':
    # host='0.0.0.0' makes it accessible from outside the container
    app.run(host='0.0.0.0', port=5000)
```

### Step 3: Create requirements.txt

```
flask
```

### Step 4: Create Dockerfile

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

### Step 5: Build and run with port mapping

```bash
docker build -t flask-app .
docker run -p 8080:5000 flask-app
```

`-p 8080:5000` = host port 8080 maps to container port 5000.  
Open `http://localhost:8080` in your browser.

```
Your Browser (8080) → Docker -p 8080:5000 → Flask inside container (5000)
```

### Step 6: Run in detached (background) mode

```bash
docker run -d -p 8080:5000 --name myflask flask-app
docker ps                            # verify it's running
docker logs myflask                  # see its output logs
docker stop myflask                  # stop it
```

### Step 7: Volume mapping — persistent data storage

```bash
# Create a folder on your machine
mkdir data

# Run container with volume — data folder is shared
docker run -v $(pwd)/data:/app/data -p 8080:5000 flask-app

# General syntax:
docker run -v /absolute/host/path:/container/path imagename
```

Files written to `/app/data` inside the container appear in `./data` on your host — and survive even after the container is deleted.

### Viva Answer

> "Port mapping (`-p hostPort:containerPort`) bridges the container's isolated network to the host. Without it, the container is completely unreachable. `host='0.0.0.0'` in Flask is required so the container accepts connections from outside its own loopback. Volumes (`-v`) solve data persistence — containers are ephemeral by default so any data written inside is lost when the container is removed. Volumes mount a host directory into the container so data persists independently."

---

## Experiment 18 — Docker Image Push to Docker Hub

**Tasks:** Push Docker image to Docker Hub.  
**Outcome:** Image distribution understanding.

### Step 1: Create a Docker Hub account

Go to [hub.docker.com](https://hub.docker.com) → Sign up → Note your **username** exactly.

### Step 2: Login from terminal

```bash
docker login
```

Enter your Docker Hub username and password. Expected: `Login Succeeded`

### Step 3: Tag your image with your Docker Hub username

Docker Hub requires image names in format `username/imagename:tag`

```bash
docker tag myapp:1.0 yourusername/myapp:1.0
```

### Step 4: Push the image to Docker Hub

```bash
docker push yourusername/myapp:1.0
```

After completion, go to `hub.docker.com/r/yourusername/myapp` to verify.

### Step 5: Verify by pulling back and running

```bash
# Remove local copy first to prove we're pulling from Hub
docker rmi yourusername/myapp:1.0

# Pull it fresh from Docker Hub
docker pull yourusername/myapp:1.0

# Run it to confirm it works
docker run yourusername/myapp:1.0
```

Expected output: `Hello from Docker container!`

### Viva Answer

> "Docker Hub is a cloud-based image registry, similar to GitHub but for container images. `docker tag` creates an alias of the image with the `username/imagename` format required by Docker Hub. `docker push` uploads the image layers to the registry. Once pushed, anyone in the world can do `docker pull username/imagename` to download and run your application without needing the source code. This enables consistent deployment across any environment."

---

## Experiment 19 — Jenkins and Docker Integration

**Tasks:** Build Docker image using Jenkins.  
**Outcome:** CI/CD integration skills.

### Step 1: Give Jenkins permission to use Docker (one-time setup)

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

### Step 2: Create Jenkinsfile in your repo root

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/youruser/yourrepo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8080:5000 --name myapp myapp:latest'
            }
        }
    }
}
```

### Step 3: Create Pipeline job in Jenkins

- Jenkins → **New Item** → Name: `Docker-Pipeline` → **Pipeline** → OK
- Pipeline → Definition: **Pipeline script from SCM**
- SCM: **Git** → Repo URL → Script Path: `Jenkinsfile`
- Save → **Build Now**

### Step 4: Verify

Watch the stage view — each stage (Checkout, Build Docker Image, Run Container) should show green.  
Visit `http://localhost:8080` to see your running app.

### Viva Answer

> "Jenkins integrates with Docker by running docker commands via `sh` steps inside pipeline stages. The Jenkinsfile defines the pipeline as code (Pipeline as Code), version-controlled alongside the application. Each stage is a logical step — Checkout pulls code from Git, Build creates the Docker image, and Run starts the container. The jenkins user needs Docker group permissions to execute docker commands on the host."

---

## Experiment 20 — Simple End-to-End CI/CD Pipeline

**Tasks:** GitHub → Jenkins → Test → Docker Build → Run.  
**Outcome:** Complete CI/CD workflow experience.

### Step 1: Full project structure needed

```
your-repo/
├── Jenkinsfile              ← CI/CD pipeline definition
├── Dockerfile               ← container definition
├── pom.xml                  ← Maven + JUnit + JaCoCo
└── src/
    ├── main/java/
    │   └── Calculator.java
    └── test/java/
        └── CalculatorTest.java
```

### Step 2: Dockerfile for the project

```dockerfile
# Use Maven + Java 11 as base
FROM maven:3.8-openjdk-11
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests
CMD ["echo", "Build complete"]
```

### Step 3: Complete end-to-end Jenkinsfile

```groovy
pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                // Pull latest code from GitHub
                git 'https://github.com/youruser/yourrepo.git'
            }
        }

        stage('Test') {
            steps {
                // Run all JUnit tests via Maven
                sh 'mvn clean test'
            }
            post {
                always {
                    // Publish JUnit XML report to Jenkins UI
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build image tagged with Jenkins build number
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }

        stage('Run Container') {
            steps {
                // Stop and remove old container
                sh 'docker stop myapp-container || true'
                sh 'docker rm myapp-container || true'
                // Start new container with latest image
                sh 'docker run -d --name myapp-container -p 8080:5000 myapp:${BUILD_NUMBER}'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! App is running at http://localhost:8080'
        }
        failure {
            echo 'Pipeline FAILED. Check the stage logs above for details.'
        }
    }
}
```

### Step 4: Stage-by-stage explanation

| Stage | What it does | Pipeline fails if... |
|-------|-------------|----------------------|
| Clone | Pulls latest code from GitHub | Repo URL wrong / no internet |
| Test | Runs `mvn clean test` — all JUnit tests execute | Any assertEquals fails |
| Build Image | Creates Docker image tagged with `${BUILD_NUMBER}` | Dockerfile has syntax errors |
| Run Container | Stops old container, starts new one on port 8080 | Port already occupied |

### Step 5: The `|| true` trick — explained

```bash
# PROBLEM: On the very first run, no container exists
# docker stop myapp-container → ERROR → pipeline FAILS

# SOLUTION: || true means "if this command errors, treat as success"
sh 'docker stop myapp-container || true'   # never fails
sh 'docker rm myapp-container || true'     # never fails

# On first build: stop/rm "fail" silently, run continues
# On subsequent builds: stop/rm succeed normally
```

> **Always include `|| true` on stop and rm steps** — examiners often ask about this.

### Step 6: Create the Pipeline job in Jenkins

- Jenkins → **New Item** → Name: `End-to-End-Pipeline` → **Pipeline** → OK
- Pipeline Definition: **Pipeline script from SCM**
- SCM: Git → Repo URL → Script Path: `Jenkinsfile`
- **Save** → **Build Now**

### Viva Answer

> "This is a complete CI/CD pipeline. CI (Continuous Integration) covers the Clone and Test stages — every code push triggers automated test execution, detecting failures early. CD (Continuous Deployment) covers Build and Run — if all tests pass, a new Docker image is automatically built and the container is redeployed. `${BUILD_NUMBER}` ensures each deployment is uniquely tagged. Pipeline as Code means the Jenkinsfile is version-controlled alongside the application, making the entire delivery process reproducible and auditable."

---

## Quick Recap — All 20 Experiments

```
Exp 1  → git init → git add → git commit → git log
Exp 2  → git branch → git checkout → git merge → resolve conflicts
Exp 3  → git remote add → git push → git clone → git pull
Exp 4  → fork → branch → PR → review → merge
Exp 5  → git tag -a → git push --tags → GitHub Release

Exp 6  → mvn archetype:generate → pom.xml → mvn package → JAR
Exp 7  → Jenkins Freestyle → Git SCM → mvn clean package → Archive *.jar
Exp 8  → Poll SCM (cron H/5) OR GitHub Webhook (instant)
Exp 9  → Jenkinsfile → Declarative Pipeline → Clone→Build→Test→Package
Exp 10 → Stage View → Console Output → Logs → Test Results → Replay

Exp 11 → Calculator.java + CalculatorTest.java with @Test annotations
Exp 12 → pom.xml with JUnit dependency → mvn clean test
Exp 13 → Jenkins Freestyle → Maven test → Publish surefire XML report
Exp 14 → JaCoCo plugin in pom.xml → mvn test → target/site/jacoco/index.html

Exp 15 → docker pull / run / ps / images / stop / rm / rmi
Exp 16 → Dockerfile → docker build -t name:tag . → docker run
Exp 17 → docker run -p 8080:5000 (port) + docker run -v host:container (volume)
Exp 18 → docker login → docker tag → docker push → docker pull

Exp 19 → Jenkinsfile → Checkout → Build Image → Run Container
Exp 20 → GitHub → Clone → Test → Docker Build → Run (with || true)
```

---

*DevOps Lab Guide | 23CSE1663 | B.N.M. Institute of Technology*
