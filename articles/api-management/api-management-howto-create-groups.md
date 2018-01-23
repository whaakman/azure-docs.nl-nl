---
title: Ontwikkelaarsaccounts met behulp van groepen in Azure API Management beheren | Microsoft Docs
description: Informatie over het gebruik van groepen in Azure API Management ontwikkelaarsaccounts beheren
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: apimpm
ms.openlocfilehash: f377e1aadc126934fb47f6371f12435d2742efa6
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Het maken en groepen gebruiken voor het beheren van de ontwikkelaarsaccounts in Azure API Management
In API Management worden groepen gebruikt voor het beheren van de zichtbaarheid van producten voor ontwikkelaars. Producten voor het eerst zichtbaar aan groepen zijn gemaakt en vervolgens de ontwikkelaars van die groepen kunnen weergeven en zich abonneren op de producten die gekoppeld aan de groepen zijn. 

API Management heeft de volgende onveranderbare systeemgroepen:

* **Beheerders** - Azure-abonnementbeheerders zijn lid van deze groep. Beheerders beheren service-exemplaren van API Management, door het maken van de API's, bewerkingen en producten die door ontwikkelaars worden gebruikt.
* **Ontwikkelaars** - Geverifieerde gebruikers van de ontwikkelaarsportal vallen in deze groep. Ontwikkelaars zijn de klanten die toepassingen bouwen met uw API's. Ontwikkelaars krijgen toegang tot de ontwikkelaarsportal en bouwen toepassingen waarmee de bewerkingen van een API worden aangeroepen.
* **Gasten** - Niet-geverifieerde gebruikers van de ontwikkelaarsportal, zoals potentiële klanten die de ontwikkelaarsportal van een API Management-exemplaar bezoeken, vallen in deze groep. Ze kunnen bepaalde alleen-lezentoegang krijgen, zoals de mogelijkheid om API's te bekijken maar niet om ze aan te roepen.

Naast deze systeemgroepen kunnen beheerders aangepaste groepen maken of [gebruikmaken van externe groepen in gekoppelde Azure Active Directory-tenants][leverage external groups in associated Azure Active Directory tenants]. Aangepaste en externe groepen kunnen naast systeemgroepen worden gebruikt om ontwikkelaars zichtbaarheid van en toegang tot API-producten te geven. U kunt bijvoorbeeld één aangepaste groep maken voor ontwikkelaars die zijn gekoppeld aan een specifieke partnerorganisatie en hen toegang geven tot de API's vanuit een product dat alleen relevante API's bevat. Een gebruiker kan lid zijn van meerdere groepen.

Deze handleiding wordt getoond hoe beheerders van een exemplaar van API Management kunnen nieuwe groepen toevoegen en koppel deze aan de producten en -ontwikkelaars.

Naast het maken en beheren van groepen in de publicatieportal bevindt, kunt u maken en beheren van uw groepen met de REST-API van API Management [groep](https://msdn.microsoft.com/library/azure/dn776329.aspx) entiteit.

## <a name="prerequisites"></a>Vereisten

Taken in dit artikel: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-group"></a>Een groep maken

Deze sectie wordt beschreven hoe u een nieuwe groep toevoegt aan uw API Management-account.

1. Selecteer **groepen** tabblad aan de linkerkant van het scherm.
2. Klik op **+ toevoegen**.
3. Voer een unieke naam voor de groep en een optionele beschrijving.
4. Kies **Maken**.

    ![Nieuwe groep toevoegen](./media/api-management-howto-create-groups/groups001.png)

Zodra de groep is gemaakt, wordt deze toegevoegd aan de **groepen** lijst. <br/>Bewerken de **naam** of **beschrijving** van de groep, klikt u op de naam van de groep en **instellingen**.<br/>Als u wilt verwijderen van de groep, klikt u op de naam van de groep en druk op **verwijderen**.

Nu dat de groep is gemaakt, kan het worden gekoppeld aan de producten en -ontwikkelaars.

## <a name="associate-group-product"></a>Koppelen van een groep aan een product

1. Selecteer **producten** tabblad naar links.
2. Klik op de naam van de gewenste product.
3. Druk op **toegangsbeheer**.
4. Klik op **+ groep toevoegen**.

    ![Nieuwe groep toevoegen](./media/api-management-howto-create-groups/groups002.png)
5. Selecteer de groep die u wilt toevoegen.

    ![Nieuwe groep toevoegen](./media/api-management-howto-create-groups/groups003.png)

    Als u wilt een groep verwijderen uit het product, klikt u op **verwijderen**.

    ![Een groep verwijderen](./media/api-management-howto-create-groups/groups004.png)

Wanneer een product gekoppeld aan een groep is, kunnen ontwikkelaars in die groep weergeven en zich abonneren op het product.

> [!NOTE]
> Zie het toevoegen van Azure Active Directory-groepen [hoe autoriseren ontwikkelaarsaccounts met Azure Active Directory in Azure API Management](api-management-howto-aad.md).

## <a name="associate-group-developer"></a>Groepen koppelen aan ontwikkelaars

Deze sectie wordt beschreven hoe u groepen koppelen aan leden.

1. Selecteer **groepen** tabblad aan de linkerkant van het scherm.
2. Selecteer **leden**.

    ![Een lid toevoegen](./media/api-management-howto-create-groups/groups005.png)
3. Druk op **+ toevoegen** en selecteer een lid.

    ![Een lid toevoegen](./media/api-management-howto-create-groups/groups006.png)
4. Druk op **Selecteer**.


Als de koppeling tussen de ontwikkelaar en de groep wordt toegevoegd, kunt u het bekijken in de **gebruikers** tabblad.

## <a name="next-steps"> </a>Volgende stappen
* Als een ontwikkelaar is toegevoegd aan een groep, kunnen ze bekijken en zich abonneren op de producten die zijn gekoppeld aan die groep. Zie voor meer informatie [maken en een product publiceren in Azure API Management][How create and publish a product in Azure API Management],
* Naast het maken en beheren van groepen in de publicatieportal bevindt, kunt u maken en beheren van uw groepen met de REST-API van API Management [groep](https://msdn.microsoft.com/library/azure/dn776329.aspx) entiteit.

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md
