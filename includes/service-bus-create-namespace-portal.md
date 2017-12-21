Als u Service Bus-berichtenentiteiten wilt gebruiken in Azure, moet u eerst een naamruimte maken met een naam die uniek is binnen Azure. Een naamruimte biedt een scoping container voor het verwerken van Service Bus-resources in uw toepassing.

Ga als volgt te werk om een naamruimte te maken:

1. Meld u aan bij [Azure Portal][Azure portal].
2. Klik in het linkernavigatievenster van de portal op **+ maken van een resource**, klikt u vervolgens op **Enterprise Integration**, en klik vervolgens op **Service Bus**.
3. Voer in het dialoogvenster **Naamruimte maken** een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.
4. Nadat u hebt gecontroleerd of de naam van de naamruimte beschikbaar is, kiest u de prijscategorie (Basic, Standard of Premium).
5. Kies in het veld **Abonnement** een Azure-abonnement waarin u de naamruimte maakt.
6. Kies in het veld **Resourcegroep** een bestaande resourcegroep waarin de naamruimte zal zijn opgenomen of maak een nieuwe resourcegroep.      
7. Kies in **Locatie** het land of regio waarin uw naamruimte moet worden gehost.
   
    ![Een naamruimte maken][create-namespace]
8. Klik op **Create**. Uw naamruimte wordt nu gemaakt en ingeschakeld. U moet wellicht enkele minuten wachten terwijl de resources voor uw account worden ingericht.

### <a name="obtain-the-management-credentials"></a>De beheerreferenties ophalen
Het automatisch maken van een nieuwe naamruimte, genereert een eerste regel van de Shared Access Signature (SAS) met een combinatie van primaire en secundaire sleutels dat elk volledige controle over alle aspecten van de naamruimte verlenen van een gekoppeld. Zie [Service Bus-verificatie en autorisatie](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md) beperkte voor informatie over het maken van verdere regels met meer rechten voor reguliere afzenders en ontvangers. Volg deze stappen om de eerste regel te kopiëren: 

1.  Klik op **alle resources**, klik vervolgens op de zojuist gemaakte naamruimtenaam.
2. Klik in het venster naamruimte **gedeeld toegangsbeleid**.
3. In de **gedeeld toegangsbeleid** scherm, klikt u op **RootManageSharedAccessKey**.
   
    ![verbinding-gegevens][connection-info]
4. In de **beleid: RootManageSharedAccessKey** venster, klikt u op de kopie bladeren... naast **Connection string – primaire sleutel**, de verbindingsreeks kopiëren naar uw Klembord voor later gebruik. Plak deze waarde in Kladblok of een andere tijdelijke locatie.
   
    ![connection-string][connection-string]

5. Herhaal de vorige stap: het kopiëren en plakken van de waarde voor de **Primaire sleutel** voor een tijdelijke locatie zodat u deze later kunt gebruiken.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
