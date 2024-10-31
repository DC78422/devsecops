# Пример генерации SBOM для языка Python

SBOM генерируется на основе зависимостей, которые используются в проекте. Генерируется SBOM с помощью плагина [cyclonedx-python](https://github.com/CycloneDX/cyclonedx-python), который создан сообществом CycloneDX.

## Использование

1. Сгенерировать SBOM можно в рамках шага пайплайна. Самый простой вариант генерации выглядит следующим образом:

```yaml
stages:
  - Audit (src)

variables:
#   PYTHON_DEPENDENCY_MANAGMENT: "environment"   # Build an SBOM from Python (virtual) environment
#   PYTHON_DEPENDENCY_MANAGMENT: "requirements"  # Build an SBOM from Pip requirements
  PYTHON_DEPENDENCY_MANAGMENT: "pipenv"        # Build an SBOM from Pipenv manifest
#   PYTHON_DEPENDENCY_MANAGMENT: "poetry"        # Build an SBOM from Poetry project

Forming CycloneDX SBOM:
  image:
    name: "python:3.12.6-alpine3.20"
  before_script:
    - python -m pip install cyclonedx-bom
    - mkdir reports/ || true
  script:
    - cyclonedx-py $PYTHON_DEPENDENCY_MANAGMENT
      --output-format json
      --outfile reports/cdx-sbom.py.json
```

## Дополнительные параметры плагина cyclonedx-gradle-plugin

С дополнительными параметрами плагина можно ознакомиться в репозитории [cyclonedx-python](https://github.com/CycloneDX/cyclonedx-python) и [cyclonedx-bom-tool.readthedocs.io](https://cyclonedx-bom-tool.readthedocs.io/en/latest/.)
