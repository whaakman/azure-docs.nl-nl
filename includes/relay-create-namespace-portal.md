1. Meld u aan bij [Azure Portal][Azure Portal].
2. Klik in het linkernavigatievenster van de portal achtereenvolgens op **Nieuw**, **Bedrijfsintegratie** en **Relay**.
3. Voer in het dialoogvenster **Naamruimte maken** een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.
4. Kies in het veld **Abonnement** een Azure-abonnement waarin u de naamruimte maakt.
5. Kies in het veld **[Resourcegroep](../articles/azure-portal/resource-group-portal.md)** een bestaande resourcegroep waarin de naamruimte zal zijn opgenomen of maak een nieuwe resourcegroep.      
6. Kies in **Locatie** het land of regio waarin uw naamruimte moet worden gehost.
   
    ![Een naamruimte maken][create-namespace]
7. Klik op **Create**. Uw naamruimte wordt nu gemaakt en ingeschakeld. U moet wellicht enkele minuten wachten terwijl de resources voor uw account worden ingericht.

### <a name="obtain-the-management-credentials"></a>De beheerreferenties ophalen
1. Klik in de lijst met naamruimten op de zojuist gemaakte naam voor de naamruimte.
2. Klik in de naamruimte-blade op **Beleid voor gedeelde toegang**.
3. Klik in de blade **Beleid voor gedeelde toegang** op **RootManageSharedAccessKey**.
   
    ![verbinding-gegevens][connection-info]
4. Klik in het blade **Beleid: RootManageSharedAccessKey** op de knop Kopiëren naast **Verbindingsreeks–primaire sleutel** om de verbindingsreeks naar het klembord te kopiëren voor later gebruik. Plak deze waarde in Kladblok of een andere tijdelijke locatie.
   
    ![connection-string][connection-string]

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[verbinding-gegevens]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[Azure Portal]: https://portal.azure.com


<!--HONumber=Nov16_HO2-->


