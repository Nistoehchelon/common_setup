
Installs Android SDK.
This is a requirement to compile the Rust code in [combined_game_client](https://github.com/Nistoehchelon/combined_game_client.git) into a .so library for the Android app, but also, in general, to compile, say, an executable for aarch64-linux-android target if one felt so inclined.

TODO: Since sdkmanager is being deprecated in favour of android sdk cli, replace the usage of sdkmanager with said new method.

```bash
mkdir -p ~/android-sdk/cmdline-tools
cd ~/android-sdk/cmdline-tools
# replace with actual latest, find at https://developer.android.com/studio#command-line-tools-only
wget https://dl.google.com/android/repository/commandlinetools-linux-15859902_latest.zip
unzip commandlinetools-linux-*.zip
mv cmdline-tools/ latest
rm commandlinetools-linux-*.zip

# tools like adb and fastboot
cd ~/android-sdk/
wget https://dl.google.com/android/repository/platform-tools-latest-linux.zip
unzip platform-tools-latest-linux.zip
rm platform-tools-latest-linux.zip

cat <<'EOF' >> ~/.bashrc

# Android SDK
export ANDROID_SDK_ROOT="$HOME/android-sdk"
export PATH="$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$PATH"
export PATH="$ANDROID_SDK_ROOT/platform-tools:$PATH"
EOF

source ~/.bashrc 
yes | sdkmanager --licenses

# find latest: sdkmanager --list | grep "build-tools"
sdkmanager "build-tools;37.0.0"
# find latest: sdkmanager --list | grep "platforms"
sdkmanager "platforms;android-37.0"

# find latest using sdkmanager --list | grep "ndk"
# previous used: 30.0.14904198
sdkmanager "ndk;30.0.15729638"

#append the latest version ndk
cat <<'EOF' >> ~/.bashrc 
# replace with actual latest ndk installed
export ANDROID_NDK_ROOT="$ANDROID_SDK_ROOT/ndk/30.0.15729638"
EOF

sudo apt install  zip -y
# Not sure if this is needed
curl -s "https://get.sdkman.io" | bash 
source "$HOME/.sdkman/bin/sdkman-init.sh" 
sdk install gradle 



source ~/.bashrc
cargo install cargo-ndk
echo "Installed Android sdk and ndk and added to path, and also installed sdkman as well as cargo-ndk" # yay!

```
