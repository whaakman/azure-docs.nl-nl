---
title: Privileged Identity Management voor Azure-Resources - uitbreiden en rollen vernieuwen | Microsoft Docs
description: Dit document wordt beschreven hoe Azure-resource-functies voor PIM bronnen vernieuwen om uit te breiden.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 2547b3793688eb51a4114f30bfcf61a9402f2cd2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---extend-or-renew"></a>Privileged Identity Management - resourcerollen - verlengen of vernieuwen

PIM voor Azure-resources introduceert nieuwe besturingselementen voor het beheren van de levenscyclus van de toegang en de toewijzing voor Azure-resources. Beheerders kunnen met behulp van begin- en datum / tijd-eigenschappen lidmaatschap toewijzen. Wanneer het einde van de toewijzing nadert, PIM e-mailmeldingen verzendt naar de betrokken leden (mogelijk een gebruiker of groep), en voor beheerders van de bron om te controleren of de juiste toegang wordt beheerd. In het geval van toegang niet is uitgebreid vanwege niet-handelen, worden de toewijzingen kunnen worden vernieuwd en zichtbaar blijft in een verlopen staat voor 30 dagen.

## <a name="who-can-extend-and-renew"></a>Wie kan uitbreiden en vernieuwen?

Alleen beheerders van de bron kunnen uitbreiden of vernieuwen van roltoewijzingen. De betrokken lid kan aanvragen uit te breiden rollen verloopt en verlenging van de aanvraag van functies is al verlopen.

## <a name="when-are-notifications-sent"></a>Wanneer worden meldingen verzonden?

PIM verzendt e-mailmeldingen voor beheerders en de betrokken leden van de rollen verlopen binnen 14 dagen en één dag vóór de vervaldatum. Een extra e-mailbericht wordt verzonden wanneer een toewijzing officieel is verlopen. 

Beheerders ontvangen meldingen wanneer een lid van een rol verlopende of verlopen aanvragen uit te breiden of te vernieuwen. Wanneer de beheerder van een specifieke oplossing voor de aanvraag alle andere beheerders worden gewaarschuwd als de beslissing resolutie (goedgekeurd of geweigerd) en de aanvragende lid van de beslissing wordt geïnformeerd. 

## <a name="extend-role-assignments"></a>Roltoewijzingen uitbreiden

De onderstaande stappen worden de stappen en de gebruikersinterface voor het aanvragen, het omzetten van of een extensie of verlenging van een roltoewijzing beheren. 

### <a name="member-extend"></a>Lid uitbreiden

Leden van een roltoewijzing kunnen aanvragen uit te breiden verlopende roltoewijzingen rechtstreeks vanuit de 'in aanmerking komende' of 'Active' tabblad op de 'Mijn rollen' pagina van een resource en van het hoogste niveau mijn rollen van de PIM-portal. Leden kunnen aanvragen uit te breiden in aanmerking komende en actieve (toegewezen) rollen die in de volgende 14 dagen verloopt.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_ui.png)

Wanneer de toewijzing einddatum en-tijd binnen 14 dagen, worden de knop 'Uitbreiden' een actieve koppeling in de gebruikersinterface. In het volgende voorbeeld wordt ervan uitgegaan dat de huidige datum 27 maart valt.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_within_14.png)

Om aan te vragen van een uitbreiding van deze rol toewijzing klikt u op 'Uitbreiden' het verzoek formulier te openen.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_role_assignment_request.png)

Vouw 'details van de toewijzing' informatie bekijken over de oorspronkelijke toewijzing. Voer een reden voor de aanvraag van de extensie en klikt u op 'Uitbreiden'.

>[!Note]
>We raden aan om de details van waarom de extensie is nodig, en hoe lang de extensie moet zijn (indien bekend).

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_form_complete.png)

Binnen een paar minuten ontvangen resource-beheerders een e-mailbericht dat ze de extensie-aanvraag controleren aanvragen. Als een aanvraag uit te breiden al is verzonden, verschijnt een pop-upmelding aan de bovenkant van de Azure-portal uitleg over de fout.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_failed_existing_request.png)

Ga naar het tabblad 'aanvragen in behandeling' in het linkernavigatievenster menu weergeven van de status van of uw aanvraag te annuleren.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_cancel_request.png)

### <a name="admin-approve"></a>Beheerder goedkeuren

Wanneer een lid een aanvraag verzendt voor het uitbreiden van een roltoewijzing, ontvangen resource beheerders een e-mailbericht met de details van de oorspronkelijke toewijzing en de reden dat is opgegeven door de aanvrager. De melding omvat een directe koppeling naar de aanvraag voor de beheerder goedkeuren of weigeren. 

Naast het volgen van de koppeling van e-mailbericht, kunnen beheerders goedkeuren of weigeren van aanvragen door te navigeren naar de PIM-beheerportal en 'Aanvragen goedkeuren' selecteren in het navigatiemenu links.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Als een beheerder goedkeuren of weigeren selecteert worden de details van de aanvraag weergegeven samen met een veld reden voor de controlelogboeken op te geven.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Bij het goedkeuren van een aanvraag uit te breiden roltoewijzing kunnen resource beheerders een nieuwe begin- en datum / tijd- en -toewijzing type. Wijzigen van het toewijzingstype kan nodig zijn als de beheerder wil de toegang beperkt voor het voltooien van een specifieke taak (bijvoorbeeld van een dag). In dit voorbeeld kan de beheerder de toewijzing van in aanmerking komende wijzigen in actief de aanvrager toegang bieden zonder deze te activeren.

### <a name="admin-extend"></a>Beheerder uitbreiden

In het geval van een lid is vergeten of kan niet worden aanvragen van functie lidmaatschap uitbreiding kan een beheerder een toewijzing namens het lid verlengen. Administratieve extensies van het lidmaatschap van een rol geen goedkeuring vereisen, maar meldingen worden verzonden naar alle andere beheerders na voltooiing van het uitbreiden van de rol.

Als u wilt uitbreiden een rol lidmaatschap gaat u naar de weergave van resources functie of een lid in PIM. Zoek het vereisen van een uitbreiding lid en klikt u op 'Uitbreiden' in de actiekolom.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_extend.png)

## <a name="renew-role-assignments"></a>Roltoewijzingen vernieuwen

Terwijl conceptueel gezien is het proces voor het vernieuwen van een verlopen roltoewijzing anders dan een extensie aanvraagt voor zowel beheerders als leden. Met behulp van de stappen hieronder leden en beheerders kan toegang tot verlopen rollen indien nodig vernieuwen.

### <a name="member-renew"></a>Lid vernieuwen

Leden die niet langer toegang bronnen tot hebben toegang tot maximaal 30 dagen van de geschiedenis van werkitemtoewijzingen verlopen door te navigeren naar Mijn rollen in het linkernavigatievenster van PIM en het tabblad 'Verlopen rollen' te selecteren in de sectie van de rollen Azure-resource.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_myroles.png)

De lijst met functies die in aanmerking komende toewijzingen de standaardwaarde is weergegeven. Gebruik de vervolgkeuzelijst schakelen tussen van in aanmerking komende en actieve rollen toegewezen.

Toewijzingen in de lijst selecteert u de actie 'Vernieuwen' en geef een reden voor de aanvraag om aan te vragen van vernieuwing voor een van de rol. Het is nuttig om een duur naast elke aanvullende context waarmee de resourcebeheerder wilt goedkeuren of weigeren.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_request_form.png)

Tijdens de verzending van de aanvraag worden resource beheerders melding van een aanvraag in behandeling voor het vernieuwen van een roltoewijzing.

### <a name="admin-approves"></a>Beheerder goedkeurt

Resource-beheerders hebben toegang tot de vernieuwingsaanvraag via de koppeling in de e-mailmelding of PIM vanuit de portal Auzre te openen en selecteert u 'Aanvragen goedkeuren' in het linkernavigatievenster-menu.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_grid.png)

Als een beheerder goedkeuren of weigeren selecteert worden de details van de aanvraag weergegeven samen met een veld reden voor de controlelogboeken op te geven.

![](media/azure-pim-resource-rbac/aadpim_rbac_extend_admin_approve_blade.png)

Bij het goedkeuren van een aanvraag voor het vernieuwen van de roltoewijzing moeten resource beheerders een nieuwe begin- en datum / tijd- en -toewijzing type opgeven. 

### <a name="admin-renew"></a>Beheerder vernieuwen

Resource-beheerders kunnen vernieuwen verlopen roltoewijzingen van het tabblad leden in het menu van de navigatiebalk aan de linkerkant van een resource of vanuit het tabblad verlopen rollen van een resourcerol.

Selecteer verlopen rollen om weer te geven een lijst met alle verlopen roltoewijzingen van het scherm leden.

![](media/azure-pim-resource-rbac/aadpim_rbac_renew_from_member_blade.png)

## <a name="next-steps"></a>Volgende stappen

[Goedkeuring vereist voor het activeren](pim-resource-roles-approval-workflow.md)

[Een rol activeren](pim-resource-roles-use-the-audit-log.md)


