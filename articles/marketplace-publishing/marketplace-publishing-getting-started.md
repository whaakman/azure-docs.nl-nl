---
title: Overzicht van het maken en implementeren van een aanbieding voor de Marketplace | Microsoft Docs
description: Ken de stappen vereist om te worden van een goedgekeurde Microsoft developer en maken en implementeren van een installatiekopie van virtuele machine, sjabloon, data-service of developer-service in Azure Marketplace
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
ms.openlocfilehash: 8fbf201343f6710d2781a4b56ae54833ed4c06cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="publish-and-manage-an-offer-in-the-azure-marketplace"></a>Publiceren en beheren van een aanbieding in Azure Marketplace
In dit artikel is bedoeld om ontwikkelaars maken, implementeren en beheren van hun oplossingen die worden vermeld in de Azure Marketplace voor andere Azure-klanten en partners aan te schaffen en gebruiken op te geven.

## <a name="marketplace-publishing"></a>Publicatie van de Marketplace
Als een Azure publisher, u kunt distribueren en uw vernieuwende oplossing of de service aan andere ontwikkelaars, onafhankelijke softwareleveranciers, verkopen en IT-professionals in de Marketplace. U kunt de klanten die willen hun cloud-gebaseerde toepassingen en mobiele oplossingen snel kunt ontwikkelen bereiken via de Marketplace. Als uw oplossing is bedoeld voor zakelijke gebruikers, kunt u overwegen de [AppSource](http://appsource.microsoft.com) marketplace.


## <a name="supported-types-of-solutions"></a>Ondersteunde typen oplossingen
Het eerste wat dat u wilt doen als een publisher te definiëren wat voor soort oplossing aangeboden door uw bedrijf. De Marketplace ondersteunt de volgende soorten aanbiedingen:

|Oplossingstype|Virtuele machine|Oplossingssjabloon|
|---|---|---|
|**Definitie**|Vooraf geconfigureerde afbeeldingen met een volledig geïnstalleerde besturingssysteem en een of meer toepassingen. De installatiekopie van een virtuele machine bevat de informatie die nodig zijn om te maken en implementeren van virtuele machines in de service Azure Virtual Machines.|Een gegevensstructuur die kunt verwijzen naar een of meer verschillende Azure-services, waaronder services gepubliceerd door andere verkopers. Azure-abonnees kunnen deze gebruiken voor het implementeren van een of meer aanbiedingen op een enkele, gecoördineerde wijze.|
|**Voorbeeld**|U hebt gemaakt en gevalideerd van een virtuele machine met een databaseservice innovatieve als een Azure publisher. Andere Azure-abonnees wilt aanschaffen en implementeren van deze virtuele machine in hun cloud service-omgeving.|Als een Azure publisher, hebt u een set services van gebundeld in Azure om het snel te implementeren van cloudservices met taakverdeling, verbeterde beveiliging en hoge beschikbaarheid. Andere Azure-abonnees kunnen tijd besparen door de oplossingssjabloon die voldoet aan hun doel te kopen. Dit is niet altijd handmatig vinden, schaft u, implementeren en configureren van de dezelfde of gelijksoortige Azure-services.|

> [!NOTE]
> Een aantal stappen worden gedeeld tussen de verschillende soorten oplossingen en andere zijn uniek voor het betreffende type oplossing. Dit artikel bevat een kort overzicht van de stappen die u moet voltooien voor elk type oplossing.

## <a name="publish-a-solution"></a>Publiceren van een oplossing
![Benoemen, registreren, publiceren](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>Uw oplossing benoemen voor voorafgaande goedkeuring
Voor het publiceren van een virtuele machine [oplossing](https://createopportunity.azurewebsites.net) om de Marketplace voltooien is de Microsoft Azure-gecertificeerd **oplossing benoeming formulier**.

>[!NOTE]
> Als u met een Partner-accountmanager of een Partner DX Manager werkt, vraagt u ze voor het benoemen van uw oplossing voor het programma Azure gecertificeerd. U kunt ook gaan naar de [Microsoft Azure-gecertificeerd](http://createopportunity.azurewebsites.net) webpagina en de toepassing formulier invullen. Voer het e-mailadres van uw Partner-accountmanager of DX Partner Manager in de **Sponsor contact op met Microsoft** vak.

Als u voldoet aan de criteria in aanmerking komt in de [Azure Marketplace deelname beleid](http://go.microsoft.com/fwlink/?LinkID=526833) en uw aanvraag wordt goedgekeurd, wordt aan de slag met u om vrij te geven uw oplossing naar de Marketplace.

### <a name="register-your-account-as-a-microsoft-seller"></a>Uw account registreren als een Microsoft-verkoper
Registreren van uw Microsoft-account als een [Microsoft Developer-account](marketplace-publishing-accounts-creation-registration.md).

### <a name="publish-your-solution"></a>Publiceren van uw oplossing
Voor het publiceren van een oplossing voor de Marketplace, de volgende stappen uit:
1. Aan de niet-technische vereisten voldoen.

    a. Voldoen aan de [niet-technische vereisten](marketplace-publishing-pre-requisites.md).

    b. Voldoen aan de [VM technische vereisten](marketplace-publishing-vm-image-creation-prerequisites.md).

    c. Voldoen aan de [technische vereisten voor oplossing](marketplace-publishing-solution-template-creation-prerequisites.md).

2. Maak uw aanbieding.

    a. Maak een [virtuele machine](marketplace-publishing-vm-image-creation.md) bieden.

    b. Maak een [oplossingssjabloon](marketplace-publishing-solution-template-creation.md) bieden.

3. Maken van uw aanbieding [inhoud marketing](marketplace-publishing-push-to-staging.md).

4. Test uw aanbieding in fasering.

    a. Testen van uw VM-aanbieding in [fasering](marketplace-publishing-vm-image-test-in-staging.md).

    b. Testen van uw oplossing sjabloon aanbieding in [fasering](marketplace-publishing-solution-template-test-in-staging.md).

5. Implementeer uw aanbieding voor de [Marketplace](marketplace-publishing-push-to-production.md).


### <a name="create-and-manage-a-virtual-machine-image"></a>Maken en beheren van de installatiekopie van een virtuele machine
Maken en beheren van een installatiekopie van een virtuele machine met behulp van deze bronnen:
* Maak een VM-installatiekopie [lokale](marketplace-publishing-vm-image-creation-on-premise.md).
* Maken van een virtuele machine met [Windows in de Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Maken van een virtuele machine met [Linux in de Azure portal](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Algemene problemen aangetroffen tijdens [VHD maken](marketplace-publishing-vm-image-creation-troubleshooting.md).

## <a name="manage-your-solution"></a>Beheren van uw oplossing
Beheren van uw oplossing met behulp van de volgende bronnen:
* [Lees de handleiding na productie voor de virtuele machine aanbiedingen](marketplace-publishing-vm-image-post-publishing.md)
* [Bijwerken van de niet-technische details van een aanbieding of een SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-nontechnical-details-of-an-offer-or-a-sku)
* [Bijwerken van de technische details van een aanbieding of een SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-technical-details-of-a-sku)
* [Voeg een nieuwe SKU onder een vermelde aanbieding](marketplace-publishing-vm-image-post-publishing.md#add-a-new-sku-under-a-listed-offer)
* [Wijzig het aantal gegevensschijven voor een lijst SKU](marketplace-publishing-vm-image-post-publishing.md#change-the-data-disk-count-for-a-listed-sku)
* [Een vermelde aanbieding verwijderen uit de Marketplace](marketplace-publishing-vm-image-post-publishing.md)
* [Een vermelde SKU verwijderen uit de Marketplace](marketplace-publishing-vm-image-post-publishing.md#delete-a-listed-sku-from-the-marketplace)
* [De huidige versie van een vermelde SKU verwijderen uit de Marketplace](marketplace-publishing-vm-image-post-publishing.md#delete-the-current-version-of-a-listed-sku-from-the-marketplace)
* [De aanbieding prijs naar productiewaarden herstellen](marketplace-publishing-vm-image-post-publishing.md#revert-the-listing-price-to-production-values)
* [Het facturering model naar productiewaarden herstellen](marketplace-publishing-vm-image-post-publishing.md#revert-the-billing-model-to-production-values)
* [De zichtbaarheidsinstelling van een vermelde SKU om de waarde van de productie te herstellen](marketplace-publishing-vm-image-post-publishing.md#revert-the-visibility-setting-of-a-listed-sku-to-the-production-value)
* [Uw wederverkoper Cloud Solution Provider stimulans wijzigen](marketplace-publishing-csp-incentive.md)
* [Inzicht in uw toekenning rapportage](marketplace-publishing-report-payout.md)
* [Ondersteuning krijgen als een publisher](marketplace-publishing-get-publisher-support.md)

## <a name="additional-resources"></a>Aanvullende bronnen
[Instellen van Azure PowerShell](marketplace-publishing-powershell-setup.md)
