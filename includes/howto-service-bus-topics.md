## <a name="what-are-service-bus-topics-and-subscriptions"></a>Wat zijn Service Bus-onderwerpen en -abonnementen?
Service Bus-onderwerpen en -abonnementen bieden ondersteuning voor een berichtencommunicatiemodel op basis van *publiceren/abonneren*. Wanneer u gebruikmaakt van onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet direct met elkaar. In plaats daarvan wisselen ze berichten uit via een onderwerp dat als intermediair fungeert.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

In tegenstelling tot het Service Bus-wachtrijen, waarbij elk bericht wordt verwerkt door een enkele gebruiker, bieden onderwerpen en abonnementen een 'één-op-veel'-vorm van communicatie, een patroon voor publiceren/abonneren. Het is mogelijk meerdere abonnementen voor een onderwerp te registreren. Wanneer een bericht naar een onderwerp wordt verzonden, wordt dit vervolgens beschikbaar gemaakt voor elk abonnement, waar het vervolgens zelfstandig wordt afgehandeld/verwerkt.

Een abonnement op een onderwerp lijkt op een virtuele wachtrij die kopieën van de berichten ontvangen die naar het onderwerp zijn verzonden. Desgewenst kunt u filterregels voor een onderwerp per abonnement op basis van een registreren. Filterregels kunt u filteren of beperken welke berichten naar een onderwerp door welke onderwerpabonnementen worden ontvangen.

Service Bus-onderwerpen en abonnementen kunnen u schalen en te verwerken van een groot aantal berichten voor veel gebruikers en toepassingen.

## <a name="create-a-namespace"></a>Een naamruimte maken
Als u Service Bus-onderwerpen en -abonnementen in Azure wilt gebruiken, moet u eerst een *servicenaamruimte maken*. Een naamruimte biedt een scoping container voor het verwerken van Service Bus-resources in uw toepassing.

Ga als volgt te werk om een naamruimte te maken:

1. Meld u aan bij [Azure Portal][Azure portal].
2. Klik in het linkernavigatievenster van de portal op **maken van een resource**, klikt u vervolgens op **Enterprise Integration**, en klik vervolgens op **Service Bus**.
3. Voer in het dialoogvenster **Naamruimte maken** een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.
4. Nadat u hebt gecontroleerd of de naam van de naamruimte beschikbaar is, kiest u de prijscategorie (Basic, Standard of Premium).
5. Kies in het veld **Abonnement** een Azure-abonnement waarin u de naamruimte maakt.
6. In de **resourcegroep** veld, kiest u een bestaande resourcegroep waarin de naamruimte woont of maak een nieuwe.      
7. Kies in **Locatie** het land of regio waarin uw naamruimte moet worden gehost.
   
    ![Een naamruimte maken][create-namespace]
8. Klik op de knop **Maken**. Uw naamruimte wordt nu gemaakt en ingeschakeld. U moet wellicht enkele minuten wachten terwijl de resources voor uw account worden ingericht.

### <a name="obtain-the-credentials"></a>De referenties ophalen
1. Klik in de lijst met naamruimten op de zojuist gemaakte naam voor de naamruimte.
2. In de **Service Bus-naamruimte** deelvenster, klikt u op **gedeeld toegangsbeleid**.
3. In de **gedeeld toegangsbeleid** deelvenster, klikt u op **RootManageSharedAccessKey**.
   
    ![verbinding-gegevens][connection-info]
4. In de **beleid: RootManageSharedAccessKey** deelvenster, klik op de kopie knop Volgende om door te **Connection string – primaire sleutel**, de verbindingsreeks kopiëren naar uw Klembord voor later gebruik.
   
    ![connection-string][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


