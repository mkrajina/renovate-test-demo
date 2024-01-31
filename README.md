# renovate-test-repo

## Run the application
```
./gradlew bootRun
```

## renovate config
### Renovate is configured as a simple git repository which contains only one file .gitlab-ci.yml with following content
```
stages:
  - renovate
renovate:
  image: renovate/renovate:slim
  stage: renovate
  variables:
    RENOVATE_PLATFORM: gitlab
    RENOVATE_ENDPOINT: $CI_API_V4_URL
    RENOVATE_BINARY_SOURCE: install
    RENOVATE_BRANCH_NAME: "PHX-0000-{{{branchPrefix}}}{{{additionalBranchPrefix}}}{{{branchTopic}}}"
    RENOVATE_EXCLUDE_COMMIT_PATHS: "['gitlab-ci/**', 'Dockerfile', 'gradle/wrapper/**', 'gradlew', 'gradlew.bat']"
    RENOVATE_MINIMUM_RELEASE_AGE: "7 days"
    RENOVATE_CONFIG: '{"extends": [":disableMajorUpdates", "group:all"],"separateMajorMinor": true}'
  tags:
    - docker
  only:
    - schedules
  script:
    - renovate $RENOVATE_EXTRA_FLAGS
```

## Current behavior
- Renovate just updates the dependency **com.h2database:h2** from **2.2.220** to **2.2.224**
- All defined plugin versions are untouched

## Expected behavior
- All plugin versions will be updated to the latest state
  - **org.springframework.boot** from version **3.1.5** to **3.2.2**
  - **io.spring.dependency-management** from version **1.1.3** to **1.1.4**
  - **kotlin("jvm")** from version **1.9.10** to **1.9.22**
  - **kotlin("plugin.spring")** from version **1.9.10** to **1.9.22**