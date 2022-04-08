# Automatically generated on 2020-08-09 UTC from https://codemagic.io/app/5f2ff8d28ff73da07aa9f03b/settings
# Note that this configuration is not an exact match to UI settings. Review and adjust as necessary.

workflows:
  default-workflow:
    name: Max2D Android
    max_build_duration: 60
    environment:
      vars:
        FCI_CLONE_UNSHALLOW: 'true'
      flutter: 1.22.6
      xcode: latest
      cocoapods: default
    scripts:
      - |
        # set up debug keystore
        rm -f ~/.android/debug.keystore
        keytool -genkeypair \
          -alias androiddebugkey \
          -keypass android \
          -keystore ~/.android/debug.keystore \
          -storepass android \
          -dname 'CN=Android Debug,O=Android,C=US' \
          -keyalg 'RSA' \
          -keysize 2048 \
          -validity 10000
      - |
        # set up local properties
        echo "flutter.sdk=$HOME/programs/flutter" > "$FCI_BUILD_DIR/android/local.properties"
      - cd . && flutter packages pub get
      - cd . && flutter build apk --release
      - cd . && flutter build appbundle --release
    artifacts:
      - build/**/outputs/**/*.apk
      - build/**/outputs/**/*.aab
      - build/**/outputs/**/mapping.txt
      - flutter_drive.log
