---
title: Virtuele StorSimple-matrix als bestandsserver instellen | Microsoft Docs
description: 'Deze derde zelfstudie in de implementatie van virtuele StorSimple-matrix: Hiermee geeft u een virtueel apparaat als bestandsserver instellen.'
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: f609f6ff-0927-48bb-a68a-6d8985d2fe34
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bf507fb21b314a6811db1c1e45a4356381caada1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-storsimple-virtual-array---set-up-as-file-server-via-azure-portal"></a>Implementeer matrix van de virtuele StorSimple - Set up als bestandsserver via Azure portal
![](./media/storsimple-virtual-array-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Inleiding
In dit artikel wordt beschreven hoe initiële installatie uitvoert, registreren van uw StorSimple-bestandsserver, de Apparaatinstelling en maken en verbinding maken met SMB-shares. Dit is het laatste artikel in de reeks van vereist voor het implementeren van uw virtuele matrix volledig als een bestandsserver of een server met iSCSI-zelfstudies voor implementatie.

De installatie- en configuratieproces kan ongeveer 10 minuten duren om te voltooien. De informatie in dit artikel geldt alleen voor de implementatie van de virtuele StorSimple-matrix. Voor de implementatie van StorSimple 8000 series apparaten, gaat u naar: [StorSimple 8000 series apparaat met Update 2 implementeren](storsimple-deployment-walkthrough-u2.md).

## <a name="setup-prerequisites"></a>Vereisten voor installatie
Voordat u configureren en van uw virtuele StorSimple-matrix instellen, zorg ervoor dat:

* U hebt een virtuele-matrix ingericht en verbonden zoals beschreven in de [inrichten van een virtueel StorSimple-matrix in Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) of [inrichten van een virtueel StorSimple-matrix in VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
* U hebt de serviceregistratiesleutel van de StorSimple-apparaat Manager-service die u hebt gemaakt voor het beheren van virtuele StorSimple-matrices. Zie voor meer informatie [stap 2: de serviceregistratiesleutel ophalen](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) voor virtuele StorSimple-matrix.
* Als dit de tweede of volgende virtuele matrix dat u met een bestaande Apparaatbeheer StorSimple-service registreren wilt, moet u de gegevensversleutelingssleutel van service hebben. Deze sleutel is gegenereerd nadat het eerste apparaat is geregistreerd met deze service. Als u deze sleutel hebt verloren, Zie [ophalen van de gegevensversleutelingssleutel van service](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) voor uw virtuele StorSimple-matrix.

## <a name="step-by-step-setup"></a>Stapsgewijze setup
Gebruik de volgende stapsgewijze instructies instellen en configureren van uw virtuele StorSimple-matrix.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Stap 1: De lokale web UI-installatie is voltooid en Registreer uw apparaat
#### <a name="to-complete-the-setup-and-register-the-device"></a>De installatie is voltooid en het apparaat registreren
1. Open een browservenster en maak verbinding met de lokale webgebruikersinterface. Type:
   
   `https://<ip-address of network interface>`
   
   Gebruik de verbindings-URL in de vorige stap hebt genoteerd. Er is een fout die aangeeft dat er een probleem met het beveiligingscertificaat van de website. Klik op **doorgaan naar deze webpagina**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image2.png)
2. Aanmelden bij de webgebruikersinterface van uw virtuele matrix als **StorSimpleAdmin**. Voer het beheerderswachtwoord voor apparaten die u hebt gewijzigd in stap 3: het starten van de virtuele-matrix in [inrichten van een virtueel StorSimple-matrix in Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) of in [inrichten van een virtueel StorSimple-matrix in VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image3.png)
3. Gaat u naar de **Start** pagina. Deze pagina beschrijft de verschillende instellingen configureren en registreren van de virtuele matrix met de service Manager voor StorSimple-apparaat vereist. De **instellingen**, **Web proxy-instellingen**, en **tijdinstellingen** zijn optioneel. De enige vereiste instellingen zijn **apparaatinstellingen** en **Cloudinstellingen**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image4.png)
4. In de **instellingen** pagina onder **netwerkinterfaces**, DATA 0 wordt automatisch voor u geconfigureerd. Elke netwerkinterface is standaard ingesteld op het IP-adres automatisch ophalen (DHCP). Een IP-adres, subnetmasker en gateway worden daarom automatisch toegewezen (voor IPv4 en IPv6).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image5.png)
   
   Als u meer dan één netwerkinterface tijdens het inrichten van het apparaat hebt toegevoegd, kunt u ze hier configureren. Houd er rekening mee dat kunt u de netwerkinterface configureren als IPv4 alleen of als zowel IPv4 als IPv6. Alleen IPv6-configuraties worden niet ondersteund.
5. DNS-servers zijn vereist, omdat ze worden gebruikt wanneer uw apparaat probeert te communiceren met uw cloudserviceproviders voor opslag of het apparaat oplossen met de naam wanneer geconfigureerd als een bestandsserver. In de **instellingen** pagina onder de **DNS-servers**:
   
   1. Een primaire en secundaire DNS-server worden automatisch geconfigureerd. Als u kiest voor het configureren van statische IP-adressen, kunt u DNS-servers. U wordt aangeraden dat u een primaire en secundaire DNS-server configureren voor hoge beschikbaarheid.
   2. Klik op **toepassen** wilt toepassen en valideren van de netwerkinstellingen.
6. In de **apparaatinstellingen** pagina:
   
   1. Wijs een uniek **naam** op uw apparaat. Deze naam mag 1-15 tekens lang zijn en mag letter, cijfers en afbreekstreepjes bevatten.
   2. Klik op de **bestandsserver** pictogram ![](./media/storsimple-virtual-array-deploy3-fs-setup/image6.png) voor de **Type** van apparaten die u maakt. Een bestandsserver kunt u gedeelde mappen te maken.
   3. Als het apparaat een bestandsserver is, moet u het apparaat toevoegen aan een domein. Voer een **domeinnaam**.
   4. Klik op **Toepassen**.
7. Een dialoogvenster wordt weergegeven. Geef de domeinreferenties van uw in de opgegeven indeling. Klik op het vinkje. De domeinreferenties worden geverifieerd. U ziet een foutbericht weergegeven als de referenties onjuist zijn.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image7.png)
8. Klik op **Toepassen**. Hiermee wordt de toepassing en valideren van de instellingen voor apparaten.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image8.png)
   
   > [!NOTE]
   > Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OE) voor Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast of overgenomen. Groepsbeleid mag toepassingen zoals antivirussoftware installeren op de virtuele StorSimple-matrix. Extra software te installeren, wordt niet ondersteund en kan leiden tot gegevensbeschadiging. 
   > 
   > 
9. (Optioneel) uw webproxyserver configureren. Hoewel webproxyconfiguratie optioneel is, worden op de hoogte gebracht als u een webproxy gebruikt, u alleen deze hier configureren kunt.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image9.png)
   
   In de **Web proxy** pagina:
   
   1. Geef de **Web-URL voor proxy** in deze indeling: *http://&lt;host-IP-adres of FDQN&gt;: poortnummer*. Houd er rekening mee dat HTTPS-URL's worden niet ondersteund.
   2. Geef **verificatie** als **Basic** of **geen**.
   3. Als verificatie gebruikt, u moet ook om te bieden een **gebruikersnaam** en **wachtwoord**.
   4. Klik op **Toepassen**. Zo valideren en past u de geconfigureerde web proxy-instellingen.
10. (Optioneel) Configureer de instellingen voor uw apparaat, zoals tijdzone en de primaire en secundaire NTP-servers. NTP-servers zijn vereist, omdat uw apparaat de tijd synchroniseren moet zodat verificatie met uw cloudserviceproviders.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/image10.png)
    
    In de **tijdinstellingen** pagina:
    
    1. Selecteer in de vervolgkeuzelijst de **tijdzone** op basis van de geografische locatie op waarin het apparaat wordt geïmplementeerd. De standaardtijdzone voor uw apparaat is PST. Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen.
    2. Geef een **primaire NTP-server** voor uw apparaat of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat in uw netwerk NTP-verkeer kan worden doorgegeven van uw datacenter naar internet.
    3. Geef desgewenst een **secundaire NTP-server** voor uw apparaat.
    4. Klik op **Toepassen**. Zo valideren en past u de geconfigureerde instellingen.
11. De cloud configureren voor uw apparaat. In deze stap maakt u de configuratie van het lokale apparaat voltooien en Registreer het apparaat bij uw StorSimple-apparaat Manager-service.
    
    1. Voer de **serviceregistratiesleutel** die u hebt verkregen [stap 2: de serviceregistratiesleutel ophalen](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) voor virtuele StorSimple-matrix.
    2. Als dit het eerste apparaat registreren bij deze service, u krijgt de **gegevensversleutelingssleutel van Service**. Kopieer deze sleutel en bewaar deze op een veilige plaats. Deze sleutel is vereist bij de serviceregistratiesleutel extra apparaten registreren bij de service Manager voor StorSimple-apparaat. 
       
       Als dit niet het eerste apparaat dat u met deze service wilt registreren, moet u de gegevensversleutelingssleutel van service leveren. Raadpleeg voor meer informatie als u de [gegevensversleutelingssleutel van service](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) op uw lokale webgebruikersinterface.
    3. Klik op **registreren**. Hiermee wordt het apparaat opnieuw opgestart. Mogelijk moet u wachten op 2-3 minuten voordat het apparaat is geregistreerd. Nadat het apparaat opnieuw is opgestart, gaat u naar de aanmeldingspagina.
       
       ![](./media/storsimple-virtual-array-deploy3-fs-setup/image13.png)
12. Ga terug naar de Azure-portal. Ga naar **alle resources**, zoeken naar uw StorSimple-apparaat Manager-service.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/searchdevicemanagerservice1.png) 
13. In de gefilterde lijst, selecteer uw service Manager voor StorSimple-apparaat en navigeer vervolgens naar **Management > apparaten**. In de **apparaten** blade, Controleer of het apparaat verbinding heeft gemaakt met de service en heeft de status **gereed is voor het instellen van**.
    
    ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png)

## <a name="step-2-configure-the-device-as-file-server"></a>Stap 2: Het apparaat als bestandsserver configureren
Voer de volgende stappen uit in de [Azure-portal](https://portal.azure.com/) om de installatie vereist apparaat te voltooien.

#### <a name="to-configure-the-device-as-file-server"></a>Het apparaat als bestandsserver configureren
1. Ga naar de service voor uw StorSimple-Apparaatbeheer en gaat u naar **Management > apparaten**. In de **apparaten** blade, selecteer het apparaat dat u zojuist hebt gemaakt. Dit apparaat wordt weergegeven als **gereed is voor het instellen van**.
   
   ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png) 
2. Klik op het apparaat en ziet u een banner waarin wordt aangegeven dat het apparaat gereed voor installatie is.
   
    ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs3m.png)
3. Klik op **configureren** op de opdrachtbalk. Hiermee opent u de **configureren** blade. In de **configureren** blade het volgende doen:
   
    1. Naam van de bestandsserver wordt automatisch gevuld.
    
    2. Zorg ervoor dat de versleuteling van cloudopslag is ingesteld op **ingeschakeld**. Hiermee worden alle gegevens die worden verzonden naar de cloud gecodeerd. 
    
    3. Een AES 256-bits sleutel wordt gebruikt door de gebruiker gedefinieerde sleutel voor versleuteling. Geef een code op 32 tekens en Bevestig op de sleutel om het te bevestigen. Noteer de sleutel in een app Sleutelbeheer voor toekomstig gebruik.
    
    4. Klik op **vereiste instellingen configureren** opslagaccountreferenties moet worden gebruikt met uw apparaat opgeven. Klik op **nieuwe toevoegen** of er geen opslagaccountreferenties geconfigureerd zijn. **Zorg ervoor dat de opslag waarmee u ondersteunt blok-blobs. Pagina-blobs worden niet ondersteund.** Meer informatie over [blobs en pagina-blobs geblokkeerd](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).
   
    ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs6m.png) 
4. In de **toevoegen van een opslagaccountreferenties** blade het volgende doen: 

    1. Kies huidige abonnement als het opslagaccount zich in hetzelfde abonnement als de service. Geef andere is de storage-account is buiten het serviceabonnement. 
    
    2. Kies een bestaand opslagaccount in de vervolgkeuzelijst. 
    
    3. De locatie automatisch ingevuld op basis van het opgegeven opslagaccount. 
    
    4. Schakel SSL om te controleren of een beveiligd netwerk communicatiekanaal tussen het apparaat en de cloud.
    
    5. Klik op **toevoegen** deze referentie storage-account toevoegen. 
   
        ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs8m.png)

5. Als de storage-account-referentie is gemaakt, de **configureren** blade wordt bijgewerkt om de accountreferenties van de opgegeven opslaggroep weer te geven. Klik op **Configureren**
   
   ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs11m.png)
   
   U ziet een dat een bestand server wordt gemaakt. Als de bestandsserver is gemaakt, wordt u gewaarschuwd.
   
   ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs13m.png)
   
   Status van het apparaat wordt ook gewijzigd in **Online**.
   
   ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs14m.png)
   
   U kunt doorgaan met het toevoegen van een share.

## <a name="step-3-add-a-share"></a>Stap 3: Een share toevoegen
Voer de volgende stappen uit in de [Azure-portal](https://portal.azure.com/) voor het maken van een share.

#### <a name="to-create-a-share"></a>Een share maken
1. Selecteer het bestand server apparaat dat u in de vorige stap hebt geconfigureerd en klik op **...**  (of met de rechtermuisknop op). Selecteer in het contextmenu **toevoegen share**. U kunt ook klikken op **+ bestandsshare toevoegen** op de opdrachtbalk apparaat.
   
   ![Share toevoegen](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs15m.png)
2. Geef de volgende share-instellingen:

    1. Een unieke naam voor de share. De naam moet een tekenreeks met 3 tot en met 127 tekens.
    
    2. Een optionele **beschrijving** voor de share. De beschrijving kunt identificeren de eigenaren van de bestandsshare.
    
    3. Een **Type** voor de share. Het type kan worden **tiers verdeelde** of **lokaal vastgemaakt**, met gelaagde wordt de standaardwaarde. Voor workloads waarvoor lokale garanties, lage latenties en betere prestaties, selecteer een **lokaal vastgemaakt** delen. Voor alle overige gegevens selecteert u een **tiers verdeelde** delen.
    Een lokaal vastgemaakt share is compact ingericht en zorgt ervoor dat de primaire gegevens op de share lokaal op het apparaat blijft en komt niet in de cloud worden gelekt. Een gelaagde share is aan de andere kant dun ingericht. Wanneer u een gelaagde share maakt, wordt 10% van de ruimte is ingericht op de lokale laag en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld, als u een volume 1 TB ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB in de cloud moet worden gebruikt wanneer de gegevenslagen. Dit betekent op zijn beurt als u buiten de lokale ruimte op het apparaat uitvoert, kunt u een gelaagde share kan niet inrichten.
   
    4. In de **standaard volledige machtigingen ingesteld op** veld, machtigingen toewijzen aan de gebruiker of de groep die toegang heeft tot deze share. Geef de naam van de gebruiker of de gebruikersgroep in  *john@contoso.com*  indeling. Het is raadzaam dat u een gebruikersgroep (in plaats van een enkele gebruiker) om toe te staan beheerdersbevoegdheden voor toegang tot deze shares. Nadat u hier de machtigingen hebt toegewezen, kunt u Windows Verkenner vervolgens gebruiken om deze machtigingen te wijzigen.
   
    5. Klik op **toevoegen** om de share te maken. 
    
        ![Share toevoegen](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs18m.png)
   
        U wordt gewaarschuwd dat het maken van de share uitgevoerd wordt.
   
        ![Share toevoegen](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs19m.png)
   
    Nadat de share is gemaakt met de opgegeven instellingen de **Shares** blade wordt bijgewerkt naar aanleiding van de nieuwe share. Controle- en back-up zijn standaard ingeschakeld voor de share.
   
    ![Share toevoegen](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs22m.png)

## <a name="step-4-connect-to-the-share"></a>Stap 4: Verbinding maken met de share
U moet nu verbinding maken met een of meer shares die u in de vorige stap hebt gemaakt. Voer deze stappen uit op uw Windows Server-host is verbonden met uw virtuele StorSimple-matrix.

#### <a name="to-connect-to-the-share"></a>Verbinding maken met de share
1. Druk op ![](./media/storsimple-virtual-array-deploy3-fs-setup/image22.png) + R. Geef in het venster uitvoeren op de *&#92; &#92;&lt; naam van de bestandsserver&gt;*  als het pad vervangen *naam van de bestandsserver* met de naam van het apparaat dat u hebt toegewezen aan de bestandsserver. Klik op **OK**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image23.png)
2. Hiermee opent u Windows Verkenner. Nu moet u de shares die u hebt gemaakt als de mappen te zien. Selecteer en dubbelklik op een share (map) om de inhoud weer te geven.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image24.png)
3. U kunt nu bestanden toevoegen aan deze shares en maak een back-up.

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van de lokale webgebruikersinterface voor [beheren van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

