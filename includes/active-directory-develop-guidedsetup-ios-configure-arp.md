
## <a name="add-the-applications-registration-information-to-your-app"></a>De registratiegegevens van de toepassing toevoegen aan uw app

In deze stap moet u de toepassings-Id aan uw project toevoegen:

1.  In `ViewController.swift`, vervangt u de regel die begint met '`let kClientID`' met:
```swift
let kClientID = "[Enter the application Id here]"
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Control + klik <code>Info.plist</code> online zetten van het contextafhankelijke menu en klik vervolgens op: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
Onder de <code>dict</code> de hoofd-knooppunt, voeg de volgende:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Enter the application Id here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
