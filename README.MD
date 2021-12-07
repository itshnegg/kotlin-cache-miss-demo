# KotlinCompile Cache Miss Demo

variant filter affects an input (commonSourceSet) of KotlinCompile task
```
// root build.gradle

subprojects { project ->
    project.plugins.withId('com.android.application') {
        boolean ide = System.getProperty('idea.platform.prefix') != null
        if (ide) {
            android {
                variantFilter { variant ->
                    variant.ignore = variant.buildType.name != 'debug'
                }
            }
        }
    }
}
```

## Steps to reproduce

1) Build the project using cmd

```bash
./gradlew app:assembleDebug // 28 actionable tasks: 28 executed
```
2) Clean
```
./gradlew clean
```
3) Build the project in Android Studio

### Expected:
```
28 actionable tasks: 3 executed, 25 from cache
```
### Actual:
```
28 actionable tasks: 4 executed, 24 from cache
```