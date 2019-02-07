---
title: Setup van Microsoft Azure StorSimple Virtual Array iSCSI-server | Microsoft Docs
description: Beschrijft hoe u uitvoeren van de eerste configuratie, uw StorSimple-iSCSI-server te registreren en Apparaatinstelling voltooien.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 4db116d1-978b-48e8-b572-a719a8425dbc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: c0a86b76622862b477d539c25dd98c925f09192c
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55812035"
---
# <a name="deploy-storsimple-virtual-array--set-up-as-an-iscsi-server-via-azure-portal"></a>Implementeren StorSimple Virtual Array – Set up als een iSCSI-server via Azure portal

![Processtroom voor iSCSI-installatie](./media/storsimple-virtual-array-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Overzicht

In deze zelfstudie implementatie geldt voor de Microsoft Azure StorSimple Virtual Array. In deze zelfstudie wordt beschreven hoe u uitvoeren van de initiële installatie, registreren van uw StorSimple-iSCSI-server, voltooi de installatie van het apparaat, en vervolgens maken, koppelen, initialiseren en formatteren van volumes op uw StorSimple Virtual Array geconfigureerd als een iSCSI-server. 

De beschreven procedures duurt ongeveer 30 minuten hier naar één uur om te voltooien. De informatie die is gepubliceerd in dit artikel is alleen van toepassing op virtuele StorSimple-matrices.

## <a name="setup-prerequisites"></a>Vereisten voor installatie

Voordat u configureren en van uw StorSimple Virtual Array instellen, zorg ervoor dat:

* U hebt een virtuele matrix ingericht en verbonden zoals beschreven in [implementeren StorSimple Virtual Array - richt een virtuele matrix in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) of [implementeren StorSimple Virtual Array - richt een virtuele-matrix in VMware ](storsimple-virtual-array-deploy2-provision-vmware.md).
* Hebt u de serviceregistratiesleutel uit de StorSimple Device Manager-service die u hebt gemaakt voor het beheren van uw virtuele StorSimple-matrices. Zie voor meer informatie, **stap 2: De serviceregistratiesleutel ophalen** in [implementeren StorSimple Virtual Array - voorbereiden van de portal](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
* Als dit de tweede of volgende virtuele matrix die u met een bestaande StorSimple Device Manager-service registreert, moet u de versleutelingssleutel voor servicegegevens hebben. Deze sleutel is gegenereerd toen het eerste apparaat is geregistreerd met deze service. Als u deze sleutel hebt verloren, Zie **ophalen van de versleutelingssleutel voor servicegegevens** in [de Webgebruikersinterface gebruiken voor het beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Stapsgewijze instelling

Gebruik de volgende stapsgewijze instructies voor het instellen en configureren uw StorSimple Virtual Array:

* [Stap 1: De lokale web-UI-installatie is voltooid en uw apparaat registreren](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
* Stap 2: De vereiste apparaatconfiguratie voltooien
* [Stap 3: Een volume toevoegen](#step-3-add-a-volume)
* [Stap 4: Koppelen, initialiseren en formatteren van een volume](#step-4-mount-initialize-and-format-a-volume)

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Stap 1: De lokale web-UI-installatie is voltooid en uw apparaat registreren

#### <a name="to-complete-the-setup-and-register-the-device"></a>De installatie is voltooid en het apparaat registreren

1. Open een browservenster. Verbinding maken met de web-UI-type:
   
    `https://<ip-address of network interface>`
   
    Gebruik de verbindings-URL in de vorige stap hebt genoteerd. U ziet een melding dat er een probleem met het beveiligingscertificaat van de website is fout. Klik op **doorgaan naar deze webpagina**.
   
    ![beveiliging-certificaatfout](./media/storsimple-virtual-array-deploy3-iscsi-setup/image3.png)
2. Aanmelden bij de webgebruikersinterface van uw virtueel apparaat als **StorSimpleAdmin**. Voer het beheerderswachtwoord voor het apparaat dat u hebt gewijzigd in stap 3: Starten van het virtuele apparaat in de [implementeren StorSimple Virtual Array - een virtueel apparaat in Hyper-V inrichten](storsimple-virtual-array-deploy2-provision-hyperv.md) of [implementeren StorSimple Virtual Array - inrichten van een virtueel apparaat in VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
   
    ![Aanmeldingspagina opgeven](./media/storsimple-virtual-array-deploy3-iscsi-setup/image4.png)
3. U gaat naar de **Start** pagina. Deze pagina beschrijft de verschillende instellingen vereist voor het configureren en het virtuele apparaat registreren bij de StorSimple Device Manager-service. Houd er rekening mee dat de **netwerkinstellingen**, **Web proxyinstellingen**, en **tijdinstellingen** zijn optioneel. De enige vereiste instellingen zijn **apparaatinstellingen** en **Cloudinstellingen**.
   
    ![Startpagina](./media/storsimple-virtual-array-deploy3-iscsi-setup/image5.png)
4. Op de **netwerkinstellingen** pagina onder **netwerkinterfaces**, DATA 0 wordt automatisch voor u geconfigureerd. Elke netwerkinterface is standaard ingesteld op een IP-adres automatisch ophalen (DHCP). Daarom wordt een IP-adres, subnetmasker en gateway automatisch toegewezen (voor zowel IPv4 als IPv6).
   
    Als u van plan bent uw apparaat implementeren als een iSCSI-server (op blokopslag inrichten), wordt aangeraden dat u CredSSP de **IP-adres automatisch ophalen** optie en configureer statische IP-adressen.
   
    ![Pagina netwerkinstellingen](./media/storsimple-virtual-array-deploy3-iscsi-setup/image6.png)
   
    Als u meer dan één netwerkinterface tijdens het inrichten van het apparaat hebt toegevoegd, kunt u deze hier configureren. Houd er rekening mee dat kunt u de netwerkinterface configureren als IPv4 alleen of als zowel IPv4 als IPv6. Alleen IPv6-configuraties worden niet ondersteund.
5. DNS-servers zijn vereist, omdat ze worden gebruikt wanneer uw apparaat probeert te communiceren met uw cloudserviceproviders voor opslag of oplossen door uw apparaat met de naam die is geconfigureerd als een bestandsserver. Op de **netwerkinstellingen** pagina onder de **DNS-servers**:
   
   1. Een primaire en secundaire DNS-server wordt automatisch geconfigureerd. Als u kiest voor het configureren van statische IP-adressen, kunt u DNS-servers opgeven. Voor hoge beschikbaarheid, wordt u aangeraden dat u een primaire en secundaire DNS-server configureren.
   2. Klik op **Toepassen**. Dit zal toepassen en valideren van de netwerkinstellingen.
6. Op de **apparaatinstellingen** pagina:
   
   1. Toewijzen van een unieke **naam** op uw apparaat. Deze naam 1-15 tekens bestaan en mag letters, cijfers en afbreekstreepjes bevatten.
   2. Klik op de **iSCSI-server** pictogram ![iSCSI-serverpictogram](./media/storsimple-virtual-array-deploy3-iscsi-setup/image7.png) voor de **Type** van apparaten die u maakt. Een iSCSI-server kunt u de blokopslag inrichten.
   3. Opgeven of u wilt dat dit apparaat aan een domein worden toegevoegd. Als het apparaat een iSCSI-server is, is klikt u vervolgens lid worden van het domein optioneel. Als u uw iSCSI-server niet toevoegen aan een domein besluit, klikt u op **toepassen**, wacht voordat de instellingen worden toegepast en ga vervolgens verder met de volgende stap.
      
       Als u deelnemen aan het apparaat aan een domein wilt. Voer een **domeinnaam**, en klik vervolgens op **toepassen**.
      
      > [!NOTE]
      > Als uw iSCSI-server toevoegen aan een domein, zorgt u ervoor dat uw virtuele array in een eigen organisatie-eenheid (OE) van Microsoft Azure Active Directory is en geen groepsbeleidsobjecten (GPO) worden toegepast op deze.
      > 
      > 
   4. Een dialoogvenster wordt weergegeven. Voer de domeinreferenties van uw in de indeling die is opgegeven. Klik op het vinkje ![vinkje](./media/storsimple-virtual-array-deploy3-iscsi-setup/image15.png). De domeinreferenties worden gecontroleerd. U ziet een foutbericht weergegeven als de referenties onjuist zijn.
      
       ![referenties](./media/storsimple-virtual-array-deploy3-iscsi-setup/image8.png)
   5. Klik op **Toepassen**. Dit zal toepassen en valideren van de instellingen voor apparaten.
7. (Optioneel) Configureer uw webproxyserver. Hoewel webproxyconfiguratie optioneel is, er rekening mee dat als u een webproxy gebruikt, u alleen deze hier configureren kunt.
   
    ![Webproxy configureren](./media/storsimple-virtual-array-deploy3-iscsi-setup/image9.png)
   
    Op de **Web proxy** pagina:
   
   1. Geef de **Web-URL van proxy** in deze indeling:  *http://host-IP adres* of *FQDN: Port number*. Houd er rekening mee dat HTTPS-URL's worden niet ondersteund.
   2. Geef **verificatie** als **Basic** of **geen**.
   3. Als u van verificatie gebruikmaakt, ook moet u voor een **gebruikersnaam** en **wachtwoord**.
   4. Klik op **Toepassen**. Dit wordt gevalideerd en de geconfigureerde web proxy-instellingen toepassen.
8. (Optioneel) Configureer de tijdinstellingen voor uw apparaat, zoals tijdzone en de primaire en secundaire NTP-servers. NTP-servers zijn vereist, omdat het apparaat de tijd synchroniseren moet zodat deze kan worden geverifieerd met uw cloud-serviceproviders.
   
    ![Tijdinstellingen](./media/storsimple-virtual-array-deploy3-iscsi-setup/image10.png)
   
    Op de **tijdinstellingen** pagina:
   
   1. Selecteer in de vervolgkeuzelijst de **tijdzone** op basis van de geografische locatie waar het apparaat wordt geïmplementeerd. De standaardtijdzone voor uw apparaat is PST. Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen.
   2. Geef een **primaire NTP-server** voor uw apparaat in of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat in uw netwerk NTP-verkeer kan worden doorgegeven van uw datacenter naar internet.
   3. Geef eventueel een **secundaire NTP-server** voor uw apparaat.
   4. Klik op **Toepassen**. Dit wordt gevalideerd en de geconfigureerde time-instellingen toepassen.
9. Configureer de cloudinstellingen voor uw apparaat. In deze stap maakt u de configuratie van het lokale apparaat voltooien en vervolgens het apparaat te registreren bij uw StorSimple Device Manager-service.
   
   1. Voer de **serviceregistratiesleutel** die u hebt verkregen **stap 2: De serviceregistratiesleutel ophalen** in [implementeren StorSimple Virtual Array - voorbereiden van de Portal](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
   2. Als dit niet het eerste apparaat dat u met deze service registreren wilt is, moet u voor de **versleutelingssleutel voor servicegegevens**. Deze sleutel is vereist bij de serviceregistratiesleutel extra apparaten registreren bij de StorSimple Device Manager-service. Raadpleeg voor meer informatie, [ophalen van de versleutelingssleutel voor servicegegevens](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) op de lokale webgebruikersinterface.
   3. Klik op **registreren**. Hiermee wordt het apparaat opnieuw opgestart. Mogelijk moet u wachten op 2-3 minuten voordat het apparaat is geregistreerd. Nadat het apparaat opnieuw is opgestart, gaat u naar de aanmeldingspagina.
      
      ![Apparaat registreren](./media/storsimple-virtual-array-deploy3-iscsi-setup/image11.png)
10. Ga terug naar de Azure-portal.
11. Navigeer naar de **apparaten** blade van uw service. Als u een groot aantal bronnen hebt, klikt u op **alle resources**, klikt u op de naam van uw service (zoek deze indien nodig) en klik vervolgens op **apparaten**.
12. Op de **apparaten** blade controleren of het apparaat heeft is verbonden met de service door het opzoeken van de status. Het apparaat moet de status **Gereed voor configuratie** hebben.
    
    ![Apparaat registreren](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png)

## <a name="step-2-configure-the-device-as-iscsi-server"></a>Stap 2: Het apparaat configureren als iSCSI-server

De volgende stappen uitvoeren in Azure portal om de installatie vereist apparaat te voltooien.

#### <a name="to-configure-the-device-as-iscsi-server"></a>Het apparaat configureren als iSCSI-server

1. Ga naar uw StorSimple Device Manager-service en ga vervolgens naar **Management > apparaten**. In de **apparaten** blade, selecteer het apparaat dat u zojuist hebt gemaakt. Dit apparaat zou worden weergegeven als **klaar om in te stellen**.
   
    ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png) 
2. Klik op het apparaat en ziet u een banner bericht waarin staat dat het apparaat gereed voor installatie is.
   
    ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis2m.png)  
3. Klik op **configureren** op de opdrachtbalk van het apparaat. Hiermee opent u de **configureren** blade. In de **configureren** blade, doet u het volgende:
   
   * De naam van de iSCSI-server wordt automatisch ingevuld.
   * Zorg ervoor dat de versleuteling van cloudopslag is ingesteld op **ingeschakeld**. Dit zorgt ervoor dat de gegevens van het apparaat verzonden naar de cloud is versleuteld.
   * Geef een versleutelingssleutel van 32 tekens op en leg deze vast in een beheerapp voor toekomstig gebruik.
   * Selecteer een opslagaccount dat moet worden gebruikt met uw apparaat. In dit abonnement, kunt u een bestaand opslagaccount selecteren of u kunt klikken op **toevoegen** een account kiezen uit een ander abonnement.
     
     ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis4m.png)
4. Klik op **configureren** om uit te voeren van het instellen van de iSCSI-server.
   
    ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis5m.png) 
5. U wordt gewaarschuwd dat de iSCSI-server gemaakt wordt. Nadat de iSCSI-server is gemaakt, de **apparaten** blade wordt bijgewerkt en wordt de status van het desbetreffende apparaat **Online**.
   
    ![Apparaat configureren als iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis9m.png)

## <a name="step-3-add-a-volume"></a>Stap 3: Een volume toevoegen

1. In de **apparaten** blade, selecteer het apparaat dat u zojuist hebt geconfigureerd als een iSCSI-server. Klik op **...**  (u kunt ook met de rechtermuisknop op in deze rij) en selecteer in het contextmenu **volume toevoegen**. U kunt ook klikken op **volume toevoegen** vanuit de opdrachtbalk. Hiermee opent u de **volume toevoegen** blade.
   
    ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis10m.png)
2. In de **volume toevoegen** blade, doet u het volgende:
   
   * In de **volumenaam** veld, voer een unieke naam voor het volume. De naam moet een tekenreeks met 3 tot en met 127 tekens.
   * In de **Type** vervolgkeuzelijst lijst, Geef op of maak een **gelaagd** of **lokaal vastgemaakt** volume. Voor workloads waarvoor lokale garanties, lage latenties en betere prestaties, selecteert u **lokaal vastgemaakt** **volume**. Voor alle andere gegevens, selecteert u **gelaagd** **volume**.
   * In de **capaciteit** veld, geeft u de grootte van het volume. Een gelaagd volume moet liggen tussen 500 GB en 5 TB en een lokaal vastgemaakt volume moet tussen de 50 GB en 500 GB.
     
     Een lokaal vastgemaakt volume is compact ingericht en zorgt ervoor dat de primaire gegevens in het volume op het apparaat blijft en wordt niet naar de cloud worden gelekt.
     
     Een gelaagd volume is aan de andere kant dun ingericht. Wanneer u een gelaagd volume maakt, wordt ongeveer 10% van de ruimte is ingericht op de lokale laag en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld, als u een volume van 1 TB hebt ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB zou in de cloud worden gebruikt wanneer de gegevenslagen. Dit impliceert op zijn beurt is dat als u van de lokale ruimte op het apparaat uitvoeren niet kan u een gelaagde share inrichten (omdat de 10% niet beschikbaar is).
     
     ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis12.png)
   * Klik op **verbonden hosts**, selecteert u een Acces controlerecord (ACR) overeenkomt met de iSCSI-initiator die u wilt verbinding maken met dit volume en klik vervolgens op **Selecteer**. <br><br> 
3. Als u wilt een nieuw verbonden host toevoegt, klikt u op **nieuwe toevoegen**, voer een naam voor de host en de iSCSI Qualified Name (IQN) en klik vervolgens op **toevoegen**. Als u het IQN niet hebt, gaat u naar [bijlage A: Het IQN ophalen van een Windows Server-host](#appendix-a-get-the-iqn-of-a-windows-server-host).
   
      ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis15m.png)
4. Wanneer u klaar bent met het volume configureren, klikt u op **OK**. Een volume wordt gemaakt met de opgegeven instellingen en u ziet een melding. Standaard wordt de controle- en back-up voor het volume worden ingeschakeld.
   
     ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis18m.png)
5. Om te bevestigen of het volume is gemaakt, gaat u naar de **Volumes** blade. Hier ziet u het volume dat wordt vermeld.
   
   ![Een volume toevoegen](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis20m.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Stap 4: Een volume koppelen, initialiseren en formatteren

Voer de volgende stappen uit als u wilt koppelen, initialiseren en formatteren van uw StorSimple-volumes op een Windows Server-host.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Een volume koppelen, initialiseren en formatteren

1. Open de **iSCSI-initiator** app op de juiste server.
2. Klik in het venster **Eigenschappen iSCSI-initiator** op het tabblad **Detectie** op **Portal detecteren**.
   
    ![detecteren van de portal](./media/storsimple-virtual-array-deploy3-iscsi-setup/image22.png)
3. Geef in het dialoogvenster **Doelportaal detecteren** het IP-adres op van de netwerkinterface met iSCSI-functionaliteit en klik vervolgens op **OK**.
   
    ![IP-adres](./media/storsimple-virtual-array-deploy3-iscsi-setup/image23.png)
4. Ga in het venster **Eigenschappen iSCSI-initiator** op het tabblad **Doelen** naar **Gedetecteerde doelen**. (Elk volume worden een doel voor gedetecteerde.) De status van het apparaat moet worden weergegeven als **Inactief**.
   
    ![Gedetecteerde doelen](./media/storsimple-virtual-array-deploy3-iscsi-setup/image24.png)
5. Selecteer een doelapparaat en klik vervolgens op **Connect**. Nadat het apparaat is verbonden, moet de status zijn gewijzigd in **Verbonden**. (Zie voor meer informatie over het gebruik van de Microsoft iSCSI-initiator [installeren en configureren van Microsoft iSCSI-Initiator][1].
   
    ![Selecteer doelapparaat](./media/storsimple-virtual-array-deploy3-iscsi-setup/image25.png)
6. Druk op uw Windows-hots op de Windows-toets + X en klik vervolgens op **Uitvoeren**.
7. Typ in het dialoogvenster **Uitvoeren** **Diskmgmt.msc**. Klik op **OK**. Het dialoogvenster **Schijfbeheer** wordt weergegeven. In het rechterdeelvenster worden de volumes op uw host weergeven.
8. In het venster **Schijfbeheer** worden de gekoppelde volumes weergegeven zoals in de volgende afbeelding. Klik met de rechtermuisknop op het gedetecteerde volume (klik op de naam van de schijf) en klik vervolgens op **Online**.
   
    ![Schijfbeheer](./media/storsimple-virtual-array-deploy3-iscsi-setup/image26.png)
9. Met de rechtermuisknop op en selecteer **schijf initialiseren**.
   
    ![Initialiseer de schijf 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image27.png)
10. In het dialoogvenster, selecteer de schijven voor het initialiseren en klik vervolgens op **OK**.
    
    ![Initialiseer de schijf 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image28.png)
11. De wizard Nieuw eenvoudig Volume wordt gestart. Selecteer een grootte van de schijf en klik vervolgens op **volgende**.
    
    ![wizard Nieuw volume 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image29.png)
12. Een stationsletter toewijzen aan het volume en klik vervolgens op **volgende**.
    
    ![wizard Nieuw volume 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image30.png)
13. Geef de parameters bij zodat het volume geformatteerd. **Op Windows Server wordt alleen NTFS ondersteund.** Stel de grootte van toewijzingseenheid tot 64 kB. Geef een label voor het volume. Het is een aanbevolen procedure voor deze naam moet gelijk zijn aan de naam van het volume dat u hebt opgegeven op de StorSimple Virtual Array. Klik op **volgende**.
    
    ![wizard Nieuw volume 3](./media/storsimple-virtual-array-deploy3-iscsi-setup/image31.png)
14. Controleer de waarden voor het volume en klik vervolgens op **voltooien**.
    
    ![wizard Nieuw volume 4](./media/storsimple-virtual-array-deploy3-iscsi-setup/image32.png)
    
    De volumes worden weergegeven als **Online** op de **Schijfbeheer** pagina.
    
    ![volumes online](./media/storsimple-virtual-array-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van de lokale web-UI te [beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Bijlage A: Het IQN ophalen van een Windows Server-host

Voer de volgende stappen uit om de IQN (iSCSI Qualified Name) van een Windows-host te verkrijgen waarop Windows Server 2012 wordt uitgevoerd.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Het IQN ophalen van een Windows-host

1. Start de Microsoft iSCSI-initiator op uw Windows-host.
2. Selecteer en kopieer in het venster **Eigenschappen iSCSI-initiator** op het tabblad **Configuratie** de tekenreeks in het veld **Naam van initiator**.
   
    ![Eigenschappen iSCSI-initiator](./media/storsimple-virtual-array-deploy3-iscsi-setup/image34.png)
3. Bewaar deze tekenreeks.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



