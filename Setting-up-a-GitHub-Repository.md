# Setting Up a GitHub Repository from Scratch and Making Your First Commit

## AHEEN – Day 2

### 1. Introduction

GitHub is a cloud-based platform used to store, manage, and collaborate on software projects. It uses Git, a version control system, to track changes made to files over time.

In this exercise, you will create a GitHub repository, connect it to a local computer, make your first commit, and push your work to GitHub.

### 2. Learning Objectives

By the end of this exercise, you should be able to:

- Create a GitHub repository.
- Configure Git on your computer.
- Initialize a local Git repository.
- Track files using Git.
- Create your first commit.
- Connect a local repository to GitHub.
- Push your work to a remote repository.

### 3. Prerequisites

Before starting, ensure you have:

1. A GitHub account.
2. Git installed on your computer.
3. Access to a terminal, such as Git Bash.
4. An internet connection.
5. A text editor, such as Visual Studio Code or Notepad.

To confirm that Git is installed, open Git Bash and run:

```bash
git --version
```

**Expected result:** Git displays its installed version.

### 4. Procedure

#### Part A: Create a GitHub Repository

**Step 1: Open GitHub**

Navigate to https://github.com.

Expected result: The GitHub homepage or dashboard appears.

Screenshot: GitHub homepage showing the signed-in account.

**Step 2: Open the repository creation page**

Click the **New repository** option.

Expected result: The repository creation form appears.

Screenshot: New repository creation page.

**Step 3: Enter the repository name**

Enter:

```text
aheen_day_2
```

Expected result: The repository name appears in the name field.

Screenshot: Repository name field showing `aheen_day_2`.

**Step 4: Enter a repository description**

Enter:

```text
AHEEN Day 2 - Introduction to Git and GitHub.
```

Expected result: The description appears in the description field.

Screenshot: Repository description field.

**Step 5: Select repository visibility**

Choose either Public or Private, according to the exercise instructions.

Expected result: Your selected visibility option is marked.

Screenshot: Repository visibility options.

**Step 6: Create the repository**

Click **Create repository**.

Expected result: GitHub creates the repository and displays its repository page.

Screenshot: Newly created `aheen_day_2` repository.

**Important:** Do not initialize the repository with a README file for this exercise. We will create the README locally.

#### Part B: Configure Git on Your Computer

**Step 7: Open Git Bash**

Launch Git Bash on your computer.

Expected result: A terminal window opens.

Screenshot: Git Bash terminal.

**Step 8: Configure your Git username**

Run:

```bash
git config --global user.name "Your Name"
```

Expected result: Git accepts the configuration command without displaying an error.

Screenshot: Git username configuration command.

**Step 9: Configure your Git email**

Run:

```bash
git config --global user.email "your-email@example.com"
```

Use the email address associated with your GitHub account where appropriate.

Expected result: Git accepts the configuration command without displaying an error.

Screenshot: Git email configuration command.

**Step 10: Verify your Git configuration**

Run:

```bash
git config --global --list
```

Expected result: Your configured username and email address are displayed.

Screenshot: Git configuration output.

#### Part C: Create and Initialize the Local Repository

**Step 11: Navigate to your working directory**

Run:

```bash
cd /d/AHEEN
```

Expected result: The terminal moves into the AHEEN directory.

Screenshot: Terminal showing the working directory.

**Step 12: Create the project directory**

Run:

```bash
mkdir aheen_day_2
```

Expected result: A new directory named `aheen_day_2` is created.

Screenshot: Directory creation command.

**Step 13: Enter the project directory**

Run:

```bash
cd aheen_day_2
```

Expected result: The terminal is now inside the project directory.

Screenshot: Terminal showing the project directory.

**Step 14: Initialize the Git repository**

Run:

```bash
git init -b main
```

Expected result: Git initializes a repository using `main` as the initial branch.

Screenshot: Git initialization confirmation.

*Note: The `-b main` option requires Git version 2.28 or later.*

#### Part D: Create and Track Your First File

**Step 15: Create a README file**

Create a file named:

```text
README.md
```

Add the following content:

```markdown
# AHEEN Day 2

This repository contains my practical exercises
on Git and GitHub.

## Topics Covered

- Introduction to Git
- GitHub repository creation
- Git initialization
- First commit
- Pushing changes to GitHub
```

Expected result: A README file containing the project information is created.

Screenshot: README.md open in your text editor.

**Step 16: Check the repository status**

Run:

```bash
git status
```

Expected result: Git identifies `README.md` as an untracked file.

Screenshot: Git status showing the untracked README file.

**Step 17: Stage the README file**

Run:

```bash
git add README.md
```

Expected result: Git stages the README file for the next commit.

Screenshot: Git add command.

**Step 18: Verify the staged file**

Run:

```bash
git status
```

Expected result: Git displays `README.md` under changes to be committed.

Screenshot: Git status showing the staged README file.

#### Part E: Make Your First Commit

**Step 19: Commit the staged file**

Run:

```bash
git commit -m "Add README"
```

Expected result: Git creates the first commit and displays a confirmation.

Screenshot: Successful first commit output.

**Step 20: Verify the commit history**

Run:

```bash
git log --oneline
```

Expected result: Git displays the newly created commit and its short commit hash.

Screenshot: Git log showing the first commit.

#### Part F: Connect the Local Repository to GitHub

**Step 21: Copy the repository URL**

Open your GitHub repository and copy its HTTPS URL.

Expected result: The repository URL is copied.

It should follow this format:

```text
https://github.com/YOUR_USERNAME/aheen_day_2.git
```

Screenshot: GitHub repository page showing the HTTPS clone URL.

**Step 22: Add the remote repository**

Run the following command, replacing `YOUR_USERNAME` with your GitHub username:

```bash
git remote add origin https://github.com/YOUR_USERNAME/aheen_day_2.git
```

Expected result: Git connects the local repository to the remote repository named `origin`.

Screenshot: Git remote add command.

**Step 23: Verify the remote connection**

Run:

```bash
git remote -v
```

Expected result: Git displays the remote repository URL for fetching and pushing.

Screenshot: Remote repository configuration output.

#### Part G: Push Your First Commit to GitHub

**Step 24: Push the local main branch**

Run:

```bash
git push -u origin main
```

Expected result: Git uploads the commit to GitHub and establishes upstream tracking for the local `main` branch.

Screenshot: Successful Git push output.

**Step 25: Verify the repository on GitHub**

Refresh your GitHub repository page.

Expected result: The `README.md` file appears in the repository, together with the commit history.

Screenshot: GitHub repository showing README.md and the first commit.

### 5. Troubleshooting

| Problem | Possible cause | Solution |
|---|---|---|
| `git: command not found` | Git is not installed or is not in PATH | Install Git and reopen the terminal |
| `fatal: not a git repository` | Terminal is outside the repository directory | Navigate into the project directory |
| `remote origin already exists` | A remote named origin has already been configured | Use `git remote set-url origin REPOSITORY_URL` |
| `src refspec main does not match any` | No commit exists yet or the branch name differs | Create a commit and verify the branch using `git branch` |
| `Repository not found` | Incorrect repository URL or insufficient access | Verify the URL and GitHub account permissions |
| `Authentication failed` | GitHub authentication was unsuccessful | Sign in using Git Credential Manager or another supported authentication method |

**Important authentication note:** GitHub does not accept normal account passwords for HTTPS Git operations. Use Git Credential Manager or an appropriately configured personal access token. Never share your token or commit it to your repository.

### 6. Important Git Commands Reference

| Command | Purpose |
|---|---|
| `git --version` | Checks the installed Git version |
| `git config --global user.name` | Configures the Git username |
| `git config --global user.email` | Configures the Git email |
| `git init -b main` | Initializes a repository with the main branch |
| `git status` | Displays the working-tree status |
| `git add README.md` | Stages the README file |
| `git commit -m "Add README"` | Creates a commit |
| `git log --oneline` | Displays a compact commit history |
| `git remote -v` | Displays configured remote repositories |
| `git push -u origin main` | Pushes the main branch to GitHub |

### 7. Conclusion

In this exercise, you created a GitHub repository, configured Git, initialized a local repository, created and staged a README file, made your first commit, and pushed the changes to GitHub.

These are foundational Git and GitHub skills that support version control, collaboration, and software development workflows.

**End of Exercise – AHEEN Day 2**