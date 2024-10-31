# Пример генерации SBOM для Node с менеджером пакетов yarn

SBOM генерируется на основе зависимостей, которые используются в проекте. Генерируется SBOM с помощью плагина [cyclonedx-node-yarn](https://github.com/CycloneDX/cyclonedx-node-yarn), который создан сообществом CycloneDX.

## Использование

Сгенерировать SBOM можно в рамках шага пайплайна.

### V1: zero-install

Установка не требуется, просто вызовите по требованию через dlx-wrapper

```yaml
stages:
  - Audit (src)

# Requirements
#   node >= 18
#   yarn >= 3 (berry)

Forming CycloneDX SBOM (yarn) v1:
  variables:
    SPEC_VERSION: "1.4"                       # (choices: "1.2", "1.3", "1.4", "1.5", "1.6")
    OUTPUT_FILE: "reports/cdx-sbom.yarn.json"
    MC_TYPE: "application"                    # (choices: "application", "firmware", "library")
  image:
    name: "repository.rt.ru/node:22.9.0-alpine3.20"
  before_script:
    - yarn dlx -q @cyclonedx/yarn-plugin-cyclonedx --help
    - mkdir reports/ || true
  script:
    - yarn dlx -q @cyclonedx/yarn-plugin-cyclonedx
      --spec-version $SPEC_VERSION
      --output-file $OUTPUT_FILE
      --mc-type $MC_TYPE
```

### V2: cli-wrapper

Как development зависимость текущего проекта

```yaml
stages:
  - Audit (src)

# Requirements
#   node >= 18
#   yarn >= 3 (berry)

Forming CycloneDX SBOM (yarn) v2:
  variables:
    SPEC_VERSION: "1.4"                       # (choices: "1.2", "1.3", "1.4", "1.5", "1.6")
    OUTPUT_FILE: "reports/cdx-sbom.yarn.json"
    MC_TYPE: "application"                    # (choices: "application", "firmware", "library")
  image:
    name: "repository.rt.ru/node:22.9.0-alpine3.20"
  before_script:
    - yarn add --dev @cyclonedx/yarn-plugin-cyclonedx
    - yarn exec cyclonedx-yarn --help
    - mkdir reports/ || true
  script:
    - yarn exec cyclonedx-yarn
      --spec-version $SPEC_VERSION
      --output-file $OUTPUT_FILE
      --mc-type $MC_TYPE
```

### V3: plugin

Установить последнюю версию из релиза GitHub в качестве плагина для текущего проекта

```yaml
stages:
  - Audit (src)

# Requirements
#   node >= 18
#   yarn >= 3 (berry)

Forming CycloneDX SBOM (yarn) v3:
  variables:
    SPEC_VERSION: "1.4"                       # (choices: "1.2", "1.3", "1.4", "1.5", "1.6")
    OUTPUT_FILE: "reports/cdx-sbom.yarn.json"
    MC_TYPE: "application"                    # (choices: "application", "firmware", "library")
  image:
    name: "repository.rt.ru/node:22.9.0-alpine3.20"
  before_script:
    - yarn plugin import https://github.com/CycloneDX/cyclonedx-node-yarn/releases/latest/download/yarn-plugin-cyclonedx.cjs
    - yarn cyclonedx --help
    - mkdir reports/ || true
  script:
    - yarn cyclonedx
      --spec-version $SPEC_VERSION
      --output-file $OUTPUT_FILE
      --mc-type $MC_TYPE
```

## Дополнительные параметры плагина cyclonedx-node-yarn

С дополнительными параметрами плагина можно ознакомиться в репозитории [cyclonedx-node-yarn](https://github.com/CycloneDX/cyclonedx-node-yarn).
