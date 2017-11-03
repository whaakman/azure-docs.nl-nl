---
title: Setup van Microsoft Azure StorSimple virtuele matrix iSCSI | Microsoft Docs
description: "Beschrijft hoe u de initiële installatie uitvoert, uw StorSimple iSCSI-server registreren en Apparaatinstelling."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4db116d1-978b-48e8-b572-a719a8425dbc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: 076df176d7cd40c009aea27004fe0f4415999c80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-storsimple-virtual-array--set-up-as-an-iscsi-server-via-azure-portal"></a>Matrix van de virtuele StorSimple-Set up als implementeert een iSCSI-server via de Azure-portal

![Processtroom voor iSCSI-installatie](./media/storsimple-virtual-array-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Overzicht

Deze zelfstudie implementatie geldt voor de Microsoft Azure StorSimple virtuele matrix. Deze zelfstudie wordt beschreven hoe de initiële installatie uitvoeren, uw StorSimple iSCSI-server registreren, de Apparaatinstelling en vervolgens maken, koppelen, initialiseren en volumes op uw virtuele StorSimple-matrix geconfigureerd als een server met iSCSI-indeling. 

De beschreven procedures wordt hier ongeveer 30 minuten duren op 1 uur te voltooien. De informatie die is gepubliceerd in dit artikel is alleen van toepassing op virtuele StorSimple-matrices.

## <a name="setup-prerequisites"></a>Vereisten voor installatie

Voordat u configureren en van uw virtuele StorSimple-matrix instellen, zorg ervoor dat:

* U hebt een virtuele-matrix ingericht en verbonden zoals beschreven in [implementeren StorSimple virtuele matrix - inrichten van een virtuele-matrix in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) of [implementeren StorSimple virtuele matrix - inrichten van een virtuele-matrix in VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
* U hebt de serviceregistratiesleutel van de StorSimple-apparaat Manager-service die u hebt gemaakt voor het beheren van uw virtuele StorSimple-matrices. Zie voor meer informatie **stap 2: de serviceregistratiesleutel ophalen** in [implementeren StorSimple virtuele matrix - voorbereiden van de portal](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
* Als dit de tweede of volgende virtuele matrix dat u met een bestaande Apparaatbeheer StorSimple-service registreren wilt, moet u de gegevensversleutelingssleutel van service hebben. Deze sleutel is gegenereerd nadat het eerste apparaat is geregistreerd met deze service. Als u deze sleutel hebt verloren, Zie **ophalen van de gegevensversleutelingssleutel van service** in [de Webgebruikersinterface gebruiken voor het beheren van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Stapsgewijze setup

Gebruik de volgende stapsgewijze instructies voor het instellen en configureren van uw virtuele StorSimple-matrix:

* [Stap 1: De lokale web UI-installatie is voltooid en Registreer uw apparaat](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
* [Stap 2: De vereiste apparaatinstelling voltooien](#step-2-complete-the-required-device-setup)
* [Stap 3: Een volume toevoegen](#step-3-add-a-volume)
* [Stap 4: Koppelen, initialiseren en formatteren van een volume](#step-4-mount-initialize-and-format-a-volume)

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Stap 1: De lokale web UI-installatie is voltooid en Registreer uw apparaat

#### <a name="to-complete-the-setup-and-register-the-device"></a>De installatie is voltooid en het apparaat registreren

1. Open een browservenster. Verbinding maken met de web-UI-type:
   
    `https://<ip-address of network interface>`
   
    Gebruik de verbindings-URL in de vorige stap hebt genoteerd. U ziet een melding dat er een probleem met het beveiligingscertificaat van de website is fout. Klik op **doorgaan naar deze webpagina**.
   
    ![Fout in beveiligingscertificaat](./media/storsimple-virtual-array-deploy3-iscsi-setup/image3.png)
2. Aanmelden bij de webgebruikersinterface van uw virtuele apparaat als **StorSimpleAdmin**. Voer het beheerderswachtwoord voor apparaten die u hebt gewijzigd in stap 3: het virtuele apparaat te starten in [implementeren StorSimple virtuele matrix - inrichten van een virtueel apparaat in Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) of [implementeren StorSimple virtuele matrix - inrichten van een virtueel apparaat in VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
   
    ![Aanmeldingspagina](./media/storsimple-virtual-array-deploy3-iscsi-setup/image4.png)
3. U gaat naar de **Start** pagina. Deze pagina beschrijft de verschillende instellingen vereist voor het configureren en registreren van het virtuele apparaat met de service Manager voor StorSimple-apparaat. Houd er rekening mee dat de **instellingen**, **Web proxy-instellingen**, en **tijdinstellingen** zijn optioneel. De enige vereiste instellingen zijn **apparaatinstellingen** en **Cloudinstellingen**.
   
    ![startpagina](./media/storsimple-virtual-array-deploy3-iscsi-setup/image5.png)
4. Op de **instellingen** pagina onder **netwerkinterfaces**, DATA 0 wordt automatisch voor u geconfigureerd. Elke netwerkinterface is standaard ingesteld op een IP-adres automatisch ophalen (DHCP). Daarom wordt een IP-adres, subnetmasker en gateway automatisch toegewezen (voor IPv4 en IPv6).
   
    Als u van plan bent uw apparaat te implementeren als een iSCSI-server (inrichten blokopslag), wordt aangeraden uit te schakelen de **IP-adres automatisch ophalen** optie en statische IP-adressen configureren.
   
    ![De pagina met netwerkinstellingen](./media/storsimple-virtual-array-deploy3-iscsi-setup/image6.png)
   
    Als u meer dan één netwerkinterface tijdens het inrichten van het apparaat hebt toegevoegd, kunt u ze hier configureren. Houd er rekening mee dat kunt u de netwerkinterface configureren als IPv4 alleen of als zowel IPv4 als IPv6. Alleen IPv6-configuraties worden niet ondersteund.
5. DNS-servers zijn vereist, omdat ze worden gebruikt wanneer uw apparaat probeert te communiceren met uw cloudserviceproviders voor opslag of uw apparaat met de naam omzetten als deze is geconfigureerd als een bestandsserver. Op de **instellingen** pagina onder de **DNS-servers**:
   
   1. Een primaire en secundaire DNS-server automatisch geconfigureerd. Als u kiest voor het configureren van statische IP-adressen, kunt u DNS-servers. U wordt aangeraden dat u een primaire en secundaire DNS-server configureren voor hoge beschikbaarheid.
   2. Klik op **Toepassen**. Hiermee wordt de toepassing en valideren van de netwerkinstellingen.
6. Op de **apparaatinstellingen** pagina:
   
   1. Wijs een uniek **naam** op uw apparaat. Deze naam mag 1-15 tekens lang zijn en mag letter, cijfers en afbreekstreepjes bevatten.
   2. Klik op de **iSCSI-server** pictogram ![serverpictogram iSCSI-](./media/storsimple-virtual-array-deploy3-iscsi-setup/image7.png) voor de **Type** van apparaten die u maakt. Een iSCSI-server kunt u blok om opslagruimte te creëren.
   3. Geef aan of dit apparaat moet lid zijn van een domein. Als uw apparaat is een server met iSCSI-, is lid worden van het domein optioneel. Als u uw iSCSI-server niet toevoegen aan een domein besluit, klikt u op **toepassen**, wacht u totdat de instellingen worden toegepast en ga vervolgens verder met de volgende stap.
      
       Als u deelnemen aan het apparaat aan een domein wilt. Voer een **domeinnaam**, en klik vervolgens op **toepassen**.
      
      > [!NOTE]
      > Als de iSCSI-server toevoegen aan een domein, zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OE) voor Microsoft Azure Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast.
      > 
      > 
   4. Een dialoogvenster wordt weergegeven. Geef de domeinreferenties van uw in de opgegeven indeling. Klik op het vinkje ![vinkje](./media/storsimple-virtual-array-deploy3-iscsi-setup/image15.png). De domeinreferenties worden gecontroleerd. U ziet een foutbericht weergegeven als de referenties onjuist zijn.
      
       ![referenties](./media/storsimple-virtual-array-deploy3-iscsi-setup/image8.png)
   5. Klik op **Toepassen**. Hiermee wordt de toepassing en valideren van de instellingen voor apparaten.
7. (Optioneel) uw webproxyserver configureren. Hoewel webproxyconfiguratie optioneel is, worden op de hoogte gebracht als u een webproxy gebruikt, u alleen deze hier configureren kunt.
   
    ![webproxy configureren](./media/storsimple-virtual-array-deploy3-iscsi-setup/image9.png)
   
    Op de **Web proxy** pagina:
   
   1. Geef de **Web-URL voor proxy** in deze indeling: *http://host-IP adres* of *FDQN:Port nummer*. Houd er rekening mee dat HTTPS-URL's worden niet ondersteund.
   2. Geef **verificatie** als **Basic** of **geen**.
   3. Als u verificatie gebruikt, wordt ook moet u bieden een **gebruikersnaam** en **wachtwoord**.
   4. Klik op **Toepassen**. Zo valideren en past u de geconfigureerde web proxy-instellingen.
8. (Optioneel) Configureer de instellingen voor uw apparaat, zoals tijdzone en de primaire en secundaire NTP-servers. NTP-servers zijn vereist, omdat uw apparaat de tijd synchroniseren moet zodat verificatie met uw cloudserviceproviders.
   
    ![Tijdinstellingen](./media/storsimple-virtual-array-deploy3-iscsi-setup/image10.png)
   
    Op de **tijdinstellingen** pagina:
   
   1. Selecteer in de vervolgkeuzelijst de **tijdzone** op basis van de geografische locatie op waarin het apparaat wordt geïmplementeerd. De standaardtijdzone voor uw apparaat is PST. Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen.
   2. Geef een **primaire NTP-server** voor uw apparaat of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat in uw netwerk NTP-verkeer kan worden doorgegeven van uw datacenter naar internet.
   3. Geef desgewenst een **secundaire NTP-server** voor uw apparaat.
   4. Klik op **Toepassen**. Zo valideren en past u de geconfigureerde instellingen.
9. De cloud configureren voor uw apparaat. In deze stap maakt u de configuratie van het lokale apparaat voltooien en Registreer het apparaat bij uw StorSimple-apparaat Manager-service.
   
   1. Voer de **serviceregistratiesleutel** die u hebt verkregen **stap 2: de serviceregistratiesleutel ophalen** in [implementeren StorSimple virtuele matrix - voorbereiden van de Portal](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
   2. Als dit niet het eerste apparaat dat u met deze service wilt registreren, moet u bieden de **gegevensversleutelingssleutel van Service**. Deze sleutel is vereist bij de serviceregistratiesleutel extra apparaten registreren bij de service Manager voor StorSimple-apparaat. Raadpleeg voor meer informatie [ophalen van de gegevensversleutelingssleutel van service](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) op uw lokale webgebruikersinterface.
   3. Klik op **registreren**. Hiermee wordt het apparaat opnieuw opgestart. Mogelijk moet u wachten op 2-3 minuten voordat het apparaat is geregistreerd. Nadat het apparaat opnieuw is opgestart, gaat u naar de aanmeldingspagina.
      
      ![Apparaat registreren](./media/storsimple-virtual-array-deploy3-iscsi-setup/image11.png)
10. Ga terug naar de Azure-portal.
11. Navigeer naar de **apparaten** blade van uw service. Als u een groot aantal bronnen hebt, klikt u op **alle resources**, klikt u op de naam van uw service (zoek deze indien nodig) en klik vervolgens op **apparaten**.
12. Op de **apparaten** blade verifiëren dat het apparaat verbinding heeft gemaakt met de service door het opzoeken van de status. Het apparaat moet de status **Gereed voor configuratie** hebben.
    
    ![Apparaat registreren](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png)

## <a name="step-2-configure-the-device-as-iscsi-server"></a>Stap 2: Het apparaat als iSCSI-server configureren

De volgende stappen uitvoeren in de Azure portal om de installatie vereist apparaat te voltooien.

#### <a name="to-configure-the-device-as-iscsi-server"></a>Het apparaat te configureren als iSCSI-server

1. Ga naar de service voor uw StorSimple-Apparaatbeheer en gaat u naar **Management > apparaten**. In de **apparaten** blade, selecteer het apparaat dat u zojuist hebt gemaakt. Dit apparaat wordt weergegeven als **gereed is voor het instellen van**.
   
    ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png) 
2. Klik op het apparaat en ziet u een banner waarin wordt aangegeven dat het apparaat gereed voor installatie is.
   
    ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis2m.png)  
3. Klik op **configureren** op de opdrachtbalk apparaat. Hiermee opent u de **configureren** blade. In de **configureren** blade het volgende doen:
   
   * De naam van de iSCSI-server wordt automatisch gevuld.
   * Zorg ervoor dat de versleuteling van cloudopslag is ingesteld op **ingeschakeld**. Dit zorgt ervoor dat de gegevens van het apparaat naar de cloud verzonden worden versleuteld.
   * Geef een versleutelingssleutel van 32 tekens en opnemen in een app Sleutelbeheer voor toekomstig gebruik.
   * Selecteer een opslagaccount moet worden gebruikt met uw apparaat. In dit abonnement, kunt u een bestaand opslagaccount selecteren, u kunt ook op **toevoegen** een account kiezen uit een ander abonnement.
     
     ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis4m.png)
4. Klik op **configureren** voltooid instellen van de iSCSI-server.
   
    ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis5m.png) 
5. U wordt gewaarschuwd dat het maken van de iSCSI-server uitgevoerd wordt. Nadat de iSCSI-server is gemaakt, de **apparaten** blade wordt bijgewerkt en de status van de bijbehorende **Online**.
   
    ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis9m.png)

## <a name="step-3-add-a-volume"></a>Stap 3: Een volume toevoegen

1. In de **apparaten** blade, selecteer het apparaat dat u zojuist hebt geconfigureerd als een iSCSI-server. Klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **volume toevoegen**. U kunt ook klikken op **+ volume toevoegen** uit de opdrachtbalk. Hiermee opent u de **volume toevoegen** blade.
   
    ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis10m.png)
2. In de **volume toevoegen** blade het volgende doen:
   
   * In de **volumenaam** en voer een unieke naam voor het volume. De naam moet een tekenreeks met 3 tot en met 127 tekens.
   * In de **Type** dropdown lijst, opgeven of maken van een **tiers verdeelde** of **lokaal vastgemaakt** volume. Selecteer voor workloads waarvoor lokale garanties, lage latenties en betere prestaties, **lokaal vastgemaakt** **volume**. Voor alle overige gegevens selecteert **tiers verdeelde** **volume**.
   * In de **capaciteit** veld, geeft u de grootte van het volume. Een gelaagd volume moet liggen tussen 500 GB en 5 TB en een lokaal vastgemaakt volume moet tussen 50 en 500 GB.
     
     Een lokaal vastgemaakt volume is compact ingericht en zorgt ervoor dat de primaire gegevens op het volume op het apparaat blijft en komt niet in de cloud worden gelekt.
     
     Een gelaagd volume is aan de andere kant dun ingericht. Wanneer u een gelaagd volume maakt, wordt ongeveer 10% van de ruimte is ingericht op de lokale laag en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld: als u een volume 1 TB ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB in de cloud moet worden gebruikt wanneer de gegevenslagen. Op zijn beurt hierbij is dat als u uitgevoerd buiten de lokale ruimte op het apparaat, u kunt geen inrichten een gelaagde share (omdat de 10% niet meer beschikbaar).
     
     ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis12.png)
   * Klik op **verbonden hosts**, selecteert u een record voor toegangscontrole (ACR) overeenkomt met de iSCSI-initiator die u wilt verbinding maken met dit volume en klik vervolgens op **Selecteer**. <br><br> 
3. Een nieuwe verbonden als host wilt toevoegen, klikt u op **nieuwe toevoegen**, voer een naam voor de host en de bijbehorende iSCSI Qualified Name (IQN) en klik vervolgens op **toevoegen**. Als u het IQN niet hebt, gaat u naar [bijlage A: het IQN ophalen van een Windows Server-host](#appendix-a-get-the-iqn-of-a-windows-server-host).
   
      ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis15m.png)
4. Wanneer u klaar bent voor het configureren van het volume, klikt u op **OK**. Een volume wordt gemaakt met de opgegeven instellingen en u ziet een melding. Standaard worden controle- en back-up ingeschakeld voor het volume.
   
     ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis18m.png)
5. Om te bevestigen dat het volume is gemaakt, gaat u naar de **Volumes** blade. Hier ziet u het volume dat wordt vermeld.
   
   ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis20m.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Stap 4: Koppelen, initialiseren en formatteren van een volume

De volgende stappen uitvoeren om te koppelen, initialiseren en formatteren van uw StorSimple-volumes op een Windows Server-host.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Een volume koppelen, initialiseren en formatteren

1. Open de **iSCSI-initiator** app op de juiste server.
2. Klik in het venster **Eigenschappen iSCSI-initiator** op het tabblad **Detectie** op **Portal detecteren**.
   
    ![portal detecteren](./media/storsimple-virtual-array-deploy3-iscsi-setup/image22.png)
3. Geef in het dialoogvenster **Doelportaal detecteren** het IP-adres op van de netwerkinterface met iSCSI-functionaliteit en klik vervolgens op **OK**.
   
    ![IP-adres](./media/storsimple-virtual-array-deploy3-iscsi-setup/image23.png)
4. Ga in het venster **Eigenschappen iSCSI-initiator** op het tabblad **Doelen** naar **Gedetecteerde doelen**. (Elk volume wordt een gedetecteerde doel zijn.) De status van het apparaat moet worden weergegeven als **Inactief**.
   
    ![Gedetecteerde doelen](./media/storsimple-virtual-array-deploy3-iscsi-setup/image24.png)
5. Selecteer een apparaat en klik vervolgens op **Connect**. Nadat het apparaat is verbonden, moet de status zijn gewijzigd in **Verbonden**. (Zie voor meer informatie over het gebruik van de Microsoft iSCSI-initiator [installeren en configureren van Microsoft iSCSI-Initiator][1].
   
    ![Selecteer doelapparaat](./media/storsimple-virtual-array-deploy3-iscsi-setup/image25.png)
6. Druk op uw Windows-hots op de Windows-toets + X en klik vervolgens op **Uitvoeren**.
7. Typ in het dialoogvenster **Uitvoeren** **Diskmgmt.msc**. Klik op **OK**. Het dialoogvenster **Schijfbeheer** wordt weergegeven. In het rechterdeelvenster worden de volumes op uw host weergeven.
8. In het venster **Schijfbeheer** worden de gekoppelde volumes weergegeven zoals in de volgende afbeelding. Klik met de rechtermuisknop op het gedetecteerde volume (klik op de naam van de schijf) en klik vervolgens op **Online**.
   
    ![Schijfbeheer](./media/storsimple-virtual-array-deploy3-iscsi-setup/image26.png)
9. Met de rechtermuisknop en selecteer **schijf initialiseren**.
   
    ![Initialiseer de schijf 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image27.png)
10. Selecteer de schijven die u wilt initialiseren, en klik vervolgens in het dialoogvenster **OK**.
    
    ![Initialiseer de schijf 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image28.png)
11. De wizard Nieuw eenvoudig Volume wordt gestart. Selecteer de schijfgrootte van een en klik vervolgens op **volgende**.
    
    ![wizard Nieuw volume 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image29.png)
12. Een stationsletter toewijzen aan het volume en klik vervolgens op **volgende**.
    
    ![wizard Nieuw volume 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image30.png)
13. Geef de parameters voor het volume formatteren. **Op Windows Server wordt alleen NTFS ondersteund.** Stelt de clustergrootte tot 64 kB. Geef een label voor het volume. Het is een aanbevolen procedure voor deze naam identiek zijn aan de naam van het volume dat u hebt opgegeven op uw virtuele StorSimple-matrix. Klik op **Volgende**.
    
    ![wizard Nieuw volume 3](./media/storsimple-virtual-array-deploy3-iscsi-setup/image31.png)
14. Controleer de waarden voor het volume en klik vervolgens op **voltooien**.
    
    ![wizard Nieuw volume 4](./media/storsimple-virtual-array-deploy3-iscsi-setup/image32.png)
    
    De volumes worden weergegeven als **Online** op de **Schijfbeheer** pagina.
    
    ![volumes online](./media/storsimple-virtual-array-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van de lokale webgebruikersinterface voor [beheren van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Bijlage A: het IQN ophalen van een Windows Server-host

Voer de volgende stappen uit om de IQN (iSCSI Qualified Name) van een Windows-host te verkrijgen waarop Windows Server 2012 wordt uitgevoerd.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Het IQN ophalen van een Windows-host

1. Start de Microsoft iSCSI-initiator op uw Windows-host.
2. Selecteer en kopieer in het venster **Eigenschappen iSCSI-initiator** op het tabblad **Configuratie** de tekenreeks in het veld **Naam van initiator**.
   
    ![Eigenschappen iSCSI-initiator](./media/storsimple-virtual-array-deploy3-iscsi-setup/image34.png)
3. Bewaar deze tekenreeks.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



