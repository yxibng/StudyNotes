

Select the Build Phases tab, click the + button and add a New Run Script Phase — name it `"Integrate Reveal Server"`. Paste in the following shell script:

```
REVEAL_APP_PATH=$(mdfind kMDItemCFBundleIdentifier="com.ittybittyapps.Reveal2" | head -n 1)
BUILD_SCRIPT_PATH="${REVEAL_APP_PATH}/Contents/SharedSupport/Scripts/reveal_server_build_phase.sh"
if [ "${REVEAL_APP_PATH}" -a -e "${BUILD_SCRIPT_PATH}" ]; then
    "${BUILD_SCRIPT_PATH}"
else
    echo "Reveal Server not loaded: Cannot find a compatible Reveal app."
fi
```