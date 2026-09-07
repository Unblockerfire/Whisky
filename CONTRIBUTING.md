# Contributing to Bourbon 🥃

Thanks for wanting to contribute to Bourbon!

Bourbon is a macOS application focused on making Windows software easier to run, manage, and troubleshoot through a native SwiftUI experience.

Contributions of all sizes are welcome, including bug fixes, UI improvements, compatibility work, documentation, diagnostics, and quality-of-life changes.

## Getting started

1. Fork the Bourbon repository.
2. Clone your fork locally.
3. Create a new branch for your change.
4. Make and test your changes.
5. Open a pull request back to Bourbon.

Please keep each branch and pull request focused on one feature, fix, or closely related group of changes whenever possible.

Avoid mixing unrelated cleanup or formatting changes into a functional pull request.

## Development environment

Bourbon is developed as a native macOS application using Swift and SwiftUI.

The project uses Xcode and Swift Package Manager for its dependencies.

Before working on Bourbon, make sure you have:

- A supported version of macOS
- A current compatible version of Xcode
- Git
- Swift Package Manager dependencies resolved

Some parts of Bourbon interact with Wine, BourbonWine, Sparkle, macOS processes, filesystem state, and other system-level components. Changes in these areas should be tested carefully.

## Apple Silicon and Intel Macs

Bourbon supports both Apple Silicon and Intel Macs.

Changes should not assume that every Mac is running on Apple Silicon.

When working with:

- CPU architecture detection
- Rosetta
- Wine processes
- bundled command-line tools
- application packaging
- executable launching

make sure your implementation continues to behave correctly on both architectures.

Where applicable, Bourbon's release builds and bundled tools should remain Universal 2.

## Code style

Bourbon uses SwiftLint to keep the codebase consistent.

Pull requests should pass SwiftLint without introducing new violations.

In general:

- Use 4 spaces for indentation.
- Follow existing Swift and SwiftUI conventions in the project.
- Prefer readable code over clever code.
- Avoid disabling SwiftLint rules unless there is a clear reason.
- Keep platform-specific behavior isolated and documented when necessary.
- Avoid large unrelated refactors inside a bug fix.

If a SwiftLint rule must be disabled, keep the disabled scope as small as possible and explain why when the reason is not obvious.

## SwiftUI and interface changes

Bourbon has its own visual identity and should not simply inherit the default appearance of every native macOS control.

When adding or changing UI:

- Follow the existing Bourbon design language.
- Keep dark surfaces, contrast, spacing, and readability consistent.
- Test controls in the actual app rather than only relying on SwiftUI previews.
- Avoid light system controls appearing unexpectedly inside Bourbon's dark interface.
- Make sure loading, disabled, error, and empty states remain readable.
- Keep layouts usable across different window sizes.

If your pull request changes the interface, include screenshots or a short screen recording when practical.

## Wine and runtime changes

Runtime changes should be treated separately from normal Bourbon application changes.

Do not change Wine, BourbonWine, runtime downloads, compatibility settings, or bundled runtime components simply to work around an application-level bug.

When investigating a compatibility problem, try to determine whether the issue belongs to:

- Bourbon's UI or state management
- Bourbon's process launching
- Bottle configuration
- BourbonWine packaging
- upstream Wine
- macOS
- the Windows application itself

Runtime modifications should include a clear explanation of why the runtime is believed to be the source of the problem.

## Process and installer handling

Bourbon launches and monitors external processes, including Windows installers.

Code in these paths must not block the macOS main thread.

Avoid:

- synchronous process waits on the main actor
- blocking filesystem scans on the main actor
- unbounded diagnostic operations
- assuming a long-running installer has failed
- assuming a launcher exiting means every child process has finished

Installer and process monitoring should distinguish between normal execution, child-process handoff, user cancellation, crashes, and abnormal termination where possible.

Diagnostics should never make the application less stable than the process being diagnosed.

## Diagnostics and privacy

Diagnostic information is extremely useful when debugging Wine and Windows applications, but it may also contain private information.

Do not intentionally log:

- usernames
- home-directory names
- API keys
- authentication tokens
- passwords
- private environment variables
- unrelated process environment contents

Paths and environment information should be redacted or minimized whenever possible.

Debug logging should also avoid becoming required for normal application behavior.

## Localization

User-facing text should use Bourbon's localization system rather than being scattered as hard-coded strings throughout the application.

When adding new user-facing strings, add the appropriate English source string.

Do not include unrelated machine-generated translations in the same pull request.

## Tests

New behavior should include tests when the affected code can reasonably be tested.

Tests are especially encouraged for:

- state transitions
- Bottle behavior
- executable discovery
- architecture detection
- licensing state
- installer lifecycle handling
- diagnostics
- runtime selection
- regressions from previously reported bugs

A regression fix should ideally include a test that would have failed before the fix.

## Before opening a pull request

Please make sure:

- The project builds successfully.
- SwiftLint passes.
- Existing tests pass.
- New functionality has been tested.
- Debug code and temporary logging have been removed unless intentionally required.
- No secrets, credentials, local paths, build artifacts, or personal files are included.
- Changes do not unintentionally modify release or runtime infrastructure.

For changes affecting packaging, architecture, Wine, or BourbonWine, include the validation you performed.

## Pull requests

Give your pull request a clear title and describe:

- What changed
- Why the change was needed
- How you tested it
- Any known limitations
- Whether the change affects Intel, Apple Silicon, Wine, BourbonWine, packaging, or application state

For bug fixes, reproduction steps are especially helpful.

For UI changes, include screenshots when possible.

For compatibility changes, include relevant logs or diagnostics with personal information removed.

## Review

Pull requests must pass the project's required checks before they are ready to merge.

A reviewer may request changes related to:

- correctness
- maintainability
- performance
- macOS behavior
- architecture compatibility
- UI consistency
- privacy
- runtime safety

Please keep follow-up commits focused on the review feedback.

Approval does not automatically mean a change will be included in the next public release. Some changes may require additional testing before release.

## Security

Do not open a public issue containing credentials, private keys, authentication tokens, or other sensitive information.

If you discover a security issue, report it privately rather than publishing an exploit or sensitive details in a public issue.

## Community

Bourbon is built with help from its users, testers, contributors, and the wider Wine and macOS development communities.

Whether you're fixing a typo, tracking down a Wine crash, improving the UI, or building an entirely new feature, thank you for helping improve Bourbon. 🥃
