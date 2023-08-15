
version_code = sh(script: "grep 'versionCode' app/build.gradle | sed -n 's/.*versionCode\\s*\\(\\d*\\).*/\\1/p'", 
                 log: false).strip
