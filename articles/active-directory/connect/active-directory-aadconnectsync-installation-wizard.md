---
title: Wizard opnieuw uitvoeren van de Azure AD Connect installeren | Microsoft Docs
description: Wordt uitgelegd hoe de installatiewizard werkt de tweede keer dat u deze uitvoert.
keywords: De Azure AD Connect-installatiewizard kunt u de tweede keer dat u het uitvoeren van onderhoudsinstellingen configureren
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: a5a55b15bbffbcfa18e07e251180820e3d9cb6d8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect-synchronisatie: uitvoeren van de installatiewizard van een tweede keer
De eerste keer dat u de wizard Azure AD Connect-installatie uitvoert begeleidt het u bij het configureren van uw installatie. Als u de installatiewizard opnieuw uitvoert, biedt opties voor onderhoud.

U kunt de installatiewizard vinden in het startmenu met de naam **Azure AD Connect**.

![Het menu Start](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Wanneer u de installatiewizard te starten, ziet u een pagina met deze opties:

![Pagina met een lijst van extra taken](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Als u AD FS hebt geïnstalleerd met Azure AD Connect, hebt u nog meer opties. De aanvullende opties die u hebt voor AD FS zijn gedocumenteerd in [ADFS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).

Selecteer een van de taken en klik op **volgende** om door te gaan.

> [!IMPORTANT]
> Terwijl u de installatiewizard open zijn, worden alle bewerkingen in de synchronisatie-engine onderbroken. Zorg ervoor dat u de installatiewizard sluiten als u uw wijzigingen in de configuratie hebt voltooid.
>
>

## <a name="view-current-configuration"></a>De huidige configuratie weergeven
Deze optie biedt een overzicht van de momenteel geconfigureerde opties.

![Pagina met een lijst met alle opties en hun status](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Klik op **vorige** om terug te gaan. Als u selecteert **afsluiten**, u de installatiewizard sluit.

## <a name="customize-synchronization-options"></a>Synchronisatieopties aanpassen
Deze optie wordt gebruikt om de configuratie van de synchronisatie te wijzigen. Er is een subset van de opties in het installatiepad van aangepaste configuratie. U ziet deze optie ook als u snelle installatie in eerste instantie worden gebruikt.

* [Toevoegen van meer mappen](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Zie voor het verwijderen van een map [verwijderen van een Connector](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
* [Wijzigen van domein en OE filteren](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
* Filteren van de groep verwijderen.
* [Optionele functies wijzigen](active-directory-aadconnect-get-started-custom.md#optional-features).

De overige opties uit de eerste installatie kunnen niet worden gewijzigd en zijn niet beschikbaar. Deze opties zijn:

* Het kenmerk moet worden gebruikt voor userPrincipalName en sourceAnchor wijzigen.
* Wijzig de gekoppelde methode voor objecten van ander forest.
* Filteren op basis van een groep inschakelen.

## <a name="refresh-directory-schema"></a>Directory-schema vernieuwen
Deze optie wordt gebruikt als u het schema hebt gewijzigd in een van uw on-premises AD DS-forests. Bijvoorbeeld, u mogelijk Exchange geïnstalleerd of bijgewerkt naar een Windows Server 2012-schema met apparaatobjecten. In dit geval moet u instrueren Azure AD Connect opnieuw lezen van het schema van AD DS en bijwerken van de cache. Deze actie wordt ook de synchronisatie-regels opnieuw gegenereerd. Als u de Exchange-schema als voorbeeld toevoegt, wordt de synchronisatie-regels voor Exchange worden toegevoegd aan de configuratie.

Wanneer u deze optie selecteert, worden de mappen in uw configuratie weergegeven. U kunt de standaardinstelling behouden en Vernieuw alle forests of selectie enkele ervan.

![Pagina met een lijst met alle mappen in de omgeving](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>De faseringsmodus configureren
Deze optie kunt u inschakelen en uitschakelen van de faseringsmodus op de server. Meer informatie over modus en hoe deze wordt gebruikt voor gefaseerde installatie vindt u in [Operations](active-directory-aadconnectsync-operations.md#staging-mode).

De optie ziet als tijdelijke momenteel is ingeschakeld of uitgeschakeld:  
![Optie dat ook de huidige status van de faseringsmodus wordt weergegeven](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Selecteer deze optie om de status wijzigt, en selecteer of schakel het selectievakje uit.  
![Optie dat ook de huidige status van de faseringsmodus wordt weergegeven](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Gebruikersaanmelding wijzigen
Deze optie kunt u wijzigen van Wachtwoordsynchronisatie in Federatie of andersom. U kunt niet wijzigen naar **niet configureert**.

Zie voor meer informatie over deze optie [gebruikersaanmelding](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel dat wordt gebruikt door Azure AD Connect-synchronisatie in [Understanding declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
