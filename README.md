# Mastering CI/CD for Snowflake Native Apps

This repository contains supporting materials for the "Mastering CI/CD for Snowflake Native Apps" presentation by Frédéric L'Anglais (Snowflake) and Sébastien Coutu (Maxa).

## Overview

Building and maintaining Snowflake Native Apps at scale requires a robust CI/CD approach. This repository provides examples, references, and best practices for implementing a comprehensive build, automation, and deployment strategy for your Snowflake Native Apps by leveraging Snowflake's built-in tools and capabilities.

## Repository Structure

This repository contains examples demonstrating how to:

1. **Build**: Implement component structure, versioning strategies, and development workflows
2. **Automate**: Leverage Snowflake and third-party tools for automating builds and tests
3. **Deploy**: Create deployment workflows, manage releases, and troubleshoot effectively

## BUILD - Links

### Versioning Strategy
The presentation covers implementing semantic versioning (vX.Y.Z) at the component level and mapping this to Snowflake Native App versioning (vM.P):
- [Semantic Versioning](https://semver.org/)
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [Semantic Release](https://semantic-release.gitbook.io/semantic-release)
- [Release Please](https://github.com/googleapis/release-please)

### Development Cycle
Tools and utilities to simplify the development cycle
- [Snow CLI](https://docs.snowflake.com/en/developer-guide/snowflake-cli/installation/installation)
- [Snow CLI - Snowpark](https://docs.snowflake.com/developer-guide/snowflake-cli/snowpark/build)
- [Streamlit App Testing - Getting Started](https://docs.streamlit.io/develop/concepts/app-testing/get-started)
- [Streamlit App Testing - API Reference](https://docs.streamlit.io/develop/api-reference/app-testing)
- [Containers - Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)
- [Containers - Docker build and push action](https://github.com/docker/build-push-action)
- [Aqua Security Trivy](https://github.com/aquasecurity/trivy)
- [Aqua Security Trivy Github Action](https://github.com/aquasecurity/trivy-action)
- [Snow CLI - Native App](https://docs.snowflake.com/en/developer-guide/snowflake-cli/native-apps/create-manage-apps)
- [Snow CLI - SPCS](https://docs.snowflake.com/en/developer-guide/snowflake-cli/services/manage-images)

Other Snowflake Documentation References:
- [Snowflake Native App - Modular Setup Scripts](https://docs.snowflake.com/en/developer-guide/native-apps/creating-setup-script#create-modular-setup-scripts)

## AUTOMATE - Links

### Repository Creation
- [Snow CLI - Custom Templates](https://github.com/snowflakedb/snowflake-cli-templates)
- [Github Template Repositories](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-template-repository)
- [RenovateBot](https://github.com/renovatebot/renovate)
- [Dependabot](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide)
- [Dependabot Github Vulnerability Alerts](https://docs.github.com/en/code-security/dependabot/dependabot-alerts/about-dependabot-alerts)

### Automating CI
- [Snowflake DevOps Best Practices](https://docs.snowflake.com/en/developer-guide/builders/devops)
- [Snowflake Key Pair Authentication](https://docs.snowflake.com/en/user-guide/key-pair-auth)
- [Snow CLI Github Action](https://github.com/snowflakedb/snowflake-cli-action)
- [Github Reusable Workflows](https://docs.github.com/en/actions/sharing-automations/reusing-workflows)
- [Aqua Security Trivy Github Action](https://github.com/aquasecurity/trivy-action)

### Recommended Tools Examples

These examples leverage Snowflake's official tools:

#### Snowflake CLI Commands
The presentation references these Snow CLI commands that can be incorporated into your workflows:
```
# App Development
snow app validate
snow app bundle
snow app run
snow app deploy
snow app version
snow app release-channel
snow app release-directive

# Snowpark Container Services
snow spcs image-registry
snow spcs image-repository
snow spcs service

# Streamlit
snow streamlit deploy
snow streamlit get-url

# Snowpark
snow snowpark package create
snow snowpark package upload
snow snowpark build
snow snowpark deploy

# Cortex
snow cortex complete
```

## DEPLOY - Release Channels

### Snow CLI commands


- Registering/Deregistering a version
```sql
ALTER APPLICATION PACKAGE REGISTER VERSION <version_name> USING ‘@stage/path’;
ALTER APPLICATION PACKAGE REGISTER PATCH FOR VERSION <version_name> USING ‘@stage/path’;
ALTER APPLICATION PACKAGE DEREGISTER VERSION <version_name>;
```
- Dropping a version from channel: A version must not be used by any of the release directives of a channel
```sql
ALTER APPLICATION PACKAGE <app_pkg> MODIFY RELEASE CHANNEL <channel_name> DROP VERSION <version_name>;
```
- Adding a version to a release channel. 
```sql
ALTER APPLICATION PACKAGE <app_pkg> MODIFY RELEASE CHANNEL <channel_name> ADD VERSION <version_name>;
```
- Dropping a version from channel: A version must not be used by any of the release directives of a channel
```sql
ALTER APPLICATION PACKAGE <app_pkg> MODIFY RELEASE CHANNEL <channel_name> DROP VERSION <version_name>;
```
- Setting release directives
```sql
ALTER APPLICATION PACKAGE <app_pkg> MODIFY RELEASE CHANNEL <channel_name> SET DEFAULT RELEASE DIRECTIVE VERSION=<version_name> PATCH=<patch>;
ALTER APPLICATION PACKAGE <app_pkg> MODIFY RELEASE CHANNEL <channel_name> SET RELEASE DIRECTIVE <directive> VERSION=<version_name> PATCH=<patch> TARGET_ACCOUNTS=org1.a1,org2.a2;
```
- Channels must be targeted to a specific consumer
```sql
ALTER APPLICATION PACKAGE <app_pkg> MODIFY RELEASE CHANNEL <channel_name> ADD TARGET_ACCOUNTS=org1.a1, org1.a2;
ALTER APPLICATION PACKAGE <app_pkg> MODIFY RELEASE CHANNEL <channel_name> REMOVE TARGET_ACCOUNTS=org1.a1, org1.a2;
```
- Default Channel if not release channel is specified. 
```sql
CREATE APPLICATION <app_name> FROM LISTING <LISTING_ID>;
```
- Consumer is allowed to specify channel when creating an instance
```sql
CREATE APPLICATION <app_name> FROM LISTING <LISTING_ID> USING RELEASE CHANNEL ALPHA;
```
- Consumer can view available channels via listing
```sql
SHOW AVAILABLE RELEASE CHANNELS IN LISTING <LISTING_ID>;
```
- New Privilege for using non-default channels or unreviewed channels
```sql
CREATE PREVIEW APPLICATION;
```

## License

This repository contains example code and documentation to support the "Mastering CI/CD for Snowflake Native Apps" presentation.

Copyright © 2025 Snowflake Inc. All Rights Reserved
