---
title: Zelfstudie voor Azure Data Box zware volgorde | Microsoft Docs
description: Meer informatie over de vereisten voor implementatie en het bestellen van een Azure Data Box-zwaar
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: c7fbd37ff8d40f27e0ca18a6f9816d3d96422ab9
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592399"
---
# <a name="tutorial-order-azure-data-box-heavy"></a>Zelfstudie: Volgorde Azure Data Box-zwaar


Azure Data Box zware is een hybride oplossing waarmee u uw on-premises gegevens importeren in Azure in een snelle, eenvoudige en betrouwbare manier. U mag uw gegevens overdragen naar een opslagapparaat 770 TB Microsoft verstrekt (bij benadering aan bruikbare capaciteit) en stuur het apparaat terug. Deze gegevens worden vervolgens geüpload in Azure.

Deze zelfstudie wordt beschreven hoe u een Azure Data Box-zwaar kunt bestellen. In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> * Vereisten voor de Data Box-zwaar
> * Volgorde van een Data Box-zwaar
> * De bestelling volgen
> * De bestelling annuleren

## <a name="prerequisites"></a>Vereisten

Zorg dat u aan de volgende configuratievereisten voor de Data Box-service en het apparaat voldoet voordat u het apparaat implementeert.

### <a name="for-installation-site"></a>Voor de van installatiesite

Zorg voordat u begint voor het volgende:

- Het apparaat voldoet door middel van standard inbegrepen en entryways. Echter, zorg dat het apparaat via alle uw entryways past. Apparaat-dimensies zijn: breedte: 26" lengte: 48" hoogte: 28”.
- Als op een verdieping dan de begane grond is geïnstalleerd, moet u toegang voor het apparaat via een lift of een ramp. Het apparaat weegt ongeveer ~ 500 lbs.
- Zorg ervoor dat u hebt een platte site in het datacenter met de nabijheid van een beschikbare netwerkverbinding die een apparaat met deze voetafdruk aankan.


### <a name="for-service"></a>Voor de service

Zorg voordat u begint voor het volgende:
- U hebt een Microsoft Azure Storage-account met toegangsreferenties.
- Zorg ervoor dat het abonnement dat u voor de Data Box-service gebruikt, een van de volgende typen is:
    - Microsoft Enterprise Agreement (EA). Meer informatie over [EA-abonnementen](https://azure.microsoft.com/pricing/enterprise-agreement/).
    - Cloud Solution Provider (CSP). Meer informatie over het [Azure CSP-programma](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview).
    - Microsoft Azure Sponsorship. Meer informatie over het [Azure Sponsorship-programma](https://azure.microsoft.com/offers/ms-azr-0036p/).

- Zorg ervoor dat u eigenaar of bijdrager toegang tot het abonnement hebben te maken van een Data Box zware volgorde.

### <a name="for-device"></a>Voor het apparaat

Zorg voordat u begint voor het volgende:
- Uw apparaat is uitgepakt.
- Er is een hostcomputer verbonden met het datacenternetwerk. Gegevens in het zware kopieert u de gegevens van deze computer. De hostcomputer een ondersteund besturingssysteem moet uitvoeren, zoals beschreven in [Azure Data Box zware systeemvereisten](data-box-system-requirements.md).
- U moet een laptop met RJ-45-kabel verbinding maken met de gebruikersinterface van lokale en het apparaat configureren. Gebruik de laptop op elk knooppunt van het apparaat eenmaal configureren.
- Uw datacenter moet een netwerk met hoge snelheid hebben. Het wordt aangeraden dat u beschikt over minstens één 10 GbE-verbinding.
- U moet een 40 Gbps of 10 Gbps met een kabel per apparaatknooppunt. Kies kabels die compatibel met zijn de [Mellanox MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html) netwerkinterface:

    - Voor de 40 Gbps-kabel moet apparaat einde van de kabel QSFP +.
    - Voor de 10 Gbps-kabel moet u een SFP +-kabel die in een switch 10 G aan het ene uiteinde, met een QSFP + SFP + adapter (of de adapter QSA) voor het end die wordt aangesloten op het apparaat.
    - De power-kabels zijn opgenomen in het apparaat.

## <a name="order-data-box-heavy"></a>Volgorde Data Box-zwaar

Voer de volgende stappen uit in de Azure-portal om een apparaat te bestellen.

1. Gebruik uw Microsoft Azure-referenties om u aan te melden op deze URL: [https://portal.azure.com](https://portal.azure.com).
2. Selecteer **+ een resource maken** en zoek naar de *Azure Data Box*. Selecteer **Azure Data Box**.
    
   [![Zoek naar Azure Data Box 1](media/data-box-deploy-ordered/search-azure-data-box1.png)](media/data-box-deploy-ordered/search-azure-data-box1.png#lightbox)

3. Selecteer **Maken**.

4. Controleer of de Data Box-service beschikbaar in uw regio is. Typ of Selecteer de volgende informatie en selecteer **toepassen**.

    |Instelling  |Value  |
    |---------|---------|
    |Abonnement     | Selecteer een EA-, CSP- of Azure Sponsorship-abonnement voor de Data Box-service. <br> Het abonnement is gekoppeld aan uw factureringsrekening.       |
    |Type overdracht     | Selecteer **Importeren in Azure**.        |
    |Bronland     | Selecteer het land/de regio waarin uw gegevens zich momenteel bevindt.         |
    |Doel-Azure-regio     | Selecteer de Azure-regio waarnaar u uw gegevens wilt overdragen.        |

    [![Data Box-familie beschikbaarheid selecteren](media/data-box-deploy-ordered/select-data-box-option1.png)](media/data-box-deploy-ordered/select-data-box-option1.png#lightbox)

5. Selecteer **Data Box-zwaar**. De maximale aan bruikbare capaciteit voor één order is 770 TB.

    [![Data Box-zwaar selecteren](media/data-box-heavy-deploy-ordered/select-data-box-heavy.png)

6. Voer in **Bestellen** de **Orderdetails** in. Typ of Selecteer de volgende informatie en selecteer **volgende**.
    
    |Instelling  |Waarde  |
    |---------|---------|
    |Name     | Geef een beschrijvende naam op om de bestelling te volgen. <br> De naam kan tussen 3 en 24 tekens bevatten (letters, cijfers en afbreekstreepjes). <br> De naam moet beginnen en eindigen met een letter of cijfer.      |
    |Resourcegroep     | Gebruik een bestaande of maak een nieuwe. <br> Een resourcegroep is een logische container voor resources die samen kunnen worden beheerd of geïmplementeerd.         |
    |Doel-Azure-regio     | Selecteer een regio voor uw opslagaccount. <br> Ga naar [Beschikbaarheid in de regio](https://azure.microsoft.com/global-infrastructure/services/?products=databox) voor meer informatie.        |
    |Opslaglocatie     | Kies in de storage-account of beheerde schijven of beide. <br> Selecteer een of meer opslagaccounts in de gefilterde lijst van een bestaand opslagaccount, gebaseerd op de opgegeven Azure-regio. <br>Data Box-zwaar kan worden gekoppeld met maximaal 10 storage-accounts. <br> U kunt ook maken een nieuwe **voor algemeen gebruik v1**, **voor algemeen gebruik v2**, of **Blob storage-account**. <br> Azure Data Lake Storage Gen 2-accounts worden niet ondersteund. Zie de [opslagaccounts ondersteund bij het apparaat](data-box-heavy-system-requirements.md#supported-storage-accounts). <br>Storage-accounts met virtuele netwerken worden ondersteund. Schakelt de vertrouwde services binnen de firewall-instellingen van storage account network zodat Data Box-service om te werken met beveiligde storage-accounts. Zie voor meer informatie over het [toevoegen Azure Data Box-service als een vertrouwde service](../storage/common/storage-network-security.md#exceptions).|

    Als u de storage-account als de opslaglocatie, ziet u de volgende schermafbeelding:

    ![Data Box-zwaar volgorde voor storage-account](media/data-box-heavy-deploy-ordered/order-storage-account.png)

    Als naast het storage-account als de opslaglocatie, u ook van gegevens in het zware gebruikmaakt voor het maken van beheerde schijven van de on-premises VHD's, moet u de volgende informatie:

    |Instelling  |Value  |
    |---------|---------|
    |Resourcegroepen     | Maak nieuwe resourcegroepen als u van plan bent voor het maken van beheerde schijven vanaf on-premises VHD's. U kunt een bestaande resourcegroep gebruiken alleen als de resourcegroep is eerder hebt gemaakt bij het maken van een Data Box zware volgorde voor beheerde schijf door Data Box-service. <br> Geef meerdere resourcegroepen, gescheiden door puntkomma's. Maximaal 10 resourcegroepen worden ondersteund.|

    ![Data Box-zwaar volgorde voor beheerde schijf](media/data-box-heavy-deploy-ordered/order-managed-disks.png)

    Het opslagaccount dat is opgegeven voor beheerde schijven wordt als een tijdelijke opslagaccount dat gebruikt. De uploads van de Data Box-service is de VHD's als pagina-blobs naar het tijdelijke opslagaccount voordat u deze converteren naar beheerde schijven en verplaatsen naar de resourcegroepen. Zie voor meer informatie, [controleren of gegevens uploaden naar Azure](data-box-deploy-picked-up.md#verify-data-upload-to-azure).

7. Bij **Verzendadres** geeft u uw voor- en achternaam, de naam en het postadres van het bedrijf en een geldig telefoonnummer op. Selecteer **adres valideren**. 

    De service controleert of de service beschikbaar is voor de regio van het verzendadres. Als de service beschikbaar is voor het opgegeven verzendadres, ontvangt u daarover een melding. Selecteer **Next**.

8. In de **Meldingsdetails** geeft u een e-mailadres op. De service stuurt e-mailmeldingen naar het opgegeven e-mailadres over updates van de bestelstatus.

    We raden u aan een e-mailadres van een groep te gebruiken, zodat u meldingen blijft ontvangen als een beheerder de groep verlaat.

9. Bekijk de gegevens met betrekking tot de bestelling, het contact, de meldingen en de privacyvoorwaarden in **Samenvatting**. Vink het selectievakje aan waarmee u akkoord gaat met de privacyvoorwaarden.

10. Selecteer **volgorde**. Het duurt een paar minuten voordat de bestelling is gemaakt.


## <a name="track-the-order"></a>De bestelling volgen

Nadat u uw bestelling hebt geplaatst, kunt u de status van de bestelling volgen via de Azure-portal. Ga naar uw Data Box zware volgorde en ga vervolgens naar **overzicht** om de status weer te geven. In de portal ziet u dat de bestelling de status **Besteld** heeft.

Als het apparaat niet beschikbaar is, ontvangt u een melding. Als het apparaat wel beschikbaar is, identificeert Microsoft het apparaat dat moet worden verzonden en bereidt Microsoft de verzending voor. Tijdens de voorbereiding van het apparaat vinden de volgende acties plaats:

- Voor elk opslagaccount dat aan het apparaat is gekoppeld, worden SMB-shares gemaakt.
- Voor elke share worden toegangsreferenties zoals een gebruikersnaam en wachtwoord gegenereerd.
- Er wordt ook een apparaatwachtwoord gegenereerd om het apparaat mee te ontgrendelen.
- De Data Box-zwaar is vergrendeld om te voorkomen dat onbevoegde toegang tot op het apparaat op elk gewenst moment.

Wanneer de apparaatvoorbereiding is voltooid, wordt de bestelling in de portal weergegeven met de status **Verwerkt**.

![Data Box-zwaar volgorde verwerkt](media/data-box-overview/data-box-order-status-processed.png)

Microsoft bereidt de verzending vervolgens voor en verzendt het apparaat met een regionale vervoerder. U ontvangt uw volgnummer zodra het apparaat is verzonden. In de portal wordt bestelling weergegeven met de status **Verzonden**.

![Data Box-zwaar volgorde verzonden](media/data-box-overview/data-box-order-status-dispatched.png)

## <a name="cancel-the-order"></a>De bestelling annuleren

Als u deze bestelling wilt annuleren, gaat u in de Azure-portal naar **Overzicht** en klikt u in de opdrachtbalk op **Annuleren**.

Nadat u een bestelling hebt geplaatst, kunt u deze op elk moment annuleren voordat de status op Verwerkt is ingesteld.
 
Als u een geannuleerde bestelling wilt verwijderen, gaat u naar **Overzicht** en klikt u in de opdrachtbalk op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over Azure Data Box zware onderwerpen, zoals:

> [!div class="checklist"]
> * Vereisten
> * Volgorde Data Box-zwaar
> * De bestelling volgen
> * De bestelling annuleren

Ga naar de volgende zelfstudie voor meer informatie over het instellen van uw Data Box-zwaar.

> [!div class="nextstepaction"]
> [Instellen van uw Azure Data Box zware](./data-box-heavy-deploy-set-up.md)
