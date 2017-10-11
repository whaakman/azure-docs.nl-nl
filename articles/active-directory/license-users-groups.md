---
title: Licentie-gebruikers in Azure Active Directory | Microsoft Docs
description: Informatie over het licentie-uzelf en uw gebruikers in Azure Active Directory.
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
custom: it-pro
ms.openlocfilehash: c4509cdb003687083d0456c1957b19cf35ee056a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="quickstart-license-users-in-azure-active-directory"></a>Snelstartgids: Licentie gebruikers in Azure Active Directory
Azure AD op basis van een licentie services werk door een abonnement voor Azure Active Directory (Azure AD) in uw Azure-tenant activeren. Nadat het abonnement actief is, zijn de servicefuncties beheerd door beheerders van Azure AD en gebruikt door gebruikers met een licentie. Bij de aankoop van Enterprise Mobility + Security, Azure AD Premium of Azure AD Basic, wordt uw tenant wordt bijgewerkt met het abonnement, met inbegrip van de geldigheidsperiode en vooruitbetaalde licenties. Uw abonnementsgegevens, waaronder het aantal licenties toegewezen of beschikbaar is, is beschikbaar via de Azure-portal onder **Azure Active Directory** door het openen van de **licenties** tegel. De **licenties** blade is ook de beste plaats voor het beheren van uw licentietoewijzingen.

Hoewel het verkrijgen van een abonnement hoeft u alleen betaald mogelijkheden configureren, moet u nog steeds gebruikerslicenties toewijzen voor betaalde Azure AD betaalde functies. Elke gebruiker die toegang moeten hebben tot of die wordt beheerd via een Azure AD betaald functie moet een licentie worden toegewezen. De licentietoewijzing is geen toewijzing tussen een gebruiker en een gekochte service, zoals Azure AD Premium, Basic of Enterprise Mobility + Security.

U kunt [op basis van een groep licentietoewijzing](active-directory-licensing-whatis-azure-portal.md) voor het instellen van regels, zoals het volgende:
* Alle gebruikers in uw directory ophalen automatisch een licentie
* Iedereen met de juiste functie een licentie opgehaald
* U kunt het besluit om andere beheerders in de organisatie te delegeren (met behulp van [selfservice groepen](active-directory-accessmanagement-self-service-group-management.md))

> [!TIP]
> Voor een gedetailleerde bespreking van de licentietoewijzing voor groepen, met inbegrip van geavanceerde scenario's en Office 365-licentieverlening scenario's, Zie [licenties toewijzen aan gebruikers door het lidmaatschap in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="assign-licenses-to-users-and-groups"></a>Licenties toewijzen aan gebruikers en groepen
Met behulp van een actief abonnement, moet u eerst een licentie toewijzen aan uzelf en vernieuw de browser om ervoor te zorgen dat u alle verwachte functies die deel uitmaakt van uw abonnement. De volgende stap is om licenties te wijzen aan de gebruikers die toegang nodig tot betaalde Azure AD-functies. Er is een eenvoudige manier om licenties toe te wijzen licenties toewijzen aan groepen gebruikers in plaats van aan personen. Als u licenties aan een groep toewijzen, worden alle groepsleden een licentie toegewezen. Als gebruikers worden toegevoegd of verwijderd uit de groep, wordt automatisch de juiste licentie toegewezen of verwijderd. 

> [!NOTE]
> Sommige Microsoft-services zijn niet beschikbaar op alle locaties. Voordat u een licentie kan worden toegewezen aan een gebruiker, de beheerder moet opgeven de **gebruikslocatie** eigenschap voor de gebruiker. U kunt instellen dat deze eigenschap onder **gebruiker** &gt; **profiel** &gt; **instellingen** in de Azure portal. Wanneer u de licentietoewijzing van de groep, krijgt een gebruiker waarvan gebruikslocatie is niet opgegeven dan de locatie van de map.

Wijs een licentie onder **Azure Active Directory** &gt; **licenties** &gt; **alle producten**, selecteer een of meerdere producten, en selecteer vervolgens **toewijzen** op de opdrachtbalk.

![Selecteer een licentie toewijzen](media/license-users-groups/select-license-to-assign.png)

U kunt de **gebruikers en groepen** blade voor meerdere gebruikers of groepen kiezen of uitschakelen van de service-abonnementen in het product. Gebruik het zoekvak op de voorgrond om te zoeken naar gebruikers- en groepsnamen.

![Selecteer een gebruiker of groep voor de licentietoewijzing](media/license-users-groups/select-user-for-license-assignment.png)

Wanneer u licenties aan een groep toewijzen, kan het even duren voordat alle gebruikers de licentie, afhankelijk van de grootte van de groep overnemen. U kunt de verwerkingsstatus controleren op de **groep** blade onder de **licenties** tegel.

![Licentie-toewijzingsstatus](media/license-users-groups/license-assignment-status.png)

Toewijzingsfouten kunnen optreden tijdens de toewijzing van Azure AD-licentie, maar relatief zeldzame zijn bij het beheren van Azure AD en Enterprise Mobility + Security-producten. Mogelijke toewijzingsfouten zijn beperkt tot:
- Toewijzing conflict: wanneer een gebruiker een licentie die niet compatibel is met de huidige licentie was toegewezen. In dit geval moet de nieuwe licentie toewijzen de huidige verwijdert.
- Beschikbare licenties overschreden: wanneer het aantal gebruikers in de toegewezen groepen de beschikbare licenties overschrijdt, status van de toewijzing van een gebruiker duidt op een mislukt als gevolg van ontbrekende licenties toewijzen.

### <a name="azure-ad-b2b-collaboration-licensing"></a>Azure AD B2B-samenwerking licentieverlening

B2B-samenwerking kunt u gastgebruikers uitnodigen in uw Azure AD-tenant voor toegang tot Azure AD-services en alle Azure-resources u beschikbaar maken.  

Er zijn geen kosten voor B2B-gebruikers en eraan toe te wijzen aan een toepassing in Azure AD. Maximaal 10 apps per gastgebruiker en 3 Basisrapporten zijn ook gratis voor B2B-samenwerking gebruikers. Als uw gastgebruiker juiste licenties in Azure AD-tenant van de partner toegewezen heeft, wordt ze ook in jouw e-mailadres worden licentie.

Dit is niet vereist, maar als u toegang bieden tot betaalde Azure AD-functies wilt, die gebruikers van de Gast B2B moeten een licentie hebben met de desbetreffende Azure AD-licenties. Een uitnodiging tenant met een Azure AD betaalde licentie kunt B2B-samenwerking gebruikersrechten toewijzen aan een extra vijf gastgebruikers uitgenodigd voor de tenant. Zie voor scenario's en informatie [B2B-samenwerking richtlijnen licentieverlening](active-directory-b2b-licensing.md).

## <a name="view-assigned-licenses"></a>Weergave toegewezen licenties

Een samenvatting weergegeven van de licenties toegewezen en beschikbaar wordt weergegeven onder **Azure Active Directory** &gt; **licenties** &gt; **alle producten**.

![Een samenvatting weergeven van licenties](media/license-users-groups/view-license-summary.png)

Een gedetailleerde lijst met toegewezen gebruikers en groepen is beschikbaar bij het selecteren van een specifiek product. De **gebruikers met een licentie** lijst bevat alle gebruikers die momenteel die een licentie en of de licentie is toegewezen aan de gebruiker rechtstreeks of als deze is overgenomen van een groep.

![Details van de licentie weergeven](media/license-users-groups/view-license-detail.png)

Op dezelfde manier de **groepen in licentie gegeven** lijst ziet u alle groepen waarop licenties zijn toegewezen. Selecteer een gebruiker of groep openen de **licenties** blade waarin alle licenties die zijn toegewezen aan dit object.

## <a name="remove-a-license"></a>Een licentie verwijderen

Als u wilt verwijderen van een licentie, gaat u naar de gebruiker of groep en open de **licenties** tegel. Selecteer de licentie, en klik op **verwijderen**.

![Een licentie verwijderen](media/license-users-groups/remove-license.png)

Licenties die zijn overgenomen door de gebruiker uit een groep kunnen niet rechtstreeks worden verwijderd. De gebruiker in plaats daarvan verwijderd uit de groep waarvan ze de licentie worden overgenomen.


## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u geleerd hoe licenties toewijzen aan gebruikers en groepen in Azure AD-directory. 

U kunt de volgende koppeling abonnement licentietoewijzingen configureren in Azure AD via de Azure portal gebruiken.

> [!div class="nextstepaction"]
> [Azure AD-licenties toewijzen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Overview) 