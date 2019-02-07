---
title: Inrichten van StorSimple Virtual Array in Hyper-V | Microsoft Docs
description: Deze tweede zelfstudie in StorSimple Virtual Array implementatie omvat het inrichten van een virtuele matrix in Hyper-V.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5104d630e2b4e97b80a6fedfb6d863061c2722fb
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55810538"
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a>StorSimple Virtual Array - inrichten in Hyper-V implementeren
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Overzicht
Deze zelfstudie wordt beschreven hoe u voor het inrichten van een StorSimple Virtual Array op een hostsysteem met Hyper-V op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2. In dit artikel is van toepassing op de implementatie van virtuele StorSimple-matrices in Azure portal en Microsoft Azure Government-Cloud.

U moet administrator-bevoegdheden om in te richten en een virtuele matrix configureren. De inrichting en de initiële installatie kan circa tien minuten duren.

## <a name="provisioning-prerequisites"></a>Vereisten voor inrichting
Hier vindt u de vereisten voor het inrichten van een virtuele matrix op een hostsysteem met Hyper-V op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2.

### <a name="for-the-storsimple-device-manager-service"></a>Voor de StorSimple-apparaatbeheerfunctie
Zorg voordat u begint voor het volgende:

* U hebt de stappen in [voorbereiden van de portal voor StorSimple Virtual Array](storsimple-virtual-array-deploy1-portal-prep.md).
* U kunt de installatiekopie van het virtuele matrix voor Hyper-V hebt gedownload vanuit Azure portal. Zie voor meer informatie, **stap 3: De installatiekopie van de virtuele matrix downloaden** van [voorbereiden van de portal voor StorSimple Virtual Array handleiding](storsimple-virtual-array-deploy1-portal-prep.md).

  > [!IMPORTANT]
  > De software die wordt uitgevoerd op de StorSimple Virtual Array kan alleen worden gebruikt met de StorSimple Device Manager-service.
  >
  >

### <a name="for-the-storsimple-virtual-array"></a>Voor de StorSimple Virtual Array
Voordat u een virtuele matrix implementeert, controleert u of:

* U hebt toegang tot een hostsysteem met Hyper-V op Windows Server 2008 R2 of hoger die kan worden gebruikt voor het inrichten van een een-apparaat.
* Het hostsysteem kan besteden aan de volgende bronnen voor het inrichten van uw virtuele matrix:

  * Minimaal 4 kerngeheugens.
  * Ten minste 8 GB RAM-geheugen. Als u van plan bent de virtuele matrix configureren als bestandsserver, ondersteunt 8 GB in minder dan 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
  * Eén netwerkinterface.
  * Een schijf voor virtuele 500 GB voor gegevens.

### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter
Voordat u begint, controleert u de vereisten voor het implementeren van een StorSimple Virtual Array en het netwerk van datacenters op de juiste wijze configureren. Zie voor meer informatie, [StorSimple Virtual Array netwerkvereisten](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Stapsgewijze inrichten
Als u wilt inrichten en maak verbinding met een virtuele-matrix, moet u de volgende stappen uitvoeren:

1. Zorg ervoor dat het hostsysteem voldoende bronnen om te voldoen aan de minimale virtuele matrix: vereisten.
2. Richt een virtuele-matrix in de hypervisor.
3. De virtuele matrix starten en het IP-adres.

Elk van deze stappen wordt uitgelegd in de volgende secties.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements"></a>Stap 1: Zorg ervoor dat het hostsysteem voldoet aan de minimale virtuele matrix: vereisten
Voor het maken van een virtuele-matrix, hebt u het volgende nodig:

* De Hyper-V-rol geïnstalleerd op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 SP1.
* Microsoft Hyper-V-beheer op een Microsoft Windows-client moet zijn verbonden met de host.

Zorg ervoor dat de onderliggende hardware (hostsysteem) waarop u de virtuele matrix maakt kunnen de volgende bronnen reserveren voor uw virtuele matrix:

* Minimaal 4 kerngeheugens.
* Ten minste 8 GB RAM-geheugen. Als u van plan bent de virtuele matrix configureren als bestandsserver, ondersteunt 8 GB in minder dan 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
* Eén netwerkinterface.
* Een 500 GB virtuele schijf voor systeemgegevens.

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a>Stap 2: Een virtuele matrix in hypervisor inrichten
Voer de volgende stappen uit om een apparaat in de hypervisor in te richten.

#### <a name="to-provision-a-virtual-array"></a>Voor het inrichten van een virtuele matrix
1. Kopieer de installatiekopie van het virtuele matrix op een lokaal station op uw Windows Server-host. U hebt deze installatiekopie (VHD of VHDX) gedownload via de Azure-portal. Noteer de locatie waar u de installatiekopie naartoe hebt gekopieerd, want u hebt deze installatiekopie verderop in de procedure nodig.
2. Open **Serverbeheer**. Klik in de rechterbovenhoek op **Hulpprogramma's** en selecteer **Hyper-V-beheer**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   Als u Windows Server 2008 R2 uitvoert, opent u de Hyper-V-beheer. Klik in Serverbeheer op **rollen > Hyper-V > Hyper-V-beheer**.
3. In **Hyper-V-beheer** klikt u in het scopedeelvenster met de rechtermuisknop op uw systeemknooppunt om het contextmenu te openen. Klik vervolgens op **Nieuw** > **Virtuele machine**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. Klik op de pagina **Voordat u begint** van de wizard Nieuwe virtuele machine op **Volgende**.
5. Op de **naam en locatie opgeven** pagina een **naam** voor uw virtuele matrix. Klik op **volgende**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. Op de **generatie opgeven** pagina, kies het afbeeldingstype en klik vervolgens op **volgende**. Deze pagina niet wordt weergegeven als u Windows Server 2008 R2.

   * Kies **generatie 2** als u een vhdx-installatiekopie voor Windows Server 2012 of hoger hebt gedownload.
   * Kies **generatie 1** als u een VHD-installatiekopie voor Windows Server 2008 R2 of hoger hebt gedownload.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. Op de pagina **Geheugen toewijzen** geeft u een **Opstartgeheugen** van ten minste **8192 MB** op. Schakel Dynamisch geheugen niet in en klik op **Volgende**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. Op de pagina **Netwerk configureren** geeft de virtuele switch op die is verbonden met internet en klikt u op **Volgende**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. Op de **virtuele harde schijf aansluiten** pagina, kies **gebruik een bestaande virtuele harde schijf**, geef de locatie van de installatiekopie van het virtuele matrix (vhdx of VHD) en klik vervolgens op **volgende**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. Controleer de **Samenvatting** en klik op **Voltooien** om de virtuele machine te maken.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. U hebt 4 kerngeheugens nodig om te voldoen aan de minimale vereisten. Selecteer uw hostsysteem in het venster **Hyper-V-beheer** om 4 virtuele processors toe te voegen. Zoek in het rechterdeelvenster onder de lijst met **Virtuele Machines** de virtuele machine die u zojuist hebt gemaakt. Klik met de rechtermuisknop op de naam van de machine en selecteer **Instellingen**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. Klik op de pagina **Instellingen** in het linkerdeelvenster op **Processor**. Stel in het rechterdeelvenster **Aantal virtuele processors** in op 4 (of meer). Klik op **Toepassen**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. Om te voldoen aan de minimale vereisten voldoet, moet u ook een virtuele gegevensschijf van 500 GB toevoegen. Op de pagina **Instellingen** doet u het volgende:

    1. Selecteer in het linkerdeelvenster de optie **SCSI-controller**.
    2. In het rechterdeelvenster selecteert u de optie **Harde schijf** en klikt u op **Toevoegen**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. Op de pagina **Harde schijf** selecteert u de optie **Virtuele harde schijf** en klikt u op **Nieuw**. De **Wizard Nieuwe virtuele harde schijf** wordt gestart.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. Op de pagina **Voordat u begint** van de wizard Nieuwe virtuele harde schijf klikt u op **Volgende**.
16. Op de pagina **Schijfindeling kiezen** accepteert u de standaardindeling **VHDX**. Klik op **volgende**. Dit scherm wordt niet weergegeven als met Windows Server 2008 R2.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. Op de pagina **Schijftype kiezen** stelt u het type virtuele harde schijf in op **Dynamisch uitbreidbaar** (aanbevolen). Een schijf van het type **Vaste grootte** werkt waarschijnlijk ook, maar dan moet u mogelijk lang wachten. U wordt geadviseerd om niet de optie **Differentiërende** te gebruiken. Klik op **volgende**. In Windows Server 2012 R2 en Windows Server 2012, **dynamisch uitbreidbare** is de standaardoptie dat in Windows Server 2008 R2, de standaardwaarde is **vaste grootte**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. Op de pagina **Naam en locatie opgeven** geeft u een **Naam** en een **Locatie** voor de gegevensschijf op (u kunt naar een locatie bladeren). Klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. Op de **schijf configureren** pagina, selecteert u de optie **nieuwe lege virtuele harde schijf maken** en geef de grootte als **500 GB** (of meer). 500 GB is de minimumvereiste, kunt u altijd een grotere schijf inrichten. Houd er rekening mee dat u niet kunt uitbreiden of verkleinen van de schijf eenmaal ingericht. Voor meer informatie over de grootte van de schijf die u wilt inrichten, Raadpleeg de sectie van de grootte in de [best practices document](storsimple-ova-best-practices.md). Klik op **volgende**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. Op de pagina **Samenvatting** controleert u de details van uw virtuele gegevensschijf en als u tevreden bent, klikt u op **Voltooien** om de schijf te maken. De wizard wordt gesloten en er wordt een virtuele harde schijf aan uw machine toegevoegd.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. Ga terug naar de pagina **Instellingen**. Klik op **OK** om de pagina **Instellingen** te sluiten en terug te keren naar het venster Hyper-V-beheer.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-array-and-get-the-ip"></a>Stap 3: De virtuele matrix starten en het IP-adres ophalen
De volgende stappen uitvoeren om uw virtuele matrix starten en er verbinding mee maken.

#### <a name="to-start-the-virtual-array"></a>De virtuele matrix starten
1. De virtuele matrix starten.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. Wanneer het apparaat is gestart, selecteert u het apparaat, klikt u met de rechtermuisknop en selecteert u **Verbinding maken**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. U moet wachten 5-10 minuten voor het apparaat gereed is. Er wordt een statusbericht op de console weergegeven om de voortgang aan te geven. Wanneer het apparaat gereed is, gaat u naar **Actie**. Druk op `Ctrl + Alt + Delete` aanmelden bij de virtuele matrix. De standaardwaarde is *StorSimpleAdmin* en is het standaardwachtwoord *Wachtwoord1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. Uit veiligheidsoverwegingen verloopt het beheerderswachtwoord voor het apparaat na de eerste aanmelding. U wordt gevraagd om het wachtwoord te wijzigen.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   Voer een wachtwoord in van minimaal acht tekens. Het wachtwoord moet voldoen aan ten minste 3 van de volgende 4 vereisten: hoofdletters, kleine letters, cijfers en speciale tekens. Voer het wachtwoord opnieuw in ter bevestiging. U ontvangt een melding dat het wachtwoord is gewijzigd.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. Nadat het wachtwoord is gewijzigd, wordt de virtuele matrix wordt opnieuw opgestart. Wacht tot het apparaat is gestart.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    De Windows PowerShell-console van het apparaat wordt weergegeven, samen met een voortgangsbalk.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. De stappen 6-8 gelden alleen als het apparaat wordt opgestart in een niet-DHCP-omgeving. In een DHCP-omgeving kunt u deze stappen overslaan en verder gaan met stap 9. Als u uw apparaat in niet-DHCP-omgeving opgestart, ziet u het volgende scherm.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    Vervolgens configureert u het netwerk.
7. Gebruik de `Get-HcsIpAddress` opdracht om een lijst van de netwerkinterfaces die zijn ingeschakeld op uw virtuele matrix. Als voor uw apparaat één netwerkinterface is ingeschakeld, wordt `Ethernet` als standaardnaam aan deze interface toegewezen.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. Gebruik de cmdlet `Set-HcsIpAddress` om het netwerk te configureren. Zie het volgende voorbeeld:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. Wanneer de initiële installatie is voltooid en het apparaat is opgestart, ziet u de bannertekst van het apparaat. Noteer het IP-adres en de URL die in de bannertekst worden weergegeven om het apparaat te beheren. Gebruik dit IP-adres verbinding maken met de webgebruikersinterface van uw virtuele matrix en de lokale instelling en de registratie te voltooien.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. (Optioneel) Voer deze stap alleen als u uw apparaat in Government-Cloud implementeert. U kunt nu de Verenigde Staten Federal Information Processing Standard (FIPS)-modus op uw apparaat. De FIPS 140-standaard definieert cryptografische algoritmen die zijn goedgekeurd voor gebruik door de Amerikaanse federale overheid computersystemen voor de beveiliging van gevoelige gegevens.

    1. Als u wilt de FIPS-modus inschakelen, moet u de volgende cmdlet uitvoeren:

        `Enable-HcsFIPSMode`
    2. Uw apparaat opnieuw opstarten nadat u hebt de FIPS-modus ingeschakeld zodat de cryptografische validaties van kracht.

       > [!NOTE]
       > U kunt inschakelen of uitschakelen van FIPS-modus op uw apparaat. Afwisselende van het apparaat tussen FIPS en niet-FIPS-modus wordt niet ondersteund.
       >
       >

Als uw apparaat voldoet niet aan de minimale configuratievereisten, ziet u de volgende fout in de bannertekst (Zie hieronder). Wijzig de apparaatconfiguratie zo dat de virtuele machine voldoende resources heeft om aan de minimale vereisten te voldoen. Daarna kunt u het apparaat opnieuw opstarten en verbinding maken met het apparaat. Verwijzen naar de minimale configuratievereisten in stap 1: Zorg ervoor dat het hostsysteem minimaal virtuele matrix: vereisten voldoet.

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

Als u een andere fout tijdens de eerste configuratie met behulp van de lokale webgebruikersinterface worden geconfronteerd, raadpleegt u de volgende werkstromen:

* Voer diagnostische tests uit om te [oplossen van de installatie van de web-UI](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Logboek pakket genereren en weergeven van logboekbestanden](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Volgende stappen
* [Instellen van uw StorSimple Virtual Array als bestandsserver](storsimple-virtual-array-deploy3-fs-setup.md)
* [Instellen van uw StorSimple Virtual Array als een iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md)
