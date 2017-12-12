---
title: Overzicht van Azure PIM Resource RBAC | Microsoft Docs
description: Krijgt u een overzicht van de functie RBAC in PIM inclusief terminologie en meldingen
services: active-directory
documentationcenter: 
author: barclayn
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: barclayn
ms.openlocfilehash: 19715f800e7d8d40336d8e9fa3bf8073795dce5b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="pim-for-azure-resources-preview"></a>PIM voor Azure-resources (Preview)

Met Azure Active Directory Privileged Identity Management (PIM), kunt u nu beheren, beheren en te controleren, toegang tot Azure-Resources (Preview) binnen uw organisatie. Dit omvat abonnementen en resourcegroepen zelfs virtuele Machines. Een bron binnen de Azure-portal die gebruikmaakt van de functionaliteit van Azure rollen gebaseerd toegangsbeheer (RBAC) kan profiteren van alle betrouwbaarheid en beheermogelijkheden van de levenscyclus van die Azure AD PIM te bieden heeft en een aantal geweldige nieuwe functies die we willen meenemen naar Azure AD-rollen snel. 

## <a name="pim-for-azure-resources-preview-helps-resource-administrators"></a>PIM voor Azure-Resources (Preview) kan beheerders een resource

- Zien welke gebruikers en groepen zijn toegewezen rollen voor de Azure-resources beheren
- Inschakelen van on-demand 'just in time' toegang tot resources zoals abonnementen en resourcegroepen beheren
- Toegang tot bedrijfsbronnen toegewezen gebruikers/groepen automatisch met nieuwe tijdsgebonden toewijzingsinstellingen verlopen
- Tijdelijke brontoegang voor snelle taken of op oproep schema's toewijzen
- Afdwingen van multi-factor Authentication voor toegang tot bronnen op eventuele ingebouwde of aangepaste rol 
- Rapporten over resourceactiviteit voor resource gecorreleerde toegang tijdens een actieve gebruikerssessie ophalen
- Ontvang waarschuwingen wanneer nieuwe gebruikers of groepen worden toegewezen toegang tot bedrijfsbronnen, en wanneer ze in aanmerking komende toewijzingen activeren

Azure AD PIM kunt beheren de ingebouwde Azure Resource rollen, evenals aangepaste rollen (RBAC), inclusief (maar niet beperkt tot):

- Eigenaar
- Beheerder van gebruikerstoegang
- Inzender
- De beheerder beveiliging
- Beveiligingsbeheer en meer

>[!NOTE]
Gebruikers of leden van een groep die is toegewezen aan de eigenaar of beheerder voor gebruikerstoegang rollen en globale beheerders die beheer van abonnementen inschakelen in Azure AD zijn Resource-beheerders. Deze beheerders kunnen rollen toewijzen, serverfunctie-instellingen configureren en controleren via toegang tot PIM voor Azure-Resources. De lijst weergeven met [ingebouwde functies voor Azure-resources](../role-based-access-built-in-roles.md).

## <a name="tasks"></a>Taken

PIM biedt snel toegang tot rollen activeren, bekijken in behandeling zijnde activeringen/aanvragen in behandeling genomen goedkeuringen (voor [functies van Azure AD-directory](azure-ad-pim-approval-workflow.md)), en in afwachting van uw antwoord in het gedeelte van de taken van het navigatiemenu links beoordeelt.

Bij het openen van een van de taken menu-items van het toegangspunt overzicht, bevat het resulterende weergave resultaten voor Azure AD-directory-functies en functies van Azure Resource (Preview). 

![](media/azure-pim-resource-rbac/role-settings-details.png)

Mijn rollen bevatten een lijst met uw actieve en in aanmerking komende functietoewijzingen voor Azure AD-directory-functies en Azure Resource-rollen (Preview).

## <a name="activate-roles"></a>Rollen activeren

Rollen activeren voor Azure-Resources (Preview) introduceert een nieuwe ervaring waarmee de leden van een in aanmerking komende rol activeren voor een toekomstige datum/tijd plannen en selecteert u een specifieke activeringsduur binnen de maximale (geconfigureerd door beheerders). Meer informatie over [hier Azure AD-rollen activeren](../active-directory-privileged-identity-management-how-to-activate-role.md).

![](media/azure-pim-resource-rbac/contributor.png)

Voer in het menu activeringen, de gewenste begindatum en de tijd om de rol te activeren. Eventueel de activeringsduur van de te verlagen (hoe lang de functie actief is) en voer een reden indien nodig; klikt u op activeren.

Als de begindatum en -tijd niet worden gewijzigd, wordt de rol binnen enkele seconden worden geactiveerd. U ziet dat een rol voor activering bannerbericht aangegeven op de pagina Mijn rollen in de wachtrij. Klik op de knop Vernieuwen om te wissen van dit bericht.

![](media/azure-pim-resource-rbac/my-roles.png)

Als de activering is gepland voor een tijdstip in de toekomst, wordt de aanvraag in behandeling op het tabblad aanvragen in behandeling van het navigatiemenu links weergegeven. In het geval van de rolactivering van de is niet langer vereist, kan de gebruiker de aanvraag annuleren door te klikken op de knop Annuleren aan de rechterkant van de pagina.

![](media/azure-pim-resource-rbac/pending-requests.png)

## <a name="discover-and-manage-azure-resources"></a>Detecteren en beheren van Azure-resources

Selecteer de Azure-Resources (Preview) onder het tabblad beheren in het navigatiemenu links om te zoeken en beheren van rollen voor een Azure-Resource. Met de filters of zoekbalk boven aan de pagina zoeken naar een resource.

![](media/azure-pim-resource-rbac/azure-resources.png)

## <a name="resource-dashboards"></a>Resource-dashboards

Het dashboard Admin weergave bestaat uit vier primaire onderdelen. Een grafische weergave van de resource rol activeringen gedurende de afgelopen zeven dagen. Deze gegevens is afgestemd op de geselecteerde resource en geeft weer activeringen voor de meest voorkomende rollen (eigenaar, bijdrager, beheerder voor gebruikerstoegang) en alle rollen die worden gecombineerd.

Aan de rechterkant van de grafiek activeringen zijn twee grafieken die de distributie van roltoewijzingen door toewijzingstype voor gebruikers en groepen weergeven. Selecteren van een segment van de grafiek wordt de waarde in een percentage (of andersom).

![](media/azure-pim-resource-rbac/admin-view.png)

U ziet het aantal gebruikers en groepen met nieuwe roltoewijzingen gedurende de afgelopen 30 dagen (links) en een lijst met rollen gesorteerd op basis van totale toewijzingen (aflopend) onder de grafieken.

![](media/azure-pim-resource-rbac/role-settings.png)

## <a name="manage-role-assignments"></a>Roltoewijzingen beheren

Beheerders kunnen roltoewijzingen beheren door functies of leden selecteren in de linkernavigatiebalk. Functies selecteren, kunnen beheerders hun beheertaken voor een specifieke rol bereik terwijl de leden wordt weergegeven voor alle gebruikers en groepen roltoewijzingen voor de resource.

![](media/azure-pim-resource-rbac/roles.png)

![](media/azure-pim-resource-rbac/members.png)

>[!NOTE]
Als u een rol in behandeling zijnde activering hebt, wordt een meldingsbanner wordt weergegeven boven aan de pagina wanneer lidmaatschap weer te geven.

## <a name="assign-roles"></a>Rollen toewijzen

Als u wilt een gebruiker of groep toewijzen aan een rol, selecteer de rol (als bekijkt rollen) of klikt u op toevoegen in de actiebalk (als op de weergave leden).

![](media/azure-pim-resource-rbac/members2.png)

>[!NOTE]
Als een gebruiker of groep op het tabblad leden wilt toevoegen, moet u een rol selecteren in het menu toevoegen voordat u kunt een gebruiker of groep selecteren.

![](media/azure-pim-resource-rbac/select-role.png)

Kies een gebruiker of groep in de map.

![](media/azure-pim-resource-rbac/choose.png)

Kies het toewijzingstype van de juiste in de vervolgkeuzelijst. 

**Just In Time toewijzing:** u biedt de leden van de gebruiker of groep in aanmerking komende maar geen permanente toegang tot de functie voor een opgegeven periode of voor onbepaalde tijd (indien geconfigureerd in de functie-instellingen). 

**Rechtstreekse toewijzing:** hoeft niet de leden van de gebruiker of groep voor het activeren van de roltoewijzing (bekend als permanente toegang). Microsoft raadt het gebruik van rechtstreekse toewijzing op korte termijn gebruikt zoals ploegendiensten op oproep of tijd gevoelige activiteiten, waarbij toegang niet vereist wanneer de taak voltooid is.

![](media/azure-pim-resource-rbac/membership-settings.png)

Een selectievakje onder de vervolgkeuzelijst toewijzing kunt u opgeven als de toewijzing permanente moet (permanent in aanmerking voor het activeren van Just in Time toewijzing/permanent active voor rechtstreekse toewijzing). Geef de duur van een specifieke toewijzing, schakel het selectievakje in en start wijzigen en/of datum- en tijdvelden eindigen.

>[!NOTE]
Het is mogelijk dat het selectievakje unmodifiable als een andere beheerder de toewijzing van de maximale duur voor elk toewijzingstype is opgegeven in de functie-instellingen.

![](media/azure-pim-resource-rbac/calendar.png)

## <a name="view-activation-and-azure-resource-activity"></a>Activering en Azure Resource activiteit weergeven

In het geval u zien welke acties die een specifieke gebruiker heeft uitgevoerd op verschillende bronnen wilt, kunt u de Azure Resource-activiteit die is gekoppeld aan een bepaalde activeringsperiode (voor gebruikers in aanmerking komende) kunt bekijken. Begin met het selecteren van een gebruiker vanuit de weergave van de leden of in de lijst met leden in een specifieke functie. Het resultaat wordt een grafische weergave van de gebruiker acties op Azure-Resources op datum en de recente functie-activeringen via dezelfde periode.

![](media/azure-pim-resource-rbac/user-details.png)

Selecteren van een specifieke rol moet worden geactiveerd, ziet de rolgegevens voor activering en de bijbehorende Azure Resource-activiteit die is opgetreden tijdens het die gebruiker actief was.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="modify-existing-assignments"></a>Bestaande toewijzingen wijzigen

Selecteer de instellingen wijzigen van de actiebalk boven aan de pagina voor het wijzigen van bestaande toewijzingen van de gebruiker of groep detailweergave. Wijzig het toewijzingstype in NET In tijdtoewijzing of rechtstreekse toewijzing toe.

![](media/azure-pim-resource-rbac/change-settings.png)

## <a name="review-who-has-access-in-a-subscription"></a>Bekijk wie toegang in een abonnement heeft

Als u wilt controleren roltoewijzingen in uw abonnement, selecteer het tabblad leden in de linkernavigatiebalk of Selecteer rollen en kiest u een specifieke rol leden bekijken. 

Selecteer controleren in de actiebalk bestaande toegang beoordelingen weergeven en selecteren van toevoegen aan een nieuwe revisie te maken.

![](media/azure-pim-resource-rbac/owner.png)

[Meer informatie over toegang beoordelingen](../active-directory-privileged-identity-management-how-to-perform-security-review.md)

>[!NOTE]
Beoordelingen worden alleen ondersteund voor resourcetypen abonnement op dit moment.

## <a name="configure-role-settings"></a>Functie-instellingen configureren

Configuratie van de functie instellingen definiëren de standaardwaarden die wordt toegepast op toewijzingen in de PIM-omgeving. Deze voor uw resource definiëren het tabblad instellingen van de rol van de navigatiebalk aan de linkerkant of de knop rol instellingen van de actiebalk in een rol van de huidige opties weergeven te selecteren.

Klik op bewerken vanuit de actiebalk boven aan de pagina kunt u om elke instelling te wijzigen.

![](media/azure-pim-resource-rbac/owner.png)

![](media/azure-pim-resource-rbac/owner02.png)

Wijzigingen in de instellingen worden geregistreerd op de instellingenpagina van rol met inbegrip van de laatste keer dat bijgewerkte datum en de beheerder die de instellingen gewijzigd.

![](media/azure-pim-resource-rbac/role-settings-02.png)

## <a name="resource-audit"></a>Resource-audit

Controle van de resource hebt u een overzicht van de rol van activiteiten voor de resource. U kunt de gegevens door middel van een vooraf gedefinieerde datum of aangepaste datumbereik filteren.
![](media/azure-pim-resource-rbac/last-day.png)Controle van de resource biedt ook snel toegang om gegevens van de activiteit van een gebruiker weer te geven. In de weergave worden alle acties voor 'Rol activeren' koppelingen naar de specifieke aanvrager resourceactiviteit.
![](media/azure-pim-resource-rbac/resource-audit.png)

## <a name="just-enough-administration"></a>Net voldoende beheer

Met behulp van de aanbevolen procedures van net voldoende administration (JEA) met uw roltoewijzingen resource is heel eenvoudig met PIM voor Azure-Resources. Gebruikers en leden van de beveiligingsgroep met toewijzingen in de Azure-abonnementen of resourcegroepen kunnen hun bestaande roltoewijzing op een verminderde scope activeren. 

Zoeken naar de onderliggende resource die u wilt beheren door de zoekpagina.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

Mijn rollen selecteren in het menu linkernavigatiegedeelte en kies de juiste rol te activeren. U ziet het toewijzingstype wordt overgenomen, omdat de rol is toegewezen aan het abonnement, in plaats van de resourcegroep, zoals hieronder wordt weergegeven.

![](media/azure-pim-resource-rbac/my-roles-02.png)

## <a name="next-steps"></a>Volgende stappen

- [Ingebouwde functies voor Azure-resources](../role-based-access-built-in-roles.md)
- Meer informatie over [hier Azure AD-rollen activeren](../active-directory-privileged-identity-management-how-to-activate-role.md)
- [Werkstromen voor PIM goedkeuring](azure-ad-pim-approval-workflow.md)
