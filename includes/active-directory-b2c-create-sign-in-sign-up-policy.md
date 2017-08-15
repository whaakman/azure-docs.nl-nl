U moet een aanmeldingsbeleid maken om aanmelden in te schakelen op uw toepassing. In dit beleid wordt de ervaring van consumenten bij het aanmelden beschreven en de inhoud van tokens die de toepassing ontvangt nadat aanmeldingen zijn voltooid.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Selecteer bij instellingen in de sectie Beleid de optie **Registratie- of aanmeldingsbeleid** en klik op **+Toevoegen**.

![Beleid voor registreren of aanmelden selecteren en op de knop Toevoegen klikken](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-policy.png)

Geef een**** naam op voor het beleid waarnaar de toepassing kan verwijzen. Geef bijvoorbeeld `SiUpIn` op.

Selecteer **Id-providers** en schakel **Registreren met e-mailadres** in. U kunt er ook voor kiezen om sociale id-providers te selecteren als dit al is geconfigureerd. Klik op **OK**.

![Selecteer Registreren met e-mailadres als id-provider en klik op de knop OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-identity-providers.png)

Selecteer **Registratiekenmerken**. Kies de kenmerken van de consument die u wilt verzamelen tijdens de registratie. Schakel bijvoorbeeld **Land/regio**, **Weergavenaam** en **Postcode** in. Klik op **OK**.

![Selecteer enkele kenmerken en klik op de knop OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-attributes.png)

Selecteer **Toepassingsclaims**. Kies de claims die u wilt laten retourneren in de autorisatietokens die, na een geslaagde aanmelding, terug worden gestuurd naar de toepassing . Selecteer bijvoorbeeld **Weergavenaam**, **Id-provider**, **Postcode**, **Gebruiker is nieuw** en **Object-id van gebruiker**.

![Selecteer enkele toepassingsclaims en klik op de knop OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-application-claims.png)

Klik op **Maken** om het beleid toe te voegen. Het beleid wordt vermeld als **B2C_1_SiUpln**. Het voorvoegsel **B2C_1_** wordt toegevoegd aan de naam.

Open het beleid door **B2C_1_SiUpln** te selecteren. Controleer de instellingen die zijn opgegeven in de tabel en klik vervolgens op **Nu uitvoeren**.

![Selecteer het beleid en voer dit uit](media/active-directory-b2c-create-sign-in-sign-up-policy/run-b2c-signup-signin-policy.png)

| Instelling      | Waarde  |
| ------------ | ------ |
| **Toepassingen** | Contoso B2C-app |
| **Antwoord-URL selecteren** | `https://localhost:44316/` |

Er wordt een nieuw browsertabblad geopend. Hier kunt u controleren of het registreren of aanmelden voor consumenten werkt zoals geconfigureerd.

> [!NOTE]
> Het duurt maximaal één minuut voordat het gemaakte beleid en de updates van kracht worden.
>