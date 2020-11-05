### Requirements


> npm install -g appcenter-cli
> npm install --save react-native-code-push

### Android Setup

> **React Native > 0.60**

More info at https://docs.microsoft.com/en-us/appcenter/distribution/codepush/rn-get-started#android-setup
___
Add followings lines to `android/settings.gradle`

> include ':app', ':react-native-code-push'
> project(':react-native-code-push').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-code-push/android/app')

___
Then, `android/app/build.gradle`

> ...
> apply from: "../../node_modules/react-native/react.gradle"
> apply from: "../../node_modules/react-native-code-push/android/codepush.gradle"
> ...

___

`MainApplication.java`

```java
...
// 1. Import the plugin class.
import com.microsoft.codepush.react.CodePush;
public class MainApplication extends Application implements ReactApplication {
    private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
        ...
        // 2. Override the getJSBundleFile method in order to let
        // the CodePush runtime determine where to get the JS
        // bundle location from on each app start
        @Override
        protected String getJSBundleFile() {
            return CodePush.getJSBundleFile();
        }
    };
}
```
___
`strings.xml`

Add your deployment key to `strings.xml` file

` <string moduleConfig="true" name="CodePushDeploymentKey">DeploymentKey</string> `

> For multi-deployment settings, see https://docs.microsoft.com/en-us/appcenter/distribution/codepush/rn-deployment#multi-deployment-testing

___
### Hot Updates

1. Modify a file in the project
2. Push it to AppCenter `appcenter codepush release-react -a <owner>/<app> -d <deployment> -t <target-version>`
3. Close the running mobile android app and re-launch it. You will see the changes have been installed.
___