# GitHub Actions Maven Release POC

## Overview
This proof of concept (POC) demonstrates how to automate the release process for Maven projects using GitHub Actions.
Specifically, it showcases the use of two key workflows:
1. `create-release` - Handles version preparation and GitHub release creation
2. `publish-tag` - Manages the artifact publishing to Maven Central

## Purpose
This project serves as a reference implementation for Java developers looking to:
- Automate Maven release processes through GitHub Actions
- Publish artifacts to Maven Central following best practices
- Reduce manual steps in the software release lifecycle

## Workflow Details
### Create-Release Workflow
The `create-release` workflow is responsible for:
- Determining the next appropriate version based on semantic versioning
- Updating the Maven project version in pom.xml
- Running tests to ensure the release is stable
- Creating a GitHub Release with appropriate tags
- Generating release notes based on commits since the last release

**Key Features:**
- Configurable version increment (major, minor, patch)
- Support for pre-release and release candidates

### Publish-Tag Workflow
The `publish-tag` workflow activates when a new tag is created and handles:
- Downloading the release artifacts from GitHub Releases
- GPG signing of all artifacts (JAR, sources, javadoc)
- Publishing to Maven Central via Sonatype OSSRH
- Verification of the published artifacts
- Updating project documentation with new version information

**Key Features:**
- Secure credential handling using GitHub Secrets
- Automated GPG signing
- Proper Maven Central metadata validation
- Release staging and promotion

## Setup Instructions
### Prerequisites
- GitHub account with permissions to create workflows
- Sonatype OSSRH account for Maven Central publishing
- GPG key pair for artifact signing

### Required GitHub Secrets
Configure these secrets in your repository:
- `OSS_SONATYPE_PASSWORD`: Your Sonatype OSSRH username
- `OSS_SONATYPE_USERNAME`: Your Sonatype OSSRH password
- `GPG_PRIVATE_KEY`: Your GPG private key (exported as ASCII armor)
- `MAVEN_GPG_PASSPHRASE`: The passphrase for your GPG key
- `GITHUB_TOKEN`: Automatically provided by GitHub

_Optional:_
- `NODE_AUTH_TOKEN`: The auth token to publish to NPMjs

### Maven Configuration
Your `pom.xml` must include:
1. Required Maven Central metadata:
    - Organization details
    - Developer information
    - SCM connection details
    - License information

2. Deploy and signing plugins:
    - `maven-deploy-plugin`
    - `maven-gpg-plugin`
    - `nexus-staging-maven-plugin`

## Usage Guide
### Creating a Release
1. **Automated Release:**
    - The workflow can be triggered on schedule or manually
    - Version is automatically determined based on commit messages

2. **Manual Release:**
    - Go to the Actions tab in your repository
    - Select the "Create Release" workflow
    - Click "Run workflow"
    - Select the version increment type (patch/minor/major)
    - Optional: Add release notes or override version

### Workflow Execution
When executed:
1. The `create-release` workflow prepares the version and creates the GitHub Release
2. The new tag triggers the `publish-tag` workflow
3. Artifacts are signed and published to Maven Central
4. Staging repository is automatically closed and released

## Best Practices
- Use [Conventional Commits](https://www.conventionalcommits.org/) to automate version determination
- Keep your GPG keys secure and consider key rotation policies
- Test your release process in a fork before implementing in production repositories
- Review the generated artifacts before final publication

## Troubleshooting
Common issues:
- GPG signing errors: Verify your GPG key and passphrase
- OSSRH authentication failures: Check your Sonatype credentials
- Release validation errors: Ensure pom.xml contains all required metadata


