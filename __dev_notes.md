# RTNCalculator spike from React Native docs
## 10/25/2022

Running through the example module from here:
https://reactnative.dev/docs/the-new-architecture/pillars-turbomodules

### Action Log

#### 10/26/2022

Made a branch of nereus with no changes other than enabling new architecture.

Figured out some things needed to get it to build:

* Upgrade React Native to 0.70.4
* Downgrade gradle due to regression in latest
* Add some JVM args to get more memory (was having overflow errors)
* Found out how to exclude a lib from the new build process and updated react native config to do so
  * https://github.com/react-native-community/cli/blob/0e63e750a235062cd9bc43ed6a4a2beb8f14385a/docs/autolinking.md#how-can-i-disable-autolinking-for-new-architecture-fabric-turbomodules
  * https://github.com/react-native-datetimepicker/datetimepicker/issues/622


#### 10/25/2022

Set up a repo here:
git@github.com:agent-0028/spike-rtn-calculator.git

I set my global node to 16.12.0, and local in this folder

The docs say to use yarn, but I modified to use `npm`, since that is how our project is set up.

Got an error about not finding react-native-config, and we definitely have one.

Commented out lines: 219 - 253 in this file:
`node_modules/react-native/scripts/codegen/generate-artifacts-executor.js`
Left in the message in `handleLibrariesFromReactNativeConfig` so I can see it is being skipped...

This is the commend to do a "test build" to see what codegen does for ios:

```
cd nereus-app
npm install agent-0028/spike-rtn-calculator#add-boilerplate-from-docs --save
cd ..
node nereus-app/node_modules/react-native/scripts/generate-artifacts.js \
  --path ./nereus-app/ \
  --outputPath ../spike-rtn-calculator/generated/
```

This is the commend to do a "test build" to see what codegen does for android:
```

```

Moved generated Android code from node_modules to same folder as ios (to make easier to review)

```
mv node_modules/rtn-calculator/android/build/generated/source/codegen ../../agent-0028/spike-rtn-calculator/generated
```

Added this to nereus-app to use in `npm run update` command

```
RCT_NEW_ARCH_ENABLED=1 bundle exec pod install
```

I think maybe docs were wrong and in package.json should be:
```
"android": {
      "javaPackageName": "com.RTNCalculator"
    }
```

Getting a weird Java compile error(s).

Trying to switch to recommended zulu distribution of Java 11, but not using homebrew.

Errors with arm arch builds and apparently non-64 bit x86, removed these from build.gradle in `nereus-app`
`x86,armeabi-v7a,arm64-v8a`

Using SDKman!
```
sdk install java 11.0.16.1-zulu
sdk default java 11.0.16.1-zulu
```

Getting Java out of memory errors, tried adding flags to `gradle.properties`

Getting a compile error with react-native-community/datetimepicker, upgrading to 6.5.2


git rebase --onto 7573e4db7a7463c04db9386d1e9be26fad6c0e47 8afd37730a5dfb215fd3b44deae397dccdb1b313