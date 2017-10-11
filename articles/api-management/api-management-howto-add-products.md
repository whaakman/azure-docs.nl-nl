---
title: Het maken en een product publiceren in Azure API Management
description: Informatie over het maken en publiceren van producten in Azure API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 73bf4451ba1b71807e22440beecc73a7e8045c5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Het maken en een product publiceren in Azure API Management
In Azure API Management zijn een product bevat een of meer API's, evenals een gebruiksquotum en de gebruiksvoorwaarden. Zodra een product is gepubliceerd, kunnen ontwikkelaars abonneren op het product en begint met het gebruik van API's van het product. Het onderwerp bevat een handleiding voor het maken van een product, het toevoegen van een API en het publiceren van het voor ontwikkelaars.

## <a name="create-product"></a>Een product maken
Bewerkingen zijn toegevoegd en geconfigureerd voor een API in de publicatieportal. Voor toegang tot de publicatieportal bevindt, klikt u op **publicatieportal** in de Azure-Portal voor uw API Management-service.

![Publicatieportal][api-management-management-console]

> Als u nog geen service-exemplaar van API Management hebt gemaakt, raadpleegt u [Service-exemplaar van API Management maken][Create an API Management service instance] in de zelfstudie [Aan de slag met Azure API Management][Get started with Azure API Management].
> 
> 

Klik op **producten** in het menu aan de linkerkant om weer te geven de **producten** pagina en klik op **Product toevoegen**.

![Producten][api-management-products]

![Nieuw product][api-management-add-new-product]

Voer een beschrijvende naam voor het product in de **naam** veld en een beschrijving van het product in de **beschrijving** veld.

Producten in API Management kunnen worden **Open** of **beveiligde**. Voor beveiligde producten is een abonnement nodig voordat ze kunnen worden gebruikt, terwijl open producten zonder abonnement kunnen worden gebruikt. Controleer **abonnement vereisen** voor het maken van een beveiligd product waarvoor een abonnement. Dit is de standaardinstelling.

Controleer **goedkeuring abonnement vereisen** als u wilt dat een beheerder om te bekijken en accepteren of weigeren abonnementspogingen voor dit product. Als het selectievakje uitgeschakeld is, worden abonnementspogingen automatisch goedgekeurd. Zie voor meer informatie over abonnementen [abonnees van een product weergeven][View subscribers to a product].

Als u wilt toestaan dat ontwikkelaarsaccounts meerdere keren naar het product, Controleer de **meerdere abonnementen toestaan** selectievakje. Als dit selectievakje niet is ingeschakeld, kan elke ontwikkelaarsaccount slechts één keer aan het product abonneren.

![Onbeperkte meerdere abonnementen][api-management-unlimited-multiple-subscriptions]

Beperk het aantal meerdere gelijktijdige abonnementen, Controleer de **beperken het aantal gelijktijdige abonnementen op** selectievakje en voert u de limiet voor het abonnement. Gelijktijdige abonnementen zijn in het volgende voorbeeld wordt beperkt tot vier keer per developer-account.

![Vier meerdere abonnementen][api-management-four-multiple-subscriptions]

Nadat alle productopties voor nieuw zijn geconfigureerd, klikt u op **opslaan** voor het maken van het nieuwe product.

![Producten][api-management-products-page]

> Standaard nieuwe producten zijn niet gepubliceerd en zijn alleen zichtbaar voor de **beheerders** groep.
> 
> 

Voor het configureren van een product, klikt u op de naam van het product in de **producten** tabblad.

## <a name="add-apis"></a>API's toevoegen aan een product
De **producten** pagina bevat vier koppelingen voor de configuratie: **samenvatting**, **instellingen**, **zichtbaarheid**, en **abonnees**. De **samenvatting** tabblad kunt u API's toevoegen en publiceren of ongedaan maken van een product is.

![Samenvatting][api-management-new-product-summary]

Voordat het publiceren van uw product moet u een of meer API's toevoegen. Om dit te doen, klikt u op **API toevoegen aan product**.

![API's toevoegen][api-management-add-apis-to-product]

Selecteer de gewenste API's en klik op **opslaan**.

## <a name="add-description"></a>Beschrijvende informatie toevoegen aan een product
De **instellingen** tabblad kunt u gedetailleerde informatie over het product zoals het doel en de API's die u toegang tot krijgt andere nuttige informatie opgeven. De inhoud is gericht op de ontwikkelaars die aanroepen van de API en kunnen worden geschreven in tekst zonder opmaak of HTML-opmaak.

![Productinstellingen][api-management-product-settings]

Controleer **abonnement vereisen** voor het maken van een beveiligd product waarvoor een abonnement moet worden gebruikt of schakel het selectievakje uit om een open product die kan worden aangeroepen zonder een abonnement te maken.

Selecteer **goedkeuring abonnement vereisen** desgewenst handmatig goedkeuren van alle aanvragen van de product-abonnement. Standaard worden alle product abonnementen automatisch verleend.

Als u wilt toestaan dat ontwikkelaarsaccounts meerdere keren naar het product, Controleer de **meerdere abonnementen toestaan** selectievakje en geef optioneel een limiet. Als dit selectievakje niet is ingeschakeld, kan elke ontwikkelaarsaccount slechts één keer aan het product abonneren.

Vul eventueel de **gebruiksvoorwaarden** veld met een beschrijving van de gebruiksvoorwaarden voor het product welke abonnees accepteren moeten om te kunnen gebruiken van het product.

## <a name="publish-product"></a>Een product publiceren
Voordat de API's in een product kan worden aangeroepen, moet het product worden gepubliceerd. Op de **samenvatting** voor het product en klik op **publiceren**, en klik vervolgens op **Ja, publiceren** om te bevestigen. Een eerder gepubliceerde product als privé wilt maken, klikt u op **publicatie ongedaan maken**.

![Product publiceren][api-management-publish-product]

## <a name="make-visible"></a>Een product zichtbaar maken voor ontwikkelaars
De **zichtbaarheid** tabblad kunt u kiezen welke rollen kunnen zien van het product in de portal voor ontwikkelaars en zich abonneren op het product zijn.

![Zichtbaarheid van het product][api-management-product-visiblity]

Als u wilt in- of uitschakelen van de zichtbaarheid van een product voor de ontwikkelaars in een groep, schakel of schakel het selectievakje naast de groep en klik op **opslaan**.

> Zie voor meer informatie [maken en groepen gebruiken voor het beheren van de ontwikkelaarsaccounts in Azure API Management][How to create and use groups to manage developer accounts in Azure API Management].
> 
> 

## <a name="view-subscribers"></a>Abonnees van een product weergeven
De **abonnees** tabblad bevat de ontwikkelaars die het product zijn geabonneerd. De details en de instellingen voor elke ontwikkelaar kunnen worden bekeken door te klikken op de naam van de ontwikkelaar. In dit voorbeeld hebt nog geen ontwikkelaars geabonneerd op het product.

![Ontwikkelaars][api-management-developer-list]

## <a name="next-steps"> </a>Volgende stappen
Zodra de gewenste API's worden toegevoegd en het product gepubliceerd, kunnen ontwikkelaars abonneren op het product en begint met het aanroepen van de API's. Zie voor een zelfstudie leert u hoe wordt deze items, evenals een geavanceerde productconfiguratie [maken en configureren van geavanceerde productinstellingen in Azure API Management][How create and configure advanced product settings in Azure API Management].

Zie voor meer informatie over het werken met producten in de volgende video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[View subscribers to a product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How to create and use groups to manage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
