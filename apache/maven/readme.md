

* ${user.home}/.m2/settings.xml
    ```
    <settings>
        ...
        <mirrors>
            <mirror>
                <id>other-mirror</id>
                <name>Other Mirror Repository</name>
                <url>https://other-mirror.repo.other-company.com/maven2</url>
                <mirrorOf>central</mirrorOf>
            </mirror>
        </mirrors>
        ...
    </settings>
    ```