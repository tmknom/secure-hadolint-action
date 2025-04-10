# secure-hadolint-action

Run [hadolint][hadolint] in an isolated Docker container to securely lint Dockerfiles.

<!-- actdocs start -->

## Description

This action securely runs hadolint in a Docker container to lint Dockerfiles.
It reduces security risks from compromised or malicious third-party tools (e.g., compromised repositories or tampered container images).
To achieve this, it enforces strict container isolation, disables network connections, and drops unnecessary privileges.

## Usage

```yaml
  steps:
    - name: Secure hadolint
      uses: tmknom/secure-hadolint-action@v0
```

## Inputs

N/A

## Outputs

N/A

<!-- actdocs end -->

## Permissions

N/A

## FAQ

### What is hadolint?

[hadolint][hadolint] is a linter for Dockerfiles.
It detects errors, security issues, and best practice violations, helping you maintain safer and more reliable container images.

### Why should I use this action instead of directly using hadolint?

Running third-party tools directly in your environment may expose your repository and credentials to compromised or malicious code.
This action significantly reduces such risks by strictly isolating the environment:

- Network access is completely disabled (`--network none`)
- All unnecessary Linux capabilities are dropped (`--cap-drop all`)
- Privilege escalation is explicitly disabled (`--security-opt no-new-privileges`)
- The action runs as a non-root, restricted user  (`--user 1111:1111`)
- The filesystem is strictly read-only (`--read-only`)
- The repository directory is mounted as read-only (`--volume "${PWD}:${PWD}:ro"`)

### What specific security risks does this action protect against?

This action specifically prevents threats related to software supply chain security (attacks targeting third-party software or tools used in workflows), such as:

- Unauthorized outbound connections from runners, significantly reducing the risk of data leakage
- Malicious updates or compromised tools exploiting elevated privileges or unrestricted network access

### Are network connections permitted inside the Docker container used by this action?

No. Network connections are completely disabled within the container.
Even if the tool were compromised or contained malicious code, disabling network access effectively prevents communication with external attackers, significantly reducing the risk of data leaks.

### Does this action run with elevated (root) privileges?

No. The action runs as a non-root, restricted user without privilege escalation.

### Can I customize the hadolint parameters?

Currently, this action is intentionally designed with secure defaults and does not support custom parameters.
If you require customization, consider creating your own fork of this action.

### Does using this action significantly impact my CI/CD performance?

No. The impact is minimal, primarily due to the overhead of pulling and executing a small Docker container.
Typically, this added overhead is negligible, and the security improvements provided by the action justify its use.

### Can I run this action without Docker?

No. This action strictly requires Docker to be installed on your GitHub Actions runner.
Without Docker, the action will fail to execute, as it relies on Docker’s isolation mechanisms to run securely.

## Related projects

N/A

## Release notes

See [GitHub Releases][releases].

[hadolint]: https://github.com/hadolint/hadolint
[releases]: https://github.com/tmknom/secure-hadolint-action/releases
