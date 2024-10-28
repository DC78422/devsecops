# Пример генерации SBOM для языка Go

SBOM генерируется на основе зависимостей, которые записаны в файле go.mod (golang modules). Генерируется SBOM с помощью утилиты [cyclonedx-gomod](https://github.com/CycloneDX/cyclonedx-gomod/), которую создало сообщество CycloneDX.

## Использование

Сгенерировать SBOM можно в рамках шага пайплайна. Самый простой вариант генерации выглядит следующим образом:

```yaml
Forming CycloneDX SBOM:
  image: "$REGISTRY/openshift/golang:1.20"
  stage: cdx-sbom
  script:
    - mkdir reports/ || true
    - go install github.com/CycloneDX/cyclonedx-gomod/cmd/cyclonedx-gomod@latest
    - /go/bin/cyclonedx-gomod mod -json -output reports/cdx-sbom.json -assert-licenses -licenses .
  artifacts:
    paths:
      - reports/
    expire_in: 1 day
```

Для встраивания этого шага в пайплайн нужно объявить переменную `DEPENDENCYTRACK_SBOM_PATH`, её значением должен быть путь до сгенерированного SBOM'а.

```yaml
variables:
  REGISTRY: "registry.example.com"
  DEPENDENCYTRACK_SBOM_PATH: "reports/cdx-sbom.json"
```
