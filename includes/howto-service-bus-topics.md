---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: ef6d5d22f70d5fff38f90b457a7c945ab59fc67c
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52330909"
---
## <a name="what-are-service-bus-topics-and-subscriptions"></a>Wat zijn Service Bus-onderwerpen en -abonnementen?
Service Bus-onderwerpen en -abonnementen bieden ondersteuning voor een berichtencommunicatiemodel op basis van *publiceren/abonneren*. Wanneer u gebruikmaakt van onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet direct met elkaar. In plaats daarvan wisselen ze berichten uit via een onderwerp dat als intermediair fungeert.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Onderwerpen en -abonnementen voorzien in tegenstelling tot het Service Bus-wachtrijen, waarbij elk bericht is verwerkt door een enkele gebruiker, een "een-op-veel"-vorm van communicatie, met behulp van een patroon voor publiceren/abonneren. Het is mogelijk meerdere abonnementen voor een onderwerp te registreren. Wanneer een bericht naar een onderwerp wordt verzonden, wordt dit vervolgens beschikbaar gemaakt voor elk abonnement, waar het vervolgens zelfstandig wordt afgehandeld/verwerkt.

Een abonnement op een onderwerp lijkt op een virtuele wachtrij die kopieën van de berichten ontvangen die naar het onderwerp zijn verzonden. U kunt optioneel filterregels voor een onderwerp op basis van een per-abonnement registreren. Filterregels zorgen ervoor dat u om te filteren of beperken welke berichten voor een onderwerp worden ontvangen door welke berichtabonnementen.

Service Bus-onderwerpen en abonnementen kunnen u schalen en verwerken van een groot aantal berichten voor veel gebruikers en toepassingen.

## <a name="create-a-namespace"></a>Een naamruimte maken
Als u Service Bus-onderwerpen en -abonnementen in Azure wilt gebruiken, moet u eerst een *servicenaamruimte maken*. Een naamruimte biedt een scoping container voor het verwerken van Service Bus-resources in uw toepassing.

Ga als volgt te werk om een naamruimte te maken:

1. Meld u aan bij [Azure Portal][Azure portal].
2. Klik in het navigatiedeelvenster links in de portal, op **een resource maken**, klikt u vervolgens op **bedrijfsintegratie**, en klik vervolgens op **Service Bus**.
3. Voer in het dialoogvenster **Naamruimte maken** een naam in voor de naamruimte. In het systeem wordt onmiddellijk gecontroleerd of de naam beschikbaar is.
4. Nadat u hebt gecontroleerd of de naam van de naamruimte beschikbaar is, kiest u de prijscategorie (Basic, Standard of Premium).
5. Kies in het veld **Abonnement** een Azure-abonnement waarin u de naamruimte maakt.
6. In de **resourcegroep** veld, kiest u een bestaande resourcegroep waarin de naamruimte zich bevinden of een nieuwe maken.      
7. Kies in **Locatie** het land of regio waarin uw naamruimte moet worden gehost.
   
    ![Een naamruimte maken][create-namespace]
8. Klik op de knop **Maken**. Uw naamruimte wordt nu gemaakt en ingeschakeld. U moet wellicht enkele minuten wachten terwijl de resources voor uw account worden ingericht.

### <a name="obtain-the-credentials"></a>De referenties ophalen
1. Klik in de lijst met naamruimten op de zojuist gemaakte naam voor de naamruimte.
2. In de **Service Bus-naamruimte** deelvenster, klikt u op **beleid voor gedeelde toegang**.
3. In de **beleid voor gedeelde toegang** deelvenster, klikt u op **RootManageSharedAccessKey**.
   
    ![verbinding-gegevens][connection-info]
4. In de **beleid: RootManageSharedAccessKey** deelvenster, klikt u op de knop kopiëren naast naar **verbindingsreeks – primaire sleutel**om de verbindingsreeks kopiëren naar het Klembord voor later gebruik.
   
    ![connection-string][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


