# Пример генерации SBOM для языка Ruby

SBOm генерируется на основе зависимостей, которые используются в проекте. Генерируется SBOM с помощью плагина [cyclonedx-ruby-gem](https://github.com/CycloneDX/cyclonedx-ruby-gem), который создан сообшеством CycloneDX.

## Использование

Сгенерировать SBOM можно в рамках шага в пайплайне. Самый просто вариант генерации выглядит следующим образом:

```yaml
Forming CycloneDX SBOM:
  image: "$REGISTRY/openshift/ruby:2.7.2"
  stage: cdx-sbom
  script:
    - mkdir reports/ || true
    - gem install cyclonedx-ruby
    - cyclonedx-ruby -p . -o "reports/cdx-sbom.xml"
```

Для встраивания этого шага в пайплайн нужно объявить переменную `DEPENDENCYTRACK_SBOM_PATH`, её значением должен быть путь до сгенерированного SBOM'а.

```yaml
variables:
  REGISTRY: "registry.example.com"
  DEPENDENCYTRACK_SBOM_PATH: "reports/cdx-sbom.xml"
```
