# flutter-build-apk
1.
```
 cd android/app && keytool -genkey -v -keystore upload-keystore.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias upload

```
2. create a file named `key.properties` and place it in `android` folder
```
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=upload
storeFile=upload-keystore.jks

```
3. edit `android/app/build.gradle` as
```
 def keystoreProperties = new Properties()
 def keystorePropertiesFile = rootProject.file('key.properties')
 if (keystorePropertiesFile.exists()) {
     keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
 }
```
4. replace it with the following signing configuration info:
```
   signingConfigs {
       release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
           storePassword keystoreProperties['storePassword']
       }
   }
   buildTypes {
       release {
           signingConfig signingConfigs.release
       }
   }
```
5. run `flutter build apk` or `flutter build appbundle`
