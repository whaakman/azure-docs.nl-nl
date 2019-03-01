---
title: Wizard opnieuw uitvoeren van de Azure AD Connect installeren | Microsoft Docs
description: Wordt uitgelegd hoe de installatiewizard werkt de tweede keer dat u deze uitvoert.
keywords: De Azure AD Connect-installatiewizard kunt u de tweede keer dat u het uitvoeren van onderhoudsinstellingen configureren
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ff2caae7cb387f4f0d88cf059d01ad28861b9ad
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57191984"
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect-synchronisatie: De installatiewizard een tweede keer uitvoeren
De eerste keer dat u de wizard Azure AD Connect-installatie uitvoert helpt het u bij het configureren van uw installatie. Als u de installatiewizard opnieuw uitvoert, biedt opties voor onderhoud.

U kunt de installatiewizard vinden in het startmenu met de naam **Azure AD Connect**.

![Startmenu](./media/how-to-connect-installation-wizard/startmenu.png)

Als u de installatiewizard start, ziet u een pagina met de volgende opties:

![Pagina met een lijst van extra taken](./media/how-to-connect-installation-wizard/additionaltasks.png)

Als u AD FS met Azure AD Connect hebt geïnstalleerd, hebt u nog meer opties. De aanvullende opties die u hebt voor AD FS zijn gedocumenteerd in [AD FS management](how-to-connect-fed-management.md#manage-ad-fs).

Selecteer een van de taken en klikt u op **volgende** om door te gaan.

> [!IMPORTANT]
> Hoewel u de installatiewizard open hebben, worden alle bewerkingen in de synchronisatie-engine onderbroken. Zorg ervoor dat u de installatiewizard sluit, zodra u uw wijzigingen in de configuratie hebt voltooid.
>
>

## <a name="view-current-configuration"></a>De huidige configuratie weergeven
Met deze optie geeft u een snelle weergave van uw huidige ingestelde opties.

![Pagina met een lijst van alle opties en hun status](./media/how-to-connect-installation-wizard/viewconfig.png)

Klik op **vorige** om terug te gaan. Als u selecteert **afsluiten**, u de installatiewizard sluit.

## <a name="customize-synchronization-options"></a>Synchronisatieopties aanpassen
Deze optie wordt gebruikt om de synchronisatieconfiguratie van de te wijzigen. Ziet u een subset van de opties in het installatiepad van aangepaste configuratie. U ziet deze optie, zelfs als u snelle installatie in eerste instantie gebruikt.

* [Toevoegen van meer mappen](how-to-connect-install-custom.md#connect-your-directories). Zie voor het verwijderen van een map, [verwijderen van een Connector](how-to-connect-sync-service-manager-ui-connectors.md#delete).
* [Wijzigen van domein en OE filteren](how-to-connect-install-custom.md#domain-and-ou-filtering).
* Verwijder het filteren van groep.
* [Wijzigen van optionele functies](how-to-connect-install-custom.md#optional-features).

De overige opties uit de eerste installatie kunnen niet worden gewijzigd en zijn niet beschikbaar. Deze opties zijn:

* Het kenmerk moet worden gebruikt voor de userPrincipalName en sourceAnchor wijzigen.
* Wijzig de lid te worden methode voor objecten van een ander forest.
* Filteren op basis van een groep inschakelen.

## <a name="refresh-directory-schema"></a>Directory-schema vernieuwen
Deze optie wordt gebruikt als u het schema hebt gewijzigd in een van uw on-premises AD DS-forests. U kunt bijvoorbeeld hebt Exchange geïnstalleerd of bijgewerkt naar een Windows Server 2012-schema met apparaatobjecten. In dit geval moet u vertelt u Azure AD Connect opnieuw lezen van het schema uit AD DS en bijwerken van de cache. Deze actie wordt ook de synchronisatie-regels opnieuw gegenereerd. Als u de Exchange-schema, als voorbeeld toevoegt, worden de synchronisatieregels voor Exchange toegevoegd aan de configuratie.

Wanneer u deze optie selecteert, worden de mappen in uw configuratie weergegeven. U kunt de standaardinstelling behouden en alle forests vernieuwen of hef de selectie van sommige hiervan.

![Pagina met een lijst van alle mappen in de omgeving](./media/how-to-connect-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>De faseringsmodus configureren
Deze optie kunt u om te schakelen en schakel de faseringsmodus op de server uit. Meer informatie over de staging-modus en hoe deze wordt gebruikt kan worden gevonden [Operations](how-to-connect-sync-staging-server.md).

De optie wordt weergegeven als fasering momenteel is ingeschakeld of uitgeschakeld:  
![Optie die ook zichtbaar is voor de huidige status van de faseringsmodus](./media/how-to-connect-installation-wizard/stagingmodecurrentstate.png)

Selecteer deze optie als u wilt wijzigen van de status, en selecteer of het selectievakje uit.  
![Optie die ook zichtbaar is voor de huidige status van de faseringsmodus](./media/how-to-connect-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Gebruikersaanmelding wijzigen
Deze optie kunt u de gebruiker aanmelden methode wijzigen naar en van synchronisatie van wachtwoordhashes, pass-through-verificatie of Federatie. U kunt niet wijzigen naar **niet configureert**.

Zie voor meer informatie over deze optie [aanmelden van gebruikers](plan-connect-user-signin.md#changing-the-user-sign-in-method).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel dat wordt gebruikt door Azure AD Connect-synchronisatie in [Understanding declaratieve inrichting](concept-azure-ad-connect-sync-declarative-provisioning.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
