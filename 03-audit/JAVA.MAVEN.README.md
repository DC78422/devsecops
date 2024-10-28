# Пример генерации SBOM для языка Java со сборщиком Maven

SBOM генерируется на основе зависимостей, которые используются в проекте. Генерируется SBOM с помощью плагина [cyclonedx-maven-plugin](https://github.com/CycloneDX/cyclonedx-maven-plugin), который создан сообществом CycloneDX.

## Использование

1. Требуется добавить плагин в раздел с плагинами

```xml
<plugins>
    <plugin>
        <groupId>org.cyclonedx</groupId>
        <artifactId>cyclonedx-maven-plugin</artifactId>
        <version>2.8.0</version>
        <executions>
            <execution>
                <phase>package</phase>
                <goals>
                    <goal>makeAggregateBom</goal>
                </goals>
            </execution>
        </executions>
    </plugin>
</plugins>
```

2. Сгенерировать SBOM можно в рамках шага пайплайна. Самый простой вариант генерации выглядит следующим образом:

```yaml
variables:
  SBOM_PATH: "target/bom.json"
Forming CycloneDX SBOM:
  image:
    name: "maven:3.9.6-eclipse-temurin-17"
  before_script:
    - cp -r settings.xml /usr/share/maven/conf/
  script:
    - mvn install cyclonedx:makeAggregateBom
  artifacts:
    paths:
      - target/bom.json
    expire_in: 1 day
```

## Дополнительные параметры плагина cyclonedx-maven-plugin

С дополнительными параметрами плагина можно ознакомиться в репозитории [cyclonedx-maven-plugin](https://github.com/CycloneDX/cyclonedx-maven-plugin).
