# How to Contribute

First off, thank you for considering contributing to **Grafana Final Scanner**. Contributions like yours help make security assessment tools more reliable and effective for everyone who uses them responsibly.

This document covers the key guidelines for contributing. Whether you're a first‑time contributor or a seasoned open source veteran, you're welcome here.

</br>

## Table of Contents

- [Ways to Contribute](#ways-to-contribute)
  - [Report a Bug](#report-a-bug)
  - [Suggest an Enhancement](#suggest-an-enhancement)
  - [Contribute Code](#contribute-code)
- [Getting Started](#getting-started)
  - [Branching Strategy](#branching-strategy)
  - [Writing Commit Messages](#writing-commit-messages)
  - [Submitting a Pull Request](#submitting-a-pull-request)
  - [PR Checklist](#pr-checklist)
- [Coding Standards](#coding-standards)
  - [Python Style](#python-style)
  - [Code Organization](#code-organization)
- [Adding Tests for New Features](#adding-tests-for-new-features)
- [Reporting Security Vulnerabilities](#reporting-security-vulnerabilities)
- [Getting Help](#getting-help)
- [Recognition](#recognition)

</br>

## Ways to Contribute

</br>

**There are many ways to contribute, not just by writing code.**

</br>

### Report a Bug

Found something unexpected? Please open an issue with:

- **Use a clear and descriptive title** for the issue to identify the problem.
- **Describe the exact steps which reproduce the problem** in as many details as possible.
- **Describe the behavior you observed after following the steps** and point out what exactly is the problem with that behavior.
- **Include screenshots and animated GIFs** which show you following the described steps and clearly demonstrate the problem.
- **Describe your environment you are using Grafana Scanner** (e.g. OS, Python version).
- **Other relevant logs or error messages** that's necessary and may help us undrestand the situation you are going through.

### Suggest an Enhancement

Have an idea to make the scanner better? Open an issue with:

- A clear description of the feature or improvement
- Why this would be valuable
- Any implementation ideas you might have (not required, but appreciated)

### Contribute Code

Ready to write some code? Great! Here’s what we’re looking for:

- New CVE detection modules
- False positive / false negative fixes
- Configuration analysis enhancements
- Web dashboard improvements
- Documentation updates
- Test cases
- Performance optimizations

</br>

**Before diving into a large change, please open an issue first to discuss your approach. This ensures we're aligned and saves you potential rework.**

</br>

## Getting Started

### Branching Strategy

- **`main`** - Stable, production‑ready code. Everything here should work.
- ***Create separate branches other than main, so you can group relevant commits into a single, focused branch.***
   - For example, if you are working on UI improvements for the web dashboard in scanner.py, create a branch like `ui/improve-dashboard-readability` and commit only UI related changes there. This makes it easier to track what happened and why.
- After modifying the repository and committing related changes to each branch, ***you should create a separate branch that consolidates all the work you have done so far across those branches.***

  - Following the previous example, once you finish your UI modifications, create a branch named `ui/latest-changes`. Then, merge all branches following the pattern `ui/ui-related-name` (such as `ui/improve-dashboard-readability`) into `ui/latest-changes`. This `ui/latest-changes` branch represents the complete set of UI changes up to that moment.
  - This rule applies to all feature, fix, or other change branches - with one exception: documentational branches.

  - Documentational branches are those that hold changes only to `*.md` files. Changes shown in scanner.py or other code files are not considered documentation branches; they belong to the UI subgroup of the scanner. Documentation branches should be merged directly into your main branch after you have finished merging all other branches.
- ***When you have completed all work on a specific functionality or area of the project, merge your latest changes branch into*** **`main`*****.***
   - As noted earlier, documentation changes (committed to branches like `docs/docs-related-name`) should be merged directly into main as well (there is no need to create a `docs/latest-changes` branch). Merge each branch one by one.

### Writing Commit Messages

Good commit messages make everyone’s life easier. We follow the [Conventional Commits](https://www.conventionalcommits.org/) format but with a tiny difference. The difference is instead of `<type>: ...`(with a space before the colon) we write `<type> : ...`(without a space)
> [!note]
> If you prefer the standard `<type>: ...` format, that is also perfectly acceptable. Both styles are welcome. For reference, you can see [my own commit history](https://github.com/amirrezafatemi) on this project as an example.

</br>

```
<type>(<scope (optional)>) : <subject>

<body (optional)>

<footer (optional)>
```

**Types**:
- `feat` - A new feature
- `fix` - A bug fix
- `docs` - Documentation updates
- `style` - Code style changes (formatting, semicolons, etc.)
- `refactor` - Code changes that neither fix bugs nor add features
- `test` - Adding or updating tests
- `chore` - Maintenance tasks (dependencies, config, etc.)

**Examples**:
```
feat : add detection for CVE-2025-1234
```
```
fix : correct version detection for Grafana
```
```
style(ui) : improve banner options

'python' was missing in using examples
```
```
docs(readme) : update installation instructions for Windows
```
```
docs : update README.md

An update for Command-Line Options of v3.1
```

</br>

> [!NOTE]
> Keep your commits focused - one logical change per commit.
>
> Basically, `<scope>` tells us what part of code did you modified.
> 
> Writing the `<scope>` is optional, but it helps avoid confusion about which part of the codebase changed. You can also omit the `<scope>` entirely and provide a complete, sufficient description in the `<body>` instead, or 

</br>

### Submitting a Pull Request

When you're ready to submit your changes after reviewing our [pull request checklist](#pr-checklist), follow these general steps:

1. **Keep your fork synchronized**
    - Ensure your local `main` branch is up to date with the upstream repository before starting any work.

3. **Create a feature branch**
    - Always make your changes in a dedicated branch. Name it clearly based on what you're working on (e.g., `feature/add-cve-1234` or `fix/false-positive-issue`).

5. **Test your changes thoroughly**
    - Run the scanner against test instances to verify detection accuracy. Ensure existing functionality remains unaffected.

7. **Write clear commit messages**
    - Follow the Conventional Commits format described above. Each commit should represent one logical change.

9. **Push your branch to your fork**
     - Upload your feature branch to your GitHub fork so it becomes available for review.

11. **Open a Pull Request**
     - Go to the original repository on GitHub and click “Compare & pull request”. Select your feature branch as the source. In the PR description, include:
      - What your changes do
      - Why they are needed
      - Any testing you have performed
      - Screenshots for UI changes (if applicable)

11. **Respond to feedback**
     - Be open to questions and suggestions from maintainers. Pull requests are a collaborative conversation, not a judgment. You may need to update your branch based on review comments.

13. **Keep your branch updated**
     - If the `main` branch advances while your PR is open, rebase your feature branch on the latest `main` and push the updated version.

Once approved, a maintainer will merge your contribution. Thank you for helping improve this project.

### PR Checklist

Before submitting a pull request, please ensure:

- [ ] Your code follows the project’s coding standards
- [ ] You have added or updated tests where appropriate
- [ ] You have updated documentation (README, comments, etc.)
- [ ] Your commits are atomic and well‑described
- [ ] Your branch is based on the latest `main`
- [ ] All existing tests pass

</br>

## Coding Standards

We value clean, readable, and maintainable code. Please follow these guidelines.

### Python Style

- Follow [PEP 8](https://peps.python.org/pep-0008/)
- Use 4 spaces for indentation (no tabs)
- Maximum line length of 88 characters (compatible with Black formatter)
- Use meaningful variable and function names
- Include docstrings for public functions and classes

### Code Organization

- Add new CVEs in the appropriate module
- Keep detection logic focused and testable
- Avoid duplication - reuse existing helper functions
- Handle errors gracefully

</br>

## Adding Tests for New Features

When adding a new CVE detection:

1. Test against a known vulnerable Grafana instance (in a controlled environment)
2. Test against a patched version to verify no false positives
3. Document your test cases

When fixing false positives:

1. Reproduce the issue
2. Verify your fix resolves it
3. Confirm you did not introduce new false positives

</br>

## Reporting Security Vulnerabilities

**Please do NOT report security vulnerabilities through public GitHub issues.**

Instead, please contact the maintainers directly. For critical vulnerabilities, we practice responsible disclosure.

To report a security issue:

1. **Create a private security advisory**
    - GitHub allows this directly in the repository.

    - Go to `Security -> Advisories -> New draft security advisory`.
3. **Provide details**
    - Include steps to reproduce, affected versions, and potential impact.
5. **Allow time for a fix**
    - We will work on a patch and coordinate disclosure.

> [!important]
> As stated in the README, this tool is only for authorized security assessments. **Never use it against systems you do not own or lack explicit permission to test.**

</br>

## Getting Help

Stuck? Have questions? Here is where to find help:

- **Open an issue** for bugs, feature requests, or questions
- **Comment on an existing issue** - Many hands make light work
- **Check the [README](https://github.com/amirrezafatemi/Grafana-Final-Scanner/blob/main/README.md)** - It contains detailed usage information

</br>

## Recognition

Every contribution matters, and we want to celebrate that. Contributors will be:

- Listed in the project's **Contributors** section
- Mentioned in release notes for significant changes
- Recognized in the repository's GitHub Insights

</br>

## Credits

**Checkout all these people helped this repo out:**

[![GitHub contributors](https://contrib.rocks/image?repo=amirrezafatemi/Grafana-Final-Scanner)](https://github.com/amirrezafatemi/Grafana-Final-Scanner/graphs/contributors)

**Thank you again for helping make Grafana Final Scanner better. Your time and effort are genuinely appreciated. ❤️**

---

_<p align=center>This guide is inspired by the best practices of the open source community. Questions or suggestions for improving it? Open an issue!</p>_
