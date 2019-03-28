---
title: Azure Data Box Edge share management | Microsoft Docs
description: Beschrijft hoe u de Azure portal gebruiken voor het beheren van bestandsshares voor uw Azure Data Box-edge-apparaten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: e85e006a54fcb4bb677932b3e1ff9fa79352dba9
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519830"
---
# <a name="use-the-azure-portal-to-manage-shares-on-your-azure-data-box-edge"></a>De Azure portal gebruiken voor het beheren van bestandsshares voor uw Azure Data Box-edge-apparaten

Dit artikel wordt beschreven hoe u shares voor uw Azure Data Box-edge-apparaten kunt beheren. U kunt beheren van de rand van het Azure Data Box via Azure portal of via de lokale webgebruikersinterface. De Azure portal gebruiken voor het toevoegen, verwijderen, shares of opslagsleutel synchroniseren voor storage-account dat is gekoppeld aan de shares vernieuwen.

## <a name="about-shares"></a>Info over shares

Als u wilt gegevens overdragen naar Azure, moet u bestandsshares maken voor uw Azure Data Box-edge-apparaten. De shares die u op de gegevens in het Edge-apparaat toevoegt kunnen zijn lokale shares of shares die pushen van gegevens naar de cloud.

 - **Lokale shares**: Gebruik deze shares wanneer de gegevens moeten worden lokaal op het apparaat worden verwerkt.
 - **Shares**: Gebruik deze shares als u wilt dat het apparaatgegevens automatisch worden gepusht naar uw opslagaccount in de cloud. Alle cloud functies, zoals **vernieuwen** en **opslagsleutels synchroniseren** zijn van toepassing op de shares.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een share toevoegen
> * Een share verwijderen
> * Shares vernieuwen
> * Opslagsleutel synchroniseren


## <a name="add-a-share"></a>Een share toevoegen

Voer in de Azure Portal de volgende stappen uit om een share te maken.

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **Gateway > Shares**. Selecteer **+ bestandsshare toevoegen** op de opdrachtbalk.

    ![Selecteer de share toevoegen](media/data-box-edge-manage-shares/add-share-1.png)

2. Geef in **Share toevoegen** de instellingen voor de share op. Geef een unieke naam voor de share op.
    
    Namen van shares mogen alleen cijfers, kleine letters en afbreekstreepjes bevatten. De naam van de share moet tussen de 3 en 63 tekens bevatten en met een cijfer of letter beginnen. Elk afbreekstreepje moet worden voorafgegaan en gevolgd door een cijfer of letter.

3. Selecteer een **Type** voor de share. Het type kan **SMB** of **NFS** zijn. SMB is het standaardtype. SMB is het standaardtype voor Windows-clients; NFS wordt gebruikt voor Linux-clients. De opties wijken enigszins af, afhankelijk van welk type u kiest.

4. Geef het **opslagaccount** op waar de share zich bevindt. In het opslagaccount met de naam van de share wordt een container gemaakt, als deze nog niet bestaat. Als de container al bestaat, wordt de bestaande container gebruikt.

5. Kies in de vervolgkeuzelijst de **opslagservice** van blok-blobs, pagina-blobs of -bestanden. Het type service dat u kiest, is afhankelijk van de indeling waarin u de gegevens in Azure wilt opslaan. Bijvoorbeeld, in dit exemplaar, willen we de gegevens zich bevinden als blok-blobs in Azure, dus we selecteren **blok-Blob**. Als het kiezen van **pagina-Blob**, moet u ervoor zorgen dat uw gegevens 512 bytes uitgelijnd is. Gebruik **pagina-blob** voor VHD's of VHDX die altijd 512 bytes uitgelijnd.

6. Deze stap hangt af van of u een SMB- of een NFS-share gaat maken.
    - **Als u een SMB-share maakt**: maak in het veld **All privilege local user** (Lokale gebruiker met alle bevoegdheden) een keuze uit **Nieuwe maken** of **Bestaande gebruiken**. Als u een nieuwe lokale gebruiker maakt, geeft u de **gebruikersnaam** en het **wachtwoord** op en bevestigt u het wachtwoord. Hiermee worden de bevoegdheden aan de lokale gebruiker toegewezen. Als u de bevoegdheden hier hebt toegewezen, kunt u Verkenner gebruiken om de bevoegdheden te wijzigen.

        ![SMB-share toevoegen](media/data-box-edge-manage-shares/add-smb-share.png)

        Als u ervoor kiest om alleen alleen-lezen bewerkingen toe te staan voor deze share-gegevens, kunt u gebruikers opgeven met alleen-lezen machtigingen.
    - **Als u een NFS-share maakt**: geef de **IP-adressen op van de clients** die toegang hebben tot de share.

        ![NFS-share toevoegen](media/data-box-edge-manage-shares/add-nfs-share.png)

7. Voor eenvoudig toegang tot de shares via Edge Computing-modules, gebruikt u het lokale koppelpunt. Selecteer **gebruik van de bestandsshare met Edge Computing** zodat de share is automatisch gekoppeld nadat deze is gemaakt. Wanneer deze optie is geselecteerd, kan het Edge-module de compute ook gebruiken met het lokale koppelpunt.

8. Klik op **Maken** om de share te maken. U ontvangt een melding als wordt begonnen met het maken van de share. Als de share met de opgegeven instellingen is gemaakt, wordt de blade **Shares** bijgewerkt in overeenstemming met de nieuwe share.

## <a name="add-a-local-share"></a>Een lokale share toevoegen

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **Gateway > Shares**. Selecteer **+ bestandsshare toevoegen** op de opdrachtbalk.

    ![Selecteer de share toevoegen](media/data-box-edge-manage-shares/add-local-share-1.png)

2. Geef in **Share toevoegen** de instellingen voor de share op. Geef een unieke naam voor de share op.
    
    Namen van shares mogen alleen cijfers, kleine letters en afbreekstreepjes bevatten. De naam van de share moet tussen de 3 en 63 tekens bevatten en met een cijfer of letter beginnen. Elk afbreekstreepje moet worden voorafgegaan en gevolgd door een cijfer of letter.

3. Selecteer een **Type** voor de share. Het type kan **SMB** of **NFS** zijn. SMB is het standaardtype. SMB is het standaardtype voor Windows-clients; NFS wordt gebruikt voor Linux-clients. De opties wijken enigszins af, afhankelijk van welk type u kiest.

4. Voor eenvoudig toegang tot de shares via Edge Computing-modules, gebruikt u het lokale koppelpunt. Selecteer **gebruik van de bestandsshare met Edge Computing** zodat de Edge-module de rekenkracht met het lokale koppelpunt gebruiken kunt.

5. Selecteer **configureren als lokale shares Edge**. De gegevens in lokale bestandsshares blijven lokaal op het apparaat. U kunt deze gegevens lokaal verwerken.

6. In de **alle lokale gebruiker met bevoegdheden** veld, kiest u uit **nieuw** of **gebruik bestaande**.

7. Selecteer **Maken**. 

    ![Lokale share maken](media/data-box-edge-manage-shares/add-local-share-2.png)

    U ziet een melding dat de share gemaakt wordt. Als de share met de opgegeven instellingen is gemaakt, wordt de blade **Shares** bijgewerkt in overeenstemming met de nieuwe share.

    ![Updates Shares blade weergeven](media/data-box-edge-manage-shares/add-local-share-3.png)
    
    Selecteer de share om de lokale koppelpunt voor het Edge-modules voor compute voor deze share weer te geven.

    ![Details van de lokale share weergeven](media/data-box-edge-manage-shares/add-local-share-4.png)

## <a name="mount-a-share"></a>Een bestandsshare koppelen

Als u een share gemaakt voordat u compute op uw gegevens in het Edge-apparaat hebt geconfigureerd, moet u de bestandsshare koppelen. Voer de volgende stappen uit om een bestandsshare te koppelen.


1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **Gateway > Shares**. Selecteer in de lijst van de shares, de share die u wilt koppelen. De **gebruikt voor compute** kolom wordt de status weergegeven **uitgeschakelde** voor de geselecteerde bestandsshare.

    ![Share selecteren](media/data-box-edge-manage-shares/select-share-mount.png)

2. Selecteer **koppelen**.

    ![Selecteer koppelen](media/data-box-edge-manage-shares/select-mount.png)

3. Wanneer u hierom wordt gevraagd om bevestiging, selecteert u **Ja**. Hiermee wordt de bestandsshare koppelen.

    ![Koppelpunten bevestigen](media/data-box-edge-manage-shares/confirm-mount.png)

4. Nadat de share is gekoppeld, gaat u naar de lijst met shares. U ziet dat de **gebruikt voor compute** kolom ziet u de status van de share als **ingeschakeld**.

    ![Gekoppelde share](media/data-box-edge-manage-shares/share-mounted.png)

5. Selecteer de share opnieuw om weer te geven van de lokale koppelpunt voor de share. Edge compute-module gebruikt deze lokale koppelpunt voor de share.

    ![Lokale koppelpunt voor de share](media/data-box-edge-manage-shares/share-mountpoint.png)

## <a name="unmount-a-share"></a>Een bestandsshare te ontkoppelen

Voer de volgende stappen uit in de Azure portal om een bestandsshare te ontkoppelen.

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **Gateway > Shares**.

    ![Share selecteren](media/data-box-edge-manage-shares/select-share-unmount.png)

2. Selecteer in de lijst van de shares, de share die u wilt ontkoppelen. U wilt om ervoor te zorgen dat de share die u kunt ontkoppelen niet wordt gebruikt door alle modules. Als de share wordt gebruikt door een module, ziet u problemen met de bijbehorende-module. Selecteer **ontkoppelen**.

    ![Selecteer ontkoppelen](media/data-box-edge-manage-shares/select-unmount.png)

3. Wanneer u hierom wordt gevraagd om bevestiging, selecteert u **Ja**. Hiermee wordt de bestandsshare ontkoppelen.

    ![Controleer of ontkoppelen](media/data-box-edge-manage-shares/confirm-unmount.png)

4. Nadat de share ontkoppeld is, gaat u naar de lijst met shares. U ziet dat **gebruikt voor compute** kolom ziet u de status van de share als **uitgeschakelde**.

    ![Share ontkoppeld](media/data-box-edge-manage-shares/share-unmounted.png)

## <a name="delete-a-share"></a>Een share verwijderen

Voer in de Azure Portal de volgende stappen uit om een share te verwijderen.

1. Klik in de lijst met shares op de share die u wilt verwijderen.

    ![Share selecteren](media/data-box-edge-manage-shares/delete-share-1.png)

2. Klik op **Verwijderen**.

    ![Op Verwijderen klikken](media/data-box-edge-manage-shares/delete-share-2.png)

3. Klik op **Ja** als u om bevestiging wordt gevraagd.

    ![De verwijdering bevestigen](media/data-box-edge-manage-shares/delete-share-3.png)

De lijst met shares updates in overeenstemming met het verwijderen.


## <a name="refresh-shares"></a>Shares vernieuwen

De functie vernieuwen kunt u de inhoud van een share vernieuwen. Wanneer u een share vernieuwt, wordt er een zoekopdracht gestart om de Azure-objecten te zoeken, inclusief blobs en bestanden die aan de cloud zijn toegevoegd na de laatste vernieuwing. Deze extra bestanden zijn vervolgens om te vernieuwen van de inhoud van de bestandsshare op het apparaat gedownload.

> [!IMPORTANT]
> - U kunt openbare shares niet vernieuwen.
> - Machtigingen en toegangsbeheerlijsten (ACL's) blijven niet behouden via een vernieuwingsbewerking. 

Voer in de Azure Portal de volgende stappen uit om een share te vernieuwen.

1.  Ga in de Azure Portal naar **Shares**. Klik op de share die u wilt vernieuwen.

    ![Share selecteren](media/data-box-edge-manage-shares/refresh-share-1.png)

2.  Klik op **Vernieuwen**. 

    ![Op Vernieuwen klikken](media/data-box-edge-manage-shares/refresh-share-2.png)
 
3.  Klik op **Ja** als u om bevestiging wordt gevraagd. Er wordt een taak gestart om de inhoud van de on-premises share te vernieuwen.

    ![Vernieuwen bevestigen](media/data-box-edge-manage-shares/refresh-share-3.png)
 
4.  Terwijl het vernieuwen wordt uitgevoerd, is de optie Vernieuwen niet beschikbaar in het contextmenu. Klik op de taakmelding om de status van de vernieuwingstaak weer te geven.

5.  Hoe lang de vernieuwing duurt is afhankelijk van het aantal bestanden in de Azure-container en van het aantal bestanden op het apparaat. Zodra het vernieuwen is voltooid, wordt het tijdstempel van de share bijgewerkt. Zelfs als de vernieuwing niet geheel zonder fouten is uitgevoerd, wordt de bewerking als voltooid beschouwd en wordt het tijdstempel bijgewerkt. De foutenlogboeken vernieuwen worden ook bijgewerkt.

    ![Bijgewerkt tijdstempel](media/data-box-edge-manage-shares/refresh-share-4.png)
 
Als er een fout optreedt, wordt er een waarschuwing gegenereerd. De waarschuwing bevat meer informatie over de oorzaak en een aanbeveling om het probleem te verhelpen. De waarschuwing bevat ook een koppeling naar een bestand dat een volledig overzicht bevat van de fouten en van de bestanden die niet zijn bijgewerkt of verwijderd.


## <a name="sync-storage-keys"></a>Opslagsleutels synchroniseren

Als de sleutels van uw opslagaccount zijn geroteerd, moet u de toegangssleutels voor opslag synchroniseren. Door een synchronisatie kan het apparaat de meest recente sleutels voor uw opslagaccount verkrijgen.

Voer in de Azure Portal de volgende stappen uit om uw toegangssleutel voor opslag te synchroniseren.

1. Ga in uw resource naar **Overzicht**. Klik in de lijst met shares op een share die is gekoppeld aan het opslagaccount dat u wilt synchroniseren.

    ![Delen met relevante storage-account selecteren](media/data-box-edge-manage-shares/sync-storage-key-1.png)

2. Klik op **Opslagsleutel synchroniseren**. Klik op **Ja** als u om bevestiging wordt gevraagd.

     ![Schakel synchronisatie opslagsleutel](media/data-box-edge-manage-shares/sync-storage-key-2.png)

3. Sluit het dialoogvenster wanneer de synchronisatie is voltooid.

>[!NOTE]
> U hoeft dit maar eenmaal te doen voor een gegeven opslagaccount. U hoeft deze actie niet te herhalen voor alle shares die zijn gekoppeld aan hetzelfde opslagaccount.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Gebruikers beheren via Azure Portal](data-box-edge-manage-users.md).
