---
title: StorSimple Virtual Array opslagaccountreferenties beheren | Microsoft Docs
description: Wordt uitgelegd hoe u kunt de pagina configureren van de StorSimple Device Manager toevoegen, bewerken, verwijderen of de sleutels voor de referenties van het opslagaccount dat is gekoppeld aan de StorSimple Virtual Array draaien.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 234bf8bb-d5fe-40be-9d25-721d7482bc3b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: d2b6b7a5d2baf9e46cca18aec97f31beda1e2a38
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736730"
---
# <a name="use-storsimple-device-manager-to-manage-storage-account-credentials-for-storsimple-virtual-array"></a>Gebruik StorSimple Device Manager voor het beheren van referenties van het opslagaccount voor StorSimple Virtual Array

## <a name="overview"></a>Overzicht
De **configuratie** sectie van de StorSimple Device Manager serviceblade van uw StorSimple Virtual Array geeft de algemene serviceparameters die kunnen worden gemaakt in de StorSimple Manager-service. Deze parameters kunnen worden toegepast op alle apparaten die zijn verbonden met de service, en omvatten:

* Opslagaccountreferenties
* Access control records
  
  ![Device Manager-Service-dashboard](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccts-dashboard.png)  

In deze zelfstudie wordt uitgelegd hoe u kunt toevoegen, bewerken of verwijderen van referenties van het opslagaccount voor uw StorSimple Virtual Array. De informatie in deze zelfstudie is alleen van toepassing op StorSimple Virtual Array. Zie voor meer informatie over het beheren van storage-accounts in de 8000-serie [de StorSimple Manager-service gebruiken voor het beheren van uw opslagaccount](storsimple-manage-storage-accounts.md).

Referenties voor het opslagaccount bevatten de referenties die het apparaat gebruikt voor toegang tot uw storage-account met uw cloudserviceprovider. Dit zijn referenties, zoals de accountnaam en de primaire toegangssleutel voor Microsoft Azure storage-accounts.

Op de **opslagaccountreferenties** blade, alle storage-accountreferenties die worden gemaakt voor het factureringsabonnement worden weergegeven in tabelvorm met de volgende gegevens:

* **Naam** : de unieke naam die aan het account wordt toegewezen wanneer deze is gemaakt.
* **SSL is ingeschakeld** , of het SSL is ingeschakeld en communicatie van apparaat-naar-cloud is via het beveiligde kanaal.
  
  ![Configuratiesectie](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccountcredentials-blade.png)

De meest algemene taken met betrekking tot de referenties van het opslagaccount die kunnen worden uitgevoerd op de **opslagaccountreferenties** blade zijn:

* Een opslagaccountreferentie toevoegen
* Een opslagaccountreferentie bewerken
* Een opslagaccountreferentie verwijderen

## <a name="types-of-storage-account-credentials"></a>Typen opslagaccountreferenties
Er zijn drie soorten referenties van het opslagaccount die kunnen worden gebruikt met uw StorSimple-apparaat.

* **Automatisch gegenereerde opslagaccountreferenties** – zoals de naam al aangeeft, dit type referentie voor het opslagaccount automatisch wordt gegenereerd wanneer de service is gemaakt. Zie voor meer informatie over hoe deze referentie voor het opslagaccount is gemaakt, [maken van een nieuwe service](storsimple-virtual-array-manage-service.md#create-a-service).
* **referenties van het opslagaccount in de serviceabonnement** : dit zijn de Azure storage-accountreferenties die gekoppeld aan hetzelfde abonnement als die van de service zijn. Zie voor meer informatie over hoe deze storage-accountreferenties zijn gemaakt, [over Azure Storage-Accounts](../storage/common/storage-create-storage-account.md).
* **opslagaccountreferenties buiten het serviceabonnement** : dit zijn de referenties van het Azure storage-account die niet zijn gekoppeld aan uw service en waarschijnlijk aanwezig waren voordat de service is gemaakt.

## <a name="add-a-storage-account-credential"></a>Een opslagaccountreferentie toevoegen
U kunt een opslagaccountreferentie toevoegen aan de configuratie van uw StorSimple Device Manager-service door een unieke beschrijvende naam en -referenties die zijn gekoppeld aan de storage-account te geven. U hebt ook de optie van het inschakelen van de modus voor secure sockets layer (SSL) voor het maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw apparaat en de cloud.

U kunt meerdere accounts maken voor een bepaalde cloudserviceprovider. Terwijl de opslagaccountreferentie wordt opgeslagen, wordt de service probeert te communiceren met uw cloudserviceprovider. De referenties en de toegang tot materiaal dat u hebt opgegeven worden op dit moment geverifieerd. Een opslagaccountreferentie wordt alleen gemaakt als de verificatie is geslaagd. Als de verificatie is mislukt, klikt u vervolgens een geschikt foutbericht weergegeven.

Gebruik de volgende procedures om toe te voegen van Azure storage-accountreferenties:

* Als u wilt een opslagaccountreferentie toevoegen waarvoor hetzelfde Azure-abonnement als de Device Manager-service
* Een Azure storage-accountreferenties die buiten het Device Manager serviceabonnement toevoegen

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Als u wilt een opslagaccountreferentie toevoegen waarvoor hetzelfde Azure-abonnement als de Device Manager-service

1. Navigeer naar uw Device Manager-service en selecteer en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie.
3. Klik op **Add**.
4. In de **een storage-account toevoegen** blade, doet u het volgende:
   
    1. Voor **abonnement**, selecteer **huidige**.
    2. Geef de naam van uw Azure storage-account.
    3. Selecteer **inschakelen** te maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple-apparaat en de cloud. Selecteer **uitschakelen** alleen als u binnen een privécloud werkt.
    4. Klik op **Add**. U krijgt een melding nadat het opslagaccount is gemaakt.<br></br>
   
        ![Een bestaande opslagaccountreferentie toevoegen](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

#### <a name="to-add-an-azure-storage-account-credential-that-is-outside-of-the-device-manager-service-subscription"></a>Een Azure storage-accountreferenties die buiten het Device Manager serviceabonnement toevoegen

1. Navigeer naar uw Device Manager-service en selecteer en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie. Dit geeft een lijst van alle bestaande referenties van het opslagaccount dat is gekoppeld aan de StorSimple Device Manager-service.
3. Klik op **Add**.
4. In de **een storage-account toevoegen** blade, doet u het volgende:
   
    1. Voor **abonnement**, selecteer **andere**.
   
    2. Geef de naam van de referenties van uw Azure storage-account.
   
    3. In de **toegangssleutel voor Opslagaccount** tekst vak, de primaire toegangssleutel voor uw Azure storage-accountreferenties opgeven. Voor deze sleutel gaat u naar de Azure Storage-service, selecteert u de referenties van uw storage-account en op **accountsleutels beheren**. U kunt nu de primaire toegangssleutel kopiëren.
   
    4. Als SSL, klikt u op de **inschakelen** om te maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple Device Manager-service en de cloud. Klik op de **uitschakelen** knop alleen als u binnen een privécloud werkt.
   
    5. Klik op **Add**. U krijgt een melding nadat de opslagaccountreferentie is gemaakt.

5. De referentie voor het zojuist gemaakte opslagaccount wordt weergegeven op de serviceblade configureren StorSimple-Apparaatbeheerfunctie onder **opslagaccountreferenties**.
   
    ![Een opslagaccountreferentie buiten het Device Manager serviceabonnement toevoegen](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-outside-storageacct.png)

## <a name="edit-a-storage-account-credential"></a>Een opslagaccountreferentie bewerken
U kunt een opslagaccountreferentie wordt gebruikt door uw apparaat kunt bewerken. Als u een opslagaccountreferentie die momenteel in gebruik hebt bewerkt, worden de velden die beschikbaar zijn om te wijzigen de toegangssleutel en de SSL-modus voor de opslagaccountreferentie. U kunt de nieuwe toegangssleutel voor opslag opgeven of wijzigen de **inschakelen SSL-modus** selectie en sla de bijgewerkte instellingen.

#### <a name="to-edit-a-storage-account-credential"></a>Een opslagaccountreferentie bewerken
1. Navigeer naar uw Device Manager-service en selecteer en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie. Dit geeft een lijst van alle bestaande referenties van het opslagaccount dat is gekoppeld aan de StorSimple Device Manager-service.
3. Selecteer in de lijst in tabelvorm van referenties van het opslagaccount en dubbelklik op het account dat u wilt wijzigen.
4. In de opslagaccountreferentie **eigenschappen** blade, doet u het volgende:
   
   1. Indien nodig, kunt u de **SSL inschakelen** Modusselectie.
   2. U kunt uw opslagtoegangssleutels account referentie opnieuw genereren. Zie voor meer informatie, [opnieuw genereren van sleutels voor de opslagaccount](../storage/common/storage-account-manage.md#access-keys). Geef de nieuwe referentie voor opslagaccountsleutel. Dit is de primaire toegangssleutel voor een Azure storage-account.
   3. Klik op **opslaan** aan de bovenkant van de **eigenschappen** blade de instellingen op te slaan. De instellingen zijn bijgewerkt op de **opslagaccountreferenties** blade.
      
      ![Een opslagaccountreferentie bewerken](./media/storsimple-virtual-array-manage-storage-accounts/ova-edit-storageacct.png)

## <a name="delete-a-storage-account-credential"></a>Een opslagaccountreferentie verwijderen
> [!IMPORTANT]
> U kunt een opslagaccountreferentie alleen verwijderen als deze zich niet in gebruik. Als een opslagaccountreferentie gebruikt wordt, wordt u gewaarschuwd.
> 
> 

#### <a name="to-delete-a-storage-account-credential"></a>Een opslagaccountreferentie verwijderen
1. Navigeer naar uw Device Manager-service en selecteer en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie. Dit geeft een lijst van alle bestaande referenties van het opslagaccount dat is gekoppeld aan de StorSimple Device Manager-service.
3. Selecteer in de lijst in tabelvorm van referenties van het opslagaccount en dubbelklik op het account dat u wilt verwijderen.
4. In de opslagaccountreferentie **eigenschappen** blade, doet u het volgende:
   
   1. Klik op **verwijderen** als de referenties wilt verwijderen.
   2. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja** om door te gaan met het verwijderen. De lijst in tabelvorm is bijgewerkt om de wijzigingen weer te geven.
      
      ![Een opslagaccountreferentie verwijderen](./media/storsimple-virtual-array-manage-storage-accounts/ova-del-storageacct.png)

## <a name="synchronizing-storage-account-credential-keys"></a>Referentie van opslagaccountsleutels synchroniseren
Uit veiligheidsoverwegingen is sleutelroulatie vaak een vereiste in datacenters. De beheerder van een Microsoft Azure kunt genereren of de primaire of secundaire sleutel wijzigen door rechtstreeks toegang hebben tot de referenties van de storage-account (via de Microsoft Azure Storage-service). De StorSimple Device Manager-service ziet deze wijziging niet automatisch.

Om te informeren over de StorSimple Device Manager-service van de wijziging, moet u toegang tot de service StorSimple Device Manager toegang tot de opslagaccountreferentie en synchroniseer vervolgens de primaire of secundaire sleutel (afhankelijk van welk is gewijzigd). De service vervolgens haalt de meest recente sleutel, versleutelt de sleutels, en verzendt de versleutelde sleutel naar het apparaat.

#### <a name="to-synchronize-keys-for-storage-account-credentials-in-the-same-subscription-as-the-service-azure-only"></a>Om te synchroniseren van sleutels voor de referenties van het opslagaccount in hetzelfde abonnement als de service (alleen voor Azure)
1. Selecteer op de blade van de landingspagina service uw service, dubbelklikt u op de naam van de service, en klik vervolgens in de **configuratie** sectie, klikt u op **opslagaccountreferenties**.
2. Op de **opslagaccountreferenties** blade in de lijst met referenties van het Opslagaccount, selecteer het opslagaccount dat referenties waarvan sleutels die u wilt synchroniseren.
3. In de **eigenschappen** blade voor de referentie voor het geselecteerde opslagaccount, doet u het volgende:
   
    1. Klik op **meer**, en klik vervolgens op **Synchroniseer de toegangssleutel**.
   
    2. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **sleutel synchroniseren** om de synchronisatie te voltooien.
    
4. In de StorSimple Device Manager-service moet u de sleutel die eerder is gewijzigd in de Microsoft Azure Storage-service wordt bijgewerkt. In de **opslagaccountsleutel synchroniseren** blade, als de primaire toegangssleutel is gewijzigd (opnieuw gegenereerd), klikt u op de primaire en klik vervolgens op **sleutel synchroniseren**. Als de secundaire sleutel is gewijzigd, klikt u op **secundaire**, en klik vervolgens op **sleutel synchroniseren**.
   
    ![Toegangssleutel synchroniseren](./media/storsimple-virtual-array-manage-storage-accounts/ova-sync-acess-key.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

