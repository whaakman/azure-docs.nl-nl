## Wat zijn Service Bus-onderwerpen en -abonnementen?

Service Bus-onderwerpen en -abonnementen bieden ondersteuning voor een berichtencommunicatiemodel op basis van *publiceren/abonneren*. Wanneer u gebruikmaakt van onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet direct met elkaar. In plaats daarvan wisselen ze berichten uit via een onderwerp dat als intermediair fungeert.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Anders dan bij Service Bus-wachtrijen, waarin elk bericht door een enkele gebruiker wordt verwerkt, bieden onderwerpen en abonnementen een 'één-naar-veel'-communicatiewijze, waarbij een patroon voor publiceren/abonneren wordt gebruikt. Het is mogelijk meerdere abonnementen voor een onderwerp te registreren. Wanneer een bericht naar een onderwerp wordt verzonden, wordt dit vervolgens beschikbaar gemaakt voor elk abonnement, waar het vervolgens zelfstandig wordt afgehandeld/verwerkt.

Een abonnement op een onderwerp lijkt op een virtuele wachtrij die kopieën van de berichten ontvangen die naar het onderwerp zijn verzonden. U kunt eventueel per abonnement filterregels voor een onderwerp registreren, waardoor u met behulp van een filter of een beperking kunt bepalen welke berichten naar een onderwerp door welke onderwerpabonnementen worden ontvangen.

Service Bus-onderwerpen en -abonnementen stellen u in staat om voor veel gebruikers en toepassingen een groot aantal berichten te schalen en te verwerken.

## Een naamruimte maken

Als u Service Bus-onderwerpen en -abonnementen in Azure wilt gebruiken, moet u eerst een *servicenaamruimte maken*. Een naamruimte biedt een scoping container voor het verwerken van Service Bus-resources in uw toepassing.

Ga als volgt te werk om een naamruimte te maken:

1. Meld u aan bij [Azure Portal][].

2. Klik in het linker navigatiedeelvenster van de portal achtereenvolgens op **Nieuw**, **Enterprise Integration** en **Service Bus**.

4. Voer in het dialoogvenster **Naamruimte maken** een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.

5. Nadat u hebt gecontroleerd of de naam van de naamruimte beschikbaar is, kiest u de prijscategorie (Basic, Standard of Premium).

7. Kies in het veld **Abonnement** een Azure-abonnement waarin u de naamruimte maakt.

9. Kies in het veld **Resourcegroep** een bestaande resourcegroep waarin de naamruimte zal zijn opgenomen of maak een nieuwe resourcegroep.      

8. Kies in **Locatie** het land of regio waarin uw naamruimte moet worden gehost.

    ![Een naamruimte maken][create-namespace]

6. Klik op de knop **Maken**. Uw naamruimte wordt nu gemaakt en ingeschakeld. U moet wellicht enkele minuten wachten terwijl de resources voor uw account worden ingericht.
 
### De referenties ophalen

1. Klik in de lijst met naamruimten op de zojuist gemaakte naam voor de naamruimte.
 
3. Klik in de blade **Service Bus-naamruimte** op **Beleid voor gedeelde toegang**.

4. Klik in de blade **Beleid voor gedeelde toegang** op **RootManageSharedAccessKey**.

    ![verbinding-gegevens][connection-info]

5. Klik in het blade **Beleid: RootManageSharedAccessKey** op de knop Kopiëren naast **Verbindingsreeks–primaire sleutel** om de verbindingsreeks naar het klembord te kopiëren voor later gebruik.

    ![verbinding-reeks][connection-string]

[Azure Portal]: https://portal.azure.com
[maken-naamruimte]: ./media/howto-service-bus-topics/create-namespace.png
[verbinding-gegevens]: ./media/howto-service-bus-topics/connection-info.png
[verbinding-reeks]: ./media/howto-service-bus-topics/connection-string.png




<!--HONumber=Sep16_HO3-->


