---
title: Microsoft Azure Data Box Gateway usecases | Microsoft Docs
description: Beschrijving van de usecases voor Azure Data Box-Gateway, een virtueel apparaat opslagoplossing waarmee u gegevens overdragen naar Azure
services: databox
author: alkohli
ms.service: databox
ms.topic: article
ms.date: 01/17/2019
ms.author: alkohli
ms.openlocfilehash: b4966d03e44591f01bee945c743ea83f57e6da93
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54392271"
---
# <a name="use-cases-for-azure-data-box-gateway"></a>Use cases voor Azure Data Box-Gateway

Azure Data Box-Gateway is een gatewayapparaat voor cloud-opslag die zich bevindt op uw locatie en stuurt uw installatiekopie, media en andere gegevens naar Azure. Deze cloud storage-gateway is een virtuele machine ingericht in de hypervisor. Schrijven van gegevens naar dit virtuele apparaat met behulp van de NFS- en SMB-protocollen die het verzendt vervolgens naar Azure. Dit artikel bevat een gedetailleerde beschrijving van de scenario's waarin u dit apparaat kunt implementeren.

Data Box-Gateway gebruiken voor de volgende scenario's:

- Om op te nemen continu enorme hoeveelheden gegevens.
- Voor de archivering van gegevens in een veilige en efficiënte manier cloud.
- Voor de overdracht van incrementele gegevens via het netwerk na de initiële bulksgewijs overdracht is voltooid met behulp van Data Box.

Elk van deze scenario's is beschreven in de volgende secties worden uitgelegd.

> [!IMPORTANT]
> Data Box Gateway is in de previewfase. Lees de [gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert.

## <a name="continuous-data-ingestion"></a>Continue gegevensopname

Een van de belangrijkste voordelen van Data Box-Gateway is de mogelijkheid om continu gegevens opnemen in het apparaat om te kopiëren naar de cloud, ongeacht de grootte van de.

Als de gegevens worden geschreven naar het gateway-apparaat, wordt het apparaat de gegevens geüpload naar Azure Storage. Het apparaat beheert automatisch opslag door het verwijderen van de bestanden lokaal terwijl de metagegevens behouden blijft wanneer het een bepaalde drempelwaarde bereikt. Bewaren van een lokale kopie van de metagegevens, kunt de gateway-apparaat alleen uploadt u de wijzigingen wanneer het bestand wordt bijgewerkt. De gegevens die zijn geüpload naar het gatewayapparaat moet aan de hand van de richtlijnen in [gegevens uploaden onder voorbehoud](data-box-gateway-limits.md#data-upload-caveats).

Als het apparaat gevuld met gegevens, begint deze beperking van de snelheid van ingangsgebeurtenissen (indien nodig) zodat deze overeenkomt met de snelheid waarmee gegevens worden geüpload naar de cloud. Waarschuwingen gebruiken voor het controleren van de continue opname van gegevens op het apparaat. Deze waarschuwingen worden gegenereerd wanneer de beperking wordt gestart en worden gewist nadat de beperking is gestopt.

## <a name="cloud-archival-of-data"></a>Cloud archivering van gegevens

Data Box-Gateway gebruiken als u wilt uw gegevens bewaren voor de lange termijn in de cloud. U kunt de **archief** laag van opslag voor retentie op lange termijn.

Archive-laag is geoptimaliseerd voor gegevens op te slaan die zelden worden gebruikt voor ten minste 180 dagen. De **archief** -laag biedt de laagste kosten voor opslag, maar heeft de hoogste toegangskosten. Ga voor meer informatie naar [Archive Storage-toegangslaag](/azure/storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

### <a name="move-data-to-archive-tier"></a>Gegevens verplaatsen naar de Archive-laag

Voordat u begint, zorg ervoor dat u een actief Data Box-Gateway-apparaat hebt. Volg de stappen die worden beschreven [zelfstudie: Voorbereidingen voor het implementeren van Azure Data Box Gateway](data-box-gateway-deploy-prep.md) en naar de volgende zelfstudie opkomst, totdat u een operationele apparaat hebt.

- De gegevens in het Gateway-apparaat gebruiken om gegevens te uploaden naar Azure via de gebruikelijke overdracht procedure zoals beschreven in [gegevensoverdracht via een gegevensgateway vak](data-box-gateway-deploy-add-shares.md).
- Nadat het uploaden is voltooid, moet u verplaatsen naar de Archive-laag. U kunt de blob-laag instellen op twee manieren: Azure PowerShell-script of een Azure Storage Lifecycle Management-beleid.  
    - Als u met behulp van Azure PowerShell, volgt u deze [stappen](/azure/databox/data-box-how-to-set-data-tier.md#use-azure-powershell-to-set-the-blob-tier) verplaatsen van de gegevens naar de Archive-laag.
    - Als Azure Lifecycle Management, volgt u deze stappen voor het verplaatsen van de gegevens naar de Archive-laag.
        - [Registreren](/azure/storage/common/storage-lifecycle-management-concepts.md#register-for-preview) voor de preview van Blob Lifecycle management-service te gebruiken van Archive-laag.
        - Gebruik het volgende beleid [opname van gegevens archiveren op](/azure/storage/blobs/storage-lifecycle-management-concepts.md#archive-data-at-ingest.md).
- Zodra de blobs zijn gemarkeerd als archiveren, ze kunnen niet meer worden gewijzigd door de gateway, tenzij ze worden verplaatst naar de laag warm of koud. Als het bestand zich in de lokale opslag, worden alle wijzigingen aan de lokale kopie (inclusief verwijderingen) niet als u wilt archiveren laag geüpload.
- Als u wilt lezen van gegevens in Archive storage, moet deze gereactiveerd door het veranderen van de blob-laag naar warm of koud. [Vernieuwen van de share](data-box-gateway-manage-shares.md#refresh-shares) op de gateway de blob niet rehydrate.

Voor meer informatie meer informatie over het [beheren Azure Blob Storage Lifecycle](/azure/storage/common/storage-lifecycle-management-concepts.md).

## <a name="initial-bulk-transfer-followed-by-incremental-transfer"></a>Overdracht van eerste bulksgewijs gevolgd door incrementele overdracht

Data Box en Data Box-Gateway samen gebruiken als u wilt een bulksgewijs uploaden van een grote hoeveelheid gegevens die worden gevolgd door incrementele overdrachten doen. Data Box voor de bulk-overdracht in offline modus (basiswaarde) en Data Box-Gateway gebruiken voor incrementele overdrachten (doorlopende feed) via het netwerk.

### <a name="seed-the-data-with-data-box"></a>De gegevens worden met de Data Box

Volg deze stappen voor het kopiëren van de gegevens naar Data Box en uploadt naar Azure Storage.

1. [Uw Data Box bestellen](/azure/databox/data-box-deploy-ordered.md).
2. [Instellen van uw Data Box](/azure/databox/data-box-deploy-set-up.md).
3. [Gegevens kopiëren naar Data Box via SMB](/azure/databox/data-box-deploy-copy-data.md).
4. [Resultaat van de Data Box, controleert u of het uploaden van gegevens naar Azure](/azure/databox/data-box-deploy-picked-up.md).
5. Nadat het uploaden van gegevens naar Azure voltooid is, moeten alle gegevens in Azure storage-containers. In de storage-account voor Data Box, gaat u naar de container voor Blob (en bestand) om ervoor te zorgen dat alle gegevens worden gekopieerd. Noteer de containernaam van de als u deze naam later gebruikt. Bijvoorbeeld, in de volgende schermafbeelding `databox` container wordt gebruikt voor de incrementele overdracht.

    ![Container met de gegevens op Data Box](media/data-box-gateway-use-cases/data-container1.png)

Deze overdracht bulksgewijs is de eerste seeding fase voltooid.

### <a name="ongoing-feed-with-data-box-gateway"></a>Doorlopende met gegevens in het Gateway-feed

Volg deze stappen voor continue opname door Data Box-Gateway.

1. Maak een cloud-share op Data Box-Gateway. Deze share worden automatisch alle gegevens geüpload naar de Azure Storage-account. Ga naar **Shares** in uw Data Box-Gateway-resource en klik op **+ bestandsshare toevoegen**.

    ![Klik op + bestandsshare toevoegen](media/data-box-gateway-use-cases/add-share1.png)

2. Zorg ervoor dat deze share worden toegewezen aan de container met de geseede gegevens. Voor **Selecteer blob-container**, kiest u **gebruik bestaande** en blader naar de container waar de gegevens van Data Box is overgedragen.

    ![De instellingen voor delen](media/data-box-gateway-use-cases/share-settings-select-existing-container1.png)

3. Nadat de share is gemaakt, vernieuwt u de share. Met deze bewerking wordt de on-premises-share met de inhoud van de Azure vernieuwd.

    ![Share vernieuwen](media/data-box-gateway-use-cases/refresh-share1.png)

    Wanneer de share is gesynchroniseerd, wordt de Data Box-Gateway de incrementele wijzigingen uploaden als de bestanden zijn gewijzigd op de client.

## <a name="next-steps"></a>Volgende stappen

- Lees de [Systeemvereisten voor Data Box Gateway](data-box-gateway-system-requirements.md).
- Begrijp de [limieten voor Data Box Gateway](data-box-gateway-limits.md).
- Implementeer [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) in de Azure Portal.