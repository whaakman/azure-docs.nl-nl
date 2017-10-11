---
title: De installatiekopie van een virtuele machine maken voor Azure Marketplace | Microsoft Docs
description: Gedetailleerde instructies voor het maken van de installatiekopie van een virtuele machine voor Azure Marketplace voor anderen om aan te schaffen.
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio; v-divte
ms.openlocfilehash: 046ce7af40301014746c6aef07d08d81ab4adcc2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="guide-to-create-a-virtual-machine-image-for-the-azure-marketplace"></a>Handleiding voor het maken van de installatiekopie van een virtuele machine voor Azure Marketplace
In dit artikel **stap 2**, wordt u begeleid bij het voorbereiden van de virtuele harde schijven (VHD's) dat u naar Azure Marketplace implementeren wilt. Uw VHD's vormen de basis van uw SKU. Het proces is afhankelijk van of u een SKU op basis van Linux of op basis van Windows biedt. In dit artikel komen beide scenario's. Dit proces kan worden uitgevoerd in combinatie met [accountaanmaking en registratie][link-acct-creation].

## <a name="1-define-offers-and-skus"></a>1. Definieer aanbiedingen en SKU 's
In deze sectie leert u de aanbiedingen en hun bijbehorende SKU's definiëren.

Een aanbieding fungeert als "ouder" voor alle bijbehorende SKU's. U kunt meerdere aanbiedingen hebben. Het is aan u om te besluiten hoe u uw aanbiedingen wilt structureren. Wanneer een aanbieding wordt doorgestuurd voor fasering, wordt deze samen met alle bijbehorende SKU's doorgestuurd. Overweeg zorgvuldig de SKU-id's, omdat ze zichtbaar in de URL:

* Azure.com: http://azure.microsoft.com/marketplace/partners/ {PartnerNamespace} / {OfferIdentifier}-{SKUidentifier}
* Azure preview-portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {SKUIDdentifier}  

Een SKU is de naam van de commerciële voor een VM-installatiekopie. Een VM-installatiekopie bevat de schijf van een besturingssysteem en nul of meer gegevensschijven. In essentie is dit het volledige opslagprofiel voor een virtuele machine. Een VHD is per schijf nodig. Zelfs leeg gegevensschijven vereisen een VHD moet worden gemaakt.

Ongeacht het te gebruiken besturingssysteem voegt u alleen het minimum aantal gegevensschijven toe dat voor de SKU is vereist. Klanten schijven die deel van een afbeelding op het moment van implementatie uitmaken kunnen niet worden verwijderd, maar kunnen altijd schijven toevoegen tijdens of na de implementatie als ze deze nodig.

> [!IMPORTANT]
> **Het aantal schijven in een nieuwe Afbeeldingversie niet wijzigen.** Als u gegevensschijven in de afbeelding configureren moet, definieert u een nieuwe SKU. Publiceren van een nieuwe Afbeeldingversie met een andere schijf aantallen zal hebben de mogelijkheden van de nieuwe implementatie van recente gebaseerd op de nieuwe afbeeldingsversie in geval van automatisch schalen, automatische implementaties van oplossingen op basis van ARM-sjablonen en andere scenario's.
>
>

### <a name="11-add-an-offer"></a>1.1 een aanbieding toevoegen
1. Aanmelden bij de [Publishing Portal] [ link-pubportal] met behulp van uw verkopersaccount.
2. Selecteer de **virtuele Machines** tabblad van de Portal voor publiceren. Voer de aanbiedingsnaam van uw in het veld vraag vermelding. De aanbiedingsnaam van de is doorgaans de naam van het product of service die u van plan bent om te verkopen in Azure Marketplace.
3. Selecteer **Maken**.

### <a name="12-define-a-sku"></a>1.2 een SKU definiëren
Nadat u een aanbieding hebt toegevoegd, moet u voor het definiëren en uw SKU's identificeren. U kunt meerdere aanbiedingen hebben kan, en elke aanbieding meerdere SKU's onder deze. Wanneer een aanbieding wordt doorgestuurd voor fasering, wordt deze samen met alle bijbehorende SKU's doorgestuurd.

1. **Voeg een SKU.** De SKU is een id die wordt gebruikt in de URL vereist. De id moet uniek zijn binnen uw publicatieprofiel, maar er is geen risico bestaat dat ID conflicten met andere uitgevers.

   > [!NOTE]
   > De aanbieding en SKU-id's worden weergegeven in de URL van de aanbieding in de Marketplace.
   >
   >
2. **Voeg een beknopte beschrijving voor de SKU.** Samenvatting beschrijvingen zijn zichtbaar voor klanten, dus moet u ze gemakkelijk leesbare. Deze informatie hoeft niet te worden vergrendeld totdat de fase 'Push naar fasering'. Tot dat moment bent u vrij om de informatie te bewerken.
3. Als u op Windows gebaseerde SKU's gebruikt, volgt u de voorgestelde koppelingen voor het verkrijgen van de goedgekeurde versies van Windows Server.

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a>2. Maken van een Azure-compatibele VHD (op basis van Linux)
Deze sectie richt zich op de aanbevolen procedures voor het maken van een Linux-gebaseerde VM-installatiekopie voor Azure Marketplace. Raadpleeg de volgende documentatie voor een stapsgewijze: [maakt en uploadt u een virtuele harde schijf met het Linux-besturingssysteem](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a>3. Maken van een Azure-compatibele VHD (gebaseerd op Windows)
Deze sectie richt zich op de stappen voor het maken van een SKU op basis van Windows Server voor Azure Marketplace.

### <a name="31-ensure-that-you-are-using-the-correct-base-vhds"></a>3.1 Zorg ervoor dat u de juiste base VHD 's
Het besturingssysteem VHD voor VM-installatiekopie moet worden gebaseerd op een Azure-goedgekeurde basisinstallatiekopie met Windows Server of SQL Server.

Een virtuele machine maken van een van de volgende afbeeldingen om te beginnen te vinden op de [Microsoft Azure-portal][link-azure-portal]:

* WindowsServer ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1][link-datactr-2008-r2])
* SQL Server 2014 ([Enterprise][link-sql-2014-ent], [standaard][link-sql-2014-std], [Web][link-sql-2014-web])
* SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [standaard][link-sql-2012-std], [Web][link-sql-2012-web])

Deze koppelingen zijn ook te vinden in de Portal voor Publiceren onder aan de SKU-pagina.

> [!TIP]
> Als u van de huidige Azure-portal of PowerShell gebruikmaakt, worden Windows Server-installatiekopieën gepubliceerde op 8 September 2014 of hoger goedgekeurd.
>
>

### <a name="32-create-your-windows-based-vm"></a>3.2 maken van de VM op basis van Windows
U kunt uw virtuele machine op basis van een goedgekeurde basisinstallatiekopie in een paar eenvoudige stappen maken vanuit de Microsoft Azure-portal. Hier volgt een overzicht van het proces:

1. Selecteer in de pagina basisinstallatiekopie **virtuele Machine maken** om te worden omgeleid naar de nieuwe [Microsoft Azure-portal][link-azure-portal].

    ![tekenen][img-acom-1]
2. Aanmelden bij de portal met de Microsoft-account en wachtwoord voor het Azure-abonnement dat u wilt gebruiken.
3. Volg de aanwijzingen voor het maken van een virtuele machine met behulp van de basisinstallatiekopie die u hebt geselecteerd. U moet een host (naam van de computer), (geregistreerd als een beheerder) gebruikersnaam en wachtwoord opgeven voor de virtuele machine.

    ![tekenen][img-portal-vm-create]
4. Selecteer de grootte van de virtuele machine te implementeren:

    a.    Als u van plan bent voor het ontwikkelen van de VHD on-premises, wordt de grootte is niet belangrijk. Overweeg het gebruikt van een van de kleinere VM's.

    b.    Als u van plan bent de installatiekopie in Azure te ontwikkelen, kunt u het gebruik van een van de aanbevolen VM-groottes voor de geselecteerde installatiekopie overwegen.

    c.    Raadpleeg voor informatie over prijzen, de **aanbevolen Prijscategorieën** selector op de portal weergegeven. Deze biedt de drie aanbevolen groottes die door de uitgever worden geleverd. (In dit geval is Microsoft de uitgever.)

    ![tekenen][img-portal-vm-size]
5. Eigenschappen instellen:

    a.    Voor snelle implementatie kunt u de standaardwaarden voor de eigenschappen onder laten **optionele configuratie** en **resourcegroep**.

    b.    Onder **Opslagaccount**, kunt u eventueel de storage-account waarin het VHD-besturingssysteem wordt opgeslagen selecteren.

    c.    Onder **resourcegroep**, kunt u eventueel de logische groep voor de virtuele machine te selecteren.
6. Selecteer de **locatie** voor implementatie:

    a.    Als u van plan bent voor het ontwikkelen van de VHD on-premises, wordt de locatie is niet belangrijk omdat u de installatiekopie naar Azure later uploaden zult.

    b.    Als u van plan bent de installatiekopie in Azure te ontwikkelen, kunt u het gebruik van een van de in de VS gebaseerde Microsoft Azure-regio's vanaf het begin overwegen. Dit versnelt de VHD kopiëren die Microsoft namens jou worden uitgevoerd als u uw afbeelding voor de certificeringsinstantie indienen.

    ![tekenen][img-portal-vm-location]
7. Klik op **Create**. Voor het implementeren van de VM start. Binnen enkele minuten beschikt u over een geslaagde implementatie en kunt u beginnen met het maken van de installatiekopie voor uw SKU.

### <a name="33-develop-your-vhd-in-the-cloud"></a>3.3 ontwikkelen van uw VHD in de cloud
Wij raden u aan uw VHD in de cloud te ontwikkelen met behulp van Remote Desktop Protocol (RDP). U verbinding met RDP met de gebruikersnaam en wachtwoord die zijn opgegeven tijdens het inrichten.

> [!IMPORTANT]
> Als u uw VHD ontwikkelt op locatie (dit wordt niet aanbevolen), Zie [maken van de installatiekopie van een virtuele machine op de lokale](marketplace-publishing-vm-image-creation-on-premise.md). Downloaden van de VHD is niet nodig als u in de cloud ontwikkelt.
>
>

**Verbinden via RDP met behulp van de [Microsoft Azure-portal][link-azure-portal]**

1. Selecteer **Bladeren** > **VMs**.
2. De virtuele machines-blade wordt geopend. Zorg dat de virtuele machine die u wilt verbinden met wordt uitgevoerd en selecteer vervolgens in de lijst met geïmplementeerde virtuele machines.
3. Er wordt een blade geopend die worden beschreven van de geselecteerde virtuele machine. Klik aan de bovenkant **Connect**.
4. U wordt gevraagd om in te voeren de gebruikersnaam en wachtwoord die u hebt opgegeven tijdens het inrichten.

**Verbinden via RDP met behulp van PowerShell**

Als u wilt downloaden van een extern bureaublad-bestand naar een lokale computer de [cmdlet Get-AzureRemoteDesktopFile][link-technet-2]. U moet deze cmdlet gebruikt, de naam van de service en de naam van de virtuele machine. Als u hebt gemaakt van de virtuele machine van de [Microsoft Azure-portal][link-azure-portal], u vindt deze informatie onder VM-eigenschappen:

1. Selecteer in de Microsoft Azure-portal **Bladeren** > **VMs**.
2. De virtuele machines-blade wordt geopend. Selecteer de virtuele machine die u hebt geïmplementeerd.
3. Er wordt een blade geopend die worden beschreven van de geselecteerde virtuele machine.
4. Klik op **Eigenschappen**.
5. Het eerste deel van de domeinnaam is de servicenaam. De hostnaam is de naam van de VM.

    ![tekenen][img-portal-vm-rdp]
6. Download het RDP-bestand voor de gemaakte virtuele machine voor de lokale bureaublad van de beheerder van de cmdlet is als volgt.

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

Meer informatie over RDP op MSDN kan worden gevonden in het artikel [verbinding maken met een Azure-VM met RDP of SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).

**Een virtuele machine te configureren en uw SKU maken**

Nadat het besturingssysteem dat VHD wordt gedownload, gebruik van Hyper-v en configureren van een virtuele machine om te beginnen met het maken van uw SKU. Gedetailleerde stappen kunnen worden gevonden op de volgende TechNet-koppeling: [Hyper-v installeren en configureren van een virtuele machine](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="34-choose-the-correct-vhd-size"></a>3.4 Kies de juiste grootte van de VHD
De Windows-besturingssysteem VHD in uw VM-installatiekopie moet worden gemaakt als een vaste indeling 128 GB VHD.  

Als de fysieke grootte minder dan 128 GB is, moet de VHD sparse zijn. De Windows- en SQL Server basisinstallatiekopieën opgegeven al aan deze vereisten voldoet, dus Wijzig de indeling of niet de grootte van de VHD die is verkregen.  

Gegevensschijven mag even groot zijn als 1 TB. Bij het kiezen van de schijfgrootte, houd er rekening mee dat klanten VHD's binnen een afbeelding kunnen niet op het moment van implementatie aangepast. Gegevens schijf VHD's moeten worden gemaakt als een vaste indeling VHD. Ze moeten ook worden verspreid. Gegevensschijven kunnen leeg zijn of gegevens bevatten.

### <a name="35-install-the-latest-windows-patches"></a>3.5 installeren de nieuwste patches voor Windows
De basisinstallatiekopieën bevatten de meest recente patches tot aan de publicatiedatum. Zorg ervoor dat Windows Update is uitgevoerd en dat alle essentiële en belangrijke beveiligingsupdates zijn geïnstalleerd voordat het publiceren van het besturingssysteem-VHD die u hebt gemaakt.

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a>3.6 uitvoeren van extra taken voor de configuratie en planning indien nodig
Wanneer u aanvullende configuratie nodig hebt, overweeg het gebruik van een geplande taak die wordt uitgevoerd bij het opstarten van een definitieve wijzigingen aanbrengen in de virtuele machine nadat deze is geïmplementeerd:

* Het wordt aanbevolen om de taak zichzelf na een geslaagde uitvoering te laten verwijderen.
* Er is geen configuratie verstandig stations dan stations C of D, omdat dit de slechts twee stations die gegarandeerd altijd aanwezig zijn. Station C is de besturingssysteemschijf en station D is de tijdelijke lokale schijf.

### <a name="37-generalize-the-image"></a>3.7 generaliseer de installatiekopie
Alle installatiekopieën in Azure Marketplace moet herbruikbare op algemene wijze. Met andere woorden, moet de besturingssysteem-VHD worden gegeneraliseerd:

* Voor Windows, moet de installatiekopie van het 'Sysprep voorbereide' en geen configuraties worden uitgevoerd die geen ondersteuning voor de **sysprep** opdracht.
* U kunt de volgende opdracht uitvoeren vanuit de map % windir%\System32\Sysprep.

        sysprep.exe /generalize /oobe /shutdown

  Informatie over hoe het besturingssysteem naar sysprep is opgegeven in stap van de volgende MSDN-artikel: [een Windows Server-VHD naar Azure maken en uploaden](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="4-deploy-a-vm-from-your-vhds"></a>4. Geen VM implementeren vanaf uw VHD 's
Nadat u uw VHD's (de algemene besturingssysteem-VHD en nul of meer gegevens schijf VHD's) naar een Azure storage-account hebt geüpload, kunt u ze kunt registreren als een installatiekopie van een virtuele machine. Vervolgens kunt u die afbeelding testen. Houd er rekening mee omdat uw besturingssysteem-VHD is gegeneraliseerd, u de VM rechtstreeks implementeren kunt door de URL van de VHD.

Voor meer informatie over de VM-installatiekopieën, controleert u de volgende blogberichten:

* [VM-installatiekopie](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [VM Image PowerShell hoe](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [Over de installatiekopieën van de virtuele machine in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-the-necessary-tools-powershell-and-azure-cli"></a>De benodigde hulpprogramma's, PowerShell en Azure CLI instellen
* [Het instellen van PowerShell](/powershell/azure/overview)
* [Het instellen van Azure CLI](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a>4.1 een installatiekopie van een virtuele machine maken
#### <a name="capture-vm"></a>Vastleggen van VM
Lees de koppelingen hieronder voor hulp bij het vastleggen van de virtuele machine met behulp van PowerShell-API/Azure CLI.

* [API](https://msdn.microsoft.com/library/mt163560.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure-CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a>Generaliseer installatiekopie
Lees de koppelingen hieronder voor hulp bij het vastleggen van de virtuele machine met behulp van PowerShell-API/Azure CLI.

* [API](https://msdn.microsoft.com/library/mt269439.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure-CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a>4.2 geen VM implementeren vanaf een installatiekopie van een virtuele machine
Voor het implementeren van een virtuele machine van een installatiekopie van een virtuele machine, kunt u de huidige [Azure-portal](https://manage.windowsazure.com) of PowerShell.

**Geen VM implementeren vanaf de huidige Azure-portal**

1. Ga naar **nieuw** > **Compute** > **virtuele machine** > **vanuit galerie**.

    ![tekenen][img-manage-vm-new]
2. Ga naar **Mijn afbeeldingen**, en selecteer vervolgens de VM-installatiekopie waaruit een virtuele machine implementeren:

   1. Aandacht besteedt aan de afbeelding die u selecteert, omdat de **Mijn afbeeldingen** weergave bevat zowel installatiekopieën van besturingssystemen en VM-installatiekopieën.
   2. Kijken naar het aantal schijven kunt u bepalen welk type installatiekopie die u implementeert, omdat het merendeel van de VM-installatiekopieën meer dan één schijf. Het is echter nog steeds mogelijk om een VM-afbeelding met slechts één besturingssysteemschijf, die vervolgens **aantal schijven** ingesteld op 1.

      ![tekenen][img-manage-vm-select]
3. Volg de wizard virtuele machine maken en de VM naam, VM grootte, locatie, gebruikersnaam en wachtwoord opgeven.

**Een virtuele machine vanuit PowerShell implementeren**

U kunt de volgende cmdlets kunt gebruiken voor het implementeren van een grote virtuele machine van de algemene VM-installatiekopie zojuist hebt gemaakt.

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> Zie [probleemoplossing problemen aangetroffen tijdens het maken van de VHD] voor assistentie.
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a>5. Certificeringsinstantie voor uw VM-installatiekopie verkrijgen
De volgende stap bij het voorbereiden van uw VM-installatiekopie op Azure Marketplace is dat het gecertificeerd.

Dit proces omvat het uitvoeren van een certificeringsinstantie speciale hulpprogramma, de verificatieresultaten uploaden naar de Azure-container waarin uw VHD's zich bevinden, toe te voegen een aanbieding, definiëren van uw SKU en verzenden van uw VM-installatiekopie voor certificering.

### <a name="51-download-and-run-the-certification-test-tool-for-azure-certified"></a>5.1 downloaden en uitvoeren van de certificeringsinstantie Test-hulpprogramma voor Azure gecertificeerd
Het hulpprogramma certificeringsinstantie wordt uitgevoerd op een actieve virtuele machine, ingericht vanuit uw gebruiker VM-installatiekopie, om ervoor te zorgen dat de VM-installatiekopie compatibel met Microsoft Azure is. Het hulpprogramma controleert of is voldaan aan de richtlijnen en vereisten voor het voorbereiden van uw VHD. De uitvoer van het hulpprogramma is een compatibiliteitsrapport die moet worden geüpload in de Publishing Portal tijdens aanvragende certificeringsinstantie.

Het hulpprogramma voor de certificeringsinstantie kan worden gebruikt met Windows- en Linux-machines. Deze verbinding maakt met Windows-VM's via PowerShell en verbinding maakt met virtuele machines van Linux via SSH.Net:

1. Downloadt u eerst het hulpprogramma certificeringsinstantie op de [Microsoft-website][link-msft-download].
2. Open het hulpprogramma voor certificering en klik vervolgens op de **nieuwe Test Start** knop.
3. Van de **informatie testen** scherm, voer een naam voor de test uitgevoerd.
4. Bepaal of uw VM op Linux of op Windows draait. Afhankelijk van uw keuze selecteert u de vervolgopties.

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>**Verbinding maken met het hulpprogramma certificeringsinstantie op een installatiekopie van het Linux-VM**
1. Selecteer de SSH-verificatiemodus: wachtwoord of sleutelbestand.
2. Als verificatie op basis van wachtwoorden, voer de naam, gebruikersnaam en wachtwoord van Domain Name System (DNS).
3. Als u sleutelbestand authenticatie gebruikt, voert u de DNS-naam, de gebruikersnaam en de locatie van de persoonlijke sleutel.

   ![Wachtwoordverificatie van Linux VM-installatiekopie][img-cert-vm-pswd-lnx]

   ![Sleutelbestand verificatie van Linux VM-installatiekopie][img-cert-vm-key-lnx]

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Verbinding maken met het hulpprogramma certificeringsinstantie van een VM op basis van Windows-installatiekopie**
1. Geef de volledig gekwalificeerde VM DNS-naam (bijvoorbeeld MyVMName.Cloudapp.net).
2. Geef de gebruikersnaam en wachtwoord.

   ![Wachtwoordverificatie van Windows VM-installatiekopie][img-cert-vm-pswd-win]

Nadat u de juiste opties voor uw Linux- of virtuele machine op basis van Windows-installatiekopie hebt geselecteerd, selecteert u **testverbinding** om ervoor te zorgen dat SSH.Net of PowerShell een geldige verbinding is voor testdoeleinden. Nadat een verbinding tot stand is gebracht, selecteert u **volgende** om de test te starten.

Wanneer de test is voltooid, ontvangt u de resultaten ('Pass'/'Fail'/'Warning' (Geslaagd/Mislukt/Waarschuwing)) voor elk testelement.

![Testscenario's voor Linux VM-installatiekopie][img-cert-vm-test-lnx]

![Testscenario's voor VM-installatiekopie voor Windows][img-cert-vm-test-win]

Als een van de tests mislukt, wordt de afbeelding niet gecertificeerd. Als dit het geval is, de vereisten doornemen en breng de gewenste wijzigingen.

Na de geautomatiseerde test, wordt u gevraagd naar aanvullende gegevens over uw VM-installatiekopie via een scherm vragenlijst opgeven.  Voltooien van de vragen en selecteer vervolgens **volgende**.

![Certificeringsinstantie hulpprogramma vragenlijst][img-cert-vm-questionnaire]

![Certificeringsinstantie hulpprogramma vragenlijst][img-cert-vm-questionnaire-2]

Nadat u de vragenlijst hebt voltooid, kunt u aanvullende informatie zoals SSH toegang tot gegevens van de Linux-VM bieden installatiekopie en een uitleg voor elke mislukte beoordelingen zijn tegengekomen. U kunt de testresultaten en de logboekbestanden voor de uitgevoerde testcases naast uw antwoorden op de vragenlijst downloaden. De resultaten opslaan in dezelfde container als uw virtuele harde schijven.

![Certificeringsinstantie testresultaten opslaan][img-cert-vm-results]

### <a name="52-get-the-shared-access-signature-uri-for-your-vm-images"></a>5.2 ophalen van de shared access signature URI voor uw VM-installatiekopieën
Tijdens het publicatieproces, moet u de uniform resource-id's (URI's) die leiden tot elk van de VHD's die u hebt gemaakt voor de SKU opgeven. Tijdens het certificeringsproces moet Microsoft toegang hebben tot deze VHD's. Daarom moet u een shared access signature URI voor elke VHD te maken. Dit is de URI die moet worden ingevoerd in de **installatiekopieën** tabblad in de Portal voor publiceren.

De shared access signature die URI gemaakt moet voldoen aan de volgende vereisten:

* Bij het genereren van shared access signature voor URI's voor uw virtuele harde schijven, zijn machtigingen lijst en lezen voldoende. Verleen geen toegang voor schrijven ('Write') of verwijderen ('Delete').
* De duur voor toegang moet minimaal drie (3) de weken van wanneer de shared access signature URI wordt gemaakt.
* Selecteer om te waarborgen voor UTC-tijd, de dag vóór de huidige datum. Als de huidige datum 6 oktober 2014 valt, selecteert u bijvoorbeeld 5-10-2014.

SAS-URL kan worden gegenereerd op meerdere manieren voor het delen van uw VHD voor Azure Marketplace.
Hieronder volgen de 3 aanbevolen hulpprogramma's:

1.  Azure Opslagverkenner
2.  Microsoft Opslagverkenner
3.  Azure CLI

**Azure Opslagverkenner (aanbevolen voor Windows-gebruikers)**

Hierna volgen de stappen voor het genereren van SAS-URL met behulp van Azure Storage Explorer

1. Download [Azure Storage Explorer 6 Preview 3](https://azurestorageexplorer.codeplex.com/) van CodePlex. Ga naar [Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) en klik op **'Downloads'**.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. Download [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) en na deze ritsen installeren.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. Nadat deze is geïnstalleerd, opent u de toepassing.
4. Klik op **Account toevoegen**.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. Geef de naam van het opslagaccount, opslagaccountsleutel en opslag eindpunten domein. Dit is het opslagaccount in uw Azure-abonnement waar u uw VHD hebt opgeslagen in de Azure-portal.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. Wanneer Azure Opslagverkenner is verbonden met uw specifieke storage-account, start het alle de binnen het opslagaccount bevat. Selecteer de container waarin het besturingssysteem schijf VHD-bestand (ook gegevensschijven als ze van toepassing zijn voor uw scenario).

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. Na het selecteren van de blob-container, Azure Storage Explorer begint met de bestanden in de container. Selecteer het installatiekopiebestand (VHD) die moeten worden verzonden.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  Selecteer het VHD-bestand in de container en klik op de **beveiliging** tabblad.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  In de **Blob-Container beveiliging** dialoogvenster vak, laat u de standaardinstellingen op de **toegangsniveau** tabblad en klik vervolgens op **Shared Access Signatures** tabblad.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. Volg onderstaande stappen voor het genereren van een shared access signature URI voor de VHD-installatiekopie:

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    a. **Toegang van toegestaan:** om te waarborgen voor UTC-tijd, selecteer de dag vóór de huidige datum. Als de huidige datum 6 oktober 2014 valt, selecteert u bijvoorbeeld 5-10-2014.

    b. **Toegang is toegestaan voor:** selecteert u een datum die ten minste drie weken na de **toegang toegestaan van** datum.

    c. **Acties die zijn toegestaan:** selecteert u de **lijst** en **lezen** machtigingen.

    d. Als u uw VHD-bestand juist hebt geselecteerd, wordt het bestand wordt weergegeven in **Blob-naam voor toegang tot** met extensie VHD.

    e. Klik op **Signature genereren**.

    f. In **gegenereerd Shared Access Signature URI van deze container**, controleren op de volgende stappen uit als gemarkeerde bovenstaande:

       - Zorg ervoor dat uw installatiekopie bestandsnaam en **".vhd"** zijn in de URI.
       - Controleer aan het einde van de handtekening **"rl ="** wordt weergegeven. Dit toont aan dat toegang voor lezen en de lijst met succes is opgegeven.
       - Controleer in het midden van de handtekening, **' sr c = "** wordt weergegeven. Dit toont aan dat er toegangsniveau van de container

11. Om ervoor te zorgen dat de gegenereerde gedeeld toegang handtekening URI werkt, klikt u op **testen in de Browser**. Deze moet eerst het downloadproces.

12. Kopieer de shared access signature URI. Dit is de URI die u in de Portal voor Publiceren moet plakken.

13. Herhaal stap 6-10 voor elke VHD in de SKU.

**Microsoft Azure Opslagverkenner (Windows of MAC/Linux)**

Hierna volgen de stappen voor het genereren van SAS-URL met behulp van Microsoft Azure Storage Explorer

1.  Downloaden van Microsoft Azure Storage Explorer formulier [http://storageexplorer.com/](http://storageexplorer.com/) website. Ga naar [Microsoft Azure Storage Explorer](http://storageexplorer.com/releasenotes.html) en klik op **'Downloaden voor Windows'**.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  Nadat deze is geïnstalleerd, opent u de toepassing.

3.  Klik op **Account toevoegen**.

4.  Microsoft Azure Storage Explorer configureren voor uw abonnement met aanmelden bij uw account

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  Ga naar de storage-account en selecteer de Container

6.  Selecteer **'Share Access Signature ophalen'...** met behulp van de rechtermuisknop te klikken op van de **container**

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  Begintijd van de update, verlooptijd en machtigingen volgens de volgende

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    a.  **Begintijd:** om te waarborgen voor UTC-tijd, selecteer de dag vóór de huidige datum. Als de huidige datum 6 oktober 2014 valt, selecteert u bijvoorbeeld 5-10-2014.

    b.  **Verlooptijd:** selecteert u een datum die ten minste drie weken na de **begintijd** datum.

    c.  **Machtigingen:** selecteert u de **lijst** en **lezen** machtigingen

8.  Shared access signature voor containers URI kopiëren

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    Gegenereerde SAS-URL is voor container niveau en nu moeten de naam van de VHD toevoegen in het.

    Indeling van de Container niveau SAS-URL:`https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    De naam van de VHD invoegen na de containernaam van de in SAS-URL zoals hieronder`https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Voorbeeld:

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    TestRGVM201631920152.vhd is de naam van de VHD en vervolgens de URL van de VHD-SAS`https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    - Zorg ervoor dat uw installatiekopie bestandsnaam en **".vhd"** zijn in de URI.
    - Controleer in het midden van de handtekening, **"sp rl ="** wordt weergegeven. Dit toont aan dat toegang voor lezen en de lijst met succes is opgegeven.
    - Controleer in het midden van de handtekening, **' sr c = "** wordt weergegeven. Dit toont aan dat er toegangsniveau van de container

9.  Om ervoor te zorgen dat de gegenereerde gedeeld toegang handtekening URI werkt, test u deze in de browser. Het downloadproces moet worden gestart

10. Kopieer de shared access signature URI. Dit is de URI die u in de Portal voor Publiceren moet plakken.

11. Herhaal deze stappen voor elke VHD in de SKU.

**Azure CLI (aanbevolen voor niet-Windows & continue integratie)**

Hierna volgen de stappen voor het genereren van SAS-URL met Azure CLI

1.  Downloaden van Microsoft Azure CLI van [hier](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/). Ook vindt u koppelingen naar de andere  **[Windows](http://aka.ms/webpi-azure-cli)**  en  **[MAC OS](http://aka.ms/mac-azure-cli)**.

2.  Zodra deze is gedownload, installeer

3.  Een PowerShell-bestand met de volgende code maken en opslaan in de lokale

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    Bijwerken van de volgende parameters in hierboven

    a. **`<StorageAccountName>`**: Geef de naam van uw opslagaccount

    b. **`<Storage Account Key>`**: Geef de sleutel van uw opslagaccount

    c. **`<Permission Start Date>`**: Selecteer om te waarborgen voor UTC-tijd, de dag vóór de huidige datum. Bijvoorbeeld, als de huidige datum 26 oktober 2016 is waarde dan 25-10-2016

    d. **`<Permission End Date>`**: Selecteer een datum die ten minste drie weken na de **begindatum**. Waarde moet **02-11-2016**.

    Hieronder vindt u in de voorbeeldcode na het bijwerken van de juiste parameters

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=TIQE5QWMKHpT5q2VnF1bb+NUV7NVMY2xmzVx1rdgIVsw7h0pcI5nMM6+DVFO65i4bQevx21dmrflA91r0Vh2Yw=="
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  Powershell-editor te openen met 'Als Administrator uitvoeren'-modus en open bestand in stap #3.

5.  Voer het script, vindt u de SAS-URL voor het toegangsniveau van de container

    Hieronder wordt de uitvoer van de SAS-handtekening en kopieer het gemarkeerde onderdeel in een Kladblok

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  Nu u krijgt container niveau SAS-URL en moet u de naam van de VHD toevoegen in het.

    Container niveau SAS-URL #

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  De naam van de VHD invoegen na de containernaam van de in SAS-URL, zoals hieronder wordt weergegeven`https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    Voorbeeld:

    TestRGVM201631920152.vhd is de naam van de VHD en vervolgens de URL van de VHD-SAS

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - Zorg ervoor dat uw afbeeldingsbestandsnaam en ".vhd" in de URI.
    -   Controleer in het midden van de handtekening 'sp rl =' wordt weergegeven. Dit toont aan dat toegang voor lezen en de lijst met succes is opgegeven.
    -   Controleer in het midden van de handtekening ' sr = c ' wordt weergegeven. Dit toont aan dat er toegangsniveau van de container

8.  Om ervoor te zorgen dat de gegenereerde gedeeld toegang handtekening URI werkt, test u deze in de browser. Het downloadproces moet worden gestart

9.  Kopieer de shared access signature URI. Dit is de URI die u in de Portal voor Publiceren moet plakken.

10. Herhaal deze stappen voor elke VHD in de SKU.


### <a name="53-provide-information-about-the-vm-image-and-request-certification-in-the-publishing-portal"></a>5.3 bieden informatie over de VM-installatiekopie en aanvragen van de certificeringsinstantie in de Portal publiceren
Nadat u uw aanbieding en SKU hebt gemaakt, moet u de details van de afbeelding die is gekoppeld aan deze SKU invoeren:

1. Ga naar de [Publishing Portal][link-pubportal], en meld u aan met uw verkopersaccount.
2. Selecteer de **VM-installatiekopieën** tabblad.
3. De id die wordt vermeld op de bovenkant van de pagina is feitelijk de id van de aanbieding en niet de SKU-id.
4. Vul de eigenschappen onder de **SKU's** sectie.
5. Onder **besturingssysteemgroep**, klikt u op het type van het besturingssysteem die is gekoppeld aan de besturingssysteem-VHD.
6. In de **besturingssysteem** vak, beschrijven van het besturingssysteem. U kunt een notatie zoals-besturingssystemen, type, versie en updates. Een voorbeeld is 'Windows Server Datacenter 2014 R2'.
7. Selecteer de grootte van maximaal zes aanbevolen virtuele machines. Dit zijn de aanbevelingen die aan de klant in de blade prijzen laag in de Azure Portal worden weergegeven wanneer u ze wilt aanschaffen en implementeren van uw installatiekopie. **Dit zijn alleen aanbevelingen. De klant is kunt selecteren van elke VM-grootte die geschikt is voor de schijven die zijn opgegeven in uw installatiekopie.**
8. De versie in te voeren. Het versieveld ingekapseld een semantische versie om het product en de updates te identificeren:
   * Versies moet van het formulier X.Y.Z, waarbij X, Y en Z gehele getallen zijn.
   * Afbeeldingen in verschillende SKU's kunnen verschillende primaire en secundaire versies hebben.
   * Versies binnen een SKU moet alleen incrementele wijzigingen, die de patch-versie (Z van X.Y.Z) te verhogen.
9. In de **OS VHD URL** Voer de shared access signature URI voor de besturingssysteem-VHD gemaakt.
10. Als er gegevensschijven gekoppeld aan deze SKU, selecteert u de logische eenheid number (LUN) waarnaar u deze gegevensschijf dat wilt aan bij de implementatie worden gekoppeld.
11. In de **LUN X VHD URL** Voer de shared access signature URI voor de eerste gegevens VHD gemaakt.

    ![tekenen](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a>Algemene SAS-URL problemen en oplossingen

|Probleem|Foutbericht|Oplossen|Koppeling van documentatie|
|---|---|---|---|
|Fout bij het kopiëren van afbeeldingen - '? ' is niet gevonden in het SAS-url|Fout: Afbeeldingen kopiëren. Kan geen downloaden blob met behulp van SAS-Uri.|Update de SAS-Url met behulp van aanbevolen hulpprogramma 's|[https://Azure.Microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Fout bij het kopiëren van afbeeldingen - parameters voor 'st' en 'se' niet in het SAS-url|Fout: Afbeeldingen kopiëren. Kan geen downloaden blob met behulp van SAS-Uri.|De SAS-Url met de begin- en einddatums erop bijwerken|[https://Azure.Microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Fout bij het kopiëren van afbeeldingen - 'sp = rl' niet in het SAS-url|Fout: Afbeeldingen kopiëren. Kan niet worden gedownload van de blob met behulp van SAS-Uri|Bijwerken van de SAS-Url met machtigingen zijn ingesteld als 'Lezen' en 'lijst|[https://Azure.Microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Fout bij het kopiëren van afbeeldingen - SAS-url spaties hebben in de naam van de vhd|Fout: Afbeeldingen kopiëren. Kan geen downloaden blob met behulp van SAS-Uri.|De SAS-Url zonder spaties bijwerken|[https://Azure.Microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Fout bij het kopiëren van afbeeldingen – SAS-Url-autorisatie-fout|Fout: Afbeeldingen kopiëren. Er kan geen blob vanwege Autorisatiefout downloaden|Opnieuw genereren van SAS-Url|[https://Azure.Microsoft.com/en-us/Documentation/articles/Storage-DotNet-Shared-Access-Signature-Part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|


## <a name="next-step"></a>Volgende stap
Nadat u klaar bent met de SKU-details, kunt u verder gaan naar de [Azure Marketplace marketing inhoud handleiding][link-pushstaging]. In deze stap van het publicatieproces, bieden u de marketing inhoud, prijzen en andere informatie die nodig zijn vóór **stap 3: uw virtuele machine testen bieden in fasering**, waarin u verschillende scenario's voor use case testen voordat u de aanbieding voor Azure Marketplace voor openbare zichtbaarheid en inkoop implementeert.  

## <a name="see-also"></a>Zie ook
* [Aan de slag: hoe een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md)

[img-acom-1]:media/marketplace-publishing-vm-image-creation/vm-image-acom-datacenter.png
[img-portal-vm-size]:media/marketplace-publishing-vm-image-creation/vm-image-portal-size.png
[img-portal-vm-create]:media/marketplace-publishing-vm-image-creation/vm-image-portal-create-vm.png
[img-portal-vm-location]:media/marketplace-publishing-vm-image-creation/vm-image-portal-location.png
[img-portal-vm-rdp]:media/marketplace-publishing-vm-image-creation/vm-image-portal-rdp.png
[img-azstg-add]:media/marketplace-publishing-vm-image-creation/vm-image-storage-add.png
[img-manage-vm-new]:media/marketplace-publishing-vm-image-creation/vm-image-manage-new.png
[img-manage-vm-select]:media/marketplace-publishing-vm-image-creation/vm-image-manage-select.png
[img-cert-vm-key-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-keyfile-linux.png
[img-cert-vm-pswd-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-linux.png
[img-cert-vm-pswd-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-win.png
[img-cert-vm-test-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-linux.png
[img-cert-vm-test-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-win.png
[img-cert-vm-results]:media/marketplace-publishing-vm-image-creation/vm-image-certification-results.png
[img-cert-vm-questionnaire]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire.png
[img-cert-vm-questionnaire-2]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire-2.png
[img-pubportal-vm-skus]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus.png
[img-pubportal-vm-skus-2]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-2.png

[link-pushstaging]:marketplace-publishing-push-to-staging.md
[link-github-waagent]:https://github.com/Azure/WALinuxAgent
[link-azure-codeplex]:https://azurestorageexplorer.codeplex.com/
[link-azure-2]:../storage/blobs/storage-dotnet-shared-access-signature-part-2.md
[link-azure-1]:../storage/common/storage-dotnet-shared-access-signature-part-1.md
[link-msft-download]:http://www.microsoft.com/download/details.aspx?id=44299
[link-technet-3]:https://technet.microsoft.com/library/hh846766.aspx
[link-technet-2]:https://msdn.microsoft.com/library/dn495261.aspx
[link-azure-portal]:https://portal.azure.com
[link-pubportal]:https://publish.windowsazure.com
[link-sql-2014-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/
[link-sql-2014-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/
[link-sql-2014-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/
[link-sql-2012-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/
[link-sql-2012-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/
[link-sql-2012-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/
[link-datactr-2012-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/
[link-datactr-2012]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/
[link-datactr-2008-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/
