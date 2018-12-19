---
title: De auteur van VM-aanbieding | Microsoft Docs
description: Een virtuele machine op Azure Marketplace publiceren.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: d77dad52e75253de02fd079d791861356c4c5e1f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247072"
---
# <a name="publish-a-virtual-machine-to-azure-marketplace"></a>Een virtuele machine op Azure Marketplace publiceren

Dit artikel bevat de stappen die u moet een virtuele machine-aanbieding publiceren op Azure Marketplace.

## <a name="prerequisites"></a>Vereisten

De volgende technische en niet-technische vereisten van toepassing op een virtuele machine op Azure Marketplace publiceren

### <a name="technical"></a>Technisch

-   [Technische vereisten voor het maken van een VM-installatiekopie voor de Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-prerequisites)

-   [Een Linux VHD maken en uploaden](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

-   [Maken en testen van een Linux-VM vanaf een installatiekopie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)

-   [Een Windows VHD maken en uploaden ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)

-   [Maken en testen van een Windows-VM op basis van een installatiekopie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)

-   [Het oplossen van veelvoorkomende problemen aangetroffen tijdens het maken van VHD](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting)

-   [Aanbevelingen voor beveiliging voor Azure Marketplace-installatiekopieën](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images)


### <a name="non-technical-business-requirements"></a>Niet-technische (zakelijke vereisten)

 -   Uw bedrijf (of een dochteronderneming) bevindt zich in een land/regio de verkopen via de Azure Marketplace wordt ondersteund

-   Uw product een licentie moet worden verleend op een manier die compatibel is met de factureringsmodellen ondersteund door de Azure Marketplace

-   U bent verantwoordelijk voor het maken van technische ondersteuning beschikbaar voor klanten in een commercieel redelijke manier. Deze ondersteuning kan gratis worden betaald, of via communityondersteuning.

-   U bent zelf verantwoordelijk voor licentiëring van uw software en eventuele afhankelijkheden voor software van derden.

-   U kunt u inhoud die voldoet aan de criteria voor uw aanbieding wilt laten vermelden op Azure Marketplace en in de Azure Management Portal.

-   U gaat ermee akkoord met de voorwaarden van de Deelnamebeleid voor Azure Marketplace en de overeenkomst voor uitgevers.

-   U stemt ermee in om te voldoen aan de [gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/website-terms-of-use/) , [privacyverklaring van Microsoft](https://www.microsoft.com/privacystatement/default.aspx), en [overeenkomst van Microsoft Azure Certified-programma](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="before-you-begin"></a>Voordat u begint

Als u aan alle vereisten, kunt u beginnen met het ontwerpen van uw aanbieding oplossingssjabloon. Voordat u begint, Controleer u de volgende aanbieding en SKU-gegevens.

**Aanbieding**

Een Azure-toepassing-aanbieding komt overeen met een klasse van het product van een uitgever. Als u een nieuw type oplossing/toepassing die u graag beschikbaar zijn in Azure Marketplace hebt, is een nieuwe aanbieding voor de beste benadering. Een aanbieding is een verzameling SKU's. Elke aanbieding wordt weergegeven als een eigen entiteit in Azure Marketplace.

**SKU**

Een SKU is de kleinst mogelijke eenheid van een aanbieding die kan worden gekocht. Vanuit de dezelfde productklasse (aanbieding), kunnen u onderscheid maken tussen verschillende functies van de ondersteunde SKU's. Bijvoorbeeld, de aanbieding wordt beheerd of niet-beheerde en verschillende factureringsmodellen worden ondersteund.

Voeg meerdere SKU's in de volgende scenario's:
- Wilt u ondersteuning voor verschillende factureringsmodellen, zoals Breng uw eigen licentie (BYOL) of betalen (betalen per gebruik).
- Elke SKU ondersteunt een reeks verschillende functies en de prijs van elke reeks functies is anders.

Een SKU wordt weergegeven onder de bovenliggende aanbieding in Azure Marketplace en wordt weergegeven als een eigen entiteit die kunnen worden gekocht in Azure portal.

## <a name="to-create-a-new-offer"></a>Een nieuwe aanbieding maken

1.  Aanmelden bij de [Cloud partner-portal](http://cloudpartner.azure.com/).

2.  Selecteer op de linker navigatiebalk **+ nieuwe aanbieding**, en selecteer vervolgens **virtuele Machines**.

    ![Nieuwe aanbieding voor virtuele machines](./media/cloud-partner-portal-publish-virtual-machine/publishvm1.png)

3.  Onder **nieuwe aanbieding**, selecteer **Editor**.

![Nieuwe aanbieding Editor](./media/cloud-partner-portal-publish-virtual-machine/publishvm2.png)

4.  Onder **Editor**, biedt u informatie in de volgende weergaven:
    - Aanbiedingsinstellingen
    - Voorraadeenheden
    - Marketplace
    - Ondersteuning voor die elke weergave bevat een set velden voor u om in te vullen. Vereiste velden worden aangegeven met een rode akterisksymbool (\*)

## <a name="to-configure-offer-settings"></a>Het configureren van instellingen bieden

1. Configureer de volgende **identiteit bieden** velden in de instellingen bieden.

    **Aanbiedings-ID**

     Een unieke id voor de aanbieding binnen een publisher-profiel. Deze ID wordt weergegeven in de product-URL's, Azure Resource Manager-sjablonen, en facturering rapporten. U kunt alleen kleine letters, alfanumerieke tekens en streepjes (-). De ID kan mag niet eindigen met een streepje en een maximum van 50 tekens. Bijvoorbeeld, als een uitgever **contoso** publiceert een aanbieding met aanbiedings-ID **voorbeeld-vm**, wordt deze weergegeven in Azure marketplace als **https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview**  
    >[!Note]
    >Dit veld wordt vergrendeld wanneer een aanbieding live meteen.

    **Uitgevers-ID**

    Een vervolgkeuzelijst voor uitgeverprofiel. Kies het profiel dat u wilt publiceren van de aanbieding onder. 
    >[!Note]
    >Dit veld wordt vergrendeld wanneer een aanbieding live meteen.

    **Naam**

    De weergavenaam voor uw aanbieding. Deze naam wordt weergegeven in Azure Marketplace en in Azure Portal. De naam mag maximaal 50 tekens bevatten. Gebruik de volgende richtlijnen voor de aanbiedingsnaam van:
    -  Gebruik een merknaam voor het product die makkelijk te herkennen is. 
    - Neem hier de naam van uw bedrijf niet, tenzij dat is hoe de aanbieding is gebracht.
    - Als u bent deze aanbieding op uw eigen website marketing, zorg er dan voor dat de naam is identiek aan de naam op uw website.

2. Selecteer **opslaan** instellingen bieden voltooid.

## <a name="to-create-a-sku"></a>Een SKU maken

1. Selecteer **SKU's**. 
2. Selecteer **Voeg een SKU toe** in te voeren een SKU-ID. De SKU-ID is een unieke id voor de SKU in een aanbieding. Deze ID wordt weergegeven in de product-URL's, Azure Resource Manager-sjablonen, en facturering rapporten. De SKU-ID:
    - Kan alleen een maximum van 50 tekens hebben.
    - Kan alleen worden samengesteld uit kleine letters, alfanumerieke tekens en streepjes (-).
    - De id mag niet eindigen op een streepje.

    ![Voeg een SKU toe](./media/cloud-partner-portal-publish-virtual-machine/publishvm4.png)


## <a name="configure-sku-details"></a>Configureren van de SKU-details

Nadat u een SKU toegevoegd, wordt deze weergegeven in de lijst van SKU's in de weergave van de SKU's. Selecteer de SKU-naam voor de SKU-informatie. De detailweergave kunt u gegevens in de volgende velden toevoegen.

### <a name="hide-this-sku"></a>Deze SKU verbergen

Gebruik deze instelling voor het beheren van inzicht SKU. Als 'Deze SKU verbergen' is uitgeschakeld, de SKU is zichtbaar in [Azure Marketplace](https://azuremarketplace.microsoft.com) en [Azure Portal](https://portal.azure.com/) aan klanten. U kunt de SKU verbergen als u wilt dat alleen het beschikbaar is via oplossingssjablonen en niet voor aankoop afzonderlijk.

### <a name="cloud-availability"></a>Beschikbaarheid van de cloudservice

Dit veld kunt u de beschikbaarheid van uw SKU definiëren in de verschillende Azure-Clouds.

**Openbare Azure**

Deze SKU kan worden geïmplementeerd voor klanten in alle openbare Azure-regio's die geïntegreerd met Marketplace is.

**Azure Government-Cloud**

Deze SKU kan worden geïmplementeerd in de Azure Government-Cloud. Voordat u publiceren naar [Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners), wordt aangeraden uitgevers testen en valideren van de oplossing werkt zoals verwacht. Fase te testen, [een proefaccount aanvragen](https://azure.microsoft.com/offers/ms-azr-usgov-0044p). 

>[!Note]
>Microsoft Azure Government is een overheidscommunitycloud met beperkte toegang voor klanten van de Amerikaanse federale overheid, status, lokale of Tribale, en partners komen in aanmerking voor het bieden van deze entiteiten.

### <a name="countryregion-availability"></a>Beschikbaarheid van het land/regio

Dit veld geeft de regio's waarin uw SKU gaat worden aangeschaft. U moet zorgvuldig overwegen waar u uw SKU's beschikbaar maken. Sommige landen zijn geclassificeerd als 'Microsoft belasting overgeschreven land'.

-   In 'Microsoft belasting overgeschreven land', Microsoft belastingen voorheft van klanten en betaalt belastingen (mandaten) aan de overheid.

-   Partners zijn verantwoordelijk voor het verzamelen van klanten belastingen en belastingen betalen van de overheid in andere landen. Als u ervoor kiest om te mogen verkopen in deze landen, moet u de mogelijkheid om te berekenen en belasting betalen in ze hebben.

![Beschikbaarheid van het land/regio selecteren](./media/cloud-partner-portal-publish-virtual-machine/publishvm5.png)

### <a name="pricing"></a>Prijzen

Twee prijsmodellen worden die momenteel ondersteund.

#### <a name="bring-your-own-license-byol"></a>Bring-Your-Own-License (BYOL)

U beheert de licentieverlening van de software die wordt uitgevoerd op de virtuele machine. Microsoft brengt alleen de kosten van de infrastructuur.

#### <a name="usage-based-monthly-billed-sku"></a>Gebruik op basis van de maand gefactureerd SKU

Klanten in rekening gebracht op basis van per uur op basis van de tarieven die zijn ingesteld door de uitgevers op de VM-grootten. In geval van **facturering per uur** model van de SKU's, worden de totale prijs de som van de softwarekosten in rekening gebracht door de uitgever en de infrastructuur hoeft te investeren in rekening gebracht door Microsoft. De totale kosten wordt weergegeven aan de klant als een prijs per uur en maandelijkse wanneer ze de aankoop overweegt. De facturering worden in dit geval wordt op maandbasis.

In het model op basis van gebruik, moet u er extra instellingen die vereist zijn door u zijn.

**Gratis proefversie**

U kunt opgeven als u wilt dat voor een gratis proefversie van uw klanten.
Hier de klant niet in rekening gebracht voor softwarekosten voor de eerste 30/90 dagen (afhankelijk van de selectie) na de implementatie van de virtuele machine. Nadat de gratis proefperiode is verstreken ze in rekening gebracht op basis van per uur op basis van de tarieven die zijn ingesteld door de uitgevers in het model per uur.

**Per Core prijzen**

U kunt instellen voor uw SKU per kern-prijzen. Voor dit, u hoeft in te voeren een basisprijs voor een enkele kern en we auto-compute de prijzen voor de rest van de kernen. De prijzen in Amerikaanse dollars opgeven in de portal en we auto-berekent de prijzen voor andere regio's. U kunt de prijzen in de andere regio's controleren met behulp van **prijzen gegevens exporteren**

![Per Core prijzen](./media/cloud-partner-portal-publish-virtual-machine/publishvm6.png)


**Discrete prijzen**

U kunt instellen met de prijzen voor elke sets kernen afzonderlijk als u graag zou willen elk kerngeheugen afzonderlijk prijs.

![Discrete prijzen](./media/cloud-partner-portal-publish-virtual-machine/publishvm7.png)

**Prijzen voor exporteren / importeren**

U hebt de flexibiliteit voor het exporteren van de prijzen die via de portal te wijzigen via de excel-interface is geconfigureerd. Hiermee kunt u ook controleren of de prijzen per regio en de prijzen in de lokale valuta's.
Te klikken op **Export prijzen** worden gedownload van een excel-bestand met prijsinformatie vooraf is ingevuld. Kunt u zich aan deze in de excel bewerken en gebruik vervolgens **importeren prijzen** voor het importeren van de wijzigingen die zijn aangebracht.
De geïmporteerde prijzen weer in de portal ook.

In deze prijzen excel, worden de prijzen voor de verschillende regio's weergegeven in de lokale valuta. De exchange-snelheid die we gebruiken wordt dagelijks vernieuwd.

![Exporteren / importeren prijs met wisselkoers voorbeelden](./media/cloud-partner-portal-publish-virtual-machine/publishvm8.png)

>[!IMPORTANT]
>-   Prijzen kunnen niet worden gewijzigd nadat een aanbieding live meteen. Maar u nog steeds mogelijk kunnen toevoegen of verwijderen van ondersteunde regio's.
>-   Dit bedrag wordt in rekening gebracht voor de gebruiker naast [Azure\'prijzen voor virtuele machines s](https://aka.ms/vmpricingdetails).
>-   Prijzen zijn ingesteld voor alle regio's in de lokale valuta, met behulp van de beschikbare wisselkoersen op het moment van prijzen.
>-   Instellen of de prijs van elke regio afzonderlijk weergeven, de prijzen spreadsheet exporteren en importeren met aangepaste prijzen.

## <a name="vm-images"></a>VM-installatiekopieën

De volgende sectie om te voltooien, is de sectie VM-installatiekopieën. Voordat u verdergaat met deze sectie, moet u de VHD die u wilt publiceren gereed hebben. Hier volgen enkele koppelingen die u helpen u bij het maken van uw VHD:

-   [Technische vereisten voor het maken van een VM-installatiekopie voor de Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-prerequisites)

-   [Het maken en uploaden van een VHD met Linux](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-create-upload-generic?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

-   [Maken en testen van een Linux-VM vanaf een installatiekopie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-upload-vhd)

-   [Het maken en uploaden van een Windows VHD ](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-prepare-for-upload-vhd-image?toc=/azure/virtual-machines/windows/toc.json)

-   [Maken en testen van een Windows-VM op basis van een installatiekopie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-create-vm-generalized-managed?toc=/azure/virtual-machines/windows/toc.json)

-   [Het oplossen van veelvoorkomende problemen aangetroffen tijdens het maken van VHD](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting)

Zodra u uw VHD gereed hebt, kunt u beginnen met het invullen van deze sectie.
Hier volgen enkele details voor een aantal velden.

### <a name="recommended-vm-sizes"></a>Aanbevolen VM-grootten

Selecteer maximaal zes aanbevolen VM-groottes. Dit zijn aanbevelingen die aan de klant op Azure Marketplace en de blade prijscategorie in de Azure Portal worden weergegeven wanneer deze besluit uw installatiekopie te kopen en implementeren. **Dit zijn alleen aanbevelingen. De klant kan elke VM-grootte die geschikt is voor de vermelde schijven in uw installatiekopie te selecteren.**  De volgende schermafbeelding ziet u de aanbevolen VM-grootten die een klant wordt weergegeven in de Azure-Portal.


![Aanbevolen VM-grootten](./media/cloud-partner-portal-publish-virtual-machine/publishvm9.png)


### <a name="open-ports"></a>Poorten openen

Geef op de poorten die u wilt openen en beschikbaar gesteld. Deze poorten zijn geopend tijdens de implementatie van deze virtuele machines.

### <a name="adding-vm-images"></a>VM-installatiekopieën toevoegen

De volgende stap is het toevoegen van een VM-installatiekopie voor uw SKU. U kunt maximaal 8 schijf versies per SKU toevoegen. Alleen de hoogste versie schijfnummer voor een bepaalde SKU wordt weergegeven in Azure Marketplace. Anderen zijn zichtbaar via API's.

Onder **schijf versie**, selecteer **+ nieuwe versie**. Dit bevat de volgende velden die u nodig hebt om in te vullen.

#### <a name="vm-image-version"></a>De versie van de VM-installatiekopie

De versie van de VM-installatiekopie moet volgen de [semantische versie](http://semver.org/) indeling. Versies moeten zijn van het formulier X.Y.Z, waarbij X, Y en Z gehele getallen zijn. Afbeeldingen in verschillende SKU's kunnen verschillende primaire en secundaire versies hebben. Versies binnen een SKU moet alleen incrementele wijzigingen, die de patchversie (Z van X.Y.Z) te vergroten.

#### <a name="os-vhd-url"></a>URL VAN DE VHD MET BESTURINGSSYSTEEM

Voer de [handtekening voor gedeelde toegang URI](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#52-get-the-shared-access-signature-uri-for-your-vm-images) gemaakt voor het besturingssysteem VHD.

Als er gegevensschijven die zijn gekoppeld aan deze SKU zijn, kunt u deze schijven toevoegen door het selecteren van de **+ nieuwe gegevensschijf** koppeling. Deze actie geeft aanvullende velden voor u om in te vullen.

#### <a name="lun-vhd-url"></a>LUN VHD URL

Voer de [handtekening voor gedeelde toegang URI](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#52-get-the-shared-access-signature-uri-for-your-vm-images) voor de gegevensschijf van uw.

#### <a name="lun-number"></a>Aantal LUN

Een nummer toewijzen aan deze LUN. Dit nummer wordt gereserveerd voor de gegevensschijf van deze in deze SKU.

>[!Note]
>Nadat u een SKU met een bepaalde versie van de virtuele machine en de gegevensschijven publiceert, moet deze worden vergrendeld en kunnen niet worden gewijzigd. Voor elke extra VM-versies die toegevoegd aan de SKU, wijzigen het aantal gegevensschijven die nodig is voor de ondersteuning van niet.

#### <a name="common-sas-url-issues--fixes"></a>Veelvoorkomende problemen met SAS-URL en oplossingen

| Probleem                                                                 | Bericht                                                                           | Oplossen                                                           |  Koppeling naar de documentatie                                                                                |
|---------------------------------------------------------------------  |-------------------------------------------------------------------------------    |-----------------------------------------------------------    |---------------------------------------------------------------------------------------------------    |
| Fout bij het kopiëren van afbeeldingen - '? ' is niet gevonden in de SAS-url                | Fout: Kopiëren van afbeeldingen. Kan geen blob downloaden met behulp van SAS-Uri opgegeven.       | De SAS-Url met behulp van aanbevolen hulpprogramma's bijwerken                    | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Fout bij het kopiëren van afbeeldingen - parameters voor "st" en "se" niet in de SAS-url   | Fout: Kopiëren van afbeeldingen. Kan geen blob downloaden met behulp van SAS-Uri opgegeven.        | De SAS-Url met de begin- en einddatums erop bijwerken             | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Fout bij het kopiëren van afbeeldingen: "sp = rl" niet in de SAS-url                    | Fout: Kopiëren van afbeeldingen. Kan geen blob downloaden met behulp van SAS-Uri opgegeven         | De SAS-Url met de machtigingen die zijn ingesteld als "Lezen" en "lijst bijwerken     | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Fout bij het kopiëren van afbeeldingen - SAS-url hebt spaties in vhd-naam     | Fout: Kopiëren van afbeeldingen. Kan geen blob downloaden met behulp van SAS-Uri opgegeven.        | De SAS-Url zonder spaties bijwerken                       | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |
| Fout bij het kopiëren van afbeeldingen: fout van de SAS-Url-autorisatie               | Fout: Kopiëren van afbeeldingen. Kan geen blob vanwege Autorisatiefout downloaden     | De SAS-Url opnieuw genereren                                        | https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/     |


## <a name="to-configure-the-marketplace"></a>Het configureren van de Marketplace

Gebruik de Marketplace-weergave voor het configureren van de velden die worden weergegeven voor de aanbieding op [Azure Marketplace](https://azuremarketplace.microsoft.com) en klik op [Azure Portal](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Preview-versie abonnement-id 's

De lijst met Azure-abonnement-id's die u toegang hebben tot de aanbieding wilt wanneer de aanbieding wordt gepubliceerd. Deze abonnementen wit vermeld, kunt u voor het testen van de voorbeeld-aanbieding voordat u het live. In de partner-portal kunt u een acceptatielijst met maximaal 100 abonnementen maken.

### <a name="suggested-categories"></a>Aanbevolen categorieën

Selecteer maximaal 5 categorieën in de opgegeven lijst dat uw aanbieding beste is gekoppeld. De geselecteerde categorieën worden gebruikt om uw aanbieding worden toegewezen aan de productcategorieën die beschikbaar zijn in [Azure Marketplace](https://azuremarketplace.microsoft.com) en [Azure Portal](https://portal.azure.com/).

De volgende voorbeelden ziet de marketplace-gegevens in de Azure Marketplace en de Azure-Portal.

**Azure Marketplace**


![publishvm10](./media/cloud-partner-portal-publish-virtual-machine/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-virtual-machine/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-virtual-machine/publishvm15.png)


**Azure Portal**


![publishvm12](./media/cloud-partner-portal-publish-virtual-machine/publishvm12.png)



![publishvm13](./media/cloud-partner-portal-publish-virtual-machine/publishvm13.png)


### <a name="logo-guidelines"></a>Richtlijnen voor het logo

Volg deze richtlijnen voor het logo's uploaden naar de Cloud Partner-Portal:

-   Het Azure-ontwerp heeft een eenvoudig kleurenpalet. Houd het aantal primaire en secundaire kleuren op uw logo laag.

-   De themakleuren van het Azure-portal zijn wit en zwart. Vermijd het gebruik van deze kleuren als de achtergrondkleur van uw logo's. Gebruik een kleur die uw logo's opvallende in Azure portal. We adviseren eenvoudige primaire kleuren.

    >[!Note] 
    >Als u van een transparante achtergrond gebruikmaakt, moet u ervoor zorgen dat de logo's / tekst worden niet wit, zwart-wit of blauw.

-   Gebruik geen achtergrond met een kleurovergang in het logo.

-   Plaats tekst op het logo. Dit omvat uw bedrijf of een merk. Het uiterlijk van uw logo moet *platte* en vermijd kleurovergangen.

-   Het logo mag niet worden uitgebreid.

#### <a name="hero-logo"></a>Hero-Logo

De Hero-logo is optioneel. De publisher kunt niet een Hero-logo uploaden. Worden echter, nadat het logo is geüpload, deze kan niet verwijderd. De partner moet voldoen aan de Azure Marketplace-richtlijnen voor de Hero-pictogrammen.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Richtlijnen voor het pictogram van Hero-logo

-   De weergavenaam van de uitgever, plan titel en de lange samenvatting van de aanbieding worden weergegeven met een witte gekleurde lettertype. Vermijd het gebruik van een lichte kleur op de achtergrond. Zwart-wit en transparante achtergrond zijn niet toegestaan voor de Hero-pictogrammen.

-   De uitgever weergavenaam, plan titel, de aanbieding voor lange samenvatting en de knop maken zijn ingesloten via een programma in de Hero-logo wanneer van de aanbieding weergegeven. Voer de tekst bij het ontwerpen van de Hero-logo. Laat een lege ruimte aan de rechterkant van het logo. Deze ruimte moet 415 x 100 pixels en wordt gecompenseerd door 370 pixels vanaf de linkerkant.

![Voorbeeld van hero-logo](./media/cloud-partner-portal-publish-virtual-machine/publishvm14.png)

### <a name="lead-management"></a>Leads beheren

Voor het configureren van de instellingen voor potentiële klanten van de aanbieding, gaat u als volgt [deze instructies](./cloud-partner-portal-get-customer-leads.md).

## <a name="to-configure-support"></a>Het configureren van ondersteuning

Gebruik de weergave van de ondersteuning voor de volgende informatie:

- Contactpersonen van uw bedrijf, zoals technische ondersteuning.
- Contactpersonen voor ondersteuning van klant.

## <a name="to-publish-the-offer"></a>De aanbieding publiceren

De laatste stap is het publiceren van de aanbieding. Ga als volgt [deze instructies voor het live gaan met uw aanbieding](./cloud-partner-portal-make-offer-live-on-azure-marketplace.md).
