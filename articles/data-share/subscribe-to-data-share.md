---
title: Zelfstudie - accepteren en ontvangen van gegevens met behulp van Azure-Gegevensvoorbeeld delen
description: Zelfstudie - accepteren en ontvangen van gegevens met behulp van Azure-Gegevensvoorbeeld delen
author: joannapea
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: fc63c1a0b3b496de8e5ecea58f79f1db9d872e80
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838416"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share-preview"></a>Zelfstudie: Accepteren en ontvangen van gegevens met behulp van Azure-Gegevensvoorbeeld delen

In deze zelfstudie leert u hoe u een uitnodiging voor een gegevens delen met behulp van Azure gegevens delen Preview accepteert. U leert hoe u gegevens worden gedeeld met u ontvangt, evenals het inschakelen van een interval voor reguliere vernieuwen om ervoor te zorgen dat u altijd de meest recente momentopname van de gegevens worden gedeeld met u hebben. 

> [!div class="checklist"]
> * Hoe u een voorbeeld van Azure gegevens delen uitnodiging te accepteren
> * Een voorbeeld van Azure gegevens delen-account maken
> * Geef een doel voor uw gegevens
> * Maken van een abonnement op het delen van uw gegevens voor geplande vernieuwing

## <a name="prerequisites"></a>Vereisten
Voordat u een uitnodiging voor het delen van gegevens accepteren kan, moet u een aantal Azure-resources, die hieronder worden voorzien. 

Zorg ervoor dat alle vereisten voldaan is voordat u een uitnodiging voor het delen van gegevens accepteert. 

* Azure-abonnement: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Een Azure Storage-account: Als u dit niet al hebt, kunt u een [Azure Storage-account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). 
* Een uitnodiging voor het delen van gegevens: Een uitnodiging van Microsoft Azure met een onderwerp met de titel "uitnodiging Azure het delen van gegevens van **<yourdataprovider@domain.com>** '.
* Machtiging roltoewijzing toevoegen aan de storage-account, deze is beschikbaar in de *Microsoft.Authorization/role toewijzingen schrijftijd* machtiging. Deze machtiging bestaat in de rol van eigenaar. 
* Registratie van de Resourceprovider voor Microsoft.DataShare. Zie de [Azure Resourceproviders](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services) documentatie voor meer informatie over hoe u dit doet. 

> [!IMPORTANT]
> Om te accepteren en het delen van een Azure-gegevens ontvangen, moet u eerst de Microsoft.DataShare-resourceprovider te registreren en u moet een eigenaar van het opslagaccount dat u gegevens accepteert. Volg de instructies in [oplossen Azure gegevens delen Preview](data-share-troubleshoot.md) te Registreer de resourceprovider van de gegevens delen, alsmede uzelf als eigenaar van het storage-account toevoegen. 

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="open-invitation"></a>Open de uitnodiging

Controleer uw postvak in voor een uitnodiging van de gegevensprovider. De uitnodiging is van Microsoft Azure, met de titel **uitnodiging Azure het delen van gegevens van <yourdataprovider@domain.com>** . Noteer de naam van de share om ervoor te zorgen aanvaardt u de juiste share als er meerdere uitnodigingen. 

Selecteer op **uitnodiging weergeven** om te zien van de uitnodiging in Azure. Hiermee gaat u naar de weergave Shares ontvangen.

![Uitnodigingen](./media/invitations.png "lijst met uitnodigingen") 

Selecteer de share die u wilt weergeven. 

## <a name="accept-invitation"></a>Uitnodiging accepteren
Zorg ervoor dat alle velden worden gecontroleerd, met inbegrip van de **gebruiksvoorwaarden**. Als u akkoord met de gebruiksrechtovereenkomst gaat, wordt u gevraagd of het selectievakje om aan te geven dat stemt u ermee. 

![Gebruiksvoorwaarden](./media/terms-of-use.png "gebruiksvoorwaarden") 

Onder *doelaccount gegevens delen*, selecteert u het abonnement en de resourcegroep die u implementeert u het delen van uw gegevens in. 

Voor de **gegevens delen van een Account** veld **nieuw** als u geen een bestaande account voor het delen van gegevens. Selecteer anders een bestaande gegevens delen account die u wilt accepteren van het delen van uw gegevens in. 

Voor de *sharenaam ontvangen* veld, u kunt laat de standaardwaarde is opgegeven door de gegevens bevatten, of een nieuwe naam voor de ontvangen share opgeven. 

![Gegevens delen van een account als doel](./media/target-data-share.png "doelgegevens wordt gedeeld.") 

Nadat u hebt ingestemd met de gebruiksvoorwaarden en een locatie voor de share, selecteer opgegeven op *accepteren en configureren*. Als u deze optie kiest, wordt een share-abonnement wordt gemaakt en wordt het volgende scherm wordt u gevraagd om te selecteren van een doel-opslagaccount voor uw gegevens moeten worden gekopieerd naar. 

![Opties voor accepteren](./media/accept-options.png "opties accepteren") 

Als u liever nu de uitnodiging accepteren, maar het configureren van uw opslag op een later tijdstip, selecteer *accepteren en later configureren*. Deze optie kunt u uw doelopslagaccount later configureren. Om door te gaan met het configureren van uw opslag later [uw storage-account configureren](how-to-configure-mapping.md) pagina voor gedetailleerde stappen voor het hervatten van uw gegevens configuratie delen. 

Als u niet wilt dat de uitnodiging te accepteren Selecteer *afwijzen*. 

## <a name="configure-storage"></a>Opslag configureren
Onder *doel Opslaginstellingen*, selecteert u het abonnement, resourcegroep en storage-account dat u wilt ontvangen van uw gegevens in. 

![Instellingen voor de opslag als doel](./media/target-storage-settings.png "Doelopslag") 

Zorg ervoor dat u de momentopname-instellingen inschakelen voor het ontvangen van uw gegevens regelmatig worden vernieuwd. Houd er rekening mee dat u alleen een momentopname-instelling schema ziet, als de gegevensprovider heeft worden opgenomen in het delen van de gegevens. 

![Instellingen voor de momentopname](./media/snapshot-settings.png "Snapshot-instellingen") 

Selecteer *Opslaan*. 

## <a name="trigger-a-snapshot"></a>Activeren van een momentopname

U kunt een momentopname in de Shares ontvangen activeren-tabblad Details door te selecteren > **Trigger momentopname**. Hier kunt kunt u een momentopname van een volledige of incrementele van uw gegevens activeren. Als dit de eerste keer ontvangt gegevens van de gegevensprovider is, selecteert u volledige kopie. 

![Momentopname van de trigger](./media/trigger-snapshot.png "Trigger momentopname") 

Wanneer de status van de laatste uitvoering is *geslaagde*, open het storage-account om de ontvangen gegevens weer te geven. 

Als u wilt controleren welke storage-account dat u hebt gebruikt, selecteert u op **gegevenssets**. 

![Consumenten gegevenssets](./media/consumer-datasets.png "consument gegevensset toewijzing") 

## <a name="view-history"></a>Geschiedenis weergeven
Als u wilt een geschiedenis van de momentopnamen weergeven, gaat u naar Shares ontvangen -> geschiedenis. Hier vindt u een overzicht van alle momentopnamen die zijn gegenereerd voor de afgelopen 60 dagen. 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt vernomen u accepteren en het delen van een Azure-gegevens ontvangen. Voor meer informatie over het delen van gegevens Azure concepten, blijven [concepten: Azure-gegevens delen terminologie](terminology.md).