---
title: Het maken en een product publiceren in Azure API Management
description: Informatie over het maken en publiceren van producten in Azure API Management.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: e6b11145506780f9a08799c4c9daf55ba17b366d
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="create-and-publish-a-product"></a>Een product maken en publiceren  

In Azure API Management zijn een product bevat een of meer API's, evenals een gebruiksquotum en de gebruiksvoorwaarden. Zodra een product is gepubliceerd, kunnen ontwikkelaars abonneren op het product en begint met het gebruik van API's van het product.  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een product maken en publiceren
> * Een API toevoegen aan het product

![product toegevoegd](media/api-management-howto-add-products/added-product.png)

## <a name="prerequisites"></a>Vereisten

+ Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Ook de volgende zelfstudie te voltooien: [importeren en publiceren van uw eerste API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-and-publish-a-product"></a>Een product maken en publiceren

1. Klik op **producten** in het menu aan de linkerkant om weer te geven de **producten** pagina.
2. Klik op **+ Product**.

    ![product toegevoegd](media/api-management-howto-add-products/add-product.png)

    Wanneer u een product toevoegt, moet u de volgende informatie opgeven: 

    |Naam|Beschrijving|
    |---|---|
    |Weergavenaam|De naam op als u wilt dat deze worden weergegeven in de **ontwikkelaarsportal**.|
    |Naam|Een beschrijvende naam van het product.|
    |Beschrijving|De **beschrijving** veld kunt u gedetailleerde informatie over het product zoals het doel en de API's die u toegang tot krijgt andere nuttige informatie.|
    |Status|Druk op **gepubliceerde** als u wilt publiceren van het product. Voordat de API's in een product kan worden aangeroepen, moet het product worden gepubliceerd. Standaard nieuwe producten zijn niet gepubliceerd en zijn alleen zichtbaar voor de **beheerders** groep.|
    |Goedkeuring vereist|Controleer **goedkeuring abonnement vereisen** als u wilt dat een beheerder om te bekijken en accepteren of weigeren abonnementspogingen voor dit product. Als het selectievakje ingeschakeld is, worden abonnementspogingen automatisch goedgekeurd. |
    |Limiet voor het aantal abonnementen|Beperk het aantal meerdere gelijktijdige abonnementen, voer de limiet voor het abonnement. |
    |Juridische voorwaarden|U kunt de gebruiksvoorwaarden voor het product welke abonnees accepteren moeten om te kunnen gebruiken van het product kunt opnemen.|
    |API's|Producten zijn de koppelingen van een of meer API's. U kunt een aantal API's en ze bieden voor ontwikkelaars via de portal voor ontwikkelaars. <br/> U kunt een bestaande API toevoegen tijdens het maken van het product. U kunt een API toevoegen aan het product later vanuit de producten **instellingen** pagina of tijdens het maken van een API.|<br/>Ontwikkelaars zich eerst abonneren op een product ze toegang krijgen tot de API. Wanneer ze zich abonneert, krijgen ze een abonnementssleutel die geschikt is voor API in dat product.<br/> Als u de APIM exemplaar gemaakt, bent u een beheerder al, zodat u bent geabonneerd op elk product standaard.|

3. Klik op **maken** voor het maken van het nieuwe product.

### <a name="add-more-configurations"></a>Voeg meer configuraties

U kunt doorgaan met het configureren van het product na het opslaan van het door het kiezen van de **instellingen** tabblad. 

Weergave/abonnees toevoegen aan het product van de **abonnementen** tabblad.

Stel in dat een zichtbaarheid van een product voor ontwikkelaars of Gast van de **toegangsbeheer** tabblad.

## <a name="add-apis"></a>API's toevoegen aan een product

Producten zijn de koppelingen van een of meer API's. U kunt een aantal API's en ze bieden voor ontwikkelaars via de portal voor ontwikkelaars. U kunt een bestaande API toevoegen tijdens het maken van het product. U kunt een API toevoegen aan het product later vanuit de producten **instellingen** pagina of tijdens het maken van een API.

Ontwikkelaars zich eerst abonneren op een product ze toegang krijgen tot de API. Wanneer ze zich abonneert, krijgen ze een abonnementssleutel die geschikt is voor API in dat product. Als u de APIM exemplaar gemaakt, bent u een beheerder al, zodat u bent geabonneerd op elk product standaard.

### <a name="add-an-api-to-an-existing-product"></a>Een API toevoegen aan een bestaand product

1. Selecteer een product.
2. Selecteer het tabblad API's.
3. Klik op **+ API**.
4. Kies een API en klik op **maken**.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een product maken en publiceren
> * Een API toevoegen aan het product

Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Lege API maken en model van de API-antwoorden](mock-api-responses.md)
