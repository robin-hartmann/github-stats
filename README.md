# 📊 [GitHub Stats Visualization][github-stats-orig]

[<picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/robin-hartmann/github-stats/master/generated/dark/overview.svg"><img alt="GitHub Statistics for Robin Hartmann" src="https://raw.githubusercontent.com/robin-hartmann/github-stats/master/generated/light/overview.svg"></picture>][github-stats-custom]
[<picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/robin-hartmann/github-stats/master/generated/dark/languages.svg"><img alt="Languages used by Robin Hartmann" src="https://raw.githubusercontent.com/robin-hartmann/github-stats/master/generated/light/languages.svg"></picture>][github-stats-custom]

Generate visualizations of GitHub user and repository statistics using GitHub
Actions.

This project is built upon Jacob Strieb's [GitHub Stats Visualization][github-stats-orig]
with some minor changes:

- Stars, forks and views are only counted for repositories, where you are owner or collaborator
- Templates used for image generation have been reworked
- Images are generated in different themes: light, dark, and auto (i.e., sensitive to `prefers-color-scheme`)

## Background

When someone views a profile on GitHub, it is often because they are curious
about a user's open source projects and contributions. Unfortunately, that
user's stars, forks, and pinned repositories do not necessarily reflect the
contributions they make to private repositories. The data likewise does not
present a complete picture of the user's total contributions beyond the current
year.

This project aims to collect a variety of profile and repository statistics
using the GitHub API. It then generates images that can be displayed in
repository READMEs, or in a user's [Profile
README](https://docs.github.com/en/github/setting-up-and-managing-your-github-profile/managing-your-profile-readme).

Since the project runs on GitHub Actions, no server is required to regularly
regenerate the images with updated statistics. Likewise, since the user runs
the analysis code themselves via GitHub Actions, they can use their GitHub
access token to collect statistics on private repositories that an external
service would be unable to access.

## Disclaimer

If the project is used with an access token that has sufficient permissions to
read private repositories, it may leak details about those repositories in
error messages. For example, the `aiohttp` library—used for asynchronous API
requests—may include the requested URL in exceptions, which can leak the name
of private repositories. If there is an exception caused by `aiohttp`, this
exception will be viewable in the Actions tab of the repository fork, and
anyone may be able to see the name of one or more private repositories.

Due to some issues with the GitHub statistics API, there are some situations
where it returns inaccurate results. Specifically, the repository view count
statistics and total lines of code modified are probably somewhat inaccurate.
Unexpectedly, these values will become more accurate over time as GitHub
caches statistics for your repositories. Additionally, repositories that were
last contributed to more than a year ago may not be included in the statistics
due to limitations in the results returned by the API.

For more information on inaccuracies, see issue
[#2](https://github.com/jstrieb/github-stats/issues/2),
[#3](https://github.com/jstrieb/github-stats/issues/3), and
[#13](https://github.com/jstrieb/github-stats/issues/13).

# Installation

<!-- TODO: Add details and screenshots -->

1. Create a personal access token (not the default GitHub Actions token) using
   the instructions
   [here](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token).
   Personal access token must have permissions: `read:user` and `repo`. Copy
   the access token when it is generated – if you lose it, you will have to
   regenerate the token.
   - Some users are reporting that it can take a few minutes for the personal
     access token to work. For more, see
     [#30](https://github.com/jstrieb/github-stats/issues/30).
2. Create a copy of this repository by clicking
   [here](https://github.com/robin-hartmann/github-stats/generate). Note: this is
   **not** the same as forking a copy because it copies everything fresh,
   without the huge commit history.
3. Go to the "Secrets" page of your copy of the repository. If this is the
   README of your copy, click [this link](../../settings/secrets/actions) to go
   to the "Secrets" page. Otherwise, go to the "Settings" tab of the
   newly-created repository and go to the "Secrets" page (bottom left).
4. Create a new secret with the name `ACCESS_TOKEN` and paste the copied
   personal access token as the value.
5. It is possible to change the type of statistics reported by adding other
   repository secrets.
   - To ignore certain repos, add them (in owner/name format e.g.,
     `jstrieb/github-stats`) separated by commas to a new secret—created as
     before—called `EXCLUDED`.
   - To ignore certain languages, add them (separated by commas) to a new
     secret called `EXCLUDED_LANGS`. For example, to exclude HTML and TeX you
     could set the value to `html,tex`.
6. Go to the [Actions
   Page](../../actions?query=workflow%3A"Generate+Stats+Images") and press "Run
   Workflow" on the right side of the screen to generate images for the first
   time.
   - The images will be automatically regenerated every 24 hours, but they can
     be regenerated manually by running the workflow this way.
7. Take a look at the images that have been created in the following folders:
   - [`generated/auto`](generated/auto): automatic theme (i.e., sensitive to `prefers-color-scheme`)
   - [`generated/dark`](generated/dark): dark theme only
   - [`generated/light`](generated/light): light theme only
8. To add your statistics to your GitHub Profile README, copy and paste the
   following lines of code into your markdown content
   (they will [automatically match the viewer's GitHub theme](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#specifying-the-theme-an-image-is-shown-to)).
   Replace `username` with your GitHub username and `User Name` with
   your name written out in full (e.g., `Robin Hartmann`):
   ```md
   [<picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/username/github-stats/master/generated/dark/overview.svg"><img alt="GitHub Statistics for User Name" src="https://raw.githubusercontent.com/username/github-stats/master/generated/light/overview.svg"></picture>](https://github.com/username/github-stats#readme)
   ```
   ```md
   [<picture><source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/username/github-stats/master/generated/dark/languages.svg"><img alt="Languages used by User Name" src="https://raw.githubusercontent.com/username/github-stats/master/generated/light/languages.svg"></picture>](https://github.com/username/github-stats#readme)
   ```
9. To use the statistics outside of GitHub, copy and paste one of the
   following lines of code into your HTML content. Replace `username`
   with your GitHub username and `User Name` with your name written out in full
   (e.g., `Robin Hartmann`):
   - automatic theme (i.e., sensitive to `prefers-color-scheme`):
     ```html
     <img src="https://raw.githubusercontent.com/username/github-stats/master/generated/auto/overview.svg" width="320" height="210" alt="GitHub Statistics for User Name">
     <img src="https://raw.githubusercontent.com/username/github-stats/master/generated/auto/languages.svg" width="320" height="210" alt="Languages used by User Name">
     ```
   - dark theme only:
     ```html
     <img src="https://raw.githubusercontent.com/username/github-stats/master/generated/dark/overview.svg" width="320" height="210" alt="GitHub Statistics for User Name">
     <img src="https://raw.githubusercontent.com/username/github-stats/master/generated/dark/languages.svg" width="320" height="210" alt="Languages used by User Name">
     ```
   - light theme only:
     ```html
     <img src="https://raw.githubusercontent.com/username/github-stats/master/generated/light/overview.svg" width="320" height="210" alt="GitHub Statistics for User Name">
     <img src="https://raw.githubusercontent.com/username/github-stats/master/generated/light/languages.svg" width="320" height="210" alt="Languages used by User Name">
     ```
10. Link back to this repository so that others can generate their own
   statistics images.
11. Star this repo if you like it!


# Support the Project

There are a few things you can do to support the project:

- Star the repository (and follow me on GitHub for more)
- Share and upvote on sites like Twitter, Reddit, and Hacker News
- Report any bugs, glitches, or errors that you find

These things motivate me to to keep sharing what I build, and they provide
validation that my work is appreciated! They also help me improve the
project. Thanks in advance!

If you are insistent on spending money to show your support, I encourage you to
instead make a generous donation to one of the following organizations. By advocating
for Internet freedoms, organizations like these help me to feel comfortable
releasing work publicly on the Web.

- [Electronic Frontier Foundation](https://supporters.eff.org/donate/)
- [Signal Foundation](https://signal.org/donate/)
- [Mozilla](https://donate.mozilla.org/en-US/)
- [The Internet Archive](https://archive.org/donate/index.php)


# Related Projects

- Built upon [jstrieb/github-stats][github-stats-orig]
- Inspired by a desire to improve upon
  [anuraghazra/github-readme-stats](https://github.com/anuraghazra/github-readme-stats)
- Makes use of [GitHub Octicons](https://primer.style/octicons/) to precisely
  match the GitHub UI

[github-stats-orig]: https://github.com/jstrieb/github-stats#readme
[github-stats-custom]: https://github.com/robin-hartmann/github-stats#readme
