---
title: Beheren van uw StorSimple-opslagaccount | Microsoft Docs
description: Legt uit hoe u kunt de pagina configureren van de StorSimple Manager kunt toevoegen, bewerken, verwijderen of de beveiligingssleutels voor een opslagaccount draaien.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 93207c40-e0eb-489e-8724-59fb94907081
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/29/2016
ms.author: v-sharos
ms.openlocfilehash: 68b767c9c93f2daff476a21029b9813f347590b5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>De StorSimple Manager-service gebruiken voor het beheren van uw storage-account
## <a name="overview"></a>Overzicht
De **configureren** pagina geeft alle algemene service-parameters die kunnen worden gemaakt in de StorSimple Manager-service. Deze parameters kunnen worden toegepast op alle apparaten die zijn verbonden met de service en omvatten:

* Opslagaccounts 
* Bandbreedte-sjablonen 
* Access control-records 

Deze zelfstudie wordt uitgelegd hoe u kunt de **configureren** pagina als u wilt toevoegen, bewerken of verwijderen van de storage-accounts of de beveiligingssleutels voor een opslagaccount draaien.

 ![Pagina configureren](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Storage-accounts bevatten de referenties die het apparaat gebruikt voor toegang tot uw storage-account bij uw serviceprovider voor de cloud. Voor Microsoft Azure storage-accounts zijn referenties zoals de accountnaam en de primaire toegangssleutel. 

Op de **configureren** pagina alle opslagaccounts die zijn gemaakt voor de facturering abonnement worden weergegeven in tabelvorm met de volgende gegevens:

* **Naam** – de unieke naam toegewezen aan het account wanneer deze is gemaakt.
* **SSL ingeschakeld** : of het SSL is ingeschakeld en apparaat-naar-cloud-communicatie is via het beveiligde kanaal.
* **Gebruikt door** : het aantal volumes met behulp van het opslagaccount.

De meest algemene taken gerelateerd aan de storage-accounts die kunnen worden uitgevoerd op de **configureren** pagina:

* Een opslagaccount toevoegen 
* Storage-account bewerken 
* Een opslagaccount verwijderen 
* Sleutel rotatie van de storage-accounts 

## <a name="types-of-storage-accounts"></a>Typen opslagaccounts
Er zijn drie typen opslagaccounts die kunnen worden gebruikt met uw StorSimple-apparaat.

* **Automatisch gegenereerde opslagaccounts** – zoals de naam al aangeeft, dit type opslagaccount wordt automatisch gegenereerd als de service is gemaakt. Zie voor meer informatie over hoe dit opslagaccount is gemaakt, [stap 1: een nieuwe service maken](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) in [uw on-premises StorSimple-apparaat implementeren](storsimple-deployment-walkthrough.md). 
* **Storage-accounts in het serviceabonnement** : dit zijn de Azure storage-accounts die gekoppeld aan hetzelfde abonnement als die van de service zijn. Zie voor meer informatie over hoe deze storage accounts worden gemaakt, [over Azure Storage-Accounts](../storage/common/storage-create-storage-account.md). 
* **Storage-accounts buiten het serviceabonnement** : dit zijn de Azure storage-accounts die niet zijn gekoppeld aan uw service en waarschijnlijk bestonden voordat de service is gemaakt.

## <a name="add-a-storage-account"></a>Een opslagaccount toevoegen
U kunt een opslagaccount toevoegen door te geven van een unieke, beschrijvende naam en toegang tot de referenties die zijn gekoppeld aan het opslagaccount (met de opgegeven cloud serviceprovider). U hebt ook de optie voor het inschakelen van de secure sockets layer (SSL)-modus voor het maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw apparaat en de cloud.

U kunt meerdere accounts maken voor een bepaalde cloud serviceprovider. Vergeet echter nadat een opslagaccount is gemaakt, kunt u de cloudserviceprovider niet wijzigen.

Terwijl het storage-account wordt opgeslagen, wordt de service probeert om te communiceren met uw serviceprovider voor de cloud. De referenties en de toegang tot materiaal dat u hebt opgegeven, wordt op dit moment worden geverifieerd. Een opslagaccount wordt alleen gemaakt als de verificatie is geslaagd. Als de verificatie is mislukt, wordt een foutbericht weergegeven worden weergegeven.

Resource Manager storage-accounts in Azure portal hebt gemaakt, worden ook ondersteund met StorSimple. De Resource Manager-opslagaccounts wordt niet weergegeven in de vervolgkeuzelijst voor selectie wanneer bij het maken van een volumecontainer, alleen de storage-accounts die zijn gemaakt in de klassieke Azure portal weergegeven. Storage-accounts voor Resource Manager moet worden toegevoegd met de procedure voor het toevoegen van een opslagaccount die hieronder worden beschreven.

> [!NOTE]
> De procedure voor het toevoegen van een opslagaccount is op basis van de StorSimple-softwareversie die u gebruikt. Zorg ervoor dat de juiste procedure voor uw StorSimple-versie.
> 
> 

[!INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[!INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Storage-account bewerken
U kunt een opslagaccount die wordt gebruikt door een volumecontainer bewerken. Als u een opslagaccount die momenteel in gebruik hebt bewerkt, is het enige veld beschikbaar om te wijzigen in de toegangssleutel voor het opslagaccount. U kunt opgeven van de nieuwe toegangssleutel voor opslag en de bijgewerkte instellingen opslaan.

#### <a name="to-edit-a-storage-account"></a>Storage-account bewerken
1. Selecteer uw service op de pagina service aanvoer, dubbelklikt u op de servicenaam en klik op **configureren**.
2. Klik op **Storage-Accounts toevoegen/bewerken**.
3. In de **Storage-Accounts toevoegen/bewerken** in het dialoogvenster:
   
   1. In de vervolgkeuzelijst van **Opslagaccounts**, kiest u een bestaand account die u wilt wijzigen. Dit kan betekenen dat de storage-accounts die zijn automatisch worden gegenereerd wanneer de service is gemaakt.
   2. Indien nodig, kunt u de **SSL-modus inschakelen** selectie.
   3. U kunt kiezen om te roteren van de toegangssleutels van uw storage-account. Zie [sleutel rotatie van de storage-accounts](#key-rotation-of-storage-accounts) voor meer informatie over het uitvoeren van de sleutel worden gedraaid.
   4. Klik op het vinkje ![vinkje](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) de instellingen op te slaan. De instellingen worden bijgewerkt op de **configureren** pagina. Klik op **opslaan** de bijgewerkte instellingen op te slaan.
      
      ![Storage-account bewerken](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)

## <a name="delete-a-storage-account"></a>Een opslagaccount verwijderen
> [!IMPORTANT]
> U kunt een opslagaccount alleen verwijderen als deze niet wordt gebruikt door een volumecontainer. Als een opslagaccount wordt gebruikt door een volumecontainer, verwijdert u eerst de volumecontainer en verwijder vervolgens het bijbehorende opslagaccount.
> 
> 

#### <a name="to-delete-a-storage-account"></a>Een opslagaccount verwijderen
1. Selecteer uw service op de openingspagina StorSimple Manager-service, dubbelklikt u op de servicenaam en klik op **configureren**.
2. In de lijst in tabelvorm van storage-accounts, de muisaanwijzer op het account dat u wilt verwijderen.
3. Een verwijderpictogram (**x**) wordt weergegeven in de extreme rechterkolom voor dat opslagaccount. Klik op de **x** pictogram als de referenties wilt verwijderen.
4. Wanneer u om bevestiging gevraagd, klikt u op **Ja** om door te gaan met het verwijderen. In de lijst in tabelvorm worden bijgewerkt om de wijzigingen weer te geven.

## <a name="key-rotation-of-storage-accounts"></a>Sleutel rotatie van de storage-accounts
Uit veiligheidsoverwegingen is sleutel rotatie vaak een vereiste in datacenters. 

> [!NOTE]
> De volgende informatie voor de sleutel draaien en de rotatieprocedure van toepassing op Microsoft Azure storage-accounts alleen. Als u een andere cloud serviceprovider gebruikt, kunt u toegangscodes voor opslag via dashboard van de provider kunt beheren.
> 
> 

Elke Microsoft Azure-abonnement kan een of meer gekoppelde storage-accounts hebben. De toegang tot deze accounts wordt bepaald door het abonnement en toegangssleutels voor elke storage-account. 

Wanneer u een opslagaccount maakt, genereert Microsoft Azure twee 512-bits opslagtoegangssleutels die worden gebruikt voor verificatie wanneer het opslagaccount wordt geopend. Met twee toegangssleutels voor opslag kunt u opnieuw genereren van sleutels met zonder onderbreking van uw opslagservice of toegang tot deze service. De sleutel die momenteel in gebruik is de *primaire* en de back-sleutel wordt aangeduid als de *secundaire* sleutel. Een van deze twee sleutels moet worden opgegeven bij uw Microsoft Azure StorSimple-apparaat heeft toegang uw serviceprovider voor cloud-opslag tot.

## <a name="what-is-key-rotation"></a>Wat is de belangrijkste wisselen?
Toepassingen gebruiken doorgaans slechts één van de sleutels voor toegang tot uw gegevens. Na een bepaalde periode, kunt u uw toepassingen overschakelen via op het gebruik van de tweede sleutel hebben. Nadat u uw toepassingen op de secundaire sleutel zijn overgeschakeld, kunt u buiten gebruik stellen van de eerste sleutel en vervolgens een nieuwe sleutel te genereren. Door de twee sleutels op deze manier kan uw toepassingen toegang tot de gegevens zonder enige uitvaltijd.

De opslagaccountsleutels zijn altijd in de service in versleutelde vorm opgeslagen. Deze kunnen echter worden hersteld via de StorSimple Manager-service. De service kan de primaire en secundaire sleutel voor alle opslagaccounts verkrijgen tot hetzelfde abonnement, met inbegrip van accounts die zijn gemaakt in de Storage-service, evenals de opslag standaard accounts die worden gegenereerd wanneer de StorSimple Manager-service voor het eerst werd gemaakt. De StorSimple Manager-service wordt altijd deze sleutels van de klassieke Azure portal ophalen en deze vervolgens opslaan in een versleutelde manier.

## <a name="rotation-workflow"></a>Werkstroom van de draaihoek
Een Microsoft Azure-beheerder kan genereren of wijzigen van de primaire of secundaire sleutel door rechtstreeks toegang hebben tot het opslagaccount (via de Microsoft Azure Storage-service). De StorSimple Manager-service heeft deze wijziging niet automatisch weergegeven.

Om te informeren over de StorSimple Manager-service van de wijziging, moet u toegang tot de StorSimple Manager-service toegang tot het opslagaccount, en vervolgens de primaire of secundaire sleutel (afhankelijk van welk is gewijzigd) te synchroniseren. De service vervolgens de meest recente sleutel opgehaald, versleutelt de sleutels en verzendt de versleutelde sleutel naar het apparaat.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Synchroniseren van sleutels voor opslagaccounts in hetzelfde abonnement als de service (alleen Azure)
1. Op de **Services** pagina, klikt u op de **configureren** tabblad.
2. Klik op **Storage-Accounts toevoegen/bewerken**.
3. In het dialoogvenster het volgende doen:
   
   1. Selecteer het opslagaccount met de sleutel die u wilt synchroniseren. De opslagaccountsleutels zijn versleuteld wanneer ze worden weergegeven.
   2. In de StorSimple Manager-service moet u de sleutel die eerder is gewijzigd in de Microsoft Azure Storage-service wordt bijgewerkt. Als de primaire toegangssleutel is gewijzigd (opnieuw gegenereerd), klikt u op **primaire sleutel synchroniseren**. Als de secundaire sleutel is gewijzigd, klikt u op **secundaire sleutel synchroniseren**.
      
      ![sleutels synchroniseren](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Sleutels voor opslagaccounts buiten het serviceabonnement synchroniseren
1. Op de **Services** pagina, klikt u op de **configureren** tabblad.
2. Klik op **Storage-Accounts toevoegen/bewerken**.
3. In het dialoogvenster het volgende doen:
   
   1. Selecteer het opslagaccount met de toegangssleutel die u wilt bijwerken.
   2. U moet de toegangssleutel voor opslag in de StorSimple Manager-service bijwerken. In dit geval ziet u de toegangssleutel voor opslag. Voer de nieuwe sleutel in de **toegangssleutel voor Opslagaccount**vak y. 
   3. Sla uw wijzigingen op. Uw toegangssleutel voor opslagaccount moet nu worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple security](storsimple-security.md).
* Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

