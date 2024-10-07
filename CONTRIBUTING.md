# Contributing to Hacktoberfest 2024

Thank you for your interest in contributing! Please follow the instructions below.

## How to Contribute

1. **Star ⭐ & Fork 🍴** this repository.
2. **Create a directory** under your GitHub username.
3. Inside the directory, add your list of static files.
4. **Create a pull request** using the provided PR template.

Good luck on your Hacktoberfest journey!

## Steps to Create a Pull Request

1. **Fork** this repository.
2. **Clone** your forked copy of the project:
   ```bash
   git clone --depth 1 https://github.com/<your_name>/HacktoberFest2024
   ```
3. **Navigate** to the project directory:
   ```bash
   cd HacktoberFest2024
   ```
4. **Add a reference (remote)** to the original repository:
   ```bash
   git remote add upstream https://github.com/Vanshikapandey30/HacktoberFest2024.git
   ```
5. **Check the remotes** for this repository:
   ```bash
   git remote -v
   ```
6. **Update your master branch** with the latest changes from the upstream repository:
   ```bash
   git pull upstream main
   ```
7. **Create a new branch** for your changes:
   ```bash
   git checkout -b <your_branch_name>
   ```
8. **Make your desired changes** to the code base.
9. **Track your changes**:
   ```bash
   git add .
   ```
10. **Commit your changes** with a relevant message:
    ```bash
    git commit -m "Your message here"
    ```
11. **Push your committed changes** to your remote repository:
    ```bash
    git push -u origin <your_branch_name>
    ```
12. **Create a pull request** by comparing your branch with the desired branch in the repository.
13. **Add a title and description** to your pull request, explaining your changes.
14. **Click on Create Pull Request**.

### Contribution Rules

- Pull requests can be submitted to any opted-in repository on GitHub or GitLab.
- Your pull request must contain your own commits.
- If a maintainer marks your pull request as spam, it will not count toward Hacktoberfest.
- To receive a shirt, you must make four approved pull requests (PRs) between October 1-31.
- Only the first 55,000 participants can earn a T-shirt.
- A repository is considered participating in Hacktoberfest if it has the 'hacktoberfest' topic and accepts contributions.
- A pull request is considered approved once it has an approving review from maintainers, has been merged, or has the 'hacktoberfest-accepted' label.
- A pull request with any label containing 'spam' or 'invalid' will be considered ineligible.

Thank you for contributing, and happy coding!
