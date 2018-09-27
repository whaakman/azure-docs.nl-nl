---
title: Beheren van uw StorSimple-opslagaccountreferenties voor apparaten uit Microsoft Azure StorSimple 8000-serie | Microsoft Docs
description: Wordt uitgelegd hoe u kunt de pagina configureren van de StorSimple Device Manager kunt toevoegen, bewerken, verwijderen of draaien van de sleutels voor een opslagaccount.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: afaa4c090508ee5a2306c2be1515076e6bd4f100
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225722"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-storage-account-credentials"></a>De StorSimple Device Manager-service gebruiken voor het beheren van uw opslagaccountreferenties

## <a name="overview"></a>Overzicht

De **configuratie** sectie in de serviceblade StorSimple Device Manager-geeft alle globale serviceparameters die kunnen worden gemaakt in de StorSimple Device Manager-service. Deze parameters kunnen worden toegepast op alle apparaten die zijn verbonden met de service, en omvatten:

* Opslagaccountreferenties
* Bandbreedtesjablonen 
* Access control records 

In deze zelfstudie wordt uitgelegd hoe u wilt toevoegen, bewerken, of opslagaccountreferenties verwijderen of de sleutels voor een storage-account draaien.

 ![Lijst met opslagaccountreferenties](./media/storsimple-8000-manage-storage-accounts/createnewstorageacct6.png)  

Storage-accounts bevatten de referenties die het StorSimple-apparaat gebruikt voor toegang tot uw storage-account met uw cloudserviceprovider. Dit zijn referenties, zoals de accountnaam en de primaire toegangssleutel voor Microsoft Azure storage-accounts. 

Op de **opslagaccountreferenties** blade, alle opslagaccounts die zijn gemaakt voor het factureringsabonnement worden weergegeven in tabelvorm met de volgende gegevens:

* **Naam** : de unieke naam die aan het account wordt toegewezen wanneer deze is gemaakt.
* **SSL is ingeschakeld** , of het SSL is ingeschakeld en communicatie van apparaat-naar-cloud is via het beveiligde kanaal.
* **Gebruikt door** : het aantal volumes met behulp van het storage-account.

De meest algemene taken met betrekking tot de storage-accounts die kunnen worden uitgevoerd zijn:

* Een storage-account toevoegen 
* Een storage-account bewerken 
* Een opslagaccount verwijderen 
* Sleutelroulatie van storage-accounts 

## <a name="types-of-storage-accounts"></a>Typen opslagaccounts

Er zijn drie typen opslagaccounts die kunnen worden gebruikt met uw StorSimple-apparaat.

* **Automatisch gegenereerde opslagaccounts** – zoals de naam al aangeeft, dit type opslagaccount wordt automatisch gegenereerd wanneer de service voor het eerst wordt gemaakt. Zie voor meer informatie over hoe dit storage-account is gemaakt, [stap 1: een nieuwe service maken](storsimple-8000-deployment-walkthrough-u2.md#step-1-create-a-new-service) in [uw on-premises StorSimple-apparaat implementeren](storsimple-8000-deployment-walkthrough-u2.md). 
* **Storage-accounts in het service-abonnement** : dit zijn de Azure storage-accounts die gekoppeld aan hetzelfde abonnement als die van de service zijn. Zie voor meer informatie over hoe deze storage-accounts zijn gemaakt, [over Azure Storage-Accounts](../storage/common/storage-create-storage-account.md). 
* **Storage-accounts buiten het serviceabonnement** : dit zijn de Azure storage-accounts die niet zijn gekoppeld aan uw service en waarschijnlijk aanwezig waren voordat de service is gemaakt.

## <a name="add-a-storage-account"></a>Een storage-account toevoegen

U kunt een storage-account toevoegen door een unieke beschrijvende naam en -referenties die zijn gekoppeld aan de storage-account (met de opgegeven cloud serviceprovider) te geven. U hebt ook de optie van het inschakelen van de modus voor secure sockets layer (SSL) voor het maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw apparaat en de cloud.

U kunt meerdere accounts maken voor een bepaalde cloudserviceprovider. Op de hoogte, echter wel nadat een storage-account is gemaakt, kunt u de cloudserviceprovider niet wijzigen.

Terwijl het storage-account wordt opgeslagen, wordt de service probeert te communiceren met uw cloudserviceprovider. De referenties en de toegang tot materiaal dat u hebt opgegeven, wordt op dit moment worden geverifieerd. Een storage-account wordt alleen gemaakt als de verificatie is geslaagd. Als de verificatie is mislukt, klikt u vervolgens een geschikt foutbericht weergegeven.

Gebruik de volgende procedures om toe te voegen van Azure storage-accountreferenties:

* Als u wilt een opslagaccountreferentie toevoegen waarvoor hetzelfde Azure-abonnement als de Device Manager-service
* Een Azure storage-accountreferenties die buiten het Device Manager serviceabonnement toevoegen

[!INCLUDE [add-a-storage-account-update2](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

#### <a name="to-add-an-azure-storage-account-credential-outside-of-the-storsimple-device-manager-service-subscription"></a>Een Azure storage-accountreferenties buiten de StorSimple Device Manager serviceabonnement toevoegen

1. Navigeer naar uw StorSimple Device Manager-service, selecteert en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie. Dit geeft een lijst van alle bestaande referenties van het opslagaccount dat is gekoppeld aan de StorSimple Device Manager-service.
3. Klik op **Add**.
4. In de **een opslagaccountreferentie toevoegen** blade, doet u het volgende:
   
    1. Voor **abonnement**, selecteer **andere**.
   
    2. Geef de naam van de referenties van uw Azure storage-account.
   
    3. In de **toegangssleutel voor Opslagaccount** tekst vak, de primaire toegangssleutel voor uw Azure storage-accountreferenties opgeven. Voor deze sleutel gaat u naar de Azure Storage-service, selecteert u de referenties van uw storage-account en op **accountsleutels beheren**. U kunt nu de primaire toegangssleutel kopiëren.
   
    4. Als SSL, klikt u op de **inschakelen** om te maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple Device Manager-service en de cloud. Klik op de **uitschakelen** knop alleen als u binnen een privécloud werkt.
   
    5. Klik op **Add**. U krijgt een melding nadat de opslagaccountreferentie is gemaakt.

5. De referentie voor het zojuist gemaakte opslagaccount wordt weergegeven op de serviceblade configureren StorSimple-Apparaatbeheerfunctie onder **opslagaccountreferenties**.
   


## <a name="edit-a-storage-account"></a>Een storage-account bewerken

U kunt een storage-account dat wordt gebruikt door een volumecontainer bewerken. Als u een opslagaccount dat is momenteel in gebruik hebt bewerkt, is het enige veld beschikbaar om te wijzigen de toegangssleutel voor het opslagaccount. U kunt opgeven van de nieuwe toegangssleutel voor opslag en de bijgewerkte instellingen opslaan.

#### <a name="to-edit-a-storage-account"></a>Een storage-account bewerken

1. Ga naar uw StorSimple-apparaatbeheerservice. Klik in de sectie **Configuratie** op **Opslagaccountreferenties**.

    ![Opslagaccountreferenties](./media/storsimple-8000-manage-storage-accounts/editstorageacct1.png)

2. In de **opslagaccountreferenties** blade in de lijst met referenties van het opslagaccount, selecteer en klik op de computer die u wilt bewerken. 

3. U kunt de **SSL inschakelen** selectie. U kunt ook klikken op **meer...**  en selecteer vervolgens **Synchroniseer de toegangssleutel draaien** de toegangssleutels van uw storage-account. Ga naar [rotatie van de storage-accounts van sleutel](#key-rotation-of-storage-accounts) voor meer informatie over het uitvoeren van rouleren van de sleutel. Nadat u de instellingen hebt gewijzigd, klikt u op **opslaan**. 

    ![Bewerkte opslagaccountreferenties opslaan](./media/storsimple-8000-manage-storage-accounts/editstorageacct3.png)

4. Klik op **Ja** als u om bevestiging wordt gevraagd. 

    ![Wijzigingen controleren](./media/storsimple-8000-manage-storage-accounts/editstorageacct4.png)

De instellingen worden bijgewerkt en opgeslagen voor uw opslagaccount. 

## <a name="delete-a-storage-account"></a>Een opslagaccount verwijderen

> [!IMPORTANT]
> U kunt een storage-account alleen verwijderen als deze niet wordt gebruikt door een volumecontainer. Als een storage-account wordt gebruikt door een volumecontainer, verwijdert u eerst de volumecontainer en verwijder vervolgens het bijbehorende opslagaccount.

#### <a name="to-delete-a-storage-account"></a>Een storage-account verwijderen

1. Ga naar uw StorSimple-apparaatbeheerservice. Klik in de sectie **Configuratie** op **Opslagaccountreferenties**.

2. In de lijst in tabelvorm van storage-accounts, de muisaanwijzer over het account dat u wilt verwijderen. Klik met de rechtermuisknop op het aanroepen van het contextmenu en klik op **verwijderen**.

    ![Referenties voor het opslagaccount verwijderen](./media/storsimple-8000-manage-storage-accounts/deletestorageacct1.png)

3. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja** om door te gaan met het verwijderen. De lijst in tabelvorm wordt bijgewerkt om de wijzigingen weer te geven.

    ![De verwijdering bevestigen](./media/storsimple-8000-manage-storage-accounts/deletestorageacct2.png)

## <a name="key-rotation-of-storage-accounts"></a>Sleutelroulatie van storage-accounts

Uit veiligheidsoverwegingen is sleutelroulatie vaak een vereiste in datacenters. Elke Microsoft Azure-abonnement kan een of meer gekoppelde storage-accounts hebben. De toegang tot deze accounts wordt bepaald door het abonnement en toegangssleutels voor elke storage-account. 

Wanneer u een opslagaccount maakt, genereert Microsoft Azure twee 512-bits opslagtoegangssleutels die worden gebruikt voor verificatie wanneer het opslagaccount dat wordt geopend. Met twee toegangssleutels voor opslag, kunt u de sleutels met zonder onderbreking van uw opslagservice of de toegang tot deze service opnieuw genereren. De sleutel die momenteel in gebruik is de *primaire* en de back-sleutel wordt aangeduid als de *secundaire* sleutel. Een van deze twee sleutels moet worden opgegeven bij uw Microsoft Azure StorSimple-apparaat toegang heeft tot uw serviceprovider voor cloud-opslag.

## <a name="what-is-key-rotation"></a>Wat is sleutelroulatie?

Toepassingen gebruiken normaal gesproken alleen een van de sleutels voor toegang tot uw gegevens. Na een bepaalde periode, kunt u uw toepassingen zijn overgestapt naar het met behulp van de tweede sleutel hebben. Nadat u uw toepassingen naar de secundaire sleutel hebt overgeschakeld, kunt u de eerste sleutel buiten gebruik stellen en vervolgens een nieuwe sleutel te genereren. Met behulp van de twee sleutels op deze manier kunt uw toepassingen toegang tot de gegevens zonder enige uitvaltijd.

De opslagaccountsleutels zijn altijd in de service in een versleutelde vorm opgeslagen. Deze kunnen echter worden hersteld via de StorSimple Device Manager-service. De service krijgen de primaire en secundaire sleutel voor alle opslagaccounts in hetzelfde abonnement, met inbegrip van accounts die zijn gemaakt in de Storage-service, evenals de standaard-storage-accounts die worden gegenereerd wanneer de service StorSimple Device Manager voor het eerst werd gemaakt. De StorSimple Device Manager-service wordt altijd kunnen genieten van deze sleutels van de klassieke Azure portal en deze vervolgens opslaan in een versleutelde manier.

## <a name="rotation-workflow"></a>Rotatie van werkstroom

De beheerder van een Microsoft Azure kunt genereren of de primaire of secundaire sleutel wijzigen door rechtstreeks toegang hebben tot het opslagaccount (via de Microsoft Azure Storage-service). De StorSimple Device Manager-service ziet deze wijziging niet automatisch.

Om te informeren over de StorSimple Device Manager-service van de wijziging, moet u toegang tot de service StorSimple Device Manager toegang tot het opslagaccount, en vervolgens de primaire of secundaire sleutel (afhankelijk van welk is gewijzigd) te synchroniseren. De service vervolgens haalt de meest recente sleutel, versleutelt de sleutels, en verzendt de versleutelde sleutel naar het apparaat.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service"></a>Om te synchroniseren van sleutels voor opslagaccounts in hetzelfde abonnement als de service 
1. Ga naar uw StorSimple-apparaatbeheerservice. Klik in de sectie **Configuratie** op **Opslagaccountreferenties**.
2. In de lijst in tabelvorm van storage-accounts, klikt u op de map die u wilt wijzigen. 

    ![sleutels synchroniseren](./media/storsimple-8000-manage-storage-accounts/syncaccesskey1.png)

3. Klik op **... Meer** en selecteer vervolgens **Synchroniseer de toegangssleutel draaien**.   

    ![sleutels synchroniseren](./media/storsimple-8000-manage-storage-accounts/syncaccesskey2.png)

4. In de StorSimple Device Manager-service moet u de sleutel die eerder is gewijzigd in de Microsoft Azure Storage-service wordt bijgewerkt. Als de primaire toegangssleutel is gewijzigd (opnieuw gegenereerd), selecteert u **primaire** sleutel. Als de secundaire sleutel is gewijzigd, selecteert u **secundaire** sleutel. Klik op **sleutel synchroniseren**.
      
      ![sleutels synchroniseren](./media/storsimple-8000-manage-storage-accounts/syncaccesskey3.png)

U krijgt bericht wanneer de sleutel nog niet is sycnhronized.

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Om te synchroniseren van sleutels voor opslagaccounts buiten het service-abonnement
1. Op de **Services** pagina, klikt u op de **configureren** tabblad.
2. Klik op **Storage-Accounts toevoegen/bewerken**.
3. In het dialoogvenster het volgende doen:
   
   1. Selecteer het opslagaccount dat door de toegangssleutel die u wilt bijwerken.
   2. U moet de toegangssleutel voor opslag in de StorSimple Device Manager-service bijwerken. In dit geval ziet u de toegangssleutel voor opslag. Voer de nieuwe sleutel in de **toegangssleutel voor Opslagaccount** vak. 
   3. Sla uw wijzigingen op. Toegangssleutel voor uw opslagaccount moet nu worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [StorSimple security](storsimple-8000-security.md).
* Meer informatie over [met de StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

