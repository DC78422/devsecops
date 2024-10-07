# gitlab-devops-gitlab-ci

```sh
include:
  - project: dc20e6/devsecops/pipeline
    ref: main
    file: 
      - 'devsecops.gitlab-ci.yml'

variables:
  DEPENDENCYTRACK_URL: "https://dt-api.example.com"
  DD_URL: "https://defectdojo.example.com"

  # AppScreener
  PROJECT_NAME: project
  PROJECT_ID: id
  # APPSCREENER_TOKEN: 

  PYTHON_INSTALL_CMD: "pip install ."
  PYTHON_DEPENDENCY_MANAGMENT: environment  # Build an SBOM from Python (virtual) environment
  # PYTHON_DEPENDENCY_MANAGMENT: requirements # Build an SBOM from Pip requirements
  # PYTHON_DEPENDENCY_MANAGMENT: pipenv       # Build an SBOM from Pipenv manifest
  # PYTHON_DEPENDENCY_MANAGMENT: poetry       # Build an SBOM from Poetry project

  SAST_DISABLED: 'false'
  SAST_EXCLUDED_ANALYZERS: ''

  DAST_WEBSITE: https://ginandjuice.shop/
  DAST_WEBSITE_API: https://api.example.com/openapi.json
  DAST_WEBSITE_API_FORMAT: openapi #soap, or graphql

  IMAGE_TO_SCAN: $APP_IMAGE:$TEMP_IMAGE_TAG

  DT_CHILD_PROJECT_NAME: ${NAMESPACE}-${APP_NAME}
  DT_PARENT_PROJECT_NAME: ${NAMESPACE}
```
