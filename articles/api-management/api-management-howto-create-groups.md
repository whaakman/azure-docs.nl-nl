---
title: Ontwikkelaarsaccounts met behulp van groepen in Azure API Management beheren | Microsoft Docs
description: Informatie over het gebruik van groepen in Azure API Management ontwikkelaarsaccounts beheren
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: b4d71cdfbab535b02542fbb26c7555265e5f9c37
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Het maken en groepen gebruiken voor het beheren van de ontwikkelaarsaccounts in Azure API Management
In API Management worden groepen gebruikt voor het beheren van de zichtbaarheid van producten voor ontwikkelaars. Producten voor het eerst zichtbaar aan groepen zijn gemaakt en vervolgens de ontwikkelaars van die groepen kunnen weergeven en zich abonneren op de producten die gekoppeld aan de groepen zijn. 

API Management heeft de volgende onveranderbare systeemgroepen.

* **Beheerders** - Azure-abonnementbeheerders zijn lid van deze groep. Beheerders beheren service-exemplaren van API Management, door het maken van de API's, bewerkingen en producten die door ontwikkelaars worden gebruikt.
* **Ontwikkelaars** - Geverifieerde gebruikers van de ontwikkelaarsportal vallen in deze groep. Ontwikkelaars zijn de klanten die toepassingen bouwen met uw API's. Ontwikkelaars krijgen toegang tot de ontwikkelaarsportal en bouwen toepassingen waarmee de bewerkingen van een API worden aangeroepen.
* **Gasten** - Niet-geverifieerde gebruikers van de ontwikkelaarsportal, zoals potentiële klanten die de ontwikkelaarsportal van een API Management-exemplaar bezoeken, vallen in deze groep. Ze kunnen bepaalde alleen-lezentoegang krijgen, zoals de mogelijkheid om API's te bekijken maar niet om ze aan te roepen.

Naast deze systeemgroepen kunnen beheerders aangepaste groepen maken of [gebruikmaken van externe groepen in gekoppelde Azure Active Directory-tenants][leverage external groups in associated Azure Active Directory tenants]. Aangepaste en externe groepen kunnen naast systeemgroepen worden gebruikt om ontwikkelaars zichtbaarheid van en toegang tot API-producten te geven. U kunt bijvoorbeeld één aangepaste groep maken voor ontwikkelaars die zijn gekoppeld aan een specifieke partnerorganisatie en hen toegang geven tot de API's vanuit een product dat alleen relevante API's bevat. Een gebruiker kan lid zijn van meerdere groepen.

Deze handleiding wordt getoond hoe beheerders van een exemplaar van API Management kunnen nieuwe groepen toevoegen en koppel deze aan de producten en -ontwikkelaars.

> [!NOTE]
> Naast het maken en beheren van groepen in de publicatieportal bevindt, kunt u maken en beheren van uw groepen met de REST-API van API Management [groep](https://msdn.microsoft.com/library/azure/dn776329.aspx) entiteit.
> 
> 

## <a name="create-group"></a>Een groep maken
Klik op om een nieuwe groep **publicatieportal** in de Azure-Portal voor uw API Management-service. Hiermee gaat u naar de publicatieportal van API Management.

![Publicatieportal][api-management-management-console]

> Als u nog geen service-exemplaar van API Management hebt gemaakt, raadpleegt u [Service-exemplaar van API Management maken][Create an API Management service instance] in de zelfstudie [Aan de slag met Azure API Management][Get started with Azure API Management].
> 
> 

Klik op **groepen** van de **API Management** menu aan de linkerkant en klik vervolgens op **groep toevoegen**.

![Nieuwe groep toevoegen][api-management-add-group]

Voer een unieke naam voor de groep en een optionele beschrijving in en klikt u op **opslaan**.

![Nieuwe groep toevoegen][api-management-add-group-window]

De nieuwe groep wordt weergegeven in het tabblad groepen. Bewerken de **naam** of **beschrijving** van de groep, klikt u op de naam van de groep in de lijst. Als u wilt verwijderen van de groep, klikt u op **verwijderen**.

![Groep toegevoegd][api-management-new-group]

Nu dat de groep is gemaakt, kan het worden gekoppeld aan de producten en -ontwikkelaars.

## <a name="associate-group-product"></a>Koppelen van een groep aan een product
U kunt een groep koppelen met een product, **producten** van de **API Management** menu aan de linkerkant en klik vervolgens op de naam van de gewenste product.

![Zichtbaarheid instellen][api-management-add-group-to-product]

Selecteer de **zichtbaarheid** tabblad toevoegen en verwijderen van groepen en om de huidige groepen voor het product te bekijken. Als u wilt toevoegen of verwijderen van groepen, Controleer of schakel de selectievakjes uit voor de gewenste groepen en klik op **opslaan**.

![Zichtbaarheid instellen][api-management-add-group-to-product-visibility]

> [!NOTE]
> Zie het toevoegen van Azure Active Directory-groepen [hoe autoriseren ontwikkelaarsaccounts met Azure Active Directory in Azure API Management](api-management-howto-aad.md).
> 
> Voor het configureren van groepen van de **zichtbaarheid** tabblad voor een product **groepen beheren**.
> 
> 

Wanneer een product gekoppeld aan een groep is, kunnen ontwikkelaars in die groep weergeven en zich abonneren op het product.

## <a name="associate-group-developer"></a>Groepen koppelen aan ontwikkelaars
U kunt groepen koppelen aan ontwikkelaars, **gebruikers** van de **API Management** menu aan de linkerkant en vervolgens het selectievakje naast de ontwikkelaars die u wilt koppelen aan een groep.

![Ontwikkelaar toevoegen aan groep][api-management-add-group-to-developer]

Nadat de gewenste ontwikkelaars zijn geselecteerd en klikt u op de gewenste groep in de **toevoegen aan groep** vervolgkeuzelijst. Ontwikkelaars kunnen worden verwijderd uit groepen met behulp van de **verwijderen uit groep** vervolgkeuzelijst. 

![Ontwikkelaars][api-management-add-group-to-developer-saved]

Als de koppeling tussen de ontwikkelaar en de groep wordt toegevoegd, kunt u het bekijken in de **gebruikers** tabblad.

## <a name="next-steps"> </a>Volgende stappen
* Als een ontwikkelaar is toegevoegd aan een groep, kunnen ze bekijken en zich abonneren op de producten die zijn gekoppeld aan die groep. Zie voor meer informatie [maken en een product publiceren in Azure API Management][How create and publish a product in Azure API Management],
* Naast het maken en beheren van groepen in de publicatieportal bevindt, kunt u maken en beheren van uw groepen met de REST-API van API Management [groep](https://msdn.microsoft.com/library/azure/dn776329.aspx) entiteit.

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
