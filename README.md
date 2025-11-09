# How to Connect RStudio and GitHub: A Step-by-Step Guide

This guide explains how to configure RStudio and GitHub to work together seamlessly. This allows you to use Git for version control directly within RStudio, making it easy to track changes, collaborate, and back up your work.

We will use the {usethis} and {gitcreds} packages, which provide the safest and most efficient way to set this up.

## 🎯 Why Connect RStudio and GitHub?

Before we get to the "how," let's talk about the "why." Integrating RStudio with GitHub completely changes your workflow for the better:

* **Version Control**: Think of it as "track changes" for your code. You can save "snapshots" (commits) of your project, experiment freely, and easily roll back to a previous version if something breaks.

* **Backup & Security**: Your computer is one place. GitHub is a secure, remote copy of your entire project history. If your laptop is lost or damaged, your work is safe.

* **Collaboration**: This is the standard way to collaborate on code. You can work on projects with colleagues, share your work, and manage contributions.

* **Essential for R Packages**: If you are interested in creating an R package, this workflow is essential. It is the industry standard for developing, documenting, and sharing package source code.

## 📋 Prerequisites

Before you start, make sure you have:

- ✅ R and RStudio installed on your computer
- ✅ A GitHub account (free at [github.com](https://github.com))
- ✅ Internet connection
- ✅ Basic familiarity with RStudio

## 📚 Table of Contents

- [Part 1: One-Time Setup](#part-1-one-time-setup)
- [Part 2: Daily Workflow](#part-2-daily-workflow)
- [Troubleshooting](#troubleshooting)
- [Quick Reference](#quick-reference)
- [Additional Resources](#additional-resources)

---

## Part 1: One-Time Setup

You only need to do this part **once** on your machine.

### Step 1: Install and Load Packages

First, install the necessary packages in your RStudio console:

```r
install.packages("usethis")
install.packages("gitcreds")
```

### Step 2: Configure Git

Next, introduce yourself to Git. Git needs to know who you are to log your changes.

Run this command in the RStudio console, replacing the example text with **your actual name and GitHub email**:

```r
usethis::use_git_config(
  user.name = "Your Name", 
  user.email = "your.email@example.com"
)
```

⚠️ **Important**: Use the same email address associated with your GitHub account.

### Step 3: Get a GitHub Personal Access Token (PAT)

To let RStudio securely talk to GitHub, you need a Personal Access Token (PAT). Think of it as a secure password specifically for RStudio.

#### 3.1 Generate the Token

Run this command in RStudio:

```r
usethis::create_github_token()
```

This command will:
1. Open your GitHub account in a web browser
2. Take you directly to the "New personal access token" page

#### 3.2 Configure Your Token on GitHub

You may need to log in to GitHub first. Once on the token creation page:

1. **Note/Token name**: Give it a descriptive name
   - Examples: `RStudio-Token`, `My-Laptop-Token`, `Work-Computer-2024`

2. **Expiration**: Set an expiration date
   - Recommended: 90 days or 1 year
   - You'll need to create a new token when it expires (GitHub will email you)

3. **Scopes** (permissions): Select the following:
   - ✅ **repo** (Full control of private repositories) - *This is essential*
   - ✅ **workflow** (Update GitHub Actions workflows) - *Optional, but useful*

4. Scroll to the bottom and click **"Generate token"**

5. **CRITICAL**: GitHub will show you the token **only once**
   - It looks like: `ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
   - Copy it immediately and keep this browser tab open until you complete Step 4

### Step 4: Store Your Token Securely

Now, go back to RStudio. We will use {gitcreds} to securely store this token.

Run this command in the console:

```r
gitcreds::gitcreds_set()
```

When prompted:
1. Paste the token you just copied from GitHub
2. Press Enter

The token is now securely stored in your computer's credential manager. You won't need to enter it again (until it expires).

### Step 5: Verify Your Setup

Run this command to get a "situation report":

```r
usethis::git_sitrep()
```

✅ **Success looks like**:
- Git version detected
- `user.name` and `user.email` configured
- "Personal access token: '<discovered>'"
- "Token scopes: 'repo, workflow'"

If everything checks out, you're ready to go! 🎉

---

## Part 2: Daily Workflow

Now that you're set up, here's how you connect your R projects to GitHub.

### Method 1: Start a New Project in RStudio (Local-First)

Use this workflow when you're starting a **brand-new project** from scratch on your own machine.

#### Step 1: Create a New RStudio Project

1. Go to **File > New Project...**
2. Select **New Directory**
3. Select **New Project**
4. Give your project a name (e.g., `my-cool-analysis`)
5. Choose where to save it on your computer
6. Click **Create Project**

This creates the folder and the `.Rproj` file.

#### Step 2: Initialize Git

Once the project is open, run this in the console:

```r
usethis::use_git()
```

This initializes a local Git repository. RStudio will ask to restart — click **"Yes"**.

After restart, you'll see a new **Git tab** in the top-right pane! 🎉

#### Step 3: Make Your First Commit

1. Click the **Git tab**
2. Check the boxes next to `.Rproj` and `.gitignore` to "stage" them
3. Click **"Commit"**
4. In the window that opens, write a commit message: `"Initial commit"`
5. Click **"Commit"**
6. Close the commit window

#### Step 4: Connect to GitHub

Now, run this command in the console:

```r
usethis::use_github()
```

This magical command will:
- Create a new **private** repository on your GitHub account
- Connect your local project to it
- Push your first commit to GitHub
- Open the repository in your browser

✅ **Done!** Your project is now on GitHub and connected to RStudio.

---

### Method 2: Connect to an Existing GitHub Repository (Remote-First)

Use this workflow to join an **existing project** or if you prefer to create the repository on GitHub.com first.

#### Step 1: Get the Repository URL (In Your Browser)

1. Go to [GitHub](https://github.com)
2. Navigate to the repository you want to clone (or create a new one)
3. Click the green **"<> Code"** button
4. Make sure **HTTPS** is selected (not SSH)
5. Click the copy icon to copy the URL
   - It looks like: `https://github.com/Your-Name/Your-Repo.git`

#### Step 2: Clone the Repository (In RStudio)

1. Go to **File > New Project...**
2. Select **Version Control**
3. Select **Git**
4. In the **"Repository URL"** field, paste the URL you copied
5. RStudio will automatically fill in the **"Project directory name"**
6. Choose where to save the project on your computer
7. Click **"Create Project"**

RStudio will now:
- Download (or "clone") the repository from GitHub
- Create the `.Rproj` file for you
- Open the project with the Git tab already visible

✅ **Done!** You're now connected and ready to work.

---

## Your Day-to-Day Process

Regardless of which method you used, your daily workflow in the RStudio Git tab is the same:

### The Git Workflow (Pull → Work → Stage → Commit → Push)

1. **Pull** 🔵: Always click the blue **"Pull"** arrow (⬇️) before you start working
   - This gets the latest changes from GitHub
   - Prevents merge conflicts

2. **Work** ✏️: Write your code, save your files, make changes

3. **Stage** ☑️: Check the boxes for the files you've changed
   - This tells Git which files to include in your next commit

4. **Commit** 💾: Click **"Commit"**
   - Write a descriptive message (e.g., "Add data cleaning script")
   - Click **"Commit"** button
   - This saves a snapshot of your changes locally

5. **Push** 🟢: Click the green **"Push"** arrow (⬆️)
   - This sends your commits to GitHub
   - Now your changes are backed up and visible to collaborators

### Commit Message Best Practices

Good commit messages help you (and others) understand what changed:

✅ **Good**:
- "Add data cleaning function"
- "Fix bug in plot_results()"
- "Update README with installation instructions"

❌ **Bad**:
- "Update"
- "Fix stuff"
- "asdfasdf"

---

## Troubleshooting

### Git Tab Not Showing Up

**Solution**: 
1. Make sure you ran `usethis::use_git()` and restarted RStudio
2. Check that you're in an RStudio Project (look for `.Rproj` file)
3. Try closing and reopening the project

### "Authentication Failed" Error

**Possible causes**:
- Your PAT (token) has expired
- Token wasn't stored correctly

**Solution**:
1. Create a new token: `usethis::create_github_token()`
2. Store it again: `gitcreds::gitcreds_set()`
3. Verify: `usethis::git_sitrep()`

### "Push Rejected" or "Non-fast-forward" Error

**Cause**: Someone else (or you on another computer) made changes on GitHub that you don't have locally.

**Solution**:
1. Click **Pull** (⬇️) first to get the remote changes
2. Resolve any merge conflicts if they appear
3. Then **Push** (⬆️) your changes

### "Merge Conflict" Message

**Cause**: You and someone else edited the same part of the same file.

**Solution**:
1. Open the conflicted file
2. Look for conflict markers: `<<<<<<<`, `=======`, `>>>>>>>`
3. Manually choose which changes to keep
4. Remove the conflict markers
5. Save the file
6. Stage and commit the resolved file

---

## Quick Reference

### RStudio Git Tab Buttons

| Action | Button | What It Does | Terminal Equivalent |
|--------|--------|--------------|---------------------|
| Get latest changes | Pull ⬇️ (Blue) | Downloads commits from GitHub | `git pull` |
| Save snapshot | Stage + Commit | Saves changes locally | `git add .` then `git commit -m "message"` |
| Upload changes | Push ⬆️ (Green) | Uploads commits to GitHub | `git push` |
| See history | History/Clock icon | View past commits | `git log` |

### Common Terminal Commands

If you prefer using the Terminal tab in RStudio:

```bash
# Get the latest changes from GitHub
git pull

# Stage all changed files
git add .

# Commit with a message
git commit -m "Your descriptive message"

# Push to GitHub
git push

# Check status of your files
git status

# View commit history
git log --oneline
```

---

## Additional Resources

### Want to Learn More?

- 📖 [Happy Git and GitHub for the useR](https://happygitwithr.com/) by Jenny Bryan
  - The most comprehensive guide to Git/GitHub for R users

- 📖 [usethis package documentation](https://usethis.r-lib.org/)
  - Official documentation for the {usethis} package

- 📺 [GitHub's Git Guides](https://github.com/git-guides)
  - Official GitHub tutorials

### Understanding Git Concepts

- **Repository (Repo)**: A project folder tracked by Git
- **Commit**: A saved snapshot of your project at a point in time
- **Clone**: Download a copy of a repository from GitHub to your computer
- **Pull**: Download new commits from GitHub
- **Push**: Upload your commits to GitHub
- **Branch**: A parallel version of your code (advanced topic)
- **Merge**: Combine changes from different branches

### Private vs. Public Repositories

When you create a repository:
- **Private**: Only you (and people you invite) can see it
  - Good for: work projects, early-stage code, personal projects
- **Public**: Anyone on the internet can see it
  - Good for: open-source projects, portfolios, teaching materials

By default, `usethis::use_github()` creates **private** repositories.

---

## Need Help?

If you encounter issues not covered here:

1. Check the error message carefully
2. Run `usethis::git_sitrep()` to diagnose setup issues
3. Search the error message on [Stack Overflow](https://stackoverflow.com/)
4. Ask in the [RStudio Community](https://community.rstudio.com/)

---

**Happy coding! 🚀**

---

*Last updated: November 2024*