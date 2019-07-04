---
title: Snelstartgids voor Microsoft Azure Data Box-zwaar | Microsoft Docs
description: Leer hoe u snel uw Azure Data Box zware in Azure portal
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: alkohli
ms.openlocfilehash: 3467b25c085fb86d4aed3918d5446d118f76ffb8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446740"
---
# <a name="quickstart-deploy-azure-data-box-heavy-using-the-azure-portal"></a>Quickstart: Azure Data Box zware met behulp van de Azure portal implementeren

Deze snelstartgids wordt beschreven hoe u de Azure Data Box zware met behulp van de Azure portal implementeert. De stappen bevatten informatie over het bekabelen, configureren en gegevens kopiëren naar Data Box zware zodat deze wordt geüpload naar Azure. De quickstart wordt uitgevoerd in de Azure-portal en op de lokale webgebruikersinterface van het apparaat.

Ga voor gedetailleerde stapsgewijze instructies voor implementatie en tracering naar [Zelfstudie: Volgorde Azure Data Box-zwaar](data-box-heavy-deploy-ordered.md)

## <a name="prerequisites"></a>Vereisten

Voer de volgende configuratievereisten voor de van installatiesite, de Data Box-service en het apparaat voordat u het apparaat implementeert.

### <a name="for-installation-site"></a>Voor de van installatiesite

Zorg voordat u begint voor het volgende:

- Het apparaat past via alle uw entryways. Apparaat-dimensies zijn: breedte: 26" lengte: 48" hoogte: 28”.
- U hebt toegang tot het apparaat via een lift of een ramp als u van plan bent om te installeren op een verdieping dan de begane grond.
- U hebt twee personen voor het afhandelen van het apparaat. Het apparaat weegt ongeveer ~ 500 lbs. en op wheels is.
- U hebt een platte site in het datacenter met de nabijheid van een beschikbare netwerkverbinding die een apparaat met deze voetafdruk aankan.

### <a name="for-service"></a>Voor de service

Zorg voordat u begint voor het volgende:

- U hebt een Microsoft Azure Storage-account met toegangsreferenties.
- Het abonnement dat u voor de Data Box-service is [Microsoft Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), [Cloud Solution Provider (CSP)](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview), of [Microsoft Azure Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/).
- U hebben eigenaar of bijdrager toegang tot het abonnement te maken van een Data Box zware volgorde.

### <a name="for-device"></a>Voor het apparaat

Zorg voordat u begint voor het volgende:

- U hebt bekeken de [veiligheid richtlijnen voor uw Data Box-zwaar](data-box-safety.md).
- U hebt een hostcomputer die is verbonden met het netwerk van datacenters. Gegevens in het zware kopieert u de gegevens van deze computer. De hostcomputer moet worden uitgevoerd een [ondersteund besturingssysteem](data-box-heavy-system-requirements.md).
- U hebt een laptop met RJ-45-kabel verbinding maken met de gebruikersinterface van lokale en het apparaat configureren. Gebruik de laptop op elk knooppunt van het apparaat eenmaal configureren.
- Uw datacenter heeft dankzij het netwerk en u moet ten minste één 10 GbE-verbinding.
- U moet een 40 Gbps-kabel of 10 Gbps-kabel per apparaatknooppunt. Kabels die compatibel zijn met de Mellanox MCX314A-BCCT netwerkinterface kiezen:
    - Voor de kabel 40 Gbps moet apparaat einde van de kabel QSFP +.
    - Voor de kabel 10 Gbps moet u een SFP +-kabel die in een switch 10 G aan het ene uiteinde, met een QSFP + SFP + adapter (of de adapter QSA) voor het end die wordt aangesloten op het apparaat.
- De power-kabels zijn opgenomen in een lade aan het einde van het apparaat.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="order"></a>Bestellen

Deze stap neemt ongeveer 5 minuten in beslag.

1. Maak een nieuwe Azure Data Box-resource in de Azure-portal.
2. Selecteer een bestaand abonnement dat is ingeschakeld voor deze service, en kies **Importeren** als overdrachtstype. Geef het **Bronland** op waar de gegevens zijn opgeslagen, en geef de **Azure-doelregio** voor de gegevensoverdracht op.
3. Selecteer **Data Box-zwaar**. De maximale aan bruikbare capaciteit 770 TB is en u kunt meerdere orders voor grotere gegevens maken.
4. Voer de order- en verzendgegevens in. Als de service beschikbaar is in uw regio, geeft u e-mailadressen voor meldingen op, controleert u de samenvatting en maakt u vervolgens de order.

Zodra de order is gemaakt, wordt het apparaat voorbereid voor verzending.

## <a name="cable-for-power"></a>Kabel voor energiebeheer

Deze stap duurt ongeveer 5 minuten.

Wanneer u de Data Box-zwaar ontvangt, moet u de volgende stappen voor het apparaat voor power bekabelen en schakel het apparaat.

1. Ga niet verder als u vermoedt dat er met het apparaat is geknoeid of dat het is beschadigd. Neem contact op met Microsoft Ondersteuning om een vervangend apparaat naar u te laten verzenden.
2. Verplaatsen van het apparaat naar installatiesite en de achterzijde wheels vergrendelen.
3. Alle vier power-kabels verbinden met de voedingen aan het einde van het apparaat.
4. Gebruik de knoppen power in het vlak van front om in te schakelen op de knooppunten van het apparaat.

## <a name="cable-first-node-for-network"></a>Eerste knooppunt van de kabel voor netwerk

Deze stap duurt ongeveer 10-15 minuten om te voltooien.

1. Gebruik de RJ-45-netwerkkabel van het type CAT-6 om de hostcomputer te koppelen aan de MGMT-poort (Managementpoort) op het apparaat.
2. De Twinax QSFP + koperen kabel gebruiken om ten minste één 40 Gbps verbinding te maken (bij voorkeur meer dan 1 Gbps) netwerkinterface, gegevens-1 of 2 voor gegevens. Als een 10 Gbps-switch, gebruikt u een Twinax SFP + koperen kabel met een QSFP + SFP + adapter (de adapter QSA) om de 40 Gbps-netwerkinterface voor gegevens verbinding te maken.
3. Sluit de kabels aan zoals hieronder wordt weergegeven.  

    ![Gegevens in het zware bekabelde](media/data-box-heavy-quickstart-portal/data-box-heavy-ports-cabled.png)  

## <a name="configure-first-node"></a>Eerste knooppunt configureren

Deze stap neemt ongeveer 5 tot 7 minuten in beslag.

1. Als u het wachtwoord van het apparaat wilt ophalen, gaat u naar **Algemeen > Apparaatdetails** in de [Azure-portal](https://portal.azure.com). Hetzelfde wachtwoord wordt gebruikt voor beide knooppunten van het apparaat.
2. Een statisch IP-adres 192.168.100.5 en subnet 255.255.255.0 toewijzen aan de Ethernet-adapter op de computer die u gebruikt voor het verbinding maken met gegevens in het zware. U hebt op `https://192.168.100.10` toegang tot de webgebruikersinterface van het apparaat. Nadat u het apparaat hebt ingeschakeld, duurt het maximaal 5 minuten om de verbinding tot stand te brengen.
3. Meld u aan met het wachtwoord uit de Azure-portal. U ziet nu een foutmelding over een probleem met het beveiligingscertificaat van de website. Volg de browserinstructies om naar de webpagina te gaan.
4. De instellingen voor de interfaces (met uitzondering van de MGMT) zijn standaard geconfigureerd als DHCP. Indien nodig, kunt u deze interfaces configureren als statisch en een IP-adres opgeven.

## <a name="cable-and-configure-the-second-node"></a>Bekabelen en configureren van het tweede knooppunt

Deze stap duurt ongeveer 15-20 minuten om te voltooien.

Volg de stappen die worden gebruikt voor het eerste knooppunt te bekabelen en configureren van het tweede knooppunt op het apparaat.  

## <a name="copy-data"></a>Gegevens kopiëren

De tijd om deze bewerking te voltooien, is afhankelijk van de gegevensgrootte van uw en de snelheid van het netwerk op waarover de gegevens worden gekopieerd.
 
1. Gegevens kopiëren naar zowel de apparaat-knooppunten met behulp van zowel de 40 Gbps gegevensinterfaces parallel.

    - Als een hulpprogramma voor SMB-compatibel bestand kopiëren met behulp van een Windows-host, gebruiken, zoals [Robocopy](https://technet.microsoft.com/library/ee851678.aspx).
    - Voor een NFS-host gebruikt u de opdracht `cp` of `rsync` om de gegevens te kopiëren.
2. Verbinding maken met de shares op het apparaat via het pad:`\\<IP address of your device>\ShareName`. Als u de referenties voor de toegang delen, gaat u naar de **Connect & kopiëren** pagina in de lokale webgebruikersinterface van de Data Box-zwaar.
3. Zorg ervoor dat de share en mapnamen en de gegevens richtlijnen die worden beschreven volgen in de [Servicelimieten van Azure Storage en gegevens in het zware](data-box-heavy-limits.md).

## <a name="prepare-to-ship"></a>Voorbereiding voor verzending

De duur van deze bewerking hangt af van de hoeveelheid gegevens.

1. Nadat het kopiëren van gegevens voltooid zonder fouten is, gaat u naar **voorbereiding voor verzending** pagina in de lokale web-UI en de voorbereiding voor verzending te starten.
2. Na de **voorbereiding voor verzending** is voltooid op beide knooppunten, uitschakelen van het apparaat via de lokale webgebruikersinterface.

## <a name="ship-to-azure"></a>Verzenden naar Azure

Met deze bewerking duurt ongeveer 15-20 minuten om te voltooien.

1. Verwijderen van de kabels en retourneren ze in de lade aan het einde van het apparaat.
2. Een ophalen met de regionale provider plannen.
3. Contact opnemen met [vak gegevensbewerkingen](mailto:DataBoxOps@microsoft.com) om u te informeren over het ophalen en aan het retourlabel.
4. Het retourlabel moet zichtbaar zijn op het voorpaneel wissen van het apparaat zijn.

## <a name="verify-data"></a>Gegevens controleren

De duur van deze bewerking hangt af van de hoeveelheid gegevens.

1. Wanneer de gegevens in het zware apparaat is verbonden met het netwerk van Azure-datacenter, worden de gegevens automatisch geüpload naar Azure.
2. Data Box-service ontvangt u een melding dat het kopiëren van gegevens voltooid via Azure portal is.

    1. Controleer foutenlogboeken op eventuele fouten en onderneem toepasselijke acties.
    2. Controleer of uw gegevens zich in de opslagaccount(s) bevinden voordat u deze uit de bron verwijdert.

## <a name="clean-up-resources"></a>Resources opschonen

Deze stap neemt 2 tot 3 minuten in beslag.

- U kunt de volgorde van de gegevens in het zware in Azure portal annuleert voordat de bestelling is verwerkt. Zodra de order is verwerkt, kan deze niet meer worden geannuleerd. De order doorloopt verwerkingsfasen totdat deze is voltooid. Als u de order wilt annuleren, gaat u naar **Overzicht** en klikt u in de opdrachtbalk op **Annuleren**.

- U kunt de order verwijderen zodra de status als **Voltooid** of **Geannuleerd** wordt weergegeven in de Azure-portal. Als u de order wilt verwijderen, gaat u naar **Overzicht** en klikt u in de opdrachtbalk op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een Data Box-zwaar om u te helpen uw gegevens importeren in Azure geïmplementeerd. Ga naar de volgende zelfstudie voor meer informatie over het beheer van Azure Data Box zware:

> [!div class="nextstepaction"]
> [De Azure portal gebruiken voor het beheren van gegevens in het zware](data-box-portal-admin.md)
