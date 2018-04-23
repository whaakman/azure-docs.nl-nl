---
title: Aan de slag met persoonlijke sjablonen | Microsoft Docs
description: Uw persoonlijke sjablonen toevoegen, beheren en delen met de Azure-portal, de Azure CLI of PowerShell.
services: marketplace-customer
documentationcenter: ''
author: msmbaldwin
manager: asimm
editor: ''
tags: marketplace, azure-resource-manager
keywords: ''
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mbaldwin
ms.openlocfilehash: c716f54a1361d41dbad00e2e45562d5fbf8fd6ca
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Aan de slag met persoonlijke sjablonen in Azure Portal
Een [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) sjabloon is een declaratief sjabloon om uw implementatie te definiëren. U kunt de resources bepalen die u wilt implementeren in een oplossing en de parameters en variabelen vaststellen waarmee u waarden kunt invoeren voor verschillende omgevingen. De sjabloon bestaat uit JSON en expressies die u kunt gebruiken om waarden voor uw implementatie samen te stellen.

U kunt de nieuwe functie voor **sjablonen** in [Azure Portal](https://portal.azure.com) gebruiken in combinatie met de resourceprovider **Microsoft.Gallery** als extensie van [Azure Marketplace](https://azure.microsoft.com/marketplace/). Zo kunnen gebruikers persoonlijke sjablonen maken, beheren en implementeren vanuit een persoonlijke bibliotheek.

> [!NOTE]
> Microsoft raadt u aan geen persoonlijke sjablonen in de portal te gebruiken, maar in plaats daarvan een toepassing te maken voor de servicecatalogus via [Managed Applications](../managed-applications/overview.md). U kunt de toepassing voor de servicecatalogus beschikbaar maken voor gebruikers in uw organisatie.

Dit document is een handleiding voor het toevoegen, beheren en delen van een persoonlijke **sjabloon** via Azure Portal.

## <a name="guidance"></a>Richtlijnen
Met de volgende tips kunt u **sjablonen** optimaal benutten wanneer u aan de slag gaat met uw oplossingen:

* Een **sjabloon** is een resource die een Resource Manager-sjabloon en aanvullende metagegevens bevat. De sjabloon werkt op dezelfde manier als een item in Marketplace. Het belangrijkste verschil is dat het een persoonlijk item is, in plaats van een openbaar Marketplace-item.
* De **sjablonen**bibliotheek is zeer geschikt voor gebruikers die hun implementaties willen aanpassen.
* **Sjablonen** zijn handig voor gebruikers die een eenvoudige opslagplaats in Azure nodig hebben.
* Begin met een bestaand Resource Manager-sjabloon. Zoek sjablonen in [github](https://github.com/Azure/azure-quickstart-templates) of [exporteer een sjabloon](../azure-resource-manager/resource-manager-export-template.md) vanuit een bestaande resourcegroep.
* **Sjablonen** zijn gekoppeld aan de gebruiker die deze publiceert. De naam van de uitgever is zichtbaar voor alle gebruikers met leestoegang.
* **Sjablonen** zijn resources van de Resource Manager en kunnen na publicatie niet meer worden gewijzigd.

## <a name="add-a-template-resource"></a>Een sjabloonresource toevoegen
Er zijn twee manieren om een **sjabloon**resource te maken in de Azure-portal.

### <a name="method-1-create-a-new-template-resource-from-a-running-resource-group"></a>Methode 1: Een nieuwe sjabloonresource maken vanuit een bestaande resourcegroep
1. Navigeer naar een bestaande resourcegroep in Azure Portal. Klik op **Sjabloon exporteren** in **Instellingen**.
2. Zodra de Resource Manager-sjabloon is geëxporteerd, gebruikt u de knop **Sjabloon opslaan** om de sjabloon op te slaan in de  **Sjabloon**opslag. Meer informatie over het exporteren van sjablonen vindt u [hier](../azure-resource-manager/resource-manager-export-template.md).
   <br /><br />
   ![Resourcegroep exporteren](media/rg-export-portal1.PNG)
3. Selecteer de opdrachtknop **Opslaan in sjabloon**.
   <br /><br />
4. Voer de volgende informatie in:
   
   * Naam: naam van het sjabloonobject. (Let op: in dit veld geeft u een naam op basis van Azure Resource Manager op. Alle naamsbeperkingen zijn van toepassing en de naam kan achteraf niet meer worden gewijzigd).
   * Beschrijving – korte samenvatting van de sjabloon.
     
     ![Sjabloon opslaan](media/save-template-portal1.PNG)
5. Klik op **Opslaan**.
   
   > [!NOTE]
   > De portal geeft een melding weer wanneer de geëxporteerde Resource Manager-sjabloon fouten bevat. U kunt deze Resource Manager-sjabloon alsnog opslaan onder Sjablonen. Controleer en corrigeer alle problemen met de Resource Manager-sjabloon voordat u deze opnieuw implementeert.
   > 
   > 

### <a name="method-2-add-a-new-template-resource-from-browse"></a>Methode 2: Een nieuwe sjabloonresource toevoegen door middel van bladeren
U kunt ook een nieuw **Sjabloon** maken en toevoegen met behulp van de knop Toevoegen in **Bladeren > sjablonen**. Geef een naam, beschrijving en JSON op voor de Resource Manager-sjabloon.

![Sjabloon toevoegen](media/add-template-portal1.PNG)

> [!NOTE]
> Microsoft.Gallery is een op tenants gebaseerde resourceprovider van Azure. De sjabloonresource is gekoppeld aan de gebruiker die deze gemaakt heeft. Deze is niet gekoppeld aan een bepaald abonnement. Kies een abonnement bij het implementeren van een sjabloon.
> 
> 

## <a name="view-template-resources"></a>Sjabloonresources bekijken
U kunt alle beschikbare **sjablonen** bekijken via **Bladeren > sjablonen**. Tot de beschikbare sjablonen behoren sjablonen die u hebt gemaakt en sjablonen met verschillende machtigingsniveaus die met u zijn gedeeld. Zie [Toegangsbeheer](#access-control-for-a-tenant-resource-provider) voor meer informatie.

![Sjabloon weergeven](media/view-template-portal1.PNG)

U kunt de gegevens van een **sjabloon** bekijken door op een item in de lijst te klikken.

![Sjabloon weergeven](media/view-template-portal2c.png)

## <a name="edit-a-template-resource"></a>Een sjabloonresource bewerken
U kunt een **sjabloon** bewerken door met de rechtermuisknop te klikken op het item in de lijst Bladeren of door op de opdrachtknop Bewerken te klikken.

![Sjabloon bewerken](media/edit-template-portal1a.PNG)

U kunt de beschrijving of de tekst van de  Resource Manager-sjabloon bewerken. U kunt de naam niet bewerken, omdat dit de naam is van een Resource Manager-resource. Wanneer u de JSON van de Resource Manager-sjabloon bewerkt, valideren we uw bewerkingen om er zeker van te zijn dat de JSON geldig is. Klik op **OK** en vervolgens op **Opslaan** om uw bijgewerkte sjabloon op te slaan.

![Sjabloon bewerken](media/edit-template-portal2a.PNG)

Wanneer de sjabloon is opgeslagen, wordt er ter bevestiging een melding weergegeven.

![Sjabloon bewerken](media/edit-template-portal3b.png)

## <a name="deploy-a-template-resource"></a>Een sjabloonresource implementeren
U kunt elk **sjabloon** implementeren waarvoor u een **lees**machtiging heeft. Vul de parameterwaarden voor de Resource Manager-sjabloon in om verder te gaan met de implementatie.

![Sjabloon implementeren](media/deploy-template-portal1b.png)

## <a name="share-a-template-resource"></a>Een sjabloonresource delen
U kunt een **sjabloon**resource delen met anderen. Delen werkt op ongeveer dezelfde manier als [het toewijzen van rollen voor een resource in Azure](../role-based-access-control/role-assignments-portal.md). De eigenaar van de **sjabloon** geeft andere gebruikers toestemming om te werken met een sjabloonresource. De persoon of groep personen met wie u de **sjabloon** hebt gedeeld, kan de Resource Manager-sjabloon en de galerie-eigenschappen zien.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Toegangsbeheer voor Microsoft.Gallery-resources
| Rol | Machtigingen |
| --- | --- |
| Eigenaar |Heeft volledige controle over de sjabloonresource, onder andere het delen ervan |
| Lezer |Kan de sjabloonresource lezen en uitvoeren (implementeren) |
| Inzender |Kan de sjabloonresource bewerken en verwijderen. Gebruiker kan de sjabloon niet delen met anderen |

Selecteer **Delen** door met de rechtermuisknop op het item te klikken, of wanneer u een specifiek item bekijkt.

![Sjabloon delen](media/share-template-portal1a.png)

 U kunt nu een rol en een gebruiker of groep kiezen die u toegang wilt geven tot een bepaald **sjabloon**. De beschikbare rollen zijn Eigenaar, Lezer en Inzender.

![Sjabloon delen](media/share-template-portal2b.png)

![Sjabloon delen](media/share-template-portal3b.png)

Klik op **Selecteren** en vervolgens op **Ok**. U kunt nu de gebruikers of groepen bekijken die u aan de resource hebt toegevoegd.

![Sjabloon delen](media/share-template-portal4b.png)

> [!NOTE]
> U kunt een sjabloon alleen delen met gebruikers en groepen in dezelfde tenant van Azure Active Directory. Als u een sjabloon deelt met een e-mailadres dat zich niet in uw tenant bevindt, wordt een uitnodiging verstuurd waarin de gebruiker wordt gevraagd om als gast deel te nemen aan de tenant.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Zie [Sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md) voor meer informatie over het maken van Resource Manager-sjablonen
* Zie het artikel over [sjabloonfuncties](../azure-resource-manager/resource-group-template-functions.md) voor inzicht in de functies die u in een Resource Manager-sjabloon kunt gebruiken

