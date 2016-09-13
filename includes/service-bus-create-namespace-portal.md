1. Meld u aan bij [Azure Portal][].

2. Klik in het linker navigatiedeelvenster van de portal achtereenvolgens op **Nieuw**, **Enterprise Integration** en **Service Bus**.

4. Voer in het dialoogvenster **Naamruimte maken** een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.

5. Nadat u hebt gecontroleerd of de naam van de naamruimte beschikbaar is, kiest u de prijscategorie (Basic, Standard of Premium).

7. Kies in het veld **Abonnement** een Azure-abonnement waarin u de naamruimte maakt.

9. Kies in het veld **Resourcegroep** een bestaande resourcegroep waarin de naamruimte zal zijn opgenomen of maak een nieuwe resourcegroep.      

8. Kies in **Locatie** het land of regio waarin uw naamruimte moet worden gehost.

    ![Een naamruimte maken][create-namespace]

6. Klik op de knop **Maken**. Uw naamruimte wordt nu gemaakt en ingeschakeld. U moet wellicht enkele minuten wachten terwijl de resources voor uw account worden ingericht.
 
### De beheerreferenties ophalen

1. Klik in de lijst met naamruimten op de zojuist gemaakte naam voor de naamruimte.
 
3. Klik in de blade **Service Bus-naamruimte** op **Beleid voor gedeelde toegang**.

4. Klik in de blade **Beleid voor gedeelde toegang** op **RootManageSharedAccessKey**.

    ![verbinding-gegevens][connection-info]

5. Klik in het blade **Beleid: RootManageSharedAccessKey** op de knop Kopiëren naast **Verbindingsreeks–primaire sleutel** om de verbindingsreeks naar het klembord te kopiëren voor later gebruik.

    ![verbinding-reeks][connection-string]

<!--Image references-->

[maken-naamruimte]: ./media/service-bus-create-namespace-portal/create-namespace.png
[verbinding-gegevens]: ./media/service-bus-create-namespace-portal/connection-info.png
[verbinding-reeks]: ./media/service-bus-create-namespace-portal/connection-string.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[Azure Portal]: https://portal.azure.com


<!--HONumber=sep16_HO1-->


