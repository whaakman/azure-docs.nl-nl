---
title: Overzicht van Azure PIM Resource RBAC | Microsoft Docs
description: Bekijk een overzicht van de RBAC-functie in PIM met inbegrip van de terminologie en meldingen
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: 9b4980681cb3e7442211b06255d6eefd8a1b1170
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622351"
---
# <a name="pim-for-azure-resources"></a>PIM voor Azure-resources

Met Azure Active Directory Privileged Identity Management (PIM) kunt u nu toegang tot Azure-resources binnen uw organisatie beheren, bepalen en controleren. Dit omvat abonnementen, resourcegroepen en zelfs virtuele Machines. Elke resource in Azure portal die gebruikmaakt van de functionaliteit van Azure rollen gebaseerd toegangsbeheer (RBAC) kunt profiteren van alle de uitstekende beveiligings- en functionaliteit voor het beheer die Azure AD PIM te bieden heeft en aantal geweldige nieuwe functies die we om te plannen Azure AD-rollen beschikbaar. 

## <a name="pim-for-azure-resources-helps-resource-administrators"></a>PIM voor Azure-Resources kan beheerders een resource

- Zien welke gebruikers en groepen zijn toegewezen rollen voor de Azure-resources beheren
- Op aanvraag, 'just-in-time' toegang tot bronnen zoals abonnementen en resourcegroepen beheren inschakelen
- Toegang tot toegewezen gebruikers/groepen met toewijzingsinstellingen voor nieuwe tijdsgebonden automatisch verlopen
- Tijdelijke toegang voor snelle taken of op een aanroep schema's toewijzen
- Multi-factor Authentication afdwingen voor toegang tot bronnen op een ingebouwde of aangepaste rol 
- Rapporten over resourceactiviteit voor resource access gecorreleerde tijdens de actieve sessie van een gebruiker ophalen
- Waarschuwingen ontvangen wanneer nieuwe gebruikers of groepen toegang tot resources worden toegewezen, en wanneer ze in aanmerking komende toewijzingen activeren

Azure AD PIM kunt beheren de ingebouwde Azure-Resource-rollen, evenals aangepaste rollen (RBAC), inclusief (maar niet beperkt tot):

- Eigenaar
- Beheerder van gebruikerstoegang
- Inzender
- Beveiligingsbeheerder
- Beveiligingsbeheer en meer

>[!NOTE]
Gebruikers of leden van een groep die is toegewezen aan de rollen eigenaar of Administrator voor gebruikerstoegang en globale beheerders waarmee beheer van abonnementen in Azure AD zijn Resource-beheerders. Deze beheerders kunnen rollen toewijzen, configureren van instellingen voor de sitesysteemrol en toegang met behulp van PIM voor Azure-Resources beoordelen. De lijst weergeven met [ingebouwde rollen voor Azure-resources](../../role-based-access-control/built-in-roles.md).

## <a name="tasks"></a>Taken

PIM biedt snel toegang tot rollen activeren, activeringen/aanvragen, in afwachting van goedkeuring in behandeling weergeven (voor [Azure AD-maprollen](azure-ad-pim-approval-workflow.md)), en in afwachting van je reactie in het gedeelte van de taken van het linkernavigatiemenu beoordeelt.

Bij het openen van een van de taken menu-items van het toegangspunt overzicht, bevat de resulterende weergave resultaten voor Azure AD-maprollen en Azure-Resource-rollen. 

![](media/azure-pim-resource-rbac/role-settings-details.png)

Mijn rollen bevatten een lijst van uw actieve en in aanmerking komende roltoewijzingen voor Azure AD-maprollen en Azure-Resource-rollen.

## <a name="activate-roles"></a>Rollen activeren

Rollen voor Azure-Resources activeren introduceert een nieuwe ervaring biedt waarmee leden van een in aanmerking komende rol voor het plannen van activering voor een toekomstige datum/tijd en selecteer een specifieke activeringsduur binnen de maximale (geconfigureerd door beheerders). Meer informatie over [hier Azure AD-rollen activeren](pim-how-to-activate-role.md).

![](media/azure-pim-resource-rbac/contributor.png)

Voer in het menu activeringen in de gewenste datum en tijd om de rol te activeren. (Optioneel) de duur van de activering te verlagen (hoe lang de rol actief is) en voer een reden als vereist. klikt u op activeren.

Als de begindatum en -tijd is niet gewijzigd, wordt de rol binnen enkele seconden worden geactiveerd. U ziet dat een rol in de wachtrij voor activering bannerbericht aangegeven op de pagina Mijn rollen. Klik op de vernieuwknop om te wissen van dit bericht.

![](media/azure-pim-resource-rbac/my-roles.png)

Als de activering is gepland voor een toekomstige datum-tijd, wordt de aanvraag in behandeling weergegeven op het tabblad aanvragen in behandeling van het navigatiemenu links. In het geval dat de rolactivering is niet langer vereist, kan de gebruiker de aanvraag annuleren door te klikken op de knop Annuleren aan de rechterkant van de pagina.

![](media/azure-pim-resource-rbac/pending-requests.png)

## <a name="discover-and-manage-azure-resources"></a>Detecteren en beheren van Azure-resources

Selecteer om te zoeken en beheren van rollen voor een Azure-Resource, Azure-Resources op het tabblad beheren in het navigatiemenu links. Gebruik de filters of de zoekbalk boven aan de pagina te vinden van een resource.

![](media/azure-pim-resource-rbac/azure-resources.png)

## <a name="resource-dashboards"></a>Resource-dashboards

Het dashboard Beheerdersweergave heeft vier primaire onderdelen. Een grafische weergave van rolactiveringen resource gedurende de afgelopen zeven dagen. Deze gegevens is afgestemd op de geselecteerde resource en activeringen voor de meest voorkomende rollen (eigenaar, Inzender, beheerder van gebruikerstoegang) en alle rollen gecombineerd weergegeven.

Aan de rechterkant van de grafiek activeringen, zijn twee grafieken die de distributie van roltoewijzingen door dit toewijzingstype, voor zowel gebruikers en groepen weergeven. Een segment van de grafiek selecteert, verandert de waarde in een percentage (of omgekeerd).

![](media/azure-pim-resource-rbac/admin-view.png)

Onder de grafieken ziet u het aantal gebruikers en groepen met nieuwe roltoewijzingen gedurende de afgelopen 30 dagen (links) en een lijst met functies die zijn gesorteerd op basis van totaal aantal toewijzingen (aflopend).

![](media/azure-pim-resource-rbac/role-settings.png)

## <a name="manage-role-assignments"></a>Roltoewijzingen beheren

Beheerders kunnen roltoewijzingen beheren door te selecteren van functies of leden in de linkernavigatiebalk. Rollen selecteren Hiermee kunnen beheerders het bereik van hun beheertaken voor een specifieke rol, terwijl de leden wordt weergegeven voor alle gebruikers en groepen-roltoewijzingen voor de resource.

![](media/azure-pim-resource-rbac/roles.png)

![](media/azure-pim-resource-rbac/members.png)

>[!NOTE]
Hebt u een rol in behandeling zijnde activering, wordt een meldingenbanner weergegeven aan de bovenkant van de pagina bij het weergeven van het lidmaatschap.

## <a name="assign-roles"></a>Rollen toewijzen

Als u wilt een gebruiker of groep toewijzen aan een rol, selecteer de rol (als rollen weergeven) of klik op toevoegen in de actiebalk (als op de leden weergeven).

![](media/azure-pim-resource-rbac/members2.png)

>[!NOTE]
Als een gebruiker of groep op het tabblad leden wilt toevoegen, moet u een rol selecteren in het menu toevoegen voordat u kunt een gebruiker of groep selecteren.

![](media/azure-pim-resource-rbac/select-role.png)

Kies een gebruiker of groep in de map.

![](media/azure-pim-resource-rbac/choose.png)

Kies het juiste toewijzingstype in de vervolgkeuzelijst. 

**Just-In-Time-toewijzing:** biedt de leden van de gebruiker of groep met die in aanmerking komen, maar geen permanente toegang tot de rol voor een opgegeven periode of voor onbepaalde tijd (indien geconfigureerd in de rolinstellingen). 

**Directe toewijzing:** vereist niet de leden van de gebruiker of groep activeren van de roltoewijzing (bekend als permanente toegang). Microsoft adviseert om rechtstreekse toewijzing toe voor de korte termijn gebruikt, zoals op de aanroep ploegen of tijd gevoelige activiteiten, waarbij toegang wordt niet zijn vereist wanneer de taak voltooid is.

![](media/azure-pim-resource-rbac/membership-settings.png)

Een selectievakje onder de vervolgkeuzelijst toewijzing kunt u opgeven als de toewijzing permanente moet (permanent in aanmerking voor het activeren van Just-in-Time-toewijzing/permanent actief voor directe toewijzing). Als u wilt een duur van de specifieke toewijzing opgeven, schakelt u het selectievakje in en start wijzigen en/of eindigen velden voor datum en tijd.

>[!NOTE]
Het is mogelijk dat het selectievakje unmodifiable als een andere beheerder de duur van de maximale toewijzing voor elk toewijzingstype is opgegeven in de functie-instellingen.

![](media/azure-pim-resource-rbac/calendar.png)

## <a name="view-activation-and-azure-resource-activity"></a>Activering en de activiteit Azure-resources weergeven

In het geval dat u nodig hebt om te zien welke verschillende resources heeft een specifieke gebruiker ondernomen acties, kunt u de Azure-Resource-activiteit die is gekoppeld aan een bepaalde activeringsperiode (voor in aanmerking komende gebruikers) bekijken. Begin met het selecteren van een gebruiker vanuit de weergave van de leden of in de lijst met leden in een specifieke rol. Het resultaat wordt een grafische weergave van acties van de gebruiker op de Azure-Resources op datum en de recente rolactiveringen diezelfde periode van tijd weergegeven.

![](media/azure-pim-resource-rbac/user-details.png)

Selecteren van een specifieke rol moet worden geactiveerd, ziet de details van rolactivering en de bijbehorende Azure-Resource-activiteit die is opgetreden bij die gebruiker actief is.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="modify-existing-assignments"></a>Bestaande toewijzingen wijzigen

Selecteer instellingen wijzigen in de actiebalk aan de bovenkant van de pagina voor het wijzigen van bestaande toewijzingen uit de detailweergave van de gebruiker of groep. Het toewijzingstype alleen In tijdtoewijzing of rechtstreekse toewijzing toe.

![](media/azure-pim-resource-rbac/change-settings.png)

## <a name="review-who-has-access-in-a-subscription"></a>Bekijk wie toegang in een abonnement heeft

Als u wilt controleren roltoewijzingen in uw abonnement, selecteer het tabblad leden in de linkernavigatiebalk of rollen selecteren en kiest u een specifieke rol om te controleren van leden. 

Controleer in de actiebalk bestaande toegangsbeoordelingen weergeven en selecteert u toevoegen aan het maken van een nieuwe beoordeling selecteren.

![](media/azure-pim-resource-rbac/owner.png)

[Meer informatie over toegangsbeoordelingen](pim-how-to-perform-security-review.md)

>[!NOTE]
Beoordelingen worden alleen ondersteund voor resourcetypen abonnement op dit moment.

## <a name="configure-role-settings"></a>Rolinstellingen configureren

Configuratie van de rolinstellingen definiëren de standaardinstellingen die zijn toegepast op de toewijzingen in de PIM-omgeving. Voor het definiëren van deze voor uw resource, selecteer u het tabblad serverfunctie-instellingen van de navigatiebalk aan de linkerkant of de knop rol instellingen in de actiebalk in een rol om de huidige opties weer te geven.

Klik op bewerken in de actiebalk aan de bovenkant van de pagina kunt u om elke instelling te wijzigen.

![](media/azure-pim-resource-rbac/owner.png)

![](media/azure-pim-resource-rbac/owner02.png)

Wijzigingen in de instellingen worden geregistreerd op de instellingenpagina van rol met inbegrip van het laatst bijgewerkt op, en de beheerder die de instellingen gewijzigd.

![](media/azure-pim-resource-rbac/role-settings-02.png)

## <a name="resource-audit"></a>Resourcecontrole

Resourcecontrole geeft u een overzicht van alle activiteiten van de rol voor de resource. U kunt de informatie die met behulp van een vooraf gedefinieerde datum of aangepast datumbereik filteren.
![](media/azure-pim-resource-rbac/last-day.png) Resourcecontrole biedt ook snel toegang om gegevens van de activiteit van een gebruiker weer te geven. In de weergave worden alle acties voor 'Rol activeren' koppelingen naar de activiteit van de resources van de specifieke aanvrager.
![](media/azure-pim-resource-rbac/resource-audit.png)

## <a name="just-enough-administration"></a>Net voldoende beheer

Met behulp van aanbevolen procedures voor net voldoende administration (JEA) met de roltoewijzingen van uw resource is heel eenvoudig met PIM voor Azure-Resources. Gebruikers en leden van de groep met toewijzingen in de Azure-abonnementen of resourcegroepen kunnen activeren van hun bestaande roltoewijzing met een lagere bereik. 

Op de zoekpagina vinden de onderliggende resource die u wilt beheren.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

Mijn rollen selecteren in het navigatiemenu links en kies de juiste rol te activeren. U ziet dat het toewijzingstype wordt overgenomen, omdat de rol is toegewezen aan het abonnement, in plaats van de resourcegroep, zoals hieronder wordt weergegeven.

![](media/azure-pim-resource-rbac/my-roles-02.png)

## <a name="next-steps"></a>Volgende stappen

- [Ingebouwde rollen voor Azure-resources](../../role-based-access-control/built-in-roles.md)
- Meer informatie over [hier Azure AD-rollen activeren](pim-how-to-activate-role.md)
- [Goedkeuringswerkstromen voor PIM](azure-ad-pim-approval-workflow.md)
