---
title: 'Azure AD Connect-synchronisatie: de standaardconfiguratie wijzigen | Microsoft Docs'
description: Bevat de aanbevolen procedures voor het wijzigen van de standaardconfiguratie van Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b723ad800ccc0f3040eb480bb72960943b1fdb16
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect-synchronisatie: aanbevolen procedures voor het wijzigen van de standaardconfiguratie
Het doel van dit onderwerp is voor het beschrijven van ondersteunde en niet-ondersteunde wijzigingen in Azure AD Connect-synchronisatie.

De configuratie wordt gemaakt met Azure AD Connect werkt 'als zodanig' voor de meeste omgevingen die op de lokale Active Directory met Azure AD synchroniseren. In sommige gevallen is het echter nodig zijn voor sommige wijzigingen toepassen op een te voldoen aan een bepaalde nodig of de vereiste configuratie.

## <a name="changes-to-the-service-account"></a>Wijzigingen in de serviceaccount
Azure AD Connect-synchronisatie wordt uitgevoerd onder een serviceaccount dat is gemaakt door de installatiewizard. Dit serviceaccount bevat de versleutelingssleutels voor de database die wordt gebruikt door synchronisatie. Deze is gemaakt met een lang wachtwoord 127 tekens en het wachtwoord verloopt niet is ingesteld.

* Het is **niet-ondersteunde** wijzigen of opnieuw instellen van het wachtwoord van het serviceaccount. In dat geval worden de versleutelingssleutels vernietigd en de service kan geen toegang tot de database en kan niet worden gestart.

## <a name="changes-to-the-scheduler"></a>Wijzigingen in de planner
Beginnen met de releases van versie 1.1 (februari 2016) kunt u de [scheduler](active-directory-aadconnectsync-feature-scheduler.md) hebben een verschillende synchronisatiecyclus dan de standaardwaarde van 30 minuten.

## <a name="changes-to-synchronization-rules"></a>Wijzigingen van synchronisatieregels
De installatiewizard biedt een configuratie die stopt met werken voor de meest voorkomende scenario's. Als u wijzigingen aanbrengen in de configuratie wilt, moet u deze regels nog steeds een ondersteunde configuratie volgen.

* U kunt [kenmerkstromen wijzigen](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) als de standaard directe kenmerkstromen niet geschikt is voor uw organisatie zijn.
* Als u wilt [niet stromen een kenmerk](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) en wordt verwijderd, een bestaand kenmerk in Azure AD, waarden moet u een regel maken voor dit scenario.
* [Uitschakelen van een ongewenste Synchronisatieregel](#disable-an-unwanted-sync-rule) in plaats van te verwijderen. Een verwijderde regel is gemaakt tijdens een upgrade.
* Naar [wijzigen van een regel voor out-of-box](#change-an-out-of-box-rule), maak een kopie van de oorspronkelijke regel en de out-of-box-regel uitschakelen. De regeleditor synchronisatie wordt gevraagd en helpt u bij.
* Exporteer uw aangepaste synchronisatieregels met de regeleditor synchronisatie. De editor biedt u een PowerShell-script die kunt u ze gemakkelijk opnieuw maken in een noodherstelscenario.

> [!WARNING]
> De out-of-box-sync-regels hebben een vingerafdruk. Als u een wijziging in deze regels aanbrengt, wordt de vingerafdruk van het niet langer overeenkomen. Mogelijk hebt u problemen in de toekomst wanneer u probeert toe te passen van een nieuwe release van Azure AD Connect. Alleen wijzigingen aanbrengen de manier waarop die wordt beschreven in dit artikel.

### <a name="disable-an-unwanted-sync-rule"></a>Een ongewenste Synchronisatieregel uitschakelen
Een synchronisatieregel voor out-of-box-niet verwijderen. Dit wordt opnieuw gemaakt tijdens de volgende upgrade.

In sommige gevallen heeft een configuratie die niet voor uw topologie werkt worden geproduceerd door de installatiewizard. Bijvoorbeeld, als u een topologie met account-resource forest hebt, maar u het schema in het forest met de Exchange-schema hebt uitgebreid, worden vervolgens regels voor het uitwisselen van gemaakt voor het forest en de bron-forest. In dit geval moet u de synchronisatie-regel voor het uitwisselen van uitschakelen.

![Uitgeschakelde synchronisatieregel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

De installatiewizard heeft een oude Exchange 2003 schema gevonden in het forest in de bovenstaande afbeelding. Dit schema-uitbreiding is toegevoegd voordat het bronforest werd geïntroduceerd in de omgeving van Fabrikam. Geen kenmerken van de oude Exchange-implementatie worden gesynchroniseerd, zodat moet de synchronisatieregel worden uitgeschakeld zoals wordt weergegeven.

### <a name="change-an-out-of-box-rule"></a>Een regel voor out-of-box wijzigen
De enige keer die moet u een out-of-box-regel is wanneer u wilt wijzigen van de join-regel. Als u een kenmerkstroom wijzigen wilt, moet u een synchronisatieregel met hogere prioriteit dan de out-of-box-regels maken. De enige regel u vrijwel moet voor het klonen is de regel **In uit Active Directory - gebruiker toevoegen**. U kunt alle andere regels met een hogere prioriteit regel onderdrukken.

Als u moet wijzigingen aanbrengen in een regel voor out-of-box, moet u een kopie van de out-of-box-regel maken en de oorspronkelijke regel uitschakelen. Breng vervolgens de wijzigingen op de gekloonde regel. De regeleditor synchronisatie helpt u met deze stappen. Wanneer u een regel voor out-of-box opent, krijgt u dit dialoogvenster:  
![Waarschuwing buiten het vak regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Selecteer **Ja** om een kopie van de regel te maken. De gekloonde regel wordt geopend.  
![Gekloonde regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Op deze regel gekloonde Breng eventueel benodigde wijzigingen voor het bereik, join en transformaties.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
