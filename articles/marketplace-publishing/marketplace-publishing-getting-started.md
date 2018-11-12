---
title: Overzicht van hoe u maken en implementeren van een aanbieding op Marketplace | Microsoft Docs
description: Informatie over de stappen die nodig zijn om te worden van een goedgekeurde Microsoft-ontwikkelaar en maken en implementeren van een installatiekopie van virtuele machine, sjabloon, data-service of ontwikkelaar-service in de Azure Marketplace
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: 5343bd26-c6e4-4589-85b7-4a2c00bba8ab
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio
ms.openlocfilehash: 3556384d71649391d094172a136b863ed26fd2ad
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227396"
---
> [!NOTE]
> Deze documentatie is niet meer actueel en is niet nauwkeurig. In plaats daarvan gaat u naar de Azure Marketplace [Verkopershandleiding voor](https://docs.microsoft.com/azure/marketplace/seller-guide/cloud-partner-portal-seller-guide) voor hulp bij het publiceren van een aanbieding op Azure Marketplace.

# <a name="publish-and-manage-an-offer-in-the-azure-marketplace"></a>Publiceren en beheren van een aanbieding op Azure Marketplace
In dit artikel is bedoeld om ontwikkelaars maken, implementeren en beheren van hun oplossingen die worden vermeld in de Azure Marketplace voor andere Azure-klanten en partners aan te schaffen en te gebruiken.

## <a name="marketplace-publishing"></a>Marketplace-publicatie
Als een Azure-uitgever, u kunt distribueren en uw innovatieve oplossing of service verkopen aan andere ontwikkelaars, ISV's, en IT-professionals in de Marketplace. Via de Marketplace, kunt u klanten die willen ontwikkelen snel hun cloud-Apps en mobiele oplossingen bereiken. Als uw oplossing is gericht op zakelijke gebruikers, kunt u rekening houden met de [AppSource](https://appsource.microsoft.com) marketplace.


## <a name="supported-types-of-solutions"></a>Ondersteunde typen oplossingen
Het eerste wat dat u wilt doen als een uitgever is om te definiëren wat voor soort oplossing aangeboden door uw bedrijf. De Marketplace ondersteunt de volgende typen aanbiedingen:

|Oplossingstype|Virtuele machine|Oplossingssjabloon|
|---|---|---|
|**Definitie**|Vooraf geconfigureerde installatiekopieën met een volledig geïnstalleerd besturingssysteem en een of meer toepassingen. Een VM-installatiekopie bevat de informatie die nodig zijn om te maken en implementeren van virtuele machines in de Azure Virtual Machines-service.|Een gegevensstructuur die kan verwijzen naar een of meer afzonderlijke Azure-services, waaronder services die zijn gepubliceerd door andere verkopers. Azure-abonnees kunnen gebruiken op een of meer aanbiedingen implementeren op een enkele, gecoördineerde wijze.|
|**Voorbeeld**|U hebt gemaakt en een virtuele machine met een innovatieve databaseservice gevalideerd als een Azure publisher. Andere Azure-abonnees wilt aanschaffen en deze virtuele machine implementeren in hun cloud service-omgevingen.|Als een Azure-uitgever, hebt u een set services van gebundeld in Azure waarmee u snel te implementeren, cloudservices met taakverdeling, verbeterde beveiliging en hoge beschikbaarheid. Andere Azure-abonnees kunnen tijd besparen door de sjabloon van de oplossing die voldoet aan hun doel te kopen. Ze geen handmatig zoeken, aanschaffen, implementeren en configureren van de dezelfde of gelijksoortige Azure-services.|

> [!NOTE]
> Sommige stappen worden gedeeld tussen de verschillende typen oplossingen en andere resources zijn uniek voor het betreffende type oplossing. Dit artikel bevat een kort overzicht van de stappen die u nodig hebt om voor elk type oplossing te voltooien.

## <a name="publish-a-solution"></a>Publiceren van een oplossing
![Nomineer, registreren, te publiceren](media/marketplace-publishing-getting-started/img01.png)

### <a name="nominate-your-solution-for-pre-approval"></a>Nomineer uw oplossing voor voorafgaande goedkeuring
Voor het publiceren van een virtuele machine [oplossing](https://createopportunity.azurewebsites.net) voltooien in de Marketplace, de Microsoft Azure Certified **oplossing Nominatieformulier**.

>[!NOTE]
> Als u met een Partner Account Manager of een Partner DX Manager werkt, vraagt u ze aan het benoemen van uw oplossing voor het Azure Certified-programma. U kunt ook gaan naar de [Microsoft Azure Certified](http://createopportunity.azurewebsites.net) webpagina en vul het aanvraagformulier in. Voer het e-mailadres van uw Partner-accountmanager of DX Partner Manager in de **Neem contact op met Microsoft-Sponsor** vak.

Als u voldoet aan de criteria in aanmerking te komen in de [deelnamebeleid voor Azure Marketplace](https://go.microsoft.com/fwlink/?LinkID=526833) en uw toepassing is goedgekeurd, zijn we aan de slag met u voor de onboarding uw oplossing in de Marketplace.

### <a name="register-your-account-as-a-microsoft-seller"></a>Meld u aan uw account als een Microsoft-verkopers
Registreren van uw Microsoft-account als een [Microsoft Developer-account](marketplace-publishing-accounts-creation-registration.md).

### <a name="publish-your-solution"></a>Publiceer uw oplossing
Als u wilt een oplossing op Marketplace publiceert, de volgende stappen uit:
1. Aan de niet-technische vereisten voldoen.

    a. Voldoen aan de [niet-technische vereisten](marketplace-publishing-pre-requisites.md).

    b. Voldoen aan de [VM technische vereisten](marketplace-publishing-vm-image-creation-prerequisites.md).

    c. Voldoen aan de [oplossing technische vereisten voor de sjabloon](marketplace-publishing-solution-template-creation-prerequisites.md).

2. Maak uw aanbieding.

    a. Maak een [virtuele machine](marketplace-publishing-vm-image-creation.md) bieden.

    b. Maak een [oplossingssjabloon](marketplace-publishing-solution-template-creation.md) bieden.

3. Maken van uw aanbieding [marketing inhoud](marketplace-publishing-push-to-staging.md).

4. Test uw aanbieding in fasering.

    a. Testen van uw VM-aanbieding in [staging](marketplace-publishing-vm-image-test-in-staging.md).

    b. Testen van uw aanbieding van de sjabloon oplossing in [staging](marketplace-publishing-solution-template-test-in-staging.md).

5. Implementeren van uw aanbieding aan de [Marketplace](marketplace-publishing-push-to-production.md).


### <a name="create-and-manage-a-virtual-machine-image"></a>Maken en beheren van de installatiekopie van een virtuele machine
Maken en beheren van een VM-installatiekopie met behulp van deze resources:
* Maak een VM-installatiekopie [on-premises](marketplace-publishing-vm-image-creation-on-premise.md).
* Maken van een virtuele machine met [Windows in Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Maken van een virtuele machine met [Linux in Azure portal](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Oplossen van veelvoorkomende fouten die optreden tijdens [VHD maken](marketplace-publishing-vm-image-creation-troubleshooting.md).

## <a name="manage-your-solution"></a>Uw oplossing beheren
Beheren van uw oplossing met behulp van de volgende bronnen:
* [Lees de handleiding na productie voor virtuele machine-aanbiedingen](marketplace-publishing-vm-image-post-publishing.md)
* [Bijwerken van de niet-technische details van een aanbieding of een SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-nontechnical-details-of-an-offer-or-a-sku)
* [Bijwerken van de technische details van een aanbieding of een SKU](marketplace-publishing-vm-image-post-publishing.md#update-the-technical-details-of-a-sku)
* [Een nieuwe SKU voor een vermelde aanbieding toevoegen](marketplace-publishing-vm-image-post-publishing.md#add-a-new-sku-under-a-listed-offer)
* [Het aantal gegevensschijven voor een vermelde SKU wijzigen](marketplace-publishing-vm-image-post-publishing.md#change-the-data-disk-count-for-a-listed-sku)
* [Een vermelde aanbieding uit Marketplace verwijderen](marketplace-publishing-vm-image-post-publishing.md)
* [Een lijst SKU verwijderen uit de Marketplace](marketplace-publishing-vm-image-post-publishing.md#delete-a-listed-sku-from-the-marketplace)
* [De huidige versie van een vermelde SKU verwijderen uit de Marketplace](marketplace-publishing-vm-image-post-publishing.md#delete-the-current-version-of-a-listed-sku-from-the-marketplace)
* [Herstellen van de prijs van de lijst met productiewaarden](marketplace-publishing-vm-image-post-publishing.md#revert-the-listing-price-to-production-values)
* [Het factureringsmodel naar productiewaarden herstellen](marketplace-publishing-vm-image-post-publishing.md#revert-the-billing-model-to-production-values)
* [De zichtbaarheidsinstelling van een vermelde SKU op de productiewaarde ongedaan maken](marketplace-publishing-vm-image-post-publishing.md#revert-the-visibility-setting-of-a-listed-sku-to-the-production-value)

## <a name="additional-resources"></a>Aanvullende resources
[Instellen van Azure PowerShell](marketplace-publishing-powershell-setup.md)
