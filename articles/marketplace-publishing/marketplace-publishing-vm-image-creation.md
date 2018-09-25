---
title: Het maken van een VM-installatiekopie voor de Azure Marketplace | Microsoft Docs
description: Gedetailleerde instructies voor het maken van een VM-installatiekopie voor de Azure Marketplace voor anderen om aan te schaffen.
services: Azure Marketplace
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio; v-divte
ms.openlocfilehash: 893b0ee70f577d9240d577e76062eea36b704058
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989869"
---
# <a name="guide-to-create-a-virtual-machine-image-for-the-azure-marketplace"></a>Handleiding voor het maken van een VM-installatiekopie voor de Azure Marketplace
In dit artikel **stap 2**, helpt u bij het voorbereiden van de virtuele harde schijven (VHD's) dat u op Azure Marketplace implementeren wilt. Uw VHD's vormen de basis van uw SKU. Het proces verschilt, afhankelijk van of u een SKU op basis van Linux of Windows is gebaseerd. In dit artikel bevat informatie over beide scenario's. Dit proces kan worden uitgevoerd in combinatie met [van accountaanmaking en registratie][link-acct-creation].

## <a name="1-define-offers-and-skus"></a>1. Aanbiedingen en SKU's definiëren
In deze sectie leert u definieert de aanbiedingen en hun bijbehorende SKU's.

Een aanbieding fungeert als "ouder" voor alle bijbehorende SKU's. U kunt meerdere aanbiedingen hebben. Het is aan u om te besluiten hoe u uw aanbiedingen wilt structureren. Wanneer een aanbieding wordt doorgestuurd voor fasering, wordt deze samen met alle bijbehorende SKU's doorgestuurd. Overweeg zorgvuldig de SKU-id's, omdat deze zichtbaar in de URL is:

* Azure.com: http://azure.microsoft.com/marketplace/partners/{PartnerNamespace}/{OfferIdentifier}-{SKUidentifier}
* Azure preview-portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{SKUIDdentifier}  

Een SKU is de commerciële naam voor een VM-installatiekopie. Een VM-installatiekopie bevat een besturingssysteemschijf en nul of meer gegevensschijven. In essentie is dit het volledige opslagprofiel voor een virtuele machine. Één VHD is per schijf nodig. Even zelfs lege gegevensschijven vereisen dat u een VHD moet worden gemaakt.

Ongeacht het te gebruiken besturingssysteem voegt u alleen het minimum aantal gegevensschijven toe dat voor de SKU is vereist. Klanten schijven die deel van een installatiekopie op het moment van implementatie uitmaken niet verwijderen, maar kunnen altijd schijven toevoegen tijdens of na de implementatie als ze deze nodig hebt.

> [!IMPORTANT]
> **Het aantal schijven in een nieuwe Afbeeldingversie niet wijzigen.** Als u moet de configuratie van gegevensschijven in de afbeelding, definieert u een nieuwe SKU. Publiceren van een nieuwe installatiekopieversie met een andere schijf aantallen wordt hebben het potentieel van belangrijke nieuwe implementatie op basis van de versie van de nieuwe installatiekopie in geval van automatisch schalen, automatische implementaties van oplossingen op basis van ARM-sjablonen en andere scenario's.
>
>

### <a name="11-add-an-offer"></a>1.1 een aanbieding toevoegen
1. Aanmelden bij de [Portal voor publiceren] [ link-pubportal] met behulp van uw verkopersaccount.
2. Selecteer de **virtuele Machines** tabblad van de Portal voor publiceren. Voer in het veld, de aanbiedingsnaam van uw. De aanbiedingsnaam van de is doorgaans de naam van het product of service die u van plan bent om te mogen verkopen in de Azure Marketplace.
3. Selecteer **Maken**.

### <a name="12-define-a-sku"></a>1.2 een SKU definiëren
Nadat u een aanbieding hebt toegevoegd, moet u voor het definiëren en identificeren van uw SKU's. U kunt meerdere aanbiedingen hebben kan, en elke aanbieding verschillende SKU's daaronder. Wanneer een aanbieding wordt doorgestuurd voor fasering, wordt deze samen met alle bijbehorende SKU's doorgestuurd.

1. **Voeg een SKU toe.** De SKU moet een id, die wordt gebruikt in de URL. De id moet uniek zijn binnen uw publicatieprofiel, maar er is geen risico id conflicteert met andere uitgevers.

   > [!NOTE]
   > De aanbieding en SKU-id's worden weergegeven in de URL van de aanbieding op Marketplace.
   >
   >
2. **Voeg een samenvattende beschrijving toe voor uw SKU.** Samenvatting beschrijvingen zijn zichtbaar voor klanten, zodat u moet deze gemakkelijk leesbaar maken. Deze informatie hoeft niet te worden vergrendeld totdat de fase 'Naar fasering'. Tot dat moment bent u vrij om de informatie te bewerken.
3. Als u op Windows gebaseerde SKU's gebruikt, volgt u de voorgestelde koppelingen voor het verkrijgen van de goedgekeurde versies van Windows Server.

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a>2. Een Azure-compatibele VHD maken (gebaseerd op Linux)
Deze sectie richt zich op de aanbevolen procedures voor het maken van een VM op basis van Linux-installatiekopie voor de Azure Marketplace. Voor stapsgewijze instructies raadpleegt u de volgende documentatie: [een aangepaste Linux-VM-installatiekopie maken](../virtual-machines/linux/create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a>3. Een Azure-compatibele VHD maken (Windows-indeling)
Deze sectie richt zich op de stappen voor het maken van een SKU op basis van Windows Server voor de Azure Marketplace.

### <a name="31-ensure-that-you-are-using-the-correct-base-vhds"></a>3.1 ervoor te zorgen dat u gebruikmaakt van de juiste basis-VHD
Het besturingssysteem VHD voor uw VM-installatiekopie moet worden gebaseerd op een Azure goedgekeurde basisinstallatiekopie met Windows Server of SQL Server.

Om te beginnen een VM maken van een van de volgende installatiekopieën, dat zich bevindt in de [Microsoft Azure portal][link-azure-portal]:

* Windows Server ([2012 R2 Datacenter] [link-datactr-2012-r2] [2012 Datacenter] [link-datactr-2012], [2008 R2 SP1][link-datactr-2008-r2])
* SQL Server 2014 
* SQL Server 2012 SP2 

Deze koppelingen zijn ook te vinden in de Portal voor Publiceren onder aan de SKU-pagina.

> [!TIP]
> Als u van de huidige Azure-portal of PowerShell gebruikmaakt, worden Windows Server-installatiekopieën gepubliceerd op 8 September 2014 en hoger goedgekeurd.
>
>

### <a name="32-create-your-windows-based-vm"></a>3.2 Windows gebaseerde virtuele machine maken
U kunt uw virtuele machine op basis van een goedgekeurde basisinstallatiekopie in een paar eenvoudige stappen kunt maken vanuit de Microsoft Azure-portal. Hier volgt een overzicht van het proces:

1. Selecteer in de pagina basisinstallatiekopie **virtuele Machine maken** om te worden omgeleid naar de nieuwe [Microsoft Azure portal][link-azure-portal].

    ![tekenen][img-acom-1]
2. Meld u aan bij de portal met de Microsoft-account en het wachtwoord voor de Azure-abonnement die u wilt gebruiken.
3. Volg de aanwijzingen voor het maken van een virtuele machine met behulp van de basisinstallatiekopie die u hebt geselecteerd. U moet een host (naam van de computer), de naam van de gebruiker (geregistreerd als een beheerder) en wachtwoord voor de virtuele machine opgeven.

    ![tekenen][img-portal-vm-create]
4. Selecteer de grootte van de virtuele machine te implementeren:

    a.    Als u van plan de VHD bent op locatie ontwikkelen, is de grootte niet van belang. Overweeg het gebruikt van een van de kleinere VM's.

    b.    Als u van plan bent de installatiekopie in Azure te ontwikkelen, kunt u het gebruik van een van de aanbevolen VM-groottes voor de geselecteerde installatiekopie overwegen.

    c.    Raadpleeg voor informatie over de prijzen, de **aanbevolen Prijscategorieën** selector weergegeven op de portal. Deze biedt de drie aanbevolen groottes die door de uitgever worden geleverd. (In dit geval is Microsoft de uitgever.)

    ![tekenen][img-portal-vm-size]
5. Eigenschappen instellen:

    a.    Voor een snelle implementatie, kunt u de standaardwaarden voor de eigenschappen in onder **optionele configuratie** en **resourcegroep**.

    b.    Onder **Opslagaccount**, kunt u het opslagaccount waarin het VHD-besturingssysteem wordt opgeslagen (optioneel) selecteren.

    c.    Onder **resourcegroep**, kunt u eventueel de logische groep waarin de virtuele machine te selecteren.
6. Selecteer de **locatie** voor implementatie:

    a.    Als u van plan de VHD bent op locatie ontwikkelen, de locatie niet van belang omdat u de installatiekopie uploaden naar Azure later opnieuw wordt.

    b.    Als u van plan bent de installatiekopie in Azure te ontwikkelen, kunt u het gebruik van een van de in de VS gebaseerde Microsoft Azure-regio's vanaf het begin overwegen. Dit is sneller van de VHD-kopieerproces dat Microsoft namens u uitvoert wanneer u uw installatiekopie voor certificering inzendt.

    ![tekenen][img-portal-vm-location]
7. Klik op **Create**. De virtuele machine wordt gestart om te implementeren. Binnen enkele minuten beschikt u over een geslaagde implementatie en kunt u beginnen met het maken van de installatiekopie voor uw SKU.

### <a name="33-develop-your-vhd-in-the-cloud"></a>3.3 om uw VHD in de cloud ontwikkelen
Het is raadzaam dat u uw VHD in de cloud ontwikkelen met behulp van Remote Desktop Protocol (RDP). U verbinding met RDP met de gebruikersnaam en wachtwoord dat is opgegeven tijdens het inrichten.

> [!IMPORTANT]
> **Gebruik geen beheerde schijven.** De virtuele machine gebruikt voor het ontwikkelen van de VHD naar de cloud moet niet worden gebaseerd op schijven die worden beheerd, omdat deze momenteel niet het maken van een afbeelding van deze ondersteunt.
> Het maken van de virtuele machine in de optionele functiewijziging de standaardwaarde voor schijven beheerd.

> Als u uw VHD ontwikkelen op locatie (dit wordt niet aanbevolen), raadpleegt u [het maken van de installatiekopie van een virtuele machine on-premises](marketplace-publishing-vm-image-creation-on-premise.md). Downloaden van uw VHD is niet nodig als u in de cloud ontwikkelt.
>
>

**Verbinding maken via RDP met behulp van de [Microsoft Azure portal][link-azure-portal]**

1. Selecteer **alle services** > **VMs**.
2. De virtuele machines-blade wordt geopend. Zorg ervoor dat de virtuele machine die u wilt verbinden met wordt uitgevoerd, en selecteert u deze in de lijst met geïmplementeerde VM's.
3. Er wordt een blade geopend waarin wordt beschreven van de geselecteerde VM. Klik aan de bovenkant **Connect**.
4. U wordt gevraagd om in te voeren de gebruikersnaam en wachtwoord die u hebt opgegeven tijdens het inrichten.

**Maak verbinding via RDP met behulp van PowerShell**

Als u wilt een bestand met een extern bureaublad downloaden naar een lokale computer, de [cmdlet Get-AzureRemoteDesktopFile][link-technet-2]. Als u wilt gebruiken met deze cmdlet, moet u de naam van de service en de naam van de virtuele machine weten. Als u bij het maken van de VM uit de [Microsoft Azure portal][link-azure-portal], kunt u deze informatie onder ' VM properties ' vinden:

1. Selecteer in de Microsoft Azure-portal **alle services** > **VMs**.
2. De virtuele machines-blade wordt geopend. Selecteer de virtuele machine die u hebt geïmplementeerd.
3. Er wordt een blade geopend waarin wordt beschreven van de geselecteerde VM.
4. Klik op **Eigenschappen**.
5. Het eerste deel van de domeinnaam is de servicenaam. De hostnaam is de naam van de VM.

    ![tekenen][img-portal-vm-rdp]
6. De cmdlet voor het downloaden van het RDP-bestand voor de gemaakte VM naar het lokale bureaublad van de beheerder is als volgt.

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

Meer informatie over RDP kan worden gevonden op MSDN in het artikel [verbinding maken met een Azure-VM met RDP of SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).

**Een virtuele machine configureren en uw SKU maken**

Nadat het besturingssysteem dat de VHD is gedownload, gebruikt u Hyper-v en configureert u een VM om te beginnen met het maken van uw SKU. Gedetailleerde stappen kunnen u vinden op de volgende TechNet-koppeling: [Hyper-v installeren en configureren van een virtuele machine](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="34-choose-the-correct-vhd-size"></a>3.4 de juiste VHD-grootte kiezen
Het Windows-besturingssysteem VHD in uw VM-installatiekopie moet worden gemaakt als een VHD met vaste indeling van 128 GB.  

Als de fysieke grootte kleiner is dan 128 GB is, moet de VHD schaars zijn. De basis Windows en SQL Server-installatiekopieën die zijn opgegeven al aan deze vereisten voldoet, dus veranderen niet de indeling of de grootte van de verkregen VHD.  

Gegevensschijven mogen even groot zijn als 1 TB. Bij het bepalen van de schijfgrootte, houd er rekening mee dat klanten VHD's binnen een installatiekopie kunnen niet op het moment van implementatie aangepast. VHD's met gegevensschijven moeten worden gemaakt als een vaste indeling VHD. Ze moeten ook schaars zijn. Gegevensschijven kunnen leeg zijn of gegevens bevatten.

### <a name="35-install-the-latest-windows-patches"></a>3.5 installeert de meest recente patches voor Windows
De basisinstallatiekopieën bevatten de meest recente patches tot aan de publicatiedatum. Voordat u publiceert de besturingssysteem-VHD die u hebt gemaakt, moet u ervoor te zorgen dat Windows Update is uitgevoerd en dat alle essentiële en belangrijk beveiligingsupdates zijn geïnstalleerd.

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a>3.6 uitvoeren van extra taken voor de configuratie en planning zo nodig
Wanneer u aanvullende configuratie nodig hebt, kunt u overwegen een geplande taak die wordt uitgevoerd bij het opstarten alle laatste wijzigingen aanbrengen in de virtuele machine nadat deze is geïmplementeerd:

* Het wordt aanbevolen om de taak zichzelf na een geslaagde uitvoering te laten verwijderen.
* Er is geen configuratie afhankelijk zijn van andere schijven dan stations C of D, omdat dit de enige twee schijven zijn die altijd gegarandeerd aanwezig zijn. Station C is de besturingssysteemschijf en station D is de tijdelijke lokale schijf.

### <a name="37-generalize-the-image"></a>3.7 generaliseer de installatiekopie
Alle installatiekopieën in de Azure Marketplace moeten worden ingezet op een generieke manier herbruikbaar zijn. Met andere woorden, moet het besturingssysteem VHD worden gegeneraliseerd:

* Voor Windows, moet de installatiekopie van het 'Sysprep' en er geen configuraties worden uitgevoerd die geen ondersteuning voor de **sysprep** opdracht.
* U kunt de volgende opdracht uitvoeren vanuit de map directory % windir%\System32\Sysprep.

        sysprep.exe /generalize /oobe /shutdown

  Richtlijnen voor hoe het besturingssysteem Sysprep is opgegeven in stap van de volgende MSDN-artikel: [maken en uploaden van een Windows Server-VHD voor Azure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="4-deploy-a-vm-from-your-vhds"></a>4. Een VM implementeren vanaf uw VHD 's
Nadat u uw VHD's (de gegeneraliseerde VHD besturingssysteem en nul of meer gegevens VHD's) naar Azure storage-account hebt geüpload, kunt u ze kunt registreren als een gebruiker VM-installatiekopie. Vervolgens kunt u die afbeelding testen. Houd er rekening mee dat omdat uw VHD-besturingssysteem is gegeneraliseerd, u kunt geen rechtstreeks de virtuele machine implementeren door op te geven van de VHD-URL.

Voor meer informatie over VM-installatiekopieën, controleert u de volgende blogberichten:

* [VM-installatiekopie](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [Hoe u PowerShell van VM-installatiekopie naar](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [Over VM-installatiekopieën in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-the-necessary-tools-powershell-and-azure-classic-cli"></a>De benodigde hulpprogramma's, PowerShell en Azure klassieke CLI instellen
* [Het instellen van PowerShell](/powershell/azure/overview)
* [Het instellen van Azure classic CLI](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a>4.1 de VM-installatiekopie van een gebruiker maken
#### <a name="capture-vm"></a>Virtuele machine vastleggen
Lees de koppelingen hieronder voor instructies over het vastleggen van de virtuele machine met behulp van PowerShell-API/Azure CLI.

* [API](https://msdn.microsoft.com/library/mt163560.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure-CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a>Generaliseer de installatiekopie
Lees de koppelingen hieronder voor instructies over het vastleggen van de virtuele machine met behulp van PowerShell-API/Azure CLI.

* [API](https://msdn.microsoft.com/library/mt269439.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure-CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a>4.2 een virtuele machine van een gebruiker VM-installatiekopie implementeren
Voor het implementeren van een virtuele machine van de VM-installatiekopie van een gebruiker, kunt u de huidige [Azure-portal](https://manage.windowsazure.com) of PowerShell.

**Een VM implementeren vanaf de huidige Azure-portal**

1. Ga naar **nieuw** > **Compute** > **virtuele machine** > **uit galerie**.

2. Ga naar **mijn installatiekopieën**, en selecteer vervolgens de VM-installatiekopie van waaruit u een VM wilt implementeren:

   1. Let goed op welke installatiekopie u selecteert, omdat de **mijn installatiekopieën** weergave bevat zowel installatiekopieën van besturingssystemen als VM-installatiekopieën.
   2. Kijken naar het aantal schijven, kunt u eenvoudiger bepalen welk type installatiekopie die u implementeert, omdat de meeste VM-installatiekopieën meer dan één schijf bevatten. Het is echter nog steeds mogelijk om een VM-installatiekopie met alleen een enkele besturingssysteemschijf, waarvoor het **aantal schijven** ingesteld op 1.

      ![tekenen][img-manage-vm-select]
3. Volg de wizard virtuele machine maken en geeft u de VM de naam, VM-grootte, locatie, gebruikersnaam en het wachtwoord.

**Implementeren van een virtuele machine via PowerShell**

U kunt de volgende cmdlets gebruiken voor het implementeren van een grote virtuele machine via de gegeneraliseerde VM-installatiekopie zojuist hebt gemaakt.

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> Zie [veelvoorkomende problemen oplossen van problemen aangetroffen tijdens het maken van VHD] voor verdere ondersteuning.
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a>5. Certificering verkrijgen voor uw VM-installatiekopie
De volgende stap bij het voorbereiden van uw VM-installatiekopie voor de Azure Marketplace is dat certificeren.

Dit proces omvat het uitvoeren van een speciaal certificeringshulpprogramma, het uploaden van de verificatieresultaten naar de Azure-container waarin uw VHD's zich bevinden, toevoegen van een aanbieding, definiëren van uw SKU en uw VM-installatiekopie voor certificering indienen.

### <a name="51-download-and-run-the-certification-test-tool-for-azure-certified"></a>5.1 downloaden en uitvoeren van het hulpprogramma van de Test-certificering voor Azure Certified
Het certificeringshulpprogramma wordt uitgevoerd op een actieve VM die is ingericht vanuit uw gebruiker VM-installatiekopie, om ervoor te zorgen dat de VM-installatiekopie compatibel met Microsoft Azure is. Het hulpprogramma controleert of is voldaan aan de richtlijnen en vereisten voor het voorbereiden van uw VHD. De uitvoer van het hulpprogramma is een compatibiliteitsrapport, dit moet worden geüpload in de Portal voor publiceren tijdens het aanvragende certificering.

Het certificeringshulpprogramma kan worden gebruikt met zowel Windows als Linux-VM's. Deze verbinding maakt met Windows-VM's via PowerShell en maakt verbinding met Linux-VM's via SSH.Net:

1. Download eerst het certificeringshulpprogramma op de [Microsoft-downloadsite][link-msft-download].
2. Open het certificeringshulpprogramma en klik vervolgens op de **nieuwe Test Start** knop.
3. Uit de **testen informatie** scherm, voer een naam voor de test.
4. Bepaal of uw VM op Linux of op Windows draait. Afhankelijk van uw keuze selecteert u de vervolgopties.

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>**Het certificeringshulpprogramma verbinden met een Linux VM-installatiekopie**
1. Selecteer de SSH-verificatiemodus: wachtwoord of sleutelbestand.
2. Als u gebruikmaakt van verificatie op basis van wachtwoorden, voert u de Domain Name System (DNS)-naam, gebruikersnaam en wachtwoord.
3. Als u sleutelbestand verificatie gebruikt, voert u de DNS-naam, gebruikersnaam en locatie persoonlijke sleutel.

   ![Wachtwoordverificatie van Linux VM-installatiekopie][img-cert-vm-pswd-lnx]

   ![Sleutelbestand verificatie van Linux VM-installatiekopie][img-cert-vm-key-lnx]

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Het certificeringshulpprogramma verbinden met een VM op basis van een Windows-installatiekopie**
1. Voer de volledig gekwalificeerde VM DNS-naam (bijvoorbeeld: MyVMName.Cloudapp.net).
2. Voer de gebruikersnaam en wachtwoord.

   ![Wachtwoordverificatie van Windows-VM-installatiekopie][img-cert-vm-pswd-win]

Nadat u de juiste opties voor uw Linux of Windows-gebaseerde VM-installatiekopie hebt geselecteerd, selecteert u **testverbinding** om ervoor te zorgen dat SSH.Net of PowerShell een geldige verbinding is voor testdoeleinden. Nadat een verbinding tot stand is gebracht, selecteert u **volgende** om de test te starten.

Wanneer de test is voltooid, ontvangt u de resultaten ('Pass'/'Fail'/'Warning' (Geslaagd/Mislukt/Waarschuwing)) voor elk testelement.

![Testcases voor Linux VM-installatiekopie][img-cert-vm-test-lnx]

![Testcases voor Windows VM-installatiekopie][img-cert-vm-test-win]

Als een van de test mislukt, wordt uw installatiekopie niet gecertificeerd. Als dit het geval is, raadpleegt u de vereisten en breng de gewenste wijzigingen.

Na de geautomatiseerde test wordt u gevraagd extra invoer bieden op uw VM-installatiekopie via een scherm vragenlijst.  Voltooien van de vragen en selecteer vervolgens **volgende**.

![Certificering hulpprogramma vragenlijst][img-cert-vm-questionnaire]

![Certificering hulpprogramma vragenlijst][img-cert-vm-questionnaire-2]

Nadat u de vragenlijst hebt voltooid, kunt u aanvullende informatie zoals een SSH-toegang tot gegevens van de Linux-VM-installatiekopie en een uitleg voor beoordelingen van alle mislukte opgeven. U kunt de testresultaten en de logboekbestanden voor de uitgevoerde testcases naast uw antwoorden op de vragenlijst downloaden. De resultaten in dezelfde container als uw VHD's opslaan.

![Testresultaten voor certificering opslaan][img-cert-vm-results]

### <a name="52-get-the-shared-access-signature-uri-for-your-vm-images"></a>5.2 de shared access signature-URI ophalen voor uw VM-installatiekopieën
Gedurende het publicatieproces geeft u de uniform resource-id (URI's) die verwijzen naar elk van de VHD's die u voor uw SKU hebt gemaakt. Tijdens het certificeringsproces moet Microsoft toegang hebben tot deze VHD's. Daarom moet u een shared access signature URI voor elke VHD maken. Dit is de URI die moet worden ingevoerd in de **installatiekopieën** tabblad in de Portal voor publiceren.

De handtekening voor gedeelde toegang gemaakt van de URI moet voldoen aan de volgende vereisten:

Opmerking: de volgende instructies zijn alleen van toepassing op niet-beheerde schijven die het enige type dat wordt ondersteund zijn.

* Bij het genereren van handtekening voor gedeelde toegang URI's voor uw VHD's, zijn machtigingen lijst en lezen zijn voldoende. Verleen geen toegang voor schrijven ('Write') of verwijderen ('Delete').
* De duur van de toegang moet minimaal drie (3) weken uit wanneer de shared access signature URI wordt gemaakt.
* Als u wilt beveiligen voor UTC-tijd, de dag vóór de huidige datum te selecteren. Als de huidige datum 6 oktober 2014 is, selecteert u bijvoorbeeld 10/5/2014.

SAS-URL kan worden gegenereerd op meerdere manieren voor het delen van uw VHD voor Azure Marketplace.
Hier volgen de 3 aanbevolen hulpprogramma's:

1.  Azure Opslagverkenner
2.  Microsoft Storage Explorer
3.  Azure-CLI

**Azure Storage Explorer (aanbevolen voor Windows-gebruikers)**

Hieronder vindt u instructies voor het genereren van SAS-URL met behulp van Azure Storage Explorer

1. Download [Azure Storage Explorer 6 Preview 3](https://azurestorageexplorer.codeplex.com/) vanaf CodePlex. Ga naar [Azure Storage Explorer 6 Preview](https://azurestorageexplorer.codeplex.com/) en klikt u op **'Downloaden'**.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. Download [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) en te installeren nadat het.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. Nadat deze is geïnstalleerd, opent u de toepassing.
4. Klik op **Account toevoegen**.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. Geef de storage-accountnaam, de opslagaccountsleutel en het domein van opslageindpunten. Dit is het opslagaccount in uw Azure-abonnement waar u uw VHD hebt opgeslagen in Azure portal.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. Wanneer Azure Storage Explorer is verbonden met uw specifieke storage-account, wordt deze gestart waarin al de binnen het opslagaccount bevat. Selecteer de container waarnaar u het besturingssysteem schijf VHD-bestand (ook gegevensschijven als ze van toepassing zijn voor uw scenario) gekopieerd.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. Na het selecteren van de blob-container, Azure Storage Explorer begint met de bestanden in de container. Selecteer het installatiekopiebestand (VHD) die moet worden verzonden.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  Selecteer het VHD-bestand in de container en klik op de **Security** tabblad.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  In de **beveiliging van de Blob-Container** dialoogvenster vak, laat u de standaardinstellingen op de **toegangsniveau** tabblad en klik vervolgens op **Shared Access Signatures** tabblad.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. De volgende stappen voor het genereren van een shared access signature URI voor de VHD-installatiekopie:

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    a. **Toegang toegestaan vanaf:** selecteren om te waarborgen voor de UTC-tijd, de dag vóór de huidige datum. Als de huidige datum 6 oktober 2014 is, selecteert u bijvoorbeeld 10/5/2014.

    b. **Toegang toegestaan:** een datum selecteert die is ten minste drie weken na de **toegang toegestaan vanaf** datum.

    c. **Acties die zijn toegestaan:** selecteert u de **lijst** en **lezen** machtigingen.

    d. Als u uw VHD-bestand correct hebt geselecteerd, wordt het bestand wordt weergegeven in **blobnaam voor toegang tot** met de extensie .vhd.

    e. Klik op **Signature genereren**.

    f. In **gegenereerd gedeeld Access Signature-URI van deze container**, controleren op de volgende gemarkeerde bovenstaande:

       - Zorg ervoor dat uw installatiekopie bestandsnaam en **".vhd"** zijn in de URI.
       - Aan het einde van de handtekening, zorg ervoor dat **"rl ="** wordt weergegeven. Dit toont aan dat toegang voor lezen en de lijst met succes is opgegeven.
       - Controleer in het midden van de handtekening, **"sr = c '** wordt weergegeven. Dit toont aan dat u toegang tot de container niveau hebt

11. Om ervoor te zorgen dat de gegenereerde shared access signature-URI werkt, klikt u op **Test in Browser**. Deze moet het downloadproces beginnen.

12. Kopieer de shared access signature URI. Dit is de URI die u in de Portal voor Publiceren moet plakken.

13. Herhaal stap 6-10 voor elke VHD in de SKU.

**Microsoft Azure Storage Explorer (Windows, MAC en Linux)**

Hieronder vindt u instructies voor het genereren van SAS-URL met behulp van Microsoft Azure Storage Explorer

1.  Downloaden van Microsoft Azure Storage Explorer formulier [ http://storageexplorer.com/ ](http://storageexplorer.com/) website. Ga naar [Microsoft Azure Storage Explorer](http://storageexplorer.com/releasenotes.html) en klikt u op **'Downloaden voor Windows'**.

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  Nadat deze is geïnstalleerd, opent u de toepassing.

3.  Klik op **Account toevoegen**.

4.  Microsoft Azure Storage Explorer aan uw abonnement configureren met aanmelden bij uw account

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  Ga naar storage-account en selecteer de Container

6.  Selecteer **'Handtekening voor gedeelde toegang ophalen'..** met behulp van de rechtermuisknop op van de **container**

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  Begintijd, verlooptijd en machtigingen, overeenkomstig de volgende bijwerken

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    a.  **Begintijd:** selecteren om te waarborgen voor de UTC-tijd, de dag vóór de huidige datum. Als de huidige datum 6 oktober 2014 is, selecteert u bijvoorbeeld 10/5/2014.

    b.  **Verlooptijd:** een datum selecteert die is ten minste drie weken na de **begintijd** datum.

    c.  **Machtigingen:** selecteert u de **lijst** en **lezen** machtigingen

8.  Shared access signature voor containers URI kopiëren

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    Gegenereerde SAS-URL is voor de container niveau en nu moeten we de naam van de VHD toevoegen in het.

    Indeling van de Container niveau SAS-URL: `https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    De naam van de VHD invoegen na de containernaam van de in het SAS-URL als hieronder `https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Voorbeeld:

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    TestRGVM201631920152.vhd is de naam van de VHD en vervolgens VHD SAS-URL `https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    - Zorg ervoor dat uw installatiekopie bestandsnaam en **".vhd"** zijn in de URI.
    - Controleer in het midden van de handtekening, **"sp rl ="** wordt weergegeven. Dit toont aan dat toegang voor lezen en de lijst met succes is opgegeven.
    - Controleer in het midden van de handtekening, **"sr = c '** wordt weergegeven. Dit toont aan dat u toegang tot de container niveau hebt

9.  Om ervoor te zorgen dat de gegenereerde shared access signature-URI werkt, moet u deze in de browser testen. Het downloadproces moet worden gestart

10. Kopieer de shared access signature URI. Dit is de URI die u in de Portal voor Publiceren moet plakken.

11. Herhaal deze stappen voor elke VHD in de SKU.

**Azure klassieke CLI (aanbevolen voor niet-Windows & continue integratie)**

Hieronder vindt u instructies voor het genereren van SAS-URL met behulp van Azure classic CLI

[!INCLUDE [outdated-cli-content](../../includes/contains-classic-cli-content.md)]

1.  Downloaden van de klassieke Azure CLI uit [hier](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/). U vindt hier ook koppelingen naar de andere **[Windows](http://aka.ms/webpi-azure-cli)** en  **[MAC OS](http://aka.ms/mac-azure-cli)**.

2.  Zodra de App is gedownload, geïnstalleerd.

3.  Maak een PowerShell-(of andere uitvoerbare bestand van het script) bestand met de volgende code en lokaal opslaan

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    Werk de volgende parameters in bovenstaande

    a. **`<StorageAccountName>`**: Geef de naam van uw opslagaccount

    b. **`<Storage Account Key>`**: Geef de sleutel van uw opslagaccount

    c. **`<Permission Start Date>`**: Als u wilt beveiligen voor UTC-tijd, de dag vóór de huidige datum te selecteren. Bijvoorbeeld, als de huidige datum 26 oktober 2016 is waarde dan 25-10-2016. Als u Azure CLI versie 2.0 of hoger gebruikt, geeft u zowel de datum en tijd in de begin- en einddatums, bijvoorbeeld: 10-25-2016T00:00:00Z.

    d. **`<Permission End Date>`**: Selecteer een datum die ten minste drie weken na de **begindatum**. De waarde moet **02-11-2016**. Als u Azure CLI versie 2.0 of hoger gebruikt, geeft u zowel de datum en tijd in de begin- en einddatums, bijvoorbeeld: 11-02-2016T00:00:00Z.

    Hieronder volgt de voorbeeldcode na het bijwerken van de juiste parameters

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=TIQE5QWMKHpT5q2VnF1bb+NUV7NVMY2xmzVx1rdgIVsw7h0pcI5nMM6+DVFO65i4bQevx21dmrflA91r0Vh2Yw=="
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  Powershell-editor openen met 'Als Administrator uitvoeren'-modus en bestand openen in stap #3. U kunt een scripteditor die beschikbaar is op uw besturingssysteem.

5.  Voer het script uit en biedt u de SAS-URL voor het toegangsniveau van de container

    Hieronder wordt de uitvoer van de SAS-handtekening en kopieer het gemarkeerde gedeelte in Kladblok

    ![tekenen](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  Nu krijgt u het niveau van container SAS-URL en moet u de naam van de VHD toevoegen in het.

    Niveau SAS-URL voor container #

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  De naam van de VHD invoegen na de containernaam van de in het SAS-URL, zoals hieronder wordt weergegeven `https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    Voorbeeld:

    TestRGVM201631920152.vhd is de naam van de VHD en vervolgens VHD SAS-URL

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - Zorg ervoor dat uw installatiekopie-bestand met de naam en '.vhd' zich in de URI.
    -   Controleer in het midden van de handtekening, "sp rl =" wordt weergegeven. Dit toont aan dat toegang voor lezen en de lijst met succes is opgegeven.
    -   Controleer in het midden van de handtekening, "sr = c ' wordt weergegeven. Dit toont aan dat u toegang tot de container niveau hebt

8.  Om ervoor te zorgen dat de gegenereerde shared access signature-URI werkt, moet u deze in de browser testen. Het downloadproces moet worden gestart

9.  Kopieer de shared access signature URI. Dit is de URI die u in de Portal voor Publiceren moet plakken.

10. Herhaal deze stappen voor elke VHD in de SKU.


### <a name="53-provide-information-about-the-vm-image-and-request-certification-in-the-publishing-portal"></a>5.3 bevatten informatie over de VM-installatiekopie en certificering aanvragen in de Portal voor publiceren
Nadat u uw aanbieding en SKU hebt gemaakt, moet u de details van de afbeelding die is gekoppeld aan die SKU invoeren:

1. Ga naar de [Portal voor publiceren][link-pubportal], en meld u aan met uw verkopersaccount.
2. Selecteer de **VM-installatiekopieën** tabblad.
3. De id die wordt vermeld op de bovenkant van de pagina is feitelijk de aanbiedings-id en niet de SKU-id.
4. Vul de eigenschappen onder de **SKU's** sectie.
5. Onder **besturingssysteemfamilie**, klikt u op het type van het besturingssysteem die is gekoppeld aan het besturingssysteem-VHD.
6. In de **besturingssysteem** vak, beschrijven van het besturingssysteem. Denk na over een indeling, zoals besturingssysteemfamilie, type, versie en updates. Een voorbeeld is 'Windows Server Datacenter 2014 R2'.
7. Selecteer maximaal zes aanbevolen VM-groottes. Dit zijn aanbevelingen die aan de klant in de blade prijscategorie in de Azure Portal worden weergegeven wanneer deze besluit uw installatiekopie te kopen en implementeren. **Dit zijn alleen aanbevelingen. De klant kan elke VM-grootte die geschikt is voor de vermelde schijven in uw installatiekopie te selecteren.**
8. Voer de versie in. Het versieveld bevat een semantische versie om het product en de updates te identificeren:
   * Versies moeten zijn van het formulier X.Y.Z, waarbij X, Y en Z gehele getallen zijn.
   * Afbeeldingen in verschillende SKU's kunnen verschillende primaire en secundaire versies hebben.
   * Versies binnen een SKU moet alleen incrementele wijzigingen, die de patchversie (Z van X.Y.Z) te vergroten.
9. In de **OS VHD URL** voert u de shared access signature URI voor het besturingssysteem VHD gemaakt.
10. Als er gegevensschijven die zijn gekoppeld aan deze SKU, selecteert u de logische eenheid number (LUN) op die u wilt deze gegevensschijf bij de implementatie worden gekoppeld.
11. In de **LUN X VHD URL** voert u de shared access signature URI voor de eerste VHD met gegevens gemaakt.

    ![tekenen](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a>Veelvoorkomende problemen met SAS-URL en oplossingen

|Probleem|Foutbericht|Oplossen|Koppeling voor documentatie|
|---|---|---|---|
|Fout bij het kopiëren van afbeeldingen - '? ' is niet gevonden in de SAS-url|Fout: Kopiëren van afbeeldingen. Kan geen blob downloaden met behulp van SAS-Uri opgegeven.|De SAS-Url met behulp van aanbevolen hulpprogramma's bijwerken|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Fout bij het kopiëren van afbeeldingen - parameters voor "st" en "se" niet in de SAS-url|Fout: Kopiëren van afbeeldingen. Kan geen blob downloaden met behulp van SAS-Uri opgegeven.|De SAS-Url met de begin- en einddatums erop bijwerken|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Fout bij het kopiëren van afbeeldingen: "sp = rl" niet in de SAS-url|Fout: Kopiëren van afbeeldingen. Kan geen blob downloaden met behulp van SAS-Uri opgegeven|De SAS-Url met de machtigingen die zijn ingesteld als "Lezen" en "lijst bijwerken|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Fout bij het kopiëren van afbeeldingen - SAS-url hebt spaties in vhd-naam|Fout: Kopiëren van afbeeldingen. Kan geen blob downloaden met behulp van SAS-Uri opgegeven.|De SAS-Url zonder spaties bijwerken|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Fout bij het kopiëren van afbeeldingen: fout van de SAS-Url-autorisatie|Fout: Kopiëren van afbeeldingen. Kan geen blob vanwege Autorisatiefout downloaden|De SAS-Url opnieuw genereren|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Fout bij het kopiëren van installatiekopieën – SAS-Url "st" en "se" parameters nog geen datum / tijd-specificatie|Fout: Kopiëren van afbeeldingen. Kan geen blob vanwege onjuiste SAS-Url voor downloaden |SAS-Url Start- en einddatum parameters ("st", 'se') zijn vereist om volledige datum / tijd-specificatie, zoals 11-02-2017T00:00:00Z, en niet alleen de datum of verkort versies voor de tijd. Het is mogelijk aan het optreden van dit scenario met behulp van Azure CLI versie 2.0 of hoger. Zorg dat de volledige datum / tijd-specificatie en opnieuw genereren van de SAS-Url.|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|

## <a name="next-step"></a>Volgende stap
Als u klaar bent met de SKU-details, kunt u verder gaan naar de [Azure Marketplace-handleiding voor marketing content][link-pushstaging]. In deze stap van het publicatieproces, bieden u de marketinginhoud, prijzen en andere informatie die nodig is voor **stap 3: uw virtuele machine testen aanbieden in fasering**, waarin u verschillende use-casescenario's testen voordat u implementeert de aanbieding voor Azure Marketplace voor openbare zichtbaarheid en inkoop.  

## <a name="see-also"></a>Zie ook
* [Aan de slag: een aanbieding publiceren op Azure Marketplace](marketplace-publishing-getting-started.md)

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
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/
