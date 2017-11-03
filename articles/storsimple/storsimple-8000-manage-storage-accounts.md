---
title: Uw StorSimple-opslagaccountreferenties voor Microsoft Azure StorSimple 8000 series apparaten beheren | Microsoft Docs
description: Legt uit hoe u kunt de pagina StorSimple Apparaatbeheer configureren gebruiken voor toevoegen, bewerken, verwijderen of draaien de beveiligingssleutels voor een opslagaccount.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 36058ad69ea670998b50cf9038741c294a5b79ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-storage-account-credentials"></a>De service Manager voor StorSimple-apparaat gebruiken voor het beheren van referenties van het opslagaccount

## <a name="overview"></a>Overzicht

De **configuratie** gedeelte in de blade van de service Manager voor StorSimple-apparaat bevat alle algemene service-parameters die kunnen worden gemaakt in de service Manager voor StorSimple-apparaat. Deze parameters kunnen worden toegepast op alle apparaten die zijn verbonden met de service en omvatten:

* Opslagaccountreferenties
* Bandbreedte-sjablonen 
* Access control-records 

Deze zelfstudie wordt uitgelegd hoe u wilt toevoegen, bewerken, of opslagaccountreferenties verwijderen of de beveiligingssleutels voor een opslagaccount draaien.

 ![Lijst met opslagaccountreferenties](./media/storsimple-8000-manage-storage-accounts/createnewstorageacct6.png)  

Storage-accounts bevatten de referenties die het StorSimple-apparaat gebruikt voor toegang tot uw storage-account bij uw serviceprovider voor de cloud. Voor Microsoft Azure storage-accounts zijn referenties zoals de accountnaam en de primaire toegangssleutel. 

Op de **opslagaccountreferenties** blade, alle opslagaccounts die zijn gemaakt voor de facturering abonnement worden weergegeven in tabelvorm met de volgende gegevens:

* **Naam** – de unieke naam toegewezen aan het account wanneer deze is gemaakt.
* **SSL ingeschakeld** : of het SSL is ingeschakeld en apparaat-naar-cloud-communicatie is via het beveiligde kanaal.
* **Gebruikt door** : het aantal volumes met behulp van het opslagaccount.

De meest algemene taken die betrekking hebben op de storage-accounts die kunnen worden uitgevoerd zijn:

* Een opslagaccount toevoegen 
* Storage-account bewerken 
* Een opslagaccount verwijderen 
* Sleutel rotatie van de storage-accounts 

## <a name="types-of-storage-accounts"></a>Typen opslagaccounts

Er zijn drie typen opslagaccounts die kunnen worden gebruikt met uw StorSimple-apparaat.

* **Automatisch gegenereerde opslagaccounts** – zoals de naam al aangeeft, dit type opslagaccount wordt automatisch gegenereerd als de service is gemaakt. Zie voor meer informatie over hoe dit opslagaccount is gemaakt, [stap 1: een nieuwe service maken](storsimple-8000-deployment-walkthrough-u2.md#step-1-create-a-new-service) in [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md). 
* **Storage-accounts in het serviceabonnement** : dit zijn de Azure storage-accounts die gekoppeld aan hetzelfde abonnement als die van de service zijn. Zie voor meer informatie over hoe deze storage accounts worden gemaakt, [over Azure Storage-Accounts](../storage/common/storage-create-storage-account.md). 
* **Storage-accounts buiten het serviceabonnement** : dit zijn de Azure storage-accounts die niet zijn gekoppeld aan uw service en waarschijnlijk bestonden voordat de service is gemaakt.

## <a name="add-a-storage-account"></a>Een opslagaccount toevoegen

U kunt een opslagaccount toevoegen door te geven van een unieke, beschrijvende naam en toegang tot de referenties die zijn gekoppeld aan het opslagaccount (met de opgegeven cloud serviceprovider). U hebt ook de optie voor het inschakelen van de secure sockets layer (SSL)-modus voor het maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw apparaat en de cloud.

U kunt meerdere accounts maken voor een bepaalde cloud serviceprovider. Vergeet echter nadat een opslagaccount is gemaakt, kunt u de cloudserviceprovider niet wijzigen.

Terwijl het storage-account wordt opgeslagen, wordt de service probeert om te communiceren met uw serviceprovider voor de cloud. De referenties en de toegang tot materiaal dat u hebt opgegeven, wordt op dit moment worden geverifieerd. Een opslagaccount wordt alleen gemaakt als de verificatie is geslaagd. Als de verificatie is mislukt, wordt een foutbericht weergegeven worden weergegeven.

Gebruik de volgende procedures om toe te voegen aan Azure storage-accountreferenties:

* Toevoegen van een referentie storage-account met de dezelfde Azure-abonnement als de service voor Apparaatbeheer
* Toevoegen van een Azure storage-account-referentie die buiten het serviceabonnement Apparaatbeheer

[!INCLUDE [add-a-storage-account-update2](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

#### <a name="to-add-an-azure-storage-account-credential-outside-of-the-storsimple-device-manager-service-subscription"></a>Toevoegen van een Azure storage-accountreferenties buiten het serviceabonnement StorSimple-Apparaatbeheer

1. Navigeer naar uw StorSimple-apparaat Manager-service, selecteert en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie. Hiermee worden alle bestaande opslagaccountreferenties die zijn gekoppeld aan de service Manager voor StorSimple-apparaat.
3. Klik op **Add**.
4. In de **een referentie storage-account toevoegen** blade het volgende doen:
   
    1. Voor **abonnement**, selecteer **andere**.
   
    2. Geef de naam van uw Azure storage-accountreferenties.
   
    3. In de **toegangssleutel voor Opslagaccount** tekst vak, de primaire toegangssleutel voor uw Azure storage-accountreferenties opgeven. Als u deze sleutel, gaat u naar de Azure Storage-service, selecteer uw storage-referentie voor account en op **account sleutels beheren**. U kunt nu de primaire toegangssleutel kopiëren.
   
    4. Voor het inschakelen van SSL, klikt u op de **inschakelen** om te maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple-apparaat Manager-service en de cloud. Klik op de **uitschakelen** knop alleen als u in een privécloud werkt.
   
    5. Klik op **Add**. U wordt gewaarschuwd nadat de storage-account-referentie is gemaakt.

5. De zojuist gemaakte storage-accountreferenties wordt weergegeven op de blade van de service StorSimple configureren Apparaatbeheer onder **opslagaccountreferenties**.
   


## <a name="edit-a-storage-account"></a>Storage-account bewerken

U kunt een opslagaccount die wordt gebruikt door een volumecontainer bewerken. Als u een opslagaccount die momenteel in gebruik hebt bewerkt, is het enige veld beschikbaar om te wijzigen in de toegangssleutel voor het opslagaccount. U kunt opgeven van de nieuwe toegangssleutel voor opslag en de bijgewerkte instellingen opslaan.

#### <a name="to-edit-a-storage-account"></a>Storage-account bewerken

1. Ga naar uw StorSimple-apparaatbeheerservice. Klik in de sectie **Configuratie** op **Opslagaccountreferenties**.

    ![Opslagaccountreferenties](./media/storsimple-8000-manage-storage-accounts/editstorageacct1.png)

2. In de **opslagaccountreferenties** blade in de lijst met opslagaccountreferenties, selecteren en klik op de computer die u wilt bewerken. 

3. U kunt de **SSL inschakelen** selectie. U kunt ook klikken op **meer...**  en selecteer vervolgens **toegangssleutel synchronisatie moet worden gedraaid** de toegangssleutels van uw storage-account. Ga naar [sleutel rotatie van de storage-accounts](#key-rotation-of-storage-accounts) voor meer informatie over het uitvoeren van de sleutel worden gedraaid. Nadat u de instellingen hebt gewijzigd, klikt u op **opslaan**. 

    ![Bewerkte opslagaccountreferenties opslaan](./media/storsimple-8000-manage-storage-accounts/editstorageacct3.png)

4. Klik op **Ja** als u om bevestiging wordt gevraagd. 

    ![Wijzigingen bevestigen](./media/storsimple-8000-manage-storage-accounts/editstorageacct4.png)

De instellingen worden bijgewerkt en opgeslagen voor uw opslagaccount. 

## <a name="delete-a-storage-account"></a>Een opslagaccount verwijderen

> [!IMPORTANT]
> U kunt een opslagaccount alleen verwijderen als deze niet wordt gebruikt door een volumecontainer. Als een opslagaccount wordt gebruikt door een volumecontainer, verwijdert u eerst de volumecontainer en verwijder vervolgens het bijbehorende opslagaccount.

#### <a name="to-delete-a-storage-account"></a>Een opslagaccount verwijderen

1. Ga naar uw StorSimple-apparaatbeheerservice. Klik in de sectie **Configuratie** op **Opslagaccountreferenties**.

2. In de lijst in tabelvorm van storage-accounts, de muisaanwijzer op het account dat u wilt verwijderen. Klik met de rechtermuisknop het contextmenu aanroepen en klikt u op **verwijderen**.

    ![Opslagaccountreferenties verwijderen](./media/storsimple-8000-manage-storage-accounts/deletestorageacct1.png)

3. Wanneer u om bevestiging gevraagd, klikt u op **Ja** om door te gaan met het verwijderen. In de lijst in tabelvorm worden bijgewerkt om de wijzigingen weer te geven.

    ![De verwijdering bevestigen](./media/storsimple-8000-manage-storage-accounts/deletestorageacct2.png)

## <a name="key-rotation-of-storage-accounts"></a>Sleutel rotatie van de storage-accounts

Uit veiligheidsoverwegingen is sleutel rotatie vaak een vereiste in datacenters. Elke Microsoft Azure-abonnement kan een of meer gekoppelde storage-accounts hebben. De toegang tot deze accounts wordt bepaald door het abonnement en toegangssleutels voor elke storage-account. 

Wanneer u een opslagaccount maakt, genereert Microsoft Azure twee 512-bits opslagtoegangssleutels die worden gebruikt voor verificatie wanneer het opslagaccount wordt geopend. Met twee toegangssleutels voor opslag kunt u opnieuw genereren van sleutels met zonder onderbreking van uw opslagservice of toegang tot deze service. De sleutel die momenteel in gebruik is de *primaire* en de back-sleutel wordt aangeduid als de *secundaire* sleutel. Een van deze twee sleutels moet worden opgegeven bij uw Microsoft Azure StorSimple-apparaat heeft toegang uw serviceprovider voor cloud-opslag tot.

## <a name="what-is-key-rotation"></a>Wat is de belangrijkste wisselen?

Toepassingen gebruiken doorgaans slechts één van de sleutels voor toegang tot uw gegevens. Na een bepaalde periode, kunt u uw toepassingen overschakelen via op het gebruik van de tweede sleutel hebben. Nadat u uw toepassingen op de secundaire sleutel zijn overgeschakeld, kunt u buiten gebruik stellen van de eerste sleutel en vervolgens een nieuwe sleutel te genereren. Door de twee sleutels op deze manier kan uw toepassingen toegang tot de gegevens zonder enige uitvaltijd.

De opslagaccountsleutels zijn altijd in de service in versleutelde vorm opgeslagen. Deze kunnen echter worden hersteld via de service Manager voor StorSimple-apparaat. De service kan de primaire en secundaire sleutel voor alle opslagaccounts verkrijgen tot hetzelfde abonnement, met inbegrip van accounts die zijn gemaakt in de Storage-service, evenals de opslag standaard accounts die worden gegenereerd wanneer de StorSimple-apparaat Manager-service voor het eerst werd gemaakt. De service Manager voor StorSimple-apparaat wordt altijd deze sleutels ophalen uit de klassieke Azure portal en deze vervolgens opslaan in een versleutelde manier.

## <a name="rotation-workflow"></a>Werkstroom van de draaihoek

Een Microsoft Azure-beheerder kan genereren of wijzigen van de primaire of secundaire sleutel door rechtstreeks toegang hebben tot het opslagaccount (via de Microsoft Azure Storage-service). De StorSimple-apparaat Manager-service heeft deze wijziging niet automatisch weergegeven.

Om te informeren over de service Manager voor StorSimple-apparaat van de wijziging, moet u toegang tot de service Manager voor StorSimple-apparaat toegang tot het opslagaccount, en vervolgens de primaire of secundaire sleutel (afhankelijk van welk is gewijzigd) te synchroniseren. De service vervolgens de meest recente sleutel opgehaald, versleutelt de sleutels en verzendt de versleutelde sleutel naar het apparaat.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service"></a>Sleutels voor opslagaccounts in hetzelfde abonnement als de service synchroniseren 
1. Ga naar uw StorSimple-apparaatbeheerservice. Klik in de sectie **Configuratie** op **Opslagaccountreferenties**.
2. Klik in de lijst in tabelvorm van storage-accounts, die u wilt wijzigen. 

    ![sleutels synchroniseren](./media/storsimple-8000-manage-storage-accounts/syncaccesskey1.png)

3. Klik op **... Meer** en selecteer vervolgens **toegangssleutel synchronisatie moet worden gedraaid**.   

    ![sleutels synchroniseren](./media/storsimple-8000-manage-storage-accounts/syncaccesskey2.png)

4. In de service Manager voor StorSimple-apparaat moet u de sleutel die eerder is gewijzigd in de Microsoft Azure Storage-service wordt bijgewerkt. Als de primaire toegangssleutel is gewijzigd (opnieuw gegenereerd), selecteer **primaire** sleutel. Als de secundaire sleutel is gewijzigd, selecteert u **secundaire** sleutel. Klik op **synchronisatiesleutel**.
      
      ![sleutels synchroniseren](./media/storsimple-8000-manage-storage-accounts/syncaccesskey3.png)

U wordt gewaarschuwd wanneer de sleutel met succes sycnhronized is.

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Sleutels voor opslagaccounts buiten het serviceabonnement synchroniseren
1. Op de **Services** pagina, klikt u op de **configureren** tabblad.
2. Klik op **Storage-Accounts toevoegen/bewerken**.
3. In het dialoogvenster het volgende doen:
   
   1. Selecteer het opslagaccount met de toegangssleutel die u wilt bijwerken.
   2. U moet de toegangssleutel voor opslag in de service Manager voor StorSimple-apparaat bijwerken. In dit geval ziet u de toegangssleutel voor opslag. Voer de nieuwe sleutel in de **toegangssleutel voor Opslagaccount** vak. 
   3. Sla uw wijzigingen op. Uw toegangssleutel voor opslagaccount moet nu worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple security](storsimple-8000-security.md).
* Meer informatie over [de service Manager voor StorSimple-apparaat gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

