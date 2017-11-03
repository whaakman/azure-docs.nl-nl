---
title: Inrichten van virtuele StorSimple-matrix in Hyper-V | Microsoft Docs
description: Deze tweede zelfstudie in virtuele StorSimple-matrix implementatie omvat het inrichten van een virtuele-matrix in Hyper-V.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bad431c8958f7d381bb9c0410caa3a57c6e75c19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a>StorSimple virtuele matrix - inrichten in Hyper-V implementeren
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Overzicht
Deze zelfstudie wordt beschreven hoe u voor het inrichten van een virtueel StorSimple-matrix op een hostsysteem met Hyper-V op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2. In dit artikel is van toepassing op de implementatie van het virtuele StorSimple-matrices in Azure portal en Microsoft Azure Government Cloud.

U moet administrator-bevoegdheden om in te richten en configureren van een virtuele-matrix. De inrichting en de initiële installatie kan ongeveer 10 minuten duren om te voltooien.

## <a name="provisioning-prerequisites"></a>Vereiste onderdelen inrichten
Hier vindt u de vereisten voor het inrichten van een virtuele-matrix op een hostsysteem met Hyper-V op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2.

### <a name="for-the-storsimple-device-manager-service"></a>Voor de StorSimple-apparaatbeheerfunctie
Zorg voordat u begint voor het volgende:

* U hebt de stappen in [voorbereiden van de portal voor virtuele StorSimple-matrix](storsimple-virtual-array-deploy1-portal-prep.md).
* Voor Hyper-V hebt u de installatiekopie van het virtuele matrix gedownload vanuit de Azure-portal. Zie voor meer informatie **stap 3: Download de installatiekopie van het virtuele matrix** van [voorbereiden van de portal voor het virtuele StorSimple-matrix handleiding](storsimple-virtual-array-deploy1-portal-prep.md).

  > [!IMPORTANT]
  > De software die wordt uitgevoerd op de virtuele StorSimple-matrix kan alleen worden gebruikt met de service Manager voor StorSimple-apparaat.
  >
  >

### <a name="for-the-storsimple-virtual-array"></a>Voor de virtuele StorSimple-matrix
Voordat u een virtuele-matrix implementeert, zorg ervoor dat:

* U hebt toegang tot een hostsysteem met Hyper-V in Windows Server 2008 R2 of hoger die kan worden gebruikt voor een voorziening van een apparaat.
* Het hostsysteem kan toe te wijzen aan de volgende bronnen voor het inrichten van uw virtuele matrix:

  * Een minimum van 4 kernen.
  * Ten minste 8 GB aan RAM-geheugen. Als u van plan bent de virtuele matrix als bestandsserver te configureren, ondersteunt 8 GB minder dan 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
  * Één netwerkinterface.
  * Een 500 GB virtuele schijf voor gegevens.

### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter
Voordat u begint, controleert u de netwerkvereisten implementeren van een virtueel StorSimple-matrix en het datacenternetwerk op de juiste wijze configureren. Zie voor meer informatie [virtuele StorSimple-matrix netwerkvereisten](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Stapsgewijze inrichten
Als u wilt inrichten en verbinding maken met een virtuele-matrix, moet u de volgende stappen uitvoeren:

1. Zorg ervoor dat het hostsysteem voldoende resources heeft om te voldoen aan de vereisten voor de minimale virtuele matrix.
2. Inrichten van een virtuele-matrix in de hypervisor.
3. Start de virtuele matrix en het IP-adres.

Elk van deze stappen wordt uitgelegd in de volgende secties.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements"></a>Stap 1: Zorg ervoor dat het hostsysteem aan minimale virtuele matrix vereisten voldoet.
Voor het maken van een virtuele-matrix, hebt u het volgende nodig:

* De Hyper-V-rol geïnstalleerd op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 SP1.
* Microsoft Hyper-V Manager op een Microsoft Windows-client is verbonden met de host.

Zorg ervoor dat de onderliggende hardware (hostsysteem), waarop u de virtuele matrix maakt, kan het reserveren van de volgende bronnen voor uw virtuele matrix:

* Een minimum van 4 kernen.
* Ten minste 8 GB aan RAM-geheugen. Als u van plan bent de virtuele matrix als bestandsserver te configureren, ondersteunt 8 GB minder dan 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
* Één netwerkinterface.
* 500 GB virtuele schijf voor systeemgegevens.

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a>Stap 2: Een virtuele-matrix in hypervisor inrichten
Voer de volgende stappen uit voor het inrichten van een apparaat in de hypervisor.

#### <a name="to-provision-a-virtual-array"></a>Voor het inrichten van een virtuele-matrix
1. Kopiëren van de virtuele matrix-installatiekopie naar een lokaal station op de host Windows Server. U hebt deze afbeelding (VHD of VHDX) gedownload via de Azure portal. Noteer de locatie waar u de installatiekopie gekopieerd wanneer u deze installatiekopie later in de procedure gebruikt.
2. Open **Serverbeheer**. Klik in de rechterbovenhoek op **extra** en selecteer **Hyper-V-beheer**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   Als u Windows Server 2008 R2 uitvoert, opent u de Hyper-V-beheer. Klik in Serverbeheer op **functies > Hyper-V > Hyper-V-beheer**.
3. In **Hyper-V-beheer**, in het deelvenster bereik met de rechtermuisknop op uw systeem knooppunt uit om het contextmenu te openen en klik vervolgens op **nieuw** > **virtuele Machine**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. Op de **voordat u begint met** pagina van de Wizard Nieuwe virtuele Machine, klikt u op **volgende**.
5. Op de **naam en locatie opgeven** pagina een **naam** voor uw virtuele matrix. Klik op **Volgende**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. Op de **generatie opgeven** pagina, kies het apparaattype en klik vervolgens op **volgende**. Deze pagina wordt niet weergegeven als u Windows Server 2008 R2.

   * Kies **generatie 2** als u een .vhdx-installatiekopie voor Windows Server 2012 of hoger hebt gedownload.
   * Kies **generatie 1** als u een VHD-installatiekopie voor Windows Server 2008 R2 of hoger hebt gedownload.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. Op de **toewijzen van geheugen** pagina, geeft u een **opstartgeheugen** van ten minste **8192 MB**niet inschakelen van dynamisch geheugen en klik vervolgens op **volgende**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. Op de **netwerk configureren** pagina, geef de virtuele switch die is verbonden met Internet en klik vervolgens op **volgende**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. Op de **virtuele harde schijf aansluiten** pagina **gebruik een bestaande virtuele harde schijf**, geef de locatie van de installatiekopie van het virtuele matrix (vhdx of VHD) en klik vervolgens op **volgende**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. Controleer de **samenvatting** en klik vervolgens op **voltooien** voor het maken van de virtuele machine.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. Om te voldoen aan de minimale vereisten voldoet, moet u 4 kernen. Selecteer uw hostsysteem in om 4 virtuele processors toe de **Hyper-V-beheer** venster. In het rechterdeelvenster onder de lijst met **virtuele Machines**, Ga naar de virtuele machine die u zojuist hebt gemaakt. Selecteer en met de rechtermuisknop op de naam van de machine en selecteer **instellingen**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. Op de **instellingen** pagina in het linkerdeelvenster klikt u op **Processor**. Stel in het rechterdeelvenster **aantal virtuele processors** 4 (of meer). Klik op **Toepassen**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. Om te voldoen aan de minimale vereisten voldoet, moet u ook een 500 GB gegevens van virtuele schijf toe te voegen. In de **instellingen** pagina:

    1. Selecteer in het linkerdeelvenster **SCSI-Controller**.
    2. Selecteer in het rechterdeelvenster **harde schijf** en klik op **toevoegen**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. Op de **harde schijf** pagina de **virtuele hardeschijf** optie en klik op **nieuw**. De **Wizard Nieuwe virtuele hardeschijf** wordt gestart.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. Op de **voordat u begint met** pagina van de Wizard Nieuwe virtuele hardeschijf, klikt u op **volgende**.
16. Op de **schijfindeling kiezen pagina**, accepteer de standaardwaarde van **VHDX** indeling. Klik op **Volgende**. Dit scherm niet wordt weergegeven als Windows Server 2008 R2 wordt uitgevoerd.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. Op de **schijftype kiezen pagina**, instellen van het type virtuele harde schijf als **dynamisch uitbreidbare** (aanbevolen). **Een vaste grootte** schijf wilt werken, maar mogelijk moet u een lange wachttijd. Het is raadzaam dat u niet gebruikt de **Differencing** optie. Klik op **Volgende**. In Windows Server 2012 R2 en Windows Server 2012 **dynamisch uitbreidbare** is de standaardoptie dat in Windows Server 2008 R2, de standaardwaarde is **een vaste grootte**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. Op de **naam en locatie opgeven** pagina een **naam** , evenals **locatie** (u kunt bladeren naar een) voor de gegevensschijf. Klik op **Volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. Op de **schijf configureren** pagina, selecteert u de optie **maken van een nieuwe lege virtuele harde-schijf** en geef de grootte als **500 GB** (of meer). 500 GB is de minimumvereiste, kunt u altijd een grotere schijf inrichten. Houd er rekening mee u niet kunt uitbreiden of verkleinen van de schijf eenmaal ingericht. Raadpleeg voor meer informatie over de grootte van de schijf om in te richten de sizing-sectie in het [best practices document](storsimple-ova-best-practices.md). Klik op **Volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. Op de **samenvatting** pagina, bekijk de details van de van virtuele gegevensschijf en als u tevreden bent, klikt u op **voltooien** om de schijf te maken. De wizard wordt gesloten en een virtuele harde schijf wordt toegevoegd aan uw computer.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. Ga terug naar de **instellingen** pagina. Klik op **OK** sluiten de **instellingen** pagina en terug te keren naar Hyper-V-beheer.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-array-and-get-the-ip"></a>Stap 3: Start de virtuele matrix en het IP-adres ophalen
Voer de volgende stappen uit uw virtuele matrix starten en verbinding maken met het.

#### <a name="to-start-the-virtual-array"></a>Starten van de virtuele matrix
1. Start de virtuele-matrix.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. Nadat het apparaat wordt uitgevoerd, selecteert u het apparaat, klik met de rechtermuisknop en selecteer **Connect**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. U moet 5-10 minuten wachten voor het apparaat niet gereed. Een statusbericht wordt weergegeven op de console om de voortgang te geven. Nadat het apparaat klaar is, gaat u naar **actie**. Druk op `Ctrl + Alt + Delete` zich aanmelden bij de virtuele-matrix. De standaardgebruiker is *StorSimpleAdmin* en is het standaardwachtwoord *Wachtwoord1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. Uit veiligheidsoverwegingen verloopt het beheerderswachtwoord van het apparaat bij de eerste aanmelding. U wordt gevraagd het wachtwoord te wijzigen.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   Voer een wachtwoord dat ten minste 8 tekens bevat. Het wachtwoord moet ten minste 3 buiten de volgende 4 vereisten voldoen: hoofdletters, kleine letters, numerieke en speciale tekens. Geef het wachtwoord ter bevestiging opnieuw. U wordt gewaarschuwd dat het wachtwoord is gewijzigd.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. Nadat het wachtwoord is gewijzigd, kunnen de virtuele matrix mogelijk opnieuw opgestart. Wachten op het apparaat te starten.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    De Windows PowerShell-console van het apparaat wordt samen met een voortgangsbalk weergegeven.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. Stap 6-8 zijn alleen van toepassing wanneer in een omgeving met niet-DHCP-up wordt opgestart. Als u zich in een DHCP-omgeving, slaat u deze stappen over en gaat u naar stap 9. Als u van uw apparaat in niet-DHCP-omgeving opgestart, ziet u het volgende scherm.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    Configureer vervolgens het netwerk.
7. Gebruik de `Get-HcsIpAddress` opdracht om een lijst van de netwerkinterfaces die zijn ingeschakeld op uw virtuele matrix. Als uw apparaat één netwerkinterface is ingeschakeld heeft, wordt de standaardnaam die wordt toegewezen aan deze interface is `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. Gebruik de `Set-HcsIpAddress` cmdlet voor het configureren van het netwerk. Zie het volgende voorbeeld:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. Nadat de eerste installatie voltooid is en het apparaat is opgestart, ziet u de bannertekst apparaat. Noteer het IP-adres en de URL weergegeven in de bannertekst de apparaten te beheren. Gebruik dit IP-adres verbinding maken met de webgebruikersinterface van uw virtuele matrix en de lokale installatie en registratie te voltooien.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. (Optioneel) Deze stap alleen uitvoeren als u uw apparaat in de Cloud Government implementeert. U kunt nu de modus van de Verenigde Staten Federal Information Processing Standard (FIPS) op uw apparaat. De FIPS 140-standaard definieert cryptografische algoritmen die zijn goedgekeurd voor gebruik door ons Federal government computersystemen voor de bescherming van gevoelige gegevens.

    1. Als u de FIPS-modus, voert u de volgende cmdlet:

        `Enable-HcsFIPSMode`
    2. Start opnieuw op uw apparaat wanneer u de FIPS-modus hebt ingeschakeld, zodat de cryptografische validaties te treden laten.

       > [!NOTE]
       > U kunt inschakelen of uitschakelen van FIPS-modus op uw apparaat. Het apparaat tussen FIPS- en niet FIPS-modus afwisselende wordt niet ondersteund.
       >
       >

Als uw apparaat voldoet niet aan de minimale configuratievereisten voor, ziet u de volgende fout in de bannertekst (Zie hieronder). Configuratie van het apparaat zodanig aanpassen dat de machine voldoende resources heeft om te voldoen aan de minimumvereisten. U kunt starten en verbinding maken met het apparaat. Raadpleeg de minimale configuratievereisten voor in [stap 1: Zorg ervoor dat het hostsysteem voldoet met minimale virtuele matrix](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

Als u een andere fout tijdens de eerste configuratie via de lokale webgebruikersinterface geconfronteerd, raadpleegt u de volgende werkstromen:

* Diagnostische tests uitvoeren om te [web UI setup oplossen](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Logboek pakket genereren en logboekbestanden weergeven](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Volgende stappen
* [Uw virtuele StorSimple-matrix instellen als bestandsserver](storsimple-virtual-array-deploy3-fs-setup.md)
* [Uw virtuele StorSimple-matrix ingesteld als een iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md)
