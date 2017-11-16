---
title: Inrichten van virtuele StorSimple-matrix in VMware | Microsoft Docs
description: Deze tweede zelfstudie in de reeks voor virtuele StorSimple-matrix implementatie omvat het inrichten van een virtueel apparaat in VMware.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/14/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 495ef6a93ee06423495269306ad06e76dda13e10
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>StorSimple virtuele matrix - inrichten in VMware implementeren
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Overzicht
Deze zelfstudie wordt beschreven hoe in te richten en verbinding maken met een virtueel StorSimple-matrix op een hostsysteem met VMware ESXi 5.0, 5.5 of 6.0. In dit artikel is van toepassing op de implementatie van het virtuele StorSimple-matrices in Azure portal en de Microsoft Azure Government Cloud.

U moet administrator-bevoegdheden om in te richten en verbinding maken met een virtueel apparaat. De inrichting en de initiële installatie kan ongeveer 10 minuten duren om te voltooien.

## <a name="provisioning-prerequisites"></a>Vereiste onderdelen inrichten
De vereisten voor het inrichten van een virtueel apparaat op een hostsysteem met VMware ESXi 5.0, 5.5 of 6.0, zijn als volgt.

### <a name="for-the-storsimple-device-manager-service"></a>Voor de StorSimple-apparaatbeheerfunctie
Zorg voordat u begint voor het volgende:

* U hebt de stappen in [voorbereiden van de portal voor virtuele StorSimple-matrix](storsimple-virtual-array-deploy1-portal-prep.md).
* U kunt de installatiekopie van het virtuele apparaat voor VMware hebt gedownload vanuit de Azure-portal. Zie voor meer informatie **stap 3: Download de installatiekopie van het virtuele apparaat** van [voorbereiden van de portal voor het virtuele StorSimple-matrix handleiding](storsimple-virtual-array-deploy1-portal-prep.md).

### <a name="for-the-storsimple-virtual-device"></a>Voor het virtuele StorSimple-apparaat
Voordat u een virtueel apparaat implementeert, zorg ervoor dat:

* U hebt toegang tot een hostsysteem met Hyper-V (2008 R2 of hoger) die kan worden gebruikt voor een voorziening van een apparaat.
* Het hostsysteem kan toe te wijzen aan de volgende bronnen voor het inrichten van uw virtuele apparaat:

  * Een minimum van 4 kernen.
  * Ten minste 8 GB aan RAM-geheugen. Als u van plan bent de virtuele matrix als bestandsserver te configureren, ondersteunt 8 GB minder dan 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
  * Één netwerkinterface.
  * 500 GB virtuele schijf voor systeemgegevens.

### <a name="for-the-network-in-datacenter"></a>Voor het netwerk in het datacenter
Zorg voordat u begint voor het volgende:

* U hebt gecontroleerd van de vereisten voor het implementeren van een virtueel StorSimple-apparaat en het datacenternetwerk volgens de vereisten geconfigureerd. 

## <a name="step-by-step-provisioning"></a>Stapsgewijze inrichten
Als u verbinding maken met een virtueel apparaat wilt inrichten, moet u de volgende stappen uitvoeren:

1. Zorg ervoor dat het hostsysteem voldoende resources heeft om te voldoen aan de vereisten voor de minimale virtueel apparaat.
2. Een virtueel apparaat in uw hypervisor inrichten.
3. Start het virtuele apparaat en het IP-adres.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Stap 1: Voldoe hostsysteem voldoet aan de vereisten voor minimale virtuele apparaten
Voor het maken van een virtueel apparaat, moet u het:

* Toegang tot een hostsysteem met VMware ESXi Server 5.0, 5.5 of 6.0.
* VMware vSphere client op uw systeem voor het beheren van de ESXi-host.

  * Een minimum van 4 kernen.
  * Ten minste 8 GB aan RAM-geheugen. Als u van plan bent de virtuele matrix als bestandsserver te configureren, ondersteunt 8 GB minder dan 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
  * Netwerkinterfaces verbonden met het netwerk die geschikt is voor het routeren van verkeer naar Internet. De minimale bandbreedte met Internet moet 5 Mbps om toe te staan voor optimale werking van het apparaat.
  * Een 500 GB virtuele schijf voor gegevens.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Stap 2: Een virtueel apparaat in de hypervisor inrichten
Voer de volgende stappen uit voor het inrichten van een virtueel apparaat in de hypervisor.

1. Kopieer de installatiekopie van het virtuele apparaat op uw systeem. U hebt deze installatiekopie van het virtuele via de Azure portal hebt gedownload.

   1. Zorg ervoor dat u de meest recente bestand van de installatiekopie hebt gedownload. Als u de installatiekopie van het eerder hebt gedownload, downloadt u dit opnieuw zodat u de installatiekopie van het meest recente. De installatiekopie van het meest recente heeft twee bestanden (in plaats van een).
   2. Noteer de locatie waar u de installatiekopie gekopieerd wanneer u deze installatiekopie later in de procedure gebruikt.

2. Aanmelden bij de ESXi-server met behulp van de client vSphere. U moet beheerdersbevoegdheden hebben om te maken van een virtuele machine.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. Selecteer in de vSphere-client in de inventaris-sectie in het linkerdeelvenster de ESXi-Server.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. Het VMDK uploaden naar de ESXi-server. Navigeer naar de **configuratie** tabblad in het rechterdeelvenster. Onder **Hardware**, selecteer **opslag**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. In het rechterdeelvenster onder **Datastores**, selecteert u het gegevensarchief waar u wilt uploaden van de VMDK. Het gegevensarchief moet voldoende beschikbare ruimte voor het besturingssysteem en gegevensschijven hebben.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Met de rechtermuisknop en selecteer **bladeren Datastore**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. Een **Datastore Browser** venster wordt weergegeven.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. Klik in de werkbalk op ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) pictogram om een nieuwe map te maken. Geef de mapnaam op en noteer deze. U gebruikt deze mapnaam later bij het maken van een virtuele machine (aanbevolen best practice). Klik op **OK**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. De nieuwe map wordt weergegeven in het linkerdeelvenster van de **Datastore Browser**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Klik op het pictogram uploaden ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) en selecteer **-bestand uploaden**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. Bladeren en wijs de VMDK-bestanden die u hebt gedownload. Er zijn twee bestanden. Selecteer een bestand te uploaden.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. Klik op **Open**. Het uploaden van het VMDK-bestand met het opgegeven gegevensarchief wordt gestart. Duurt enkele minuten voor het bestand te uploaden.
13. Nadat het uploaden voltooid is, ziet u het bestand in het gegevensarchief in de map die u hebt gemaakt.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Het tweede VMDK-bestand nu uploaden naar het hetzelfde gegevensarchief.
14. Ga terug naar het venster vSphere-client. Met ESXi-server is geselecteerd, klik met de rechtermuisknop en selecteer **nieuwe virtuele Machine**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. Een **nieuwe virtuele Machine maken** venster weergegeven. Op de **configuratie** pagina de **aangepaste** optie. Klik op **Volgende**.
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. Op de **naam en locatie** pagina, geeft u de naam van uw virtuele machine. Deze naam moet overeenkomen met de mapnaam (aanbevolen) die u eerder in stap 8 opgeven.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. Op de **opslag** pagina, selecteert u een gegevensarchief die u gebruiken wilt voor het inrichten van uw virtuele machine.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. Op de **versie van de virtuele Machine** pagina **versie van de virtuele Machine: 8**. Versies 8 tot en met 11 worden alle ondersteund.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. Op de **gastbesturingssysteem** pagina de **gastbesturingssysteem** als **Windows**. Voor **versie**, selecteert u in de vervolgkeuzelijst **Microsoft Windows Server 2012 (64-bits)**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. Op de **CPU's** pagina, past u de **aantal virtuele sockets** en **aantal kernen per virtuele socket** zodat de **totaal aantal kernen** is 4 (of meer). Klik op **Volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. Op de **geheugen** pagina, geeft u 8 GB (of meer) RAM-geheugen. Klik op **Volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. Op de **netwerk** pagina, geeft u het nummer van de netwerkinterfaces. De minimale vereiste is één netwerkinterface.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. Op de **SCSI-Controller** pagina, accepteer de standaardinstelling **LSI Logic SAS-controller**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. Op de **selecteert u een schijf** pagina **gebruik een bestaande virtuele schijf**. Klik op **Volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. Op de **bestaande schijf selecteren** pagina onder **schijf bestandspad**, klikt u op **Bladeren**. Hiermee opent u een **Datastores Bladeren** dialoogvenster. Navigeer naar de locatie waar u de VMDK geüpload. U ziet nu slechts één bestand in het gegevensarchief zoals het samenvoegen van de twee bestanden die u in eerste instantie geüpload. Selecteer het bestand en klik op **OK**. Klik op **Volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. Op de **geavanceerde opties** pagina, accepteer de standaardinstelling en klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. Op de **gereed om te voltooien** controleert u alle instellingen die zijn gekoppeld aan de nieuwe virtuele machine. Controleer **bewerken van de instellingen van de virtuele machine voor voltooiing**. Klik op **gaan**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. Op de **eigenschappen van virtuele Machines** pagina in de **Hardware** tabblad, zoekt u de hardware. Selecteer **nieuwe vaste schijf**. Klik op **Add**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. U ziet een **Hardware toevoegen** venster. Op de **apparaattype** pagina onder **Kies het type apparaat dat u wilt toevoegen**, selecteer **harde schijf**, en klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. Op de **selecteert u een schijf** pagina **een nieuwe virtuele schijf maken**. Klik op **Volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. Op de **maken van een schijf** pagina, wijzigt u de **schijfgrootte** tot 500 GB (of meer). 500 GB is de minimumvereiste, kunt u altijd een grotere schijf inrichten. Houd er rekening mee u niet kunt uitbreiden of verkleinen van de schijf eenmaal ingericht. Raadpleeg voor meer informatie over de grootte van de schijf om in te richten de sizing-sectie in het [best practices document](storsimple-ova-best-practices.md). Onder **schijf inrichting**, selecteer **Thin provisioning**. Klik op **Volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. Op de **geavanceerde opties** pagina, accepteer de standaardinstelling.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. Op de **gereed om te voltooien** controleert u de schijfopties. Klik op **Voltooien**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Terug naar de pagina eigenschappen van virtuele Machine. Een nieuwe harde schijf is toegevoegd aan de virtuele machine. Klik op **Voltooien**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. Met de virtuele machine wordt geselecteerd in het rechterdeelvenster, gaat u naar de **samenvatting** tabblad. Controleer de instellingen voor uw virtuele machine.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

De virtuele machine is nu ingericht. De volgende stap is als u wilt inschakelen op deze computer en het IP-adres.

> [!NOTE]
> U wordt aangeraden Installeer VMware tools niet op uw virtuele matrix (zoals ingericht hierboven). Installatie van VMware-hulpprogramma's leidt tot een niet-ondersteunde configuratie.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Stap 3: Het virtuele apparaat starten en het IP-adres ophalen
Voer de volgende stappen uit om te beginnen uw virtuele apparaat en er verbinding mee maken.

#### <a name="to-start-the-virtual-device"></a>Starten van het virtuele apparaat
1. Start het virtuele apparaat. Selecteer het apparaat in de vSphere Configuration Manager in het linkerdeelvenster en met de rechtermuisknop op het contextmenu weer te geven. Selecteer **Power** en selecteer vervolgens **inschakelen**. Dit moet de virtuele machine inschakelen. U kunt de status bekijken in de onderste **recente taken** deelvenster van de client vSphere.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. De instellingstaken duurt enkele minuten om te voltooien. Zodra het apparaat wordt uitgevoerd, gaat u naar de **Console** tabblad. Ctrl + Alt + Delete voor aanmelding bij het apparaat verzenden. U kunt ook de cursor in het consolevenster wijst en druk op Ctrl + Alt + Insert. De standaardgebruiker is *StorSimpleAdmin* en is het standaardwachtwoord *Wachtwoord1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Uit veiligheidsoverwegingen verloopt het beheerderswachtwoord van het apparaat bij de eerste aanmelding. U wordt gevraagd het wachtwoord te wijzigen.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. Voer een wachtwoord dat ten minste 8 tekens bevat. Het wachtwoord moet bevatten 3 van 4 van deze vereisten voldoet: hoofdletters, kleine letters, numerieke en speciale tekens. Geef het wachtwoord ter bevestiging opnieuw. U wordt gewaarschuwd dat het wachtwoord is gewijzigd.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. Nadat het wachtwoord is gewijzigd, wordt het virtuele apparaat mogelijk opnieuw opgestart. Wacht totdat het opnieuw opstarten om te voltooien. De Windows PowerShell-console van het apparaat mogelijk weergegeven samen met een voortgangsbalk weergegeven.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. Stap 6-8 zijn alleen van toepassing wanneer in een omgeving met niet-DHCP-up wordt opgestart. Als u zich in een DHCP-omgeving, slaat u deze stappen over en gaat u naar stap 9. Als u van uw apparaat in niet-DHCP-omgeving opgestart, ziet u het volgende scherm.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Configureer vervolgens het netwerk.
7. Gebruik de `Get-HcsIpAddress` opdracht om een lijst van de netwerkinterfaces die zijn ingeschakeld op uw virtuele apparaat. Als uw apparaat één netwerkinterface is ingeschakeld heeft, wordt de standaardnaam die wordt toegewezen aan deze interface is `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Gebruik de `Set-HcsIpAddress` cmdlet voor het configureren van het netwerk. Een voorbeeld wordt hieronder weergegeven:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. Nadat de eerste installatie voltooid is en het apparaat is opgestart, ziet u de bannertekst apparaat. Noteer het IP-adres en de URL weergegeven in de bannertekst de apparaten te beheren. U gebruikt dit IP-adres verbinding maken met de webgebruikersinterface van uw virtuele apparaat en de lokale installatie en registratie te voltooien.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (Optioneel) Deze stap alleen uitvoeren als u uw apparaat in de Cloud Government implementeert. U kunt nu de modus van de Verenigde Staten Federal Information Processing Standard (FIPS) op uw apparaat. De FIPS 140-standaard definieert cryptografische algoritmen die zijn goedgekeurd voor gebruik door ons Federal government computersystemen voor de bescherming van gevoelige gegevens.

    1. Als u de FIPS-modus, voert u de volgende cmdlet:

        `Enable-HcsFIPSMode`
    2. Start opnieuw op uw apparaat wanneer u de FIPS-modus hebt ingeschakeld, zodat de cryptografische validaties te treden laten.

       > [!NOTE]
       > U kunt inschakelen of uitschakelen van FIPS-modus op uw apparaat. Het apparaat tussen FIPS- en niet FIPS-modus afwisselende wordt niet ondersteund.
       >
       >

Als uw apparaat voldoet niet aan de minimale configuratievereisten voor, ziet u een fout in de bannertekst (Zie hieronder). U moet de apparaatconfiguratie wijzigen zodat er voldoende bronnen om te voldoen aan de minimale vereisten voldoet. U kunt starten en verbinding maken met het apparaat. Raadpleeg de minimale configuratievereisten voor in [stap 1: Zorg ervoor dat het hostsysteem voldoet aan vereisten voor minimale virtuele apparaten](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

Als u een andere fout tijdens de eerste configuratie via de lokale webgebruikersinterface geconfronteerd, raadpleegt u de volgende werkstromen:

* Diagnostische tests uitvoeren om te [web UI setup oplossen](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Logboek pakket genereren en logboekbestanden weergeven](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Volgende stappen
* [Uw virtuele StorSimple-matrix instellen als bestandsserver](storsimple-virtual-array-deploy3-fs-setup.md)
* [Uw virtuele StorSimple-matrix ingesteld als een iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md)
