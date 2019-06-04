---
title: Zelfstudie voor het voorbereiden van Azure portal, datacenteromgeving naar Azure Data Box-Edge implementeren | Microsoft Docs
description: De eerste zelfstudie over het implementeren van Azure Data Box Edge omvat het voorbereiden van de Azure-portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 06/03/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 364ceabc3c9e7a577bd126b81bcd5256d947bbe3
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476772"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge"></a>Zelfstudie: Voorbereidingen voor de implementatie van Azure Data Box Edge  

Dit is de eerste zelfstudie in de reeks zelfstudies voor implementatie die noodzakelijk zijn voor het voltooien van de implementatie van Azure Data Box Edge. In deze zelfstudie wordt beschreven hoe u Azure Portal voorbereidt voor de implementatie van een Data Box Edge-resource.

U hebt beheerdersbevoegdheden nodig om het installatie- en configuratieproces uit te voeren. Het voorbereiden van de portal duurt minder dan 10 minuten.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe resource maken
> * De activeringssleutel ophalen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

### <a name="get-started"></a>Aan de slag

Raadpleeg de volgende zelfstudies in de voorgeschreven volgorde voor het implementeren van Data Box Edge.

| **#** | **In deze stap** | **Gebruikt u deze documenten** |
| --- | --- | --- | 
| 1. |**[Azure Portal voorbereiden voor Data Box Edge](data-box-edge-deploy-prep.md)** |Maak en configureer uw Data Box Edge-resource voordat u een fysiek Data Box Edge-apparaat installeert. |
| 2. |**[Data Box Edge installeren](data-box-edge-deploy-install.md)**|Het fysieke Data Box Edge-apparaat uitpakken, plaatsen en aansluiten.  |
| 3. |**[Data Box Edge verbinden, instellen en activeren](data-box-edge-deploy-connect-setup-activate.md)** |Maak verbinding met de lokale webinterface, voltooi het instellen van het apparaat en activeer het. Het apparaat is klaar om er SMB- of NFS-shares op in te stellen.  |
| 4. |**[Gegevens overdragen met Data Box Edge](data-box-edge-deploy-add-shares.md)** |Voeg shares toe en maak verbinding met shares via SMB of NFS. |
| 5. |**[Gegevens transformeren met Data Box Edge](data-box-edge-deploy-configure-compute.md)** |Compute-modules op het apparaat het transformeren van de gegevens als deze in Azure configureren. |

U kunt nu Azure Portal gaan instellen.

## <a name="prerequisites"></a>Vereisten

Hier volgen de configuratievereisten voor uw Data Box Edge-resource, uw Data Box Edge-apparaat en het datacenternetwerk.

### <a name="for-the-data-box-edge-resource"></a>Voor de Data Box Edge-resource

Zorg voordat u begint voor het volgende:

- Uw Microsoft Azure-abonnement is ingeschakeld voor een Data Box Edge-resource. Betalen per gebruik-abonnementen worden niet ondersteund.
- Hebt u eigenaar of bijdrager niveau van de resourcegroep voor de Data Box Edge/de Data Box Gateway, IoT-Hub en Azure Storage-resources te gebruiken.

    - Maken van een Data Box-Edge / Data Box-Gateway-resource, u hebt machtigingen als Inzender (of hoger) binnen het bereik op het niveau van resources. U moet ook om ervoor te zorgen dat de `Microsoft.DataBoxEdge` provider is geregistreerd. Voor informatie over het registreren, gaat u naar [registerbronprovider](data-box-edge-manage-access-power-connectivity-mode.md#register-resource-providers).
    - Voor het maken van een IoT Hub-resource, zorg ervoor dat die Microsoft.Devices-provider is geregistreerd. Voor informatie over het registreren, gaat u naar [registerbronprovider](data-box-edge-manage-access-power-connectivity-mode.md#register-resource-providers).
    - Voor het maken van de resource van een Opslagaccount, opnieuw moet u bijdrager of hoger toegang binnen het bereik op het niveau van de resource. Azure Storage is standaard een geregistreerde resourceprovider.
- U hebt de beheerder of gebruiker toegang tot Azure Active Directory Graph API. Zie voor meer informatie, [Azure Active Directory Graph API](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
- U hebt een Microsoft Azure Storage-account met toegangsreferenties.

### <a name="for-the-data-box-edge-device"></a>Voor het Data Box Edge-apparaat

Voordat u een fysiek apparaat implementeert, controleert u of:

- U kunt de veiligheidsinformatie die is opgenomen in het pakket voor verzending hebt bekeken.
- U hebt een sleuf 1U beschikbaar zijn in een standard 19-inch rack in uw datacenter voor rack koppelen van het apparaat.
- U beschikt over een vlak, stabiel en horizontaal werkoppervlak waar u het apparaat veilig kunt neerleggen.
- De locatie waar u het apparaat wilt neerzetten, beschikt over reguliere wisselstroom van een onafhankelijke bron of over een vermogenseenheid (PDU) met noodvoeding (UPS).
- U hebt toegang tot een fysiek apparaat.


### <a name="for-the-datacenter-network"></a>Voor datacenternetwerk

Zorg voordat u begint voor het volgende:

- Het netwerk in uw datacenter is geconfigureerd volgens de netwerkvereisten voor uw Data Box Edge-apparaat. Zie [Systeemvereisten voor Data Box Edge](data-box-edge-system-requirements.md) voor meer informatie.

- U hebt voor normale operationele omstandigheden van de rand van uw gegevens in:

    - Minimaal 10 Mbps bandbreedte om te controleren of het apparaat blijft bijgewerkte downloaden.
    - Een minimum van 20-Mbps toegewezen uploaden en downloaden van bandbreedte om over te dragen van bestanden.

## <a name="create-a-new-resource"></a>Een nieuwe resource maken

Als u al een Data Box Edge-resource hebt voor het beheer van uw fysieke apparaat, kunt u deze stap overslaan en verdergaan met [Activeringscode ophalen](#get-the-activation-key).

Voer de volgende stappen uit in Azure Portal om een Data Box-resource te maken.

1. Gebruik uw Microsoft Azure-referenties voor het aanmelden bij 
    
    - De Azure-portal op de volgende URL: [ https://portal.azure.com ](https://portal.azure.com).
    - Of, in de Azure Government-portal op de volgende URL: [ https://portal.azure.us ](https://portal.azure.us). Voor meer informatie gaat u naar [verbinding maken met Azure Government met behulp van de portal](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal).

2. Selecteer in het linkerdeelvenster **+ een resource maken**. Zoeken naar **Data Box Edge / Data Box Gateway**. Selecteer **Data Box Edge / Data Box Gateway**. Selecteer **Maken**.
3. Kies het abonnement dat u wilt gebruiken voor de gegevens in het Edge-apparaat. Selecteer de regio waar u de Data Box Edge-resource wilt implementeren. VS-Oost, Zuidoost-Azië en West-Europa zijn beschikbaar voor deze release. 

    Kies een locatie die het dichtst bij de geografische regio ligt waar u uw apparaat wilt implementeren. De regio slaat alleen de metagegevens voor Apparaatbeheer. De werkelijke gegevens kunnen worden opgeslagen in een storage-account. 
    
    Selecteer bij **Maken** bij de optie **Data Box Edge**.

    ![Data Box Edge-service zoeken](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Op de **basisbeginselen** tabblad Typ of Selecteer de volgende **projectdetails**.
    
    |Instelling  |Value  |
    |---------|---------|
    |Abonnement    |Dit wordt automatisch ingevuld op basis van de eerdere selectie. Abonnement is gekoppeld aan uw factureringsrekening. |
    |Resourcegroep  |Maak een nieuwe groep of selecteer een bestaande groep.<br>Meer informatie over [Azure-resourcegroepen](../azure-resource-manager/resource-group-overview.md).     |

4. Typ of Selecteer de volgende **exemplaar details**.

    |Instelling  |Waarde  |
    |---------|---------|
    |Name   | Een beschrijvende naam om de resource aan te duiden.<br>De naam is tussen 2 en 50 tekens lang en kan letters, cijfers en afbreekstreepjes bevatten.<br> De naam begint en eindigt met een letter of cijfer.        |
    |Regio     |Voor deze release zijn de VS-Oost, Zuidoost-Azië en West-Europa zijn beschikbaar voor het implementeren van uw resource. Als u Azure Government, alle government-regio's zijn beschikbaar zoals wordt weergegeven in de [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/).<br> Kies een locatie die het dichtst bij de geografische regio ligt waar u uw apparaat wilt implementeren.|

    ![Details van het project en -exemplaar](media/data-box-edge-deploy-prep/data-box-edge-resource.png)

5. Selecteer **Volgende: Verzendadres**.

    - Als u al een apparaat hebt, selecteert u de keuzelijst met invoervak voor **ik gegevens in het Edge-apparaat hebt**.
    - Als dit het nieuwe apparaat dat u bent bestellen, voer de naam van contactpersoon, bedrijf, adres voor het verzenden van het apparaat en contactgegevens.

    ![Verzendadres voor nieuw apparaat](media/data-box-edge-deploy-prep/data-box-edge-resource1.png)

6. Selecteer **Volgende: Beoordelen en maken**.

7. Op de **bekijken + maken** tabblad de **prijsinformatie**, **gebruiksvoorwaarden**, en de details voor uw resource. Selecteer de keuzelijst met invoervak voor **voorwaarden van de privacyverklaring zkontroloval jsem**.

    ![Bekijk de details van de gegevens in het Edge-resource en privacyvoorwaarden](media/data-box-edge-deploy-prep/data-box-edge-resource2.png)

8. Selecteer **Maken**.

Het maken van de resource duurt enkele minuten. Nadat de resource is gemaakt en geïmplementeerd, krijgt u een melding. Selecteer **Ga naar resource**.

![Ga naar de gegevens in het Edge-resource](media/data-box-edge-deploy-prep/data-box-edge-resource3.png)

Nadat de order is geplaatst, wordt Microsoft beoordeelt de volgorde en zoekt u (via e-mail) met de verzendgegevens.

![Kennisgeving voor beoordeling van de gegevens in het Edge-volgorde](media/data-box-edge-deploy-prep/data-box-edge-resource4.png)

## <a name="get-the-activation-key"></a>De activeringssleutel ophalen

Nadat de Azure Data Box Edge-resource is geactiveerd, hebt u de activeringssleutel nodig. Deze sleutel wordt gebruikt om uw Data Box Edge-apparaat te activeren en te verbinden met de resource. U kunt deze sleutel nu ophalen, terwijl u Azure Portal geopend hebt.

1. Selecteer de resource die u hebt gemaakt. Selecteer **overzicht** en selecteer vervolgens **Apparaatinstelling**.

    ![Selecteer de installatie van apparaat](media/data-box-edge-deploy-prep/data-box-edge-select-devicesetup.png)

2. Op de **activeren** tegel, selecteer **sleutel genereren** te maken van een activeringscode. Selecteer het kopieerpictogram om de sleutel te kopiëren en op te slaan voor later gebruik.

    ![Activeringssleutel ophalen](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - De activeringssleutel verloopt drie dagen nadat deze is gegenereerd.
> - Als de sleutel is verlopen, genereert u een nieuwe sleutel. De oudere toegangssleutel is niet langer geldig.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Data Box Edge, zoals:

> [!div class="checklist"]
> * Een nieuwe resource maken
> * De activeringssleutel ophalen

Ga naar de volgende zelfstudie om te lezen hoe u Data Box Edge installeert.

> [!div class="nextstepaction"]
> [Data Box Edge installeren](./data-box-edge-deploy-install.md)



