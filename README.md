This is a temporary repository containing a minimal reproduction of a bug
[which I am reporting to Renovate](https://github.com/renovatebot/renovate/discussions/22600).

Its `build.gradle` contains the lines:

```
def kotlinModule(String name) {
"org.jetbrains.kotlin:kotlin-$name:$kotlinVersion"
}

dependencies {
kotlinScriptImplementation(kotlinModule("stdlib"))
}
```

This should result in Renovate looking up `org.jetbrains.kotlin:kotlin-stdlib`, but it fails to handle method parameter 
substitution correctly, and attempts to look up `org.jetbrains.kotlin:kotlin-gradle-method-call-problem-demo`, which of
course fails:

```
DEBUG: Looking up org.jetbrains.kotlin:kotlin-gradle-method-call-problem-demo in repository https://repo.maven.apache.org/maven2/ (repository=michael-s-grant/gradle-method-call-problem-demo)
DEBUG: GET https://repo.maven.apache.org/maven2/org/jetbrains/kotlin/kotlin-gradle-method-call-problem-demo/maven-metadata.xml = (code=ERR_NON_2XX_3XX_RESPONSE, statusCode=404 retryCount=0, duration=368) (repository=michael-s-grant/gradle-method-call-problem-demo)
DEBUG: Content is not found for Maven url (repository=michael-s-grant/gradle-method-call-problem-demo)
"url": "https://repo.maven.apache.org/maven2/org/jetbrains/kotlin/kotlin-gradle-method-call-problem-demo/maven-metadata.xml",
"statusCode": undefined
DEBUG: Failed to look up maven package org.jetbrains.kotlin:kotlin-gradle-method-call-problem-demo (repository=michael-s-grant/gradle-method-call-problem-demo, packageFile=build.gradle, dependency=org.jetbrains.kotlin:kotlin-gradle-method-call-problem-demo)
```

The problem is easy enough to work around in the short term, so not a showstopper, but it ought still to be fixed.