# Пример генерации SBOM для языка Java со сборщиком Gradle

SBOM генерируется на основе зависимостей, которые используются в проекте. Генерируется SBOM с помощью плагина [cyclonedx-gradle-plugin](https://github.com/CycloneDX/cyclonedx-gradle-plugin), который создан сообществом CycloneDX.

## Использование

1. Требуется добавить плагин в файл build.gradle:

```json
plugins {
    id("org.cyclonedx.bom") version "1.10.0"
}

tasks.cyclonedxBom {
    setIncludeConfigs(listOf("runtimeClasspath"))
    setSkipConfigs(listOf("compileClasspath", "testCompileClasspath"))
    setProjectType("application")
    setSchemaVersion("1.6")
    setDestination(project.file("build/reports"))
    setOutputName("bom")
    setOutputFormat("json")
    setIncludeBomSerialNumber(true)
    setIncludeLicenseText(false)
    setComponentVersion("2.0.0")
}
```

2. Сгенерировать SBOM можно в рамках шага пайплайна. Самый простой вариант генерации выглядит следующим образом:

```yaml
# Необходимые переменные:
variables:
  SBOM_PATH: "build/reports/bom.json"

stages:
  - Audit (src)

Forming CycloneDX SBOM:
  stage: Audit (src)
  image:
    name: gradle:8.7.0-jdk17
  script:
    - gradle cyclonedxBom
  artifacts:
    paths:
      - build/reports/bom.json
    expire_in: 1 day
```

## Дополнительные параметры плагина cyclonedx-gradle-plugin

С дополнительными параметрами плагина можно ознакомиться в репозитории [cyclonedx-gradle-plugin](https://github.com/CycloneDX/cyclonedx-gradle-plugin).
