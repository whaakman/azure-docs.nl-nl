---
title: StorSimple Virtual Array als bestandsserver instellen | Microsoft Docs
description: Deze derde zelfstudie in StorSimple Virtual Array implementatie zorgt u voor het instellen van een virtueel apparaat als bestandsserver.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: f609f6ff-0927-48bb-a68a-6d8985d2fe34
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a931b303e40e41bc23e8b586e1d37e600625b1a8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57881058"
---
# <a name="deploy-storsimple-virtual-array---set-up-as-file-server-via-azure-portal"></a>Implementeren StorSimple Virtual Array - Set om als bestandsserver via Azure portal
![](./media/storsimple-virtual-array-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Inleiding
Dit artikel wordt beschreven hoe u uitvoeren van de eerste configuratie, registreren van uw StorSimple-bestandsserver, voltooi de installatie van het apparaat en maakt en verbinding maken met SMB-shares. Dit is de laatste artikel in de reeks van zelfstudies voor implementatie vereist voor het implementeren van uw virtuele array volledig als een bestandsserver of een iSCSI-server.

De installatie- en configuratieproces kan ongeveer tien minuten duren om uit te voeren. De informatie in dit artikel geldt alleen voor de implementatie van de StorSimple Virtual Array. Voor de implementatie van apparaten uit de StorSimple 8000-serie, gaat u naar: [Implementeren van uw StorSimple 8000-apparaat met Update 2](storsimple-deployment-walkthrough-u2.md).

## <a name="setup-prerequisites"></a>Vereisten voor installatie
Voordat u configureren en van uw StorSimple Virtual Array instellen, zorg ervoor dat:

* U hebt een virtuele matrix ingericht en verbonden zoals beschreven in de [inrichten van een StorSimple Virtual Array in Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) of [inrichten van een StorSimple Virtual Array in VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
* Hebt u de serviceregistratiesleutel uit de StorSimple Device Manager-service die u hebt gemaakt voor het beheren van virtuele StorSimple-matrices. Zie voor meer informatie, [stap 2: De serviceregistratiesleutel ophalen](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) voor StorSimple Virtual Array.
* Als dit de tweede of volgende virtuele matrix die u met een bestaande StorSimple Device Manager-service registreert, moet u de versleutelingssleutel voor servicegegevens hebben. Deze sleutel is gegenereerd toen het eerste apparaat is geregistreerd met deze service. Als u deze sleutel hebt verloren, Zie [ophalen van de versleutelingssleutel voor servicegegevens](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) voor uw StorSimple Virtual Array.

## <a name="step-by-step-setup"></a>Stapsgewijze instelling
Gebruik de volgende stapsgewijze instructies om te configureren en instellen van uw StorSimple Virtual Array.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Stap 1: De lokale web-UI-installatie is voltooid en uw apparaat registreren
#### <a name="to-complete-the-setup-and-register-the-device"></a>De installatie is voltooid en het apparaat registreren
1. Open een browservenster en maak verbinding met de lokale webgebruikersinterface. Type:
   
   `https://<ip-address of network interface>`
   
   Gebruik de verbindings-URL in de vorige stap hebt genoteerd. U ziet een foutbericht dat aangeeft dat er een probleem met het beveiligingscertificaat van de website is. Klik op **doorgaan naar deze webpagina**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image2.png)
2. Aanmelden bij de webgebruikersinterface van uw virtuele array als **StorSimpleAdmin**. Voer het beheerderswachtwoord voor het apparaat dat u hebt gewijzigd in stap 3: Start de virtuele matrix [inrichten van een StorSimple Virtual Array in Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) of in [inrichten van een StorSimple Virtual Array in VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image3.png)
3. U gaat naar de **Start** pagina. Deze pagina beschrijft de verschillende instellingen vereist voor het configureren en de virtuele array niet registreren bij de StorSimple Device Manager-service. De **netwerkinstellingen**, **Web proxyinstellingen**, en **tijdinstellingen** zijn optioneel. De enige vereiste instellingen zijn **apparaatinstellingen** en **Cloudinstellingen**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image4.png)
4. In de **netwerkinstellingen** pagina onder **netwerkinterfaces**, DATA 0 wordt automatisch voor u geconfigureerd. Elke netwerkinterface is standaard ingesteld op het IP-adres automatisch ophalen (DHCP). Een IP-adres, subnetmasker en gateway worden dus automatisch toegewezen (voor zowel IPv4 als IPv6).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image5.png)
   
   Als u meer dan één netwerkinterface tijdens het inrichten van het apparaat hebt toegevoegd, kunt u deze hier configureren. Houd er rekening mee dat kunt u de netwerkinterface configureren als IPv4 alleen of als zowel IPv4 als IPv6. Alleen IPv6-configuraties worden niet ondersteund.
5. DNS-servers zijn vereist, omdat ze worden gebruikt wanneer uw apparaat probeert te communiceren met uw cloudserviceproviders voor opslag of op te lossen van uw apparaat met de naam wanneer geconfigureerd als een bestandsserver. In de **netwerkinstellingen** pagina onder de **DNS-servers**:
   
   1. Een primaire en secundaire DNS-server worden automatisch geconfigureerd. Als u kiest voor het configureren van statische IP-adressen, kunt u DNS-servers opgeven. Voor hoge beschikbaarheid, wordt u aangeraden dat u een primaire en secundaire DNS-server configureren.
   2. Klik op **toepassen** wilt toepassen en valideren van de netwerkinstellingen.
6. In de **apparaatinstellingen** pagina:
   
   1. Toewijzen van een unieke **naam** op uw apparaat. Deze naam 1-15 tekens bestaan en mag letters, cijfers en afbreekstreepjes bevatten.
   2. Klik op de **bestandsserver** pictogram ![](./media/storsimple-virtual-array-deploy3-fs-setup/image6.png) voor de **Type** van apparaten die u maakt. Een bestandsserver kunt u gedeelde mappen te maken.
   3. Als uw apparaat een bestandsserver is, moet u het apparaat toevoegen aan een domein. Voer een **domeinnaam**.
   4. Klik op **Toepassen**.
7. Een dialoogvenster wordt weergegeven. Voer de domeinreferenties van uw in de indeling die is opgegeven. Klik op het vinkje. De domeinreferenties zijn geverifieerd. U ziet een foutbericht weergegeven als de referenties onjuist zijn.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image7.png)
8. Klik op **Toepassen**. Dit zal toepassen en valideren van de instellingen voor apparaten.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image8.png)
   
   > [!NOTE]
   > Zorg ervoor dat uw virtuele array kan in een eigen organisatie-eenheid (OE) voor Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast op deze of overgenomen. Groepsbeleid kan toepassingen zoals antivirussoftware installeren op de StorSimple Virtual Array. Aanvullende software geïnstalleerd, wordt niet ondersteund en kan leiden tot beschadigde gegevens. 
   > 
   > 
9. (Optioneel) Configureer uw webproxyserver. Hoewel webproxyconfiguratie optioneel is, er rekening mee dat als u een webproxy gebruikt, u alleen deze hier configureren kunt.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image9.png)
   
   In de **Web proxy** pagina:
   
   1. Geef de **Web-URL van proxy** in deze indeling: *http://&lt;host-IP-adres of FQDN-naam&gt;: poortnummer*. Houd er rekening mee dat HTTPS-URL's worden niet ondersteund.
   2. Geef **verificatie** als **Basic** of **geen**.
   3. Als u verificatie gebruikt, ook moet u voor een **gebruikersnaam** en **wachtwoord**.
   4. Klik op **Toepassen**. Dit wordt gevalideerd en de geconfigureerde web proxy-instellingen toepassen.
10. (Optioneel) Configureer de tijdinstellingen voor uw apparaat, zoals tijdzone en de primaire en secundaire NTP-servers. NTP-servers zijn vereist, omdat het apparaat de tijd synchroniseren moet zodat deze kan worden geverifieerd met uw cloud-serviceproviders.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/image10.png)
    
    In de **tijdinstellingen** pagina:
    
    1. Selecteer in de vervolgkeuzelijst de **tijdzone** op basis van de geografische locatie waar het apparaat wordt geïmplementeerd. De standaardtijdzone voor uw apparaat is PST. Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen.
    2. Geef een **primaire NTP-server** voor uw apparaat in of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat in uw netwerk NTP-verkeer kan worden doorgegeven van uw datacenter naar internet.
    3. Geef eventueel een **secundaire NTP-server** voor uw apparaat.
    4. Klik op **Toepassen**. Dit wordt gevalideerd en de geconfigureerde time-instellingen toepassen.
11. Configureer de cloudinstellingen voor uw apparaat. In deze stap maakt u de configuratie van het lokale apparaat voltooien en vervolgens het apparaat te registreren bij uw StorSimple Device Manager-service.
    
    1. Voer de **serviceregistratiesleutel** die u hebt verkregen [stap 2: De serviceregistratiesleutel ophalen](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) voor StorSimple Virtual Array.
    2. Als dit het eerste apparaat met deze service registreert, u krijgt de **versleutelingssleutel voor servicegegevens**. Kopieer deze sleutel en bewaar deze op een veilige plaats. Deze sleutel is vereist bij de serviceregistratiesleutel extra apparaten registreren bij de StorSimple Device Manager-service. 
       
       Als dit niet het eerste apparaat dat u met deze service registreren wilt is, moet u voor de versleutelingssleutel voor servicegegevens. Raadpleeg voor meer informatie als u de [versleutelingssleutel voor servicegegevens](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) op de lokale webgebruikersinterface.
    3. Klik op **registreren**. Hiermee wordt het apparaat opnieuw opgestart. Mogelijk moet u wachten op 2-3 minuten voordat het apparaat is geregistreerd. Nadat het apparaat opnieuw is opgestart, gaat u naar de aanmeldingspagina.
       
       ![](./media/storsimple-virtual-array-deploy3-fs-setup/image13.png)
12. Ga terug naar de Azure-portal. Ga naar **alle resources**, zoek naar uw StorSimple Device Manager-service.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/searchdevicemanagerservice1.png) 
13. In de gefilterde lijst, selecteer uw StorSimple Device Manager-service en navigeer vervolgens naar **Management > apparaten**. In de **apparaten** blade, Controleer of het apparaat is verbonden met de service en heeft de status **klaar om in te stellen**.
    
    ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png)

## <a name="step-2-configure-the-device-as-file-server"></a>Stap 2: Het apparaat configureren als bestandsserver
Voer de volgende stappen uit in de [Azure-portal](https://portal.azure.com/) om de installatie vereist apparaat te voltooien.

#### <a name="to-configure-the-device-as-file-server"></a>Het apparaat configureren als bestandsserver
1. Ga naar uw StorSimple Device Manager-service en ga vervolgens naar **Management > apparaten**. In de **apparaten** blade, selecteer het apparaat dat u zojuist hebt gemaakt. Dit apparaat zou worden weergegeven als **klaar om in te stellen**.
   
   ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png) 
2. Klik op het apparaat en ziet u een banner bericht waarin staat dat het apparaat gereed voor installatie is.
   
    ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs3m.png)
3. Klik op **configureren** op de opdrachtbalk. Hiermee opent u de **configureren** blade. In de **configureren** blade, doet u het volgende:
   
   1. Naam van de bestandsserver wordt automatisch ingevuld.
    
   2. Zorg ervoor dat de versleuteling van cloudopslag is ingesteld op **ingeschakeld**. Hiermee worden alle gegevens die worden verzonden naar de cloud gecodeerd. 
    
   3. Een 256-bits AES-sleutel wordt gebruikt door de gebruiker gedefinieerde sleutel voor versleuteling. Geef een sleutel van 32 tekens en Bevestig de sleutel om het te bevestigen. Noteer de sleutel in een beheerapp voor toekomstig gebruik.
    
   4. Klik op **vereiste instellingen configureren** om op te geven van de referenties van het opslagaccount moet worden gebruikt met uw apparaat. Klik op **nieuwe toevoegen** als er geen opslagaccountreferenties geconfigureerd zijn. **Zorg ervoor dat het opslagaccount dat u gebruikt ondersteunt blok-blobs. Pagina-blobs worden niet ondersteund.** Meer informatie over [blobs en pagina-blobs geblokkeerd](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).
   
      ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs6m.png) 
4. In de **toevoegen van een opslagaccountreferenties** blade, doet u het volgende: 

    1. Huidige abonnement kiezen als het opslagaccount zich in hetzelfde abonnement als de service. Geef andere is het storage-account is buiten het service-abonnement. 
    
    2. Kies een bestaand opslagaccount in de vervolgkeuzelijst. 
    
    3. De locatie automatisch ingevuld op basis van het opgegeven opslagaccount. 
    
    4. SSL om te controleren of een beveiligd netwerk communicatiekanaal tussen het apparaat en de cloud in te schakelen.
    
    5. Klik op **toevoegen** om toe te voegen deze referentie voor het opslagaccount. 
   
        ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs8m.png)

5. Als de opslagaccountreferentie is gemaakt, de **configureren** blade wordt bijgewerkt om referenties van het opgegeven opslagaccount weer te geven. Klik op **Configureren**
   
   ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs11m.png)
   
   U ziet een dat een bestand server wordt gemaakt. Als de server is gemaakt, wordt u krijgt.
   
   ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs13m.png)
   
   Status van het apparaat wordt ook gewijzigd in **Online**.
   
   ![Een bestandsserver configureren](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs14m.png)
   
   U kunt doorgaan met het toevoegen van een share.

## <a name="step-3-add-a-share"></a>Stap 3: Een share toevoegen
Voer in de [Azure-portal](https://portal.azure.com/) de volgende stappen uit om een share te maken.

#### <a name="to-create-a-share"></a>Een bestandsshare maken
1. Selecteer het bestand server apparaat dat u hebt geconfigureerd in de vorige stap en klikt u op **...**  (of klik met de rechtermuisknop). Selecteer in het contextmenu **toevoegen share**. U kunt ook klikken op **+ bestandsshare toevoegen** op de opdrachtbalk van het apparaat.
   
   ![Een share toevoegen](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs15m.png)
2. Geef de volgende instellingen voor delen:

   1. Een unieke naam voor de share. De naam moet een tekenreeks met 3 tot en met 127 tekens.
    
   2. Een optionele **beschrijving** voor de share. De beschrijving kunt identificeren de eigenaren van de share.
    
   3. Een **Type** voor de share. Het type kan worden **gelaagd** of **lokaal vastgemaakt**, met lagen wordt de standaardwaarde. Voor workloads waarvoor lokale garanties, lage latenties en betere prestaties, selecteert u een **lokaal vastgemaakt** delen. Voor alle andere gegevens, selecteert u een **gelaagd** delen.
      Een lokaal vastgemaakt share is compact ingericht en zorgt ervoor dat de primaire gegevens op de share lokaal op het apparaat blijft en wordt niet naar de cloud worden gelekt. Een gelaagde share wordt aan de andere kant dun ingericht. Bij het maken van een gelaagde share wordt 10% van de ruimte is ingericht op de lokale laag en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld, als u een volume van 1 TB hebt ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB zou in de cloud worden gebruikt wanneer de gegevenslagen. Dit wordt op zijn beurt betekent dat als u van de lokale ruimte op het apparaat uitvoert, kunt u een gelaagde share niet inrichten.
   
   4. In de **standaard volledige machtigingen ingesteld op** veld, de machtigingen toewijzen aan de gebruiker of de groep die toegang heeft tot deze share. Geef de naam van de gebruiker of de gebruikersgroep in *john\@contoso.com* indeling. U wordt aangeraden dat u een gebruikersgroep (in plaats van één gebruiker) gebruiken om toe te staan van beheerdersbevoegdheden voor toegang tot deze shares. Als u de bevoegdheden hier hebt toegewezen, kunt u Verkenner gebruiken om de bevoegdheden te wijzigen.
   
   5. Klik op **toevoegen** om de share te maken. 
    
       ![Een share toevoegen](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs18m.png)
   
       U ontvangt een melding als wordt begonnen met het maken van de share.
   
       ![Een share toevoegen](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs19m.png)
   
      Nadat de share is gemaakt met de opgegeven instellingen, de **Shares** blade wordt bijgewerkt om de nieuwe share weer te geven. Controle- en back-up zijn standaard ingeschakeld voor de share.
   
      ![Een share toevoegen](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs22m.png)

## <a name="step-4-connect-to-the-share"></a>Stap 4: Verbinding maken met de share
U moet nu verbinding maken met een of meer shares die u in de vorige stap hebt gemaakt. Voer deze stappen uit op uw Windows Server-host die is verbonden met uw StorSimple Virtual Array.

#### <a name="to-connect-to-the-share"></a>Verbinding maken met de share
1. Druk op ![](./media/storsimple-virtual-array-deploy3-fs-setup/image22.png) + R. Geef in het venster uitvoeren op de *&#92; &#92; &lt;bestandsservernaam&gt;* als pad voor de vervangen *bestandsservernaam* met de naam van het apparaat die u hebt toegewezen aan het bestand de server. Klik op **OK**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image23.png)
2. Hiermee opent u Windows Verkenner. U moet nu de shares kunnen zien die u als mappen hebt gemaakt. Selecteer een share (map) en dubbelklik erop om de inhoud te bekijken.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image24.png)
3. U kunt nu bestanden toevoegen aan deze shares en maak een back-up.

## <a name="next-steps"></a>Volgende stappen
Informatie over het gebruik van de lokale web-UI te [beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

