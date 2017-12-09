
## <a name="create-an-application-express"></a>Maken van een toepassing (snelle)
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Registreren van uw toepassingen via de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor begeleide Setup is ingeschakeld
4.  Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>De registratiegegevens van uw toepassing toevoegen aan uw oplossing (Geavanceerd)

1.  Ga naar [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor begeleide Setup is uitgeschakeld
4.  Klik op `Add Platform`, selecteer daarna `Native Application` en klikt u op`Save`
5.  Ga terug naar Xcode. In `ViewController.swift`, vervangt u de regel die begint met '`let kClientID`' met de toepassings-ID die u zojuist hebt geregistreerd:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
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
            <string>msal[Your_Application_Id_Here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Vervang <i> <code>[Your_Application_Id_Here]</code> </i> met de toepassings-Id die u zojuist hebt geregistreerd
</li>
</ol>
