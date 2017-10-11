---
title: Beheren van virtuele StorSimple-matrix opslagaccountreferenties | Microsoft Docs
description: Legt uit hoe u kunt de pagina StorSimple Apparaatbeheer configureren wilt toevoegen, bewerken, verwijderen of de beveiligingssleutels voor opslagaccountreferenties die zijn gekoppeld aan de virtuele StorSimple-matrix draaien.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 234bf8bb-d5fe-40be-9d25-721d7482bc3b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: a4ce2d329d0e1399cffaf886adf2b95e34b9cd7b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-storsimple-device-manager-to-manage-storage-account-credentials-for-storsimple-virtual-array"></a>Het StorSimple-Apparaatbeheer gebruiken voor het beheren van opslagaccountreferenties voor virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht
De **configuratie** sectie van de StorSimple-apparaat Manager service-blade van uw virtuele StorSimple-matrix geeft de algemene service-parameters die kunnen worden gemaakt in de StorSimple Manager-service. Deze parameters kunnen worden toegepast op alle apparaten die zijn verbonden met de service en omvatten:

* Opslagaccountreferenties
* Access control-records
  
  ![Apparaatbeheer servicedashboard](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccts-dashboard.png)  

Deze zelfstudie wordt uitgelegd hoe u kunt toevoegen, bewerken of verwijderen van opslagaccountreferenties voor uw virtuele StorSimple-matrix. De informatie in deze zelfstudie is alleen van toepassing op de virtuele StorSimple-matrix. Zie voor meer informatie over het beheren van de storage-accounts in 8000 serie [de StorSimple Manager-service gebruiken voor het beheren van uw opslagaccount](storsimple-manage-storage-accounts.md).

Opslagaccountreferenties bevatten de referenties die het apparaat gebruikt voor toegang tot uw storage-account bij uw serviceprovider voor de cloud. Voor Microsoft Azure storage-accounts zijn referenties zoals de accountnaam en de primaire toegangssleutel.

Op de **opslagaccountreferenties** blade, alle opslagaccountreferenties die zijn gemaakt voor de facturering abonnement worden weergegeven in tabelvorm met de volgende gegevens:

* **Naam** – de unieke naam toegewezen aan het account wanneer deze is gemaakt.
* **SSL ingeschakeld** : of het SSL is ingeschakeld en apparaat-naar-cloud-communicatie is via het beveiligde kanaal.
  
  ![Configuratiesectie](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccountcredentials-blade.png)

De meest algemene taken gerelateerd aan de opslagaccountreferenties die kunnen worden uitgevoerd op de **opslagaccountreferenties** blade zijn:

* Een opslagaccountreferentie toevoegen
* Een referentie storage-account bewerken
* Een referentie storage-account verwijderen

## <a name="types-of-storage-account-credentials"></a>Typen opslagaccountreferenties
Er zijn drie soorten opslagaccountreferenties die kunnen worden gebruikt met uw StorSimple-apparaat.

* **Automatisch gegenereerde opslagaccountreferenties** – zoals de naam al aangeeft, dit type van de referentie voor storage-account automatisch wordt gegenereerd wanneer de service is gemaakt. Zie voor meer informatie over hoe deze referentie storage-account is gemaakt, [een nieuwe service maken](storsimple-virtual-array-manage-service.md#create-a-service).
* **opslagaccountreferenties in het serviceabonnement** : dit zijn de Azure storage-accountreferenties die gekoppeld aan hetzelfde abonnement als die van de service zijn. Zie voor meer informatie over hoe deze storage-accountreferenties zijn gemaakt, [over Azure Storage-Accounts](../storage/common/storage-create-storage-account.md).
* **opslagaccountreferenties buiten het serviceabonnement** : dit zijn de Azure storage-accountreferenties die niet zijn gekoppeld aan uw service en waarschijnlijk bestonden voordat de service is gemaakt.

## <a name="add-a-storage-account-credential"></a>Een opslagaccountreferentie toevoegen
U kunt een referentie storage-account toevoegen aan uw serviceconfiguratie Manager voor StorSimple-apparaat door een unieke, beschrijvende naam en -referenties die zijn gekoppeld aan het storage-account te geven. U hebt ook de optie voor het inschakelen van de secure sockets layer (SSL)-modus voor het maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw apparaat en de cloud.

U kunt meerdere accounts maken voor een bepaalde cloud serviceprovider. Terwijl de storage-accountreferenties wordt opgeslagen, wordt de service probeert om te communiceren met uw serviceprovider voor de cloud. De referenties en de toegang tot materiaal dat u hebt opgegeven worden op dit moment geverifieerd. Een referentie storage-account wordt alleen gemaakt als de verificatie is geslaagd. Als de verificatie is mislukt, wordt een foutbericht weergegeven weergegeven.

Gebruik de volgende procedures om toe te voegen aan Azure storage-accountreferenties:

* Toevoegen van een referentie storage-account met de dezelfde Azure-abonnement als de service voor Apparaatbeheer
* Toevoegen van een Azure storage-account-referentie die buiten het serviceabonnement Apparaatbeheer

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Toevoegen van een referentie storage-account met de dezelfde Azure-abonnement als de service voor Apparaatbeheer

1. Navigeer naar uw service Apparaatbeheer, selecteert en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie.
3. Klik op **Add**.
4. In de **een opslagaccount toevoegen** blade het volgende doen:
   
    1. Voor **abonnement**, selecteer **huidige**.
    2. Geef de naam van uw Azure storage-account.
    3. Selecteer **inschakelen** voor het maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple-apparaat en de cloud. Selecteer **uitschakelen** alleen als u in een privécloud werkt.
    4. Klik op **Add**. U wordt gewaarschuwd nadat het opslagaccount is gemaakt.<br></br>
   
        ![Een bestaande referentie voor de storage-account toevoegen](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

#### <a name="to-add-an-azure-storage-account-credential-that-is-outside-of-the-device-manager-service-subscription"></a>Toevoegen van een Azure storage-account-referentie die buiten het serviceabonnement Apparaatbeheer

1. Navigeer naar uw service Apparaatbeheer, selecteert en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie. Hiermee worden alle bestaande opslagaccountreferenties die zijn gekoppeld aan de service Manager voor StorSimple-apparaat.
3. Klik op **Add**.
4. In de **een opslagaccount toevoegen** blade het volgende doen:
   
    1. Voor **abonnement**, selecteer **andere**.
   
    2. Geef de naam van uw Azure storage-accountreferenties.
   
    3. In de **toegangssleutel voor Opslagaccount** tekst vak, de primaire toegangssleutel voor uw Azure storage-accountreferenties opgeven. Als u deze sleutel, gaat u naar de Azure Storage-service, selecteer uw storage-referentie voor account en op **account sleutels beheren**. U kunt nu de primaire toegangssleutel kopiëren.
   
    4. Voor het inschakelen van SSL, klikt u op de **inschakelen** om te maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple-apparaat Manager-service en de cloud. Klik op de **uitschakelen** knop alleen als u in een privécloud werkt.
   
    5. Klik op **Add**. U wordt gewaarschuwd nadat de storage-account-referentie is gemaakt.

5. De zojuist gemaakte storage-accountreferenties wordt weergegeven op de blade van de service StorSimple configureren Apparaatbeheer onder **opslagaccountreferenties**.
   
    ![Een referentie storage account buiten het Apparaatbeheer serviceabonnement toevoegen](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-outside-storageacct.png)

## <a name="edit-a-storage-account-credential"></a>Een referentie storage-account bewerken
U kunt een storage accountreferentie die wordt gebruikt door uw apparaat kunt bewerken. Als u een referentie storage-account is momenteel in gebruik hebt bewerkt, zijn de velden beschikbaar om te wijzigen de toegangssleutel en de SSL-modus voor de storage-accountreferenties. U kunt de nieuwe toegangssleutel voor opslag opgeven of wijzigen de **SSL inschakelen modus** selectie en de bijgewerkte instellingen op te slaan.

#### <a name="to-edit-a-storage-account-credential"></a>Een referentie storage-account bewerken
1. Navigeer naar uw service Apparaatbeheer, selecteert en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie. Hiermee worden alle bestaande opslagaccountreferenties die zijn gekoppeld aan de service Manager voor StorSimple-apparaat.
3. Selecteer in de lijst in tabelvorm van opslagaccountreferenties en dubbelklik op het account dat u wilt wijzigen.
4. In de storage-accountreferenties **eigenschappen** blade het volgende doen:
   
   1. Indien nodig, kunt u de **SSL inschakelen** Modusselectie.
   2. U kunt uw opslagtoegangssleutels account referentie opnieuw genereren. Zie voor meer informatie [opnieuw genereren van sleutels voor de opslagaccount](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys). De sleutel referentie van nieuwe opslagaccount op te geven. Dit is de primaire toegangssleutel voor een Azure storage-account.
   3. Klik op **opslaan** boven aan de **eigenschappen** blade de instellingen op te slaan. De instellingen zijn bijgewerkt op de **opslagaccountreferenties** blade.
      
      ![Een referentie storage-account bewerken](./media/storsimple-virtual-array-manage-storage-accounts/ova-edit-storageacct.png)

## <a name="delete-a-storage-account-credential"></a>Een referentie storage-account verwijderen
> [!IMPORTANT]
> U kunt een referentie storage-account alleen verwijderen als deze zich niet in gebruik. Als u een referentie storage-account wordt gebruikt, wordt u gewaarschuwd.
> 
> 

#### <a name="to-delete-a-storage-account-credential"></a>Een referentie storage-account verwijderen
1. Navigeer naar uw service Apparaatbeheer, selecteert en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie. Hiermee worden alle bestaande opslagaccountreferenties die zijn gekoppeld aan de service Manager voor StorSimple-apparaat.
3. Selecteer in de lijst in tabelvorm van opslagaccountreferenties en dubbelklik op het account dat u wilt verwijderen.
4. In de storage-accountreferenties **eigenschappen** blade het volgende doen:
   
   1. Klik op **verwijderen** als de referenties wilt verwijderen.
   2. Wanneer u om bevestiging gevraagd, klikt u op **Ja** om door te gaan met het verwijderen. In de lijst in tabelvorm is bijgewerkt, zodat de wijzigingen.
      
      ![Een referentie storage-account verwijderen](./media/storsimple-virtual-array-manage-storage-accounts/ova-del-storageacct.png)

## <a name="synchronizing-storage-account-credential-keys"></a>Referentie opslagaccountsleutels synchroniseren
Uit veiligheidsoverwegingen is sleutel rotatie vaak een vereiste in datacenters. Een Microsoft Azure-beheerder kan genereren of wijzigen van de primaire of secundaire sleutel door rechtstreeks toegang hebben tot de storage-accountreferenties (via de Microsoft Azure Storage-service). De StorSimple-apparaat Manager-service heeft deze wijziging niet automatisch weergegeven.

Om te informeren over de service Manager voor StorSimple-apparaat van de wijziging, moet u toegang tot de service Manager voor StorSimple-apparaat toegang tot de storage-accountreferenties en synchroniseert u de primaire of secundaire sleutel (afhankelijk van welk is gewijzigd). De service vervolgens de meest recente sleutel opgehaald, versleutelt de sleutels en verzendt de versleutelde sleutel naar het apparaat.

#### <a name="to-synchronize-keys-for-storage-account-credentials-in-the-same-subscription-as-the-service-azure-only"></a>Synchroniseren van sleutels voor opslagaccountreferenties in hetzelfde abonnement als de service (alleen Azure)
1. Selecteer uw service op de blade van de landingspagina service, dubbelklikt u op de servicenaam en klik vervolgens in de **configuratie** sectie, klikt u op **opslagaccountreferenties**.
2. Op de **opslagaccountreferenties** blade in de lijst met opslagaccountreferenties, selecteer het storage-account referenties waarvan sleutels die u wilt synchroniseren.
3. In de **eigenschappen** blade voor de referentie van de account geselecteerde opslag het volgende doen:
   
    1. Klik op **meer**, en klik vervolgens op **Sync toegangssleutel**.
   
    2. Wanneer u om bevestiging gevraagd, klikt u op **synchronisatiesleutel** om de synchronisatie te voltooien.
    
4. In de service Manager voor StorSimple-apparaat moet u de sleutel die eerder is gewijzigd in de Microsoft Azure Storage-service wordt bijgewerkt. In de **synchroniseren opslagaccountsleutel** blade als de primaire toegangssleutel is gewijzigd (opnieuw gegenereerd), klikt u op de primaire en klik vervolgens op **synchronisatiesleutel**. Als de secundaire sleutel is gewijzigd, klikt u op **secundaire**, en klik vervolgens op **synchronisatiesleutel**.
   
    ![Toegangssleutel voor synchronisatie](./media/storsimple-virtual-array-manage-storage-accounts/ova-sync-acess-key.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [beheren van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

