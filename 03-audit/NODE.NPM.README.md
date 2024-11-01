# Пример генерации SBOM для Node с менеджером пакетов npm

SBOM генерируется на основе зависимостей, которые используются в проекте. Генерируется SBOM с помощью плагина [cyclonedx-node-npm](https://github.com/CycloneDX/cyclonedx-node-npm), который создан сообществом CycloneDX.

## Использование

Сгенерировать SBOM можно в рамках шага пайплайна.

### V1: npm

В качестве глобального инструмента

```yaml
stages:
  - Audit (src)

Forming CycloneDX SBOM (npm) v1:
  variables:
    PACKAGE_LOCK_ONLY: "true"
    SPEC_VERSION: "1.4"                       # (choices: "1.2", "1.3", "1.4", "1.5", "1.6")
    OUTPUT_FILE: "reports/cdx-sbom.npm.json"
    MC_TYPE: "application"                    # (choices: "application", "firmware", "library")
  image:
    name: "node:18-alpine"
  before_script:
    - npm install --global @cyclonedx/cyclonedx-npm
    - cyclonedx-npm --help
    - mkdir reports/ || true
  script:
    - cyclonedx-npm
      --package-lock-only $PACKAGE_LOCK_ONLY
      --spec-version $SPEC_VERSION
      --output-file $OUTPUT_FILE
      --mc-type $MC_TYPE
```

### V2: npx

В качестве глобального инструмента

```yaml
stages:
  - Audit (src)

Forming CycloneDX SBOM (npm) v2:
  variables:
    PACKAGE_LOCK_ONLY: "true"
    SPEC_VERSION: "1.4"                       # (choices: "1.2", "1.3", "1.4", "1.5", "1.6")
    OUTPUT_FILE: "reports/cdx-sbom.npm.json"
    MC_TYPE: "application"                    # (choices: "application", "firmware", "library")
  image:
    name: "node:18-alpine"
  before_script:
    - npx --package @cyclonedx/cyclonedx-npm --call exit
    - npx @cyclonedx/cyclonedx-npm --help
    - mkdir reports/ || true
  script:
    - npx @cyclonedx/cyclonedx-npm
      --package-lock-only $PACKAGE_LOCK_ONLY
      --spec-version $SPEC_VERSION
      --output-file $OUTPUT_FILE
      --mc-type $MC_TYPE
```

### V3: npm@latest

В качестве глобального инструмента через обновление npm до последнией версии

```yaml
stages:
  - Audit (src)

Forming CycloneDX SBOM (npm) v3:
  variables:
    PACKAGE_LOCK_ONLY: "true"
    SBOM_FROMAT: "cyclonedx"
    SBOM_TYPE: "application"                   # (choices: "application", "firmware", "library")
    SPEC_VERSION: "1.4"                        # (choices: "1.2", "1.3", "1.4", "1.5", "1.6")
    OUTPUT_FILE: "reports/cdx-sbom.npm.json"
  image:
    name: "node:18-alpine"
  before_script:
    - npm install -g npm@latest
    - npm sbom --help
    - mkdir reports/ || true
  script:
    - npm sbom \
      --package-lock-only $PACKAGE_LOCK_ONLY \
      --sbom-format $SBOM_FROMAT \
      --sbom-type $SBOM_TYPE > $OUTPUT_FILE
```

## Дополнительные параметры плагина cyclonedx-node-npm

С дополнительными параметрами плагина можно ознакомиться в репозитории [cyclonedx-node-npm](https://github.com/CycloneDX/cyclonedx-node-npm).
