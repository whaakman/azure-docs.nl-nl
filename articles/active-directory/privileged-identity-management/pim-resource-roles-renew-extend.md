---
title: Uitbreiden en rollen in Azure-resources bekijken met behulp van Privileged Identity Management | Microsoft Docs
description: Dit document wordt beschreven hoe Azure-resource-functies voor PIM bronnen vernieuwen om uit te breiden.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 44ce8c77a37d3a704c4f9516d942085c878307af
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234162"
---
# <a name="extend-and-review-roles-in-azure-resources-by-using-privileged-identity-management"></a>Uitbreiden en rollen in Azure-resources bekijken met behulp van Privileged Identity Management

Privileged Identity Management (PIM) voor Azure-resources introduceert nieuwe besturingselementen voor het beheren van de levenscyclus van de toegang en de toewijzing voor Azure-resources. Beheerders kunnen met behulp van begin- en datum / tijd-eigenschappen lidmaatschap toewijzen. Wanneer het einde van de toewijzing nadert, verzendt PIM e-mailmeldingen naar de betrokken gebruikers of groepen. Het verzendt ook e-mailmeldingen voor beheerders van de resource om ervoor te zorgen dat de juiste toegang wordt gehandhaafd. Toewijzingen kunnen worden vernieuwd en in een verlopen staat maximaal 30 dagen zichtbaar blijft, zelfs als toegang niet is uitgebreid.

## <a name="who-can-extend-and-renew"></a>Wie kan uitbreiden en vernieuwen?

Alleen beheerders van de bron kunnen uitbreiden of vernieuwen van roltoewijzingen. De betrokken lid kunt aanvragen uit te breiden functies die zijn verlopen en de aanvraag voor het vernieuwen van de rollen die al zijn verlopen.

## <a name="when-are-notifications-sent"></a>Wanneer worden meldingen verzonden?

PIM verzendt e-mailmeldingen voor beheerders en de betrokken leden van de rollen die binnen 14 dagen en één dag vóór de vervaldatum verlopen. Een extra e-mailbericht wordt verzonden wanneer een toewijzing officieel is verlopen. 

Beheerders ontvangen meldingen wanneer een lid van een rol verlopende of verlopen aanvragen uit te breiden of te vernieuwen. Als de aanvraag is opgelost door een specifieke beheerder, worden alle andere beheerders melding van de beslissing resolutie (goedgekeurd of geweigerd). Een waarschuwing vervolgens de aanvragende lid is van de beslissing. 

## <a name="extend-role-assignments"></a>Roltoewijzingen uitbreiden

De volgende stappen wordt beschreven hoe aanvragen, het omzetten van of een extensie of verlenging van een roltoewijzing beheren. 

### <a name="member-extend"></a>Lid uitbreiden

Leden van een roltoewijzing kunnen uitbreiden verlopende roltoewijzingen rechtstreeks vanuit de **in aanmerking komende** of **Active** tabblad op de **mijn rollen** pagina van een resource en van het hoogste niveau **Mijn rollen** pagina van de PIM-portal. Leden kunnen aanvragen uit te breiden in aanmerking komende en actieve (toegewezen) rollen die in de volgende 14 dagen verloopt.

![Rollen uitbreiden](media/azure-pim-resource-rbac/aadpim_rbac_extend_ui.png)

Wanneer de toewijzing einddatum en-tijd is binnen 14 dagen, op de knop **uitbreiden** verandert in een actieve koppeling in de gebruikersinterface. In het volgende voorbeeld wordt ervan uitgegaan dat de huidige datum 27 maart valt.

![De knop 'Uitbreiden'](media/azure-pim-resource-rbac/aadpim_rbac_extend_within_14.png)

Als u een uitbreiding van deze roltoewijzing, schakelt **uitbreiden** het aanvraagformulier te openen.

![Open het aanvraagformulier](media/azure-pim-resource-rbac/aadpim_rbac_extend_role_assignment_request.png)

U kunt informatie over de oorspronkelijke toewijzing, vouw **toewijzingsgegevens**. Voer een reden voor de extensie-aanvraag en selecteer vervolgens **uitbreiden**.

>[!Note]
>We raden aan om de details van waarom de extensie is nodig, en hoe lang de extensie moet worden verleend (als u deze informatie hebt).

![Roltoewijzing uitbreiden](media/azure-pim-resource-rbac/aadpim_rbac_extend_form_complete.png)

Binnen een paar minuten ontvangen resource beheerders een e-mailbericht melding aanvragen dat ze de extensie-aanvraag controleren. Als een aanvraag uit te breiden al is verzonden, verschijnt een pop-upmelding aan de bovenkant van de Azure-portal uitleg over de fout.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_failed_existing_request.png)

Ga naar de **aanvragen in behandeling** tabblad in het linkerdeelvenster om de status van uw aanvraag weer te geven of om deze te annuleren.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_cancel_request.png)

### <a name="admin-approve"></a>Beheerder goedkeuren

Wanneer een lid een aanvraag verzendt voor het uitbreiden van een roltoewijzing, ontvangen resource beheerders een e-mailbericht met de details van de oorspronkelijke toewijzing en de reden voor de aanvraag. De melding omvat een directe koppeling naar de aanvraag voor de beheerder goedkeuren of weigeren. 

Naast het gebruik van de koppeling volgen via e-mailbericht, beheerders kunnen goedkeuren of weigeren van aanvragen door te gaan naar de PIM-beheer portal en selecteren **aanvragen goedkeuren** in het linkerdeelvenster.

![Schermopname van foutbericht](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Wanneer een beheerder selecteert **goedkeuren** of **weigeren**, de details van de aanvraag worden weergegeven, samen met een veld reden voor de controlelogboeken op te geven.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Bij het goedkeuren van een aanvraag uit te breiden roltoewijzing kunnen resource beheerders een nieuwe begindatum, einddatum en toewijzingstype. Toewijzingstype wijzigen zijn vereist als de beheerder wil de toegang beperkt voor het voltooien van een specifieke taak (bijvoorbeeld één dag). In dit voorbeeld kan de beheerder de toewijzing van wijzigen **in aanmerking komende** naar **Active**. Dit betekent dat ze kunnen toegang verlenen tot de aanvrager zonder deze te activeren.

### <a name="admin-extend"></a>Beheerdersuitbreiding

Als een lid vergeet of kan geen aanvragen van een functie-extensie voor lidmaatschap, kan een beheerder een toewijzing namens het lid kunt uitbreiden. Administratieve extensies van het lidmaatschap van een rol geen goedkeuring vereisen, maar de meldingen worden verzonden naar alle andere beheerders nadat de rol is uitgebreid.

Als u wilt uitbreiden een lidmaatschap van de rol, blader naar de weergave van resources functie of een lid in PIM. Het lid dat een uitbreiding vereist vinden. Selecteer vervolgens **uitbreiden** in de actiekolom.

![Lidmaatschap van de rol uitbreiden](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_extend.png)

## <a name="renew-role-assignments"></a>Roltoewijzingen vernieuwen

Terwijl conceptueel overeen met het proces voor het aanvragen van een extensie, wordt het proces voor het vernieuwen van een verlopen roltoewijzing verschilt. Met de volgende stappen, kunnen leden en beheerders vernieuwen toegang tot verlopen rollen indien nodig.

### <a name="member-renew"></a>Lid vernieuwen

Leden die niet langer toegang bronnen tot hebben toegang tot maximaal 30 dagen van de geschiedenis van werkitemtoewijzingen verlopen. U doet dit door ze naar Bladeren **mijn rollen** in het linkerdeelvenster en selecteer vervolgens de **verlopen rollen** tabblad in de sectie van de rollen Azure-resource.

![Het tabblad 'Rollen verlopen'](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_myroles.png)

De lijst met rollen weergegeven van de standaardwaarde is **in aanmerking komende rollen**. Gebruik de vervolgkeuzelijst schakelen tussen van in aanmerking komende en actieve rollen toegewezen.

Als u wilt vernieuwen voor een van de roltoewijzingen in de lijst, schakelt de **vernieuwen** in te grijpen. Geef een reden voor de aanvraag. Het is nuttig om een duur naast elke aanvullende context waarmee de resourcebeheerder wilt goedkeuren of weigeren.

![Roltoewijzing vernieuwen](media/azure-pim-resource-rbac/aadpim_rbac_renew_request_form.png)

Nadat de aanvraag is ingediend, worden de resource-beheerders gewaarschuwd voor een aanvraag in behandeling voor het vernieuwen van een roltoewijzing.

### <a name="admin-approves"></a>Beheerder goedkeurt

Resource-beheerders hebben toegang tot de vernieuwingsaanvraag via de koppeling in de e-mailmelding of door PIM vanuit de Azure-portal te openen en te selecteren **aanvragen goedkeuren** in het linkerdeelvenster.

![Aanvragen goedkeuren](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Wanneer een beheerder selecteert **goedkeuren** of **weigeren**, de details van de aanvraag worden weergegeven samen met een veld reden voor de controlelogboeken op te geven.

![Roltoewijzing goedkeuren](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Bij het goedkeuren van een aanvraag voor het vernieuwen van de roltoewijzing moeten resource beheerders een nieuwe begindatum, einddatum en toewijzingstype invoeren. 

### <a name="admin-renew"></a>Beheerdersverlenging

Resource-beheerders kunnen vernieuwen verlopen roltoewijzingen van de **leden** tabblad in het menu van de navigatiebalk aan de linkerkant van een resource. Ze kunnen ook vernieuwen verlopen roltoewijzingen vanuit de **verlopen** tabblad rollen van een resourcerol.

Een lijst weergeven van alle roltoewijzingen, is verlopen op de **leden** Schakel in het scherm **verlopen rollen**.

![Verlopen rollen](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_member_blade.png)

## <a name="next-steps"></a>Volgende stappen

[Goedkeuring vereist voor het activeren](pim-resource-roles-approval-workflow.md)

[Een rol activeren](pim-resource-roles-use-the-audit-log.md)


