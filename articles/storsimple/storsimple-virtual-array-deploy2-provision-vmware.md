---
title: Inrichten van StorSimple Virtual Array in VMware | Microsoft Docs
description: Deze tweede zelfstudie in een serie van StorSimple Virtual Array implementatie omvat het inrichten van een virtueel apparaat in VMware.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c9fe597957057dc61da5c2b1cf6f9216711764a
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247840"
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>StorSimple Virtual Array - inrichten in VMware implementeren
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Overzicht
Deze zelfstudie wordt beschreven hoe u te richten en te verbinden met een StorSimple Virtual Array op een hostsysteem met VMware ESXi 5.0, 5.5, 6.0 of 6.5. In dit artikel is van toepassing op de implementatie van virtuele StorSimple-matrices in Azure portal en de Microsoft Azure Government-Cloud.

U hebt beheerdersmachtigingen nodig voor het inrichten van een virtueel apparaat en om er verbinding mee te maken. De inrichting en de initiële installatie kan circa tien minuten duren.

## <a name="provisioning-prerequisites"></a>Vereisten voor inrichting
De vereisten voor het inrichten van een virtueel apparaat op een hostsysteem met VMware ESXi 5.0, 5.5, 6.0 of 6.5, zijn er als volgt uit.

### <a name="for-the-storsimple-device-manager-service"></a>Voor de StorSimple-apparaatbeheerfunctie
Zorg voordat u begint voor het volgende:

* U hebt de stappen in [voorbereiden van de portal voor StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md).
* U hebt de installatiekopie van het virtuele apparaat voor VMware gedownload vanuit Azure portal. Zie voor meer informatie, **stap 3: De installatiekopie van het virtuele apparaat downloaden** van [voorbereiden van de portal voor StorSimple Virtual Array handleiding](storsimple-virtual-array-deploy1-portal-prep.md).

### <a name="for-the-storsimple-virtual-device"></a>Voor het virtuele StorSimple-apparaat
Voordat u een virtueel apparaat implementeert, controleert u of:

* U hebt toegang tot een hostsysteem met Hyper-V (2008 R2 of hoger) die kunnen worden gebruikt voor het inrichten van een een-apparaat.
* Het hostsysteem kan de volgende resources reserveren voor het inrichten van uw virtuele apparaat:

  * Minimaal 4 kerngeheugens.
  * Ten minste 8 GB RAM-geheugen. Als u van plan bent de virtuele matrix configureren als bestandsserver, ondersteunt 8 GB in minder dan 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
  * Eén netwerkinterface.
  * Een 500 GB virtuele schijf voor systeemgegevens.

### <a name="for-the-network-in-datacenter"></a>Voor het netwerk in het datacenter
Zorg voordat u begint voor het volgende:

* U hebt gecontroleerd van de vereisten voor het implementeren van een virtuele StorSimple-apparaat en het netwerk van datacenters aan de hand van de vereisten geconfigureerd. 

## <a name="step-by-step-provisioning"></a>Stapsgewijze inrichten
Als u wilt inrichten en maak verbinding met een virtueel apparaat, moet u de volgende stappen uitvoeren:

1. Zorg ervoor dat het hostsysteem voldoende bronnen om te voldoen aan de minimale virtueel apparaat.
2. Inrichten van een virtueel apparaat in de hypervisor.
3. Het virtuele apparaat starten en het IP-adres.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Stap 1: Zorg ervoor dat hostsysteem voldoet aan vereisten voor minimale virtuele apparaten
Voor het maken van een virtueel apparaat, hebt u het volgende nodig:

* Toegang tot een hostsysteem met VMware ESXi-Server 5.0, 5.5, 6.0 of 6.5.
* Een VMware vSphere-client op uw systeem voor het beheren van de ESXi-host.

  * Minimaal 4 kerngeheugens.
  * Ten minste 8 GB RAM-geheugen. Als u van plan bent de virtuele matrix configureren als bestandsserver, ondersteunt 8 GB in minder dan 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
  * Eén netwerkinterface die is verbonden met het netwerk en verkeer naar internet kan routeren. De minimale bandbreedte met Internet moet 5 Mbps om toe te staan voor optimale werking van het apparaat.
  * Een schijf voor virtuele 500 GB voor gegevens.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Stap 2: Een virtueel apparaat in hypervisor inrichten
Voer de volgende stappen uit voor het inrichten van een virtueel apparaat in de hypervisor.

1. Kopieer de installatiekopie van het virtuele apparaat naar uw systeem. U hebt deze virtuele-installatiekopie via de Azure-portal hebt gedownload.

   1. Zorg ervoor dat u de meest recente installatiekopiebestand hebt gedownload. Als u de installatiekopie van het eerder hebt gedownload, downloadt u deze opnieuw zodat u de meest recente installatiekopie. De meest recente installatiekopie heeft twee bestanden (in plaats van één).
   2. Noteer de locatie waar u de installatiekopie naartoe hebt gekopieerd, want u hebt deze installatiekopie verderop in de procedure nodig.

2. Meld u aan bij de ESXi-server met behulp van de vSphere-client. U moet beheerdersbevoegdheden hebben om een virtuele machine te kunnen maken.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. Selecteer in de vSphere-client in de inventaris-sectie in het linkerdeelvenster de ESXi-Server.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. Upload de VMDK naar de ESXi-server. Navigeer naar de **configuratie** tabblad in het rechter deelvenster. Onder **Hardware**, selecteer **opslag**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. In het rechterdeelvenster bij **Gegevensopslag** selecteert u de gegevensopslag waarin u de VMDK wilt uploaden. Het gegevensarchief moet voldoende vrije ruimte voor het besturingssysteem en gegevensschijven.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Klik met de rechtermuisknop op **Browsen in de gegevensopslag** en selecteer deze.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. Een venster met de **gegevensopslagbrowser** wordt weergegeven.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. Klik in de werkbalk op ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) pictogram om een nieuwe map te maken. Geef de mapnaam op en noteer deze. U gebruikt deze mapnaam later bij het maken van een virtuele machine (aanbevolen best practices). Klik op **OK**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. De nieuwe map wordt weergegeven in het linkerdeelvenster van de **gegevensopslagbrowser**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Klik op het pictogram uploaden ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) en selecteer **-bestand uploaden**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. Blader en wijs naar de VMDK-bestanden die u hebt gedownload. Er zijn twee bestanden. Selecteer een bestand dat u wilt uploaden.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. Klik op **Openen**. Het uploaden van het VMDK-bestand met de opgegeven gegevensopslag wordt gestart. Het duurt enkele minuten om het bestand te uploaden.
13. Nadat het uploaden is voltooid, ziet u het bestand in de gegevensopslag in de map die u hebt gemaakt.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Upload nu het tweede VMDK-bestand naar dezelfde gegevensopslag.
14. Ga terug naar het venster van de vSphere-client. Met de ESXi-server is geselecteerd, met de rechtermuisknop en selecteer **nieuwe virtuele Machine**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. Een **nieuwe virtuele Machine maken** venster wordt weergegeven. Op de **configuratie** weergeeft, schakelt de **aangepaste** optie. Klik op **volgende**.
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. Op de **naam en locatie** pagina, geef de naam van uw virtuele machine. Deze naam moet overeenkomen met de mapnaam (aanbevolen) die u eerder in stap 8 hebt opgegeven.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. Op de **opslag** pagina, selecteert u een gegevensopslag die u gebruiken wilt voor het inrichten van uw virtuele machine.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. Op de **versie van de virtuele Machine** weergeeft, schakelt **versie van de virtuele Machine: 8**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. Op de **Gast-besturingssysteem** weergeeft, schakelt de **Gast-besturingssysteem** als **Windows**. Voor **versie**, in de vervolgkeuzelijst, selecteer **Microsoft Windows Server 2012 (64-bits)**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. Op de **CPU's** pagina, aanpassen de **aantal virtuele sockets** en **aantal kernen per virtuele socket** zodat de **totale aantal kernen** is 4 (of meer). Klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. Op de **geheugen** pagina, 8 GB (of meer) aan RAM-geheugen opgeven. Klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. Op de **netwerk** pagina, geeft u het aantal van de netwerkinterfaces. De minimale vereiste is één netwerkinterface.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. Op de **SCSI-Controller** pagina, accepteer de standaardwaarde **LSI Logic SAS-controller**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. Op de **selecteert u een schijf** pagina, kies **gebruik een bestaande virtuele schijf**. Klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. Op de **bestaande schijf selecteren** pagina onder **schijf bestandspad**, klikt u op **Bladeren**. Hiermee opent u een **bladeren gegevensopslag** dialoogvenster. Navigeer naar de locatie waar u de VMDK geüpload. U ziet nu slechts één bestand in het gegevensarchief als de twee bestanden die u in eerste instantie geüpload zijn samengevoegd. Selecteer het bestand en klik op **OK**. Klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. Op de **geavanceerde opties** pagina, accepteer de standaardinstelling en klikt u op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. Op de pagina **Gereed om te voltooien** controleert u alle instellingen voor de nieuwe virtuele machine. Controleer **bewerken van de instellingen van de virtuele machine voor voltooiing**. Klik op **Doorgaan**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. Op de **eigenschappen van virtuele Machines** pagina, in de **Hardware** tabblad, Ga naar het apparaat. Selecteer **nieuwe vaste schijf**. Klik op **Add**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. U ziet een **Hardware toevoegen** venster. Op de **apparaattype** pagina onder **Kies het type apparaat dat u wilt toevoegen**, selecteer **harde schijf**, en klikt u op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. Op de **selecteert u een schijf** pagina, kies **maken van een nieuwe virtuele schijf**. Klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. Op de **maken van een schijf** pagina, wijzigt u de **schijfgrootte** tot 500 GB (of meer). 500 GB is de minimumvereiste, kunt u altijd een grotere schijf inrichten. Houd er rekening mee dat u niet kunt uitbreiden of verkleinen van de schijf eenmaal ingericht. Voor meer informatie over de grootte van de schijf die u wilt inrichten, Raadpleeg de sectie van de grootte in de [best practices document](storsimple-ova-best-practices.md). Onder **schijf inrichten**, selecteer **geconfigureerd voor Thin provisioning**. Klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. Op de **geavanceerde opties** pagina, accepteer de standaardwaarde.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. Op de **Ready to Complete** pagina, Controleer de opties. Klik op **Voltooien**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Ga terug naar de pagina eigenschappen van de virtuele Machine. Een nieuwe harde schijf wordt toegevoegd aan uw virtuele machine. Klik op **Voltooien**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. Met uw virtuele machine in het rechter deelvenster hebt geselecteerd, gaat u naar de **samenvatting** tabblad. Controleer de instellingen voor uw virtuele machine.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

Uw virtuele machine is nu ingericht. De volgende stap is om deze machine te laten werken en om het IP-adres op te halen.

> [!NOTE]
> U kunt het beste installeren VMware-hulpprogramma's niet op uw virtuele array (zoals ingericht hierboven). Als u VMware-hulpprogramma's installeert, leidt dat tot een niet-ondersteunde configuratie.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Stap 3: Het virtuele apparaat starten en het IP-adres ophalen
Voer de volgende stappen uit om uw virtuele apparaat te starten en verbinding te maken met het apparaat.

#### <a name="to-start-the-virtual-device"></a>Het virtuele apparaat starten
1. Start het virtuele apparaat. Selecteer uw apparaat in de vSphere Configuration Manager, in het linkerdeelvenster en met de rechtermuisknop op het contextmenu weer te geven. Selecteer **Aanzetten** en selecteer vervolgens **Inschakelen**. Nu zou uw virtuele machine moeten zijn ingeschakeld. U kunt de status bekijken in de onderste **recente taken** deelvenster van de vSphere-client.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. De installatietaken duurt een paar minuten om te voltooien. Nadat het apparaat wordt uitgevoerd, gaat u naar de **Console** tabblad. Ctrl + Alt + Delete voor aanmelding bij het apparaat verzenden. U kunt ook het geval is, wijst u de cursor in het consolevenster en druk op Ctrl + Alt + Insert. De standaardwaarde is *StorSimpleAdmin* en is het standaardwachtwoord *Wachtwoord1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Uit veiligheidsoverwegingen verloopt het beheerderswachtwoord voor het apparaat na de eerste aanmelding. U wordt gevraagd om het wachtwoord te wijzigen.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. Voer een wachtwoord in van minimaal acht tekens. Het wachtwoord moet 3 van 4 van deze vereisten bevatten: hoofdletters, kleine letters, cijfers en speciale tekens. Voer het wachtwoord opnieuw in ter bevestiging. U wordt gewaarschuwd dat het wachtwoord is gewijzigd.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. Nadat het wachtwoord is gewijzigd, wordt het virtuele apparaat mogelijk opnieuw opgestart. Wacht tot het opnieuw opstarten om te voltooien. De Windows PowerShell-console van het apparaat kan worden weergegeven, samen met een voortgangsbalk weergegeven.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. De stappen 6-8 gelden alleen als het apparaat wordt opgestart in een niet-DHCP-omgeving. In een DHCP-omgeving kunt u deze stappen overslaan en verder gaan met stap 9. Als u uw apparaat in niet-DHCP-omgeving opgestart, ziet u het volgende scherm.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Vervolgens configureert u het netwerk.
7. Gebruik de `Get-HcsIpAddress` opdracht om een lijst van de netwerkinterfaces die zijn ingeschakeld op uw virtuele apparaat. Als voor uw apparaat één netwerkinterface is ingeschakeld, wordt `Ethernet` als standaardnaam aan deze interface toegewezen.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Gebruik de cmdlet `Set-HcsIpAddress` om het netwerk te configureren. Hieronder kunt u een voorbeeld bekijken:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. Nadat de initiële installatie voltooid is en het apparaat is opgestart, ziet u de bannertekst van het apparaat. Noteer het IP-adres en de URL die in de bannertekst worden weergegeven om het apparaat te beheren. U gebruikt dit IP-adres verbinding maken met de webgebruikersinterface van uw virtuele apparaat en de lokale instelling en de registratie te voltooien.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (Optioneel) Voer deze stap alleen als u uw apparaat in Government-Cloud implementeert. U kunt nu de Verenigde Staten Federal Information Processing Standard (FIPS)-modus op uw apparaat. De FIPS 140-standaard definieert cryptografische algoritmen die zijn goedgekeurd voor gebruik door de Amerikaanse federale overheid computersystemen voor de beveiliging van gevoelige gegevens.

    1. Als u wilt de FIPS-modus inschakelen, moet u de volgende cmdlet uitvoeren:

        `Enable-HcsFIPSMode`
    2. Uw apparaat opnieuw opstarten nadat u hebt de FIPS-modus ingeschakeld zodat de cryptografische validaties van kracht.

       > [!NOTE]
       > U kunt inschakelen of uitschakelen van FIPS-modus op uw apparaat. Afwisselende van het apparaat tussen FIPS en niet-FIPS-modus wordt niet ondersteund.
       >
       >

Als uw apparaat niet voldoet aan de minimale configuratievereisten, wordt er een fout weergegeven in de bannertekst (zie hieronder). U moet de apparaatconfiguratie wijzigen zodat er voldoende resources zijn om aan de minimale vereisten te voldoen. Daarna kunt u het apparaat opnieuw opstarten en verbinding maken met het apparaat. Verwijzen naar de minimale configuratievereisten in [stap 1: Zorg ervoor dat het hostsysteem voldoet aan vereisten voor minimale virtuele apparaten](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

Als u een andere fout tijdens de eerste configuratie met behulp van de lokale webgebruikersinterface worden geconfronteerd, raadpleegt u de volgende werkstromen:

* Voer diagnostische tests uit om te [oplossen van de installatie van de web-UI](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Logboek pakket genereren en weergeven van logboekbestanden](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Volgende stappen
* [Instellen van uw StorSimple Virtual Array als bestandsserver](storsimple-virtual-array-deploy3-fs-setup.md)
* [Instellen van uw StorSimple Virtual Array als een iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md)
