---
name: bug-report-triage
description: Methodology for autonomously assessing the validity of a general bug report from a client about a WordPress powered website. Emphasizes reproduction on a local development environment in order to validate the report, including visual testing and creating any content needed to reproduce the issue.
---

# Bug Report Triage

## Read the report

Read the referenced report file that contains the bug report. Focus on the core issue that's being reported rather than any claimed side effects or secondary symptoms. Determine any prerequisites or dependencies in the report, in particular configuration of WordPress settings, user roles, post content, browser, or viewport size.

If the referenced report file doesn't exist, immediately stop and show a message indicating that the report file is missing and that the triage cannot proceed.

## Prepare the local development environment

Fetch the URL of the local development environment by running `wp option get home` from the current directory. Read docker-compose.yml and README.md for further configuration info.

Use Playwright MCP to access the environment at its URL, read the console, and interact with the web page. Use `browser_run_code` to speed up sequences of interactions where appropriate.

## Set up the content and state described in the report

The bug report may assume a site state that doesn't exist on the local environment. Before attempting to reproduce, create the content and configuration the report describes or implies.

* Use WP-CLI commands to check for existing users, settings, and content first to understand the state of the development site.
* Use WP-CLI commands to set up a user with the correct role, the settings, theme configuration, categories, etc.
* Create any posts, pages, custom post types, taxonomies, comments, media, or other content that the report references or depends on. If the report mentions specific content (for example "on the About page" or "a post with a featured image"), create content that matches as closely as possible.
* Use Playwright MCP to perform administrative actions such as creating posts in the block editor or using the site editor.
* There should be no need to install any new plugins, but there may be a need to activate existing ones if necessary.
* When executing PHP with WP-CLI, write the PHP to a file and then use the `eval-file` command rather than `eval`.
* Use a direct mysql database connection, the `$wpdb` global in PHP, or WP-CLI to read data from the database as necessary.
* You can write to the database directly only if there isn't an existing API in WordPress, WP-CLI command, or REST API endpoint to achieve the same.
* You can use your standard writing and editing tools to write to files. The `src` directory is mounted to the container.

## Reproduce the reported bug

Autonomously use this development environment to attempt to reproduce the bug in order to determine the validity of the report. Generally presume that the report is valid and attempt to reproduce it as described, however use your initiative if the steps don't work or aren't clear.

The bug may require a chain of actions, such as configuring the site, logging in as a user with a specific role, setting up options or menus or theme settings, creating content, and then viewing the site, the wp-admin area, the block editor, the site editor, or the REST API. Carefully follow multi-step instructions to reproduce.

### Visual testing

The bug report may involve visual or behavioural issues in the browser — layout problems, broken styles, misaligned elements, unexpected JavaScript behaviour, editor glitches, or responsive issues. When the report concerns anything visible or interactive:

* Use Playwright MCP's `browser_take_screenshot` to capture the state of the page at each relevant step. Save screenshots next to the report file for reference.
* Use `browser_snapshot` to capture the accessibility tree when the issue concerns structure, labelling, or interactive elements rather than pure visuals.
* Use `browser_resize` to test at the viewport size mentioned in the report (or common breakpoints: mobile ~375px, tablet ~768px, desktop ~1280px) if the bug is responsive in nature.
* Check `browser_console_messages` for JavaScript errors or warnings that may be related to the bug, even if the report doesn't mention them.
* Check `browser_network_requests` for failed requests, 404s, 500s, or unexpected responses when the bug may involve AJAX, REST API calls, or asset loading.
* Compare the observed behaviour against what the report describes. If the bug is visual, compare against any screenshots or mockups the client provided.
* Test both logged-in and logged-out states if the bug could be affected by authentication.

### Initiative

Use initiative if the exact steps to reproduce don't work, aren't clear, or are ambiguous. The bug report may be imprecise — it may describe symptoms rather than exact steps, use non-technical language, or omit details that seem obvious to the client but aren't to a triager. You are free to use the development environment with little consequence, however you should never commit any changes or use any git commands that write to the repo. It's unlikely you'll need to read the git log, but it's there if you need it.

## Autonomy

* Some reports may not contain enough information to clearly understand the bug being reported, or the report may contain ambiguities. In this case you should proceed with the triage in order to autonomously understand and attempt to reproduce the issue using your knowledge of using and developing on WordPress.
* It may take several attempts to determine whether the report can be reproduced and is therefore valid.
* If reproduction requires information the client didn't provide (specific content, a user action sequence, a browser or device), make a reasonable assumption, document it in the summary, and proceed.

## Determine status

* The report may be inaccurate or caused by something other than what the client assumes, for example a caching issue, a third-party plugin conflict, browser-specific behaviour, or expected WordPress behaviour that the client wasn't aware of.
* The report may be reproducible only under specific conditions that the client didn't mention — note these in the summary so the developer fixing it has the full picture.
* After determining the validity of the report, produce a summary as markdown and save it to a file next to the report file. Include:
  * Whether the bug was reproduced.
  * The exact steps that reproduced it (or the steps attempted if it couldn't be reproduced).
  * Any screenshots captured during reproduction.
  * Any console errors, failed network requests, or other technical signals observed.
  * Any assumptions made to fill gaps in the report.
  * A preliminary assessment of scope (for example: front-end only, affects all users, only affects a specific role, only at a specific viewport).
* Do not stop or otherwise reset the local development environment.
* Do not delete any test files or content you've created during determination as we might want to refer to them at a later date.
