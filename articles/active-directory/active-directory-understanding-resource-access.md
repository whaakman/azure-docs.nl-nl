---
title: Informatie over toegang tot bronnen in Azure | Microsoft Docs
description: In dit onderwerp worden enkele concepten uitgelegd over abonnementbeheerders met toegang tot bronnen in de volledige Azure-portal controleren
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: 174f1706-b959-4230-9a75-bf651227ebf6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 9492afeda8c11d9d4df866e416a2c2c7e1684569
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="understanding-resource-access-in-azure"></a>Wat is de toegang tot bronnen in Azure?

Toegangsbeheer in Azure wordt gestart vanuit het oogpunt van facturering van. De eigenaar van een Azure-account toegankelijk in via de [Azure-Accountcentrum](https://account.windowsazure.com/subscriptions), is het Account Administrator (AA). Abonnementen zijn een container voor facturering, maar ze ook fungeren als een beveiligingsgrens: elk abonnement heeft een Service Administrator (SA) die u kunt toevoegen, verwijderen en wijzigen van de Azure-resources in het desbetreffende abonnement met behulp van de [Azure-portal](https://portal.azure.com/). De SA standaard van een nieuw abonnement is de AA, maar de AA de SA in het midden van Azure-Accounts kunt wijzigen.

<br><br>![Azure-Accounts][1]

Abonnementen hebben ook een koppeling naar een map. De map definieert een set van gebruikers. Deze kunnen gebruikers van werk of school dat de map gemaakt of ze kunnen externe gebruikers (dat wil zeggen, een Microsoft-Accounts). Abonnementen zijn toegankelijk voor een subset van de directorygebruikers die zijn toegewezen als Service-beheerder (SA) of Medebeheerder (CA); de enige uitzondering hierop is dat, omwille van de oudere Microsoft-Accounts (voorheen Windows Live ID) kan worden toegewezen als SA of CA zonder aanwezig in de map.

<br><br>![Toegangsbeheer in Azure][2]

Functionaliteit in de klassieke Azure portal kunnen SA's dat u bent aangemeld met een Microsoft-Account te wijzigen van de map die een abonnement is gekoppeld met behulp van de **Directory bewerken** opdracht op de **abonnementen** pagina in **instellingen**. Houd er rekening mee dat deze bewerking gevolgen voor het toegangsbeheer van het abonnement heeft.

> [!NOTE]
> De **Directory bewerken** opdracht in de klassieke Azure portal is niet beschikbaar voor gebruikers die zijn aangemeld met een werk- of schoolaccount omdat deze accounts kunnen aanmelden alleen naar de map waartoe ze behoren.
> 
> 

<br><br>![Eenvoudige Gebruikersstroom voor aanmelding][3]

In de eenvoudige zaak wordt een organisatie (bijvoorbeeld Contoso) afdwingen facturering en toegangsbeheer voor het hele dezelfde set van abonnementen. Dat wil zeggen, is de map gekoppeld aan abonnementen die eigendom zijn van één Azure-Account. Gebruikers zien bij geslaagde aanmelding bij de klassieke Azure portal twee verzamelingen van bronnen (beschreven in oranje in de vorige illustratie):

* Directory's waar hun gebruikersaccount bestaat (afkomstig is of als een foreign principal toegevoegd). Houd er rekening mee dat de map die wordt gebruikt voor aanmelding bij niet relevant zijn voor deze berekening, zodat uw directory's altijd weergegeven worden ongeacht waar u bent aangemeld.
* Bronnen die deel van de abonnementen die zijn gekoppeld aan de map die wordt gebruikt voor aanmelding en die de gebruiker toegang heeft uitmaken (waar ze zijn een SA- of CA).

<br><br>![Gebruiker met meerdere abonnementen en mappen][4]

Gebruikers met een abonnement op meerdere mappen hebt de mogelijkheid om over te schakelen van de huidige context van de klassieke Azure portal met behulp van het abonnementfilter. Dit resulteert in een afzonderlijke aanmelding naar een andere map onder de dekt, maar dit wordt bereikt naadloos met eenmalige aanmelding (SSO).

Bewerkingen zoals het verplaatsen van resources tussen abonnementen kunnen worden moeilijker als gevolg van deze weergave één map van abonnementen. Als u wilt de bron-overdracht uitvoeren, kan het eerste gebruik nodig zijn de **Directory bewerken** opdracht op de pagina abonnementen in **instellingen** om te koppelen van de abonnementen op dezelfde map.

## <a name="next-steps"></a>Volgende stappen
* Als u meer wilt weten over het wijzigen van de beheerders van een Azure-abonnement, ziet u [Azure-beheerdersrollen toevoegen of wijzigen](../billing/billing-add-change-azure-subscription-administrator.md)
* Zie voor meer informatie over hoe Azure Active Directory is gekoppeld aan uw Azure-abonnement, [hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
* Zie [Beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md) voor meer informatie over het toewijzen van rollen in Azure AD.

<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png
