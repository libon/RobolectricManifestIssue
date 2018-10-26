This is a sample app to demonstrate an issue with manifest references in Robolectric 4.0
https://github.com/robolectric/robolectric/issues/4026

Preconditions:
* Have this in the manifest, on the `<application>` tag:

```xml
android:networkSecurityConfig="@xml/network_security_config"
```

* In `gradle.properties`, enable binary resources:
```properties
android.enableUnitTestBinaryResources=true
```

* In `app/build.gradle`, add this in the `android` section:
```gradle
    testOptions {
        unitTests {
            includeAndroidResources = true
        }
    }
```


Run unit tests:
```
./gradlew clean testDebugUnitTest

> Configure project :app
WARNING: The option setting 'android.enableUnitTestBinaryResources=true' is experimental and unsupported.
The current default is 'false'


> Task :app:testDebugUnitTest

example.com.robolectricmigration.ExampleUnitTest > exampleTest FAILED
    java.lang.RuntimeException
        Caused by: android.content.pm.PackageParser$PackageParserException
            Caused by: java.lang.NullPointerException

1 test completed, 1 failed

```

The test report shows this exception:
```
java.lang.RuntimeException: android.content.pm.PackageParser$PackageParserException: Failed to read manifest from /Users/calvarez/dev/projects/RobolectricManifestIssue/app/build/intermediates/apk_for_local_test/debugUnitTest/packageDebugUnitTestForUnitTest/apk-for-local-test.ap_
	at org.robolectric.shadows.ShadowPackageParser.callParsePackage(ShadowPackageParser.java:57)
	at org.robolectric.android.internal.ParallelUniverse.setUpApplicationState(ParallelUniverse.java:149)
	at org.robolectric.RobolectricTestRunner.beforeTest(RobolectricTestRunner.java:377)
	at org.robolectric.internal.SandboxTestRunner$2.evaluate(SandboxTestRunner.java:252)
	at org.robolectric.internal.SandboxTestRunner.runChild(SandboxTestRunner.java:130)
	at org.robolectric.internal.SandboxTestRunner.runChild(SandboxTestRunner.java:42)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.robolectric.internal.SandboxTestRunner$1.evaluate(SandboxTestRunner.java:84)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.gradle.api.internal.tasks.testing.junit.JUnitTestClassExecutor.runTestClass(JUnitTestClassExecutor.java:116)
	at org.gradle.api.internal.tasks.testing.junit.JUnitTestClassExecutor.execute(JUnitTestClassExecutor.java:59)
	at org.gradle.api.internal.tasks.testing.junit.JUnitTestClassExecutor.execute(JUnitTestClassExecutor.java:39)
	at org.gradle.api.internal.tasks.testing.junit.AbstractJUnitTestClassProcessor.processTestClass(AbstractJUnitTestClassProcessor.java:66)
	at org.gradle.api.internal.tasks.testing.SuiteTestClassProcessor.processTestClass(SuiteTestClassProcessor.java:51)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:35)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:24)
	at org.gradle.internal.dispatch.ContextClassLoaderDispatch.dispatch(ContextClassLoaderDispatch.java:32)
	at org.gradle.internal.dispatch.ProxyDispatchAdapter$DispatchingInvocationHandler.invoke(ProxyDispatchAdapter.java:93)
	at com.sun.proxy.$Proxy1.processTestClass(Unknown Source)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker.processTestClass(TestWorker.java:109)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:35)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:24)
	at org.gradle.internal.remote.internal.hub.MessageHubBackedObjectConnection$DispatchWrapper.dispatch(MessageHubBackedObjectConnection.java:146)
	at org.gradle.internal.remote.internal.hub.MessageHubBackedObjectConnection$DispatchWrapper.dispatch(MessageHubBackedObjectConnection.java:128)
	at org.gradle.internal.remote.internal.hub.MessageHub$Handler.run(MessageHub.java:404)
	at org.gradle.internal.concurrent.ExecutorPolicy$CatchAndRecordFailures.onExecute(ExecutorPolicy.java:63)
	at org.gradle.internal.concurrent.ManagedExecutorImpl$1.run(ManagedExecutorImpl.java:46)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at org.gradle.internal.concurrent.ThreadFactoryImpl$ManagedThreadRunnable.run(ThreadFactoryImpl.java:55)
	at java.lang.Thread.run(Thread.java:748)
Caused by: android.content.pm.PackageParser$PackageParserException: Failed to read manifest from /Users/calvarez/dev/projects/RobolectricManifestIssue/app/build/intermediates/apk_for_local_test/debugUnitTest/packageDebugUnitTestForUnitTest/apk-for-local-test.ap_
	at android.content.pm.PackageParser.parseBaseApk(PackageParser.java:1353)
	at android.content.pm.PackageParser.parseMonolithicPackage(PackageParser.java:1299)
	at android.content.pm.PackageParser.parsePackage(PackageParser.java:1022)
	at android.content.pm.PackageParser.parsePackage(PackageParser.java:1042)
	at org.robolectric.shadows.ShadowPackageParser.callParsePackage(ShadowPackageParser.java:31)
	at org.robolectric.android.internal.ParallelUniverse.setUpApplicationState(ParallelUniverse.java:149)
	at org.robolectric.RobolectricTestRunner.beforeTest(RobolectricTestRunner.java:377)
	at org.robolectric.internal.SandboxTestRunner$2.evaluate(SandboxTestRunner.java:252)
	at org.robolectric.internal.SandboxTestRunner.runChild(SandboxTestRunner.java:130)
	at org.robolectric.internal.SandboxTestRunner.runChild(SandboxTestRunner.java:42)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.robolectric.internal.SandboxTestRunner$1.evaluate(SandboxTestRunner.java:84)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.gradle.api.internal.tasks.testing.junit.JUnitTestClassExecutor.runTestClass(JUnitTestClassExecutor.java:116)
	at org.gradle.api.internal.tasks.testing.junit.JUnitTestClassExecutor.execute(JUnitTestClassExecutor.java:59)
	at org.gradle.api.internal.tasks.testing.junit.JUnitTestClassExecutor.execute(JUnitTestClassExecutor.java:39)
	at org.gradle.api.internal.tasks.testing.junit.AbstractJUnitTestClassProcessor.processTestClass(AbstractJUnitTestClassProcessor.java:66)
	at org.gradle.api.internal.tasks.testing.SuiteTestClassProcessor.processTestClass(SuiteTestClassProcessor.java:51)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:35)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:24)
	at org.gradle.internal.dispatch.ContextClassLoaderDispatch.dispatch(ContextClassLoaderDispatch.java:32)
	at org.gradle.internal.dispatch.ProxyDispatchAdapter$DispatchingInvocationHandler.invoke(ProxyDispatchAdapter.java:93)
	at com.sun.proxy.$Proxy1.processTestClass(Unknown Source)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker.processTestClass(TestWorker.java:109)
	... 11 more
Caused by: java.lang.NullPointerException
	at org.robolectric.res.android.CppAssetManager2.FindEntry(CppAssetManager2.java:686)
	at org.robolectric.res.android.CppAssetManager2.GetResource(CppAssetManager2.java:849)
	at org.robolectric.res.android.CppAssetManager2.ResolveReference(CppAssetManager2.java:907)
	at org.robolectric.res.android.AttributeResolution9.RetrieveAttributes(AttributeResolution9.java:484)
	at org.robolectric.shadows.ShadowArscAssetManager9.nativeRetrieveAttributes(ShadowArscAssetManager9.java:1454)
	at android.content.res.AssetManager.nativeRetrieveAttributes(AssetManager.java)
	at android.content.res.AssetManager.retrieveAttributes(AssetManager.java:1012)
	at android.content.res.Resources.obtainAttributes(Resources.java:1814)
	at android.content.res.Resources$GeneratedProxy/329790291.obtainAttributes(Unknown Source)
	at org.robolectric.shadows.ShadowResources.obtainAttributes(ShadowResources.java:90)
	at android.content.res.Resources.obtainAttributes(Resources.java)
	at android.content.pm.PackageParser.parseBaseApplication(PackageParser.java:3322)
	at android.content.pm.PackageParser.parseBaseApkCommon(PackageParser.java:2053)
	at android.content.pm.PackageParser.parseBaseApk(PackageParser.java:1942)
	at android.content.pm.PackageParser.parseBaseApk(PackageParser.java:1337)
	... 38 more
```


If any of the above three preconditions are removed, the test passes.
