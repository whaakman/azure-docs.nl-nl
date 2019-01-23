---
title: 'Azure AD Connect-synchronisatie: De standaard-configuratie wijzigen | Microsoft Docs'
description: Bevat de aanbevolen procedures voor het wijzigen van de standaardconfiguratie van Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: f071d97df6932bec5ae4161511ba33369cc1828c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54461174"
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect-synchronisatie: Aanbevolen procedures voor het wijzigen van de standaardconfiguratie
Het doel van dit onderwerp is om te beschrijven van ondersteunde en niet-ondersteunde wijzigingen in Azure AD Connect-synchronisatie.

De configuratie die is gemaakt door Azure AD Connect werkt "as is" voor de meeste omgevingen die on-premises Active Directory met Azure AD synchroniseren. In sommige gevallen is het echter nodig zijn om toe te passen van enkele wijzigingen aan een configuratie om te voldoen aan een nodig of een vereiste.

## <a name="changes-to-the-service-account"></a>Wijzigingen in de serviceaccount
Azure AD Connect-synchronisatie wordt uitgevoerd onder een serviceaccount dat is gemaakt door de installatiewizard. Deze serviceaccount bevat de versleutelingssleutels voor de database die wordt gebruikt door synchronisatie. Deze is gemaakt met een lang wachtwoord 127 tekens en het wachtwoord is ingesteld op verlopen niet.

* Het is **niet-ondersteunde** te wijzigen of opnieuw instellen van het wachtwoord van het serviceaccount. In dat geval worden de versleutelingssleutels vernietigd en de service kan geen toegang tot de database en is niet mogelijk om te starten.

## <a name="changes-to-the-scheduler"></a>Wijzigingen naar de scheduler
Beginnen met de versies van versie 1.1 (februari 2016) kunt u de [scheduler](how-to-connect-sync-feature-scheduler.md) hebben een verschillende synchronisatiecyclus dan de standaardwaarde van 30 minuten.

## <a name="changes-to-synchronization-rules"></a>Wijzigingen in synchronisatieregels
De installatiewizard biedt een configuratie die stopt met werken voor de meest voorkomende scenario's. Als u wijzigingen aanbrengen in de configuratie, moet u deze regels als u wilt nog steeds een ondersteunde configuratie volgen.

> [!WARNING]
> Als u wijzigingen in de standaardregels voor de synchronisatie aanbrengt vervolgens deze wijzigingen overschreven de volgende keer dat Azure AD Connect is bijgewerkt, wat resulteert in de synchronisatie van onverwachte en waarschijnlijk ongewenste resultaten.

* U kunt [kenmerkstromen wijzigen](how-to-connect-sync-change-the-configuration.md#other-common-attribute-flow-changes) als de standaard direct kenmerkstromen niet geschikt voor uw organisatie zijn.
* Als u wilt [niet flow een kenmerk](how-to-connect-sync-change-the-configuration.md#do-not-flow-an-attribute) en verwijder alle bestaande kenmerk in Azure AD, waarden moet u een regel maken voor dit scenario.
* [Uitschakelen van een ongewenste Synchronisatieregel](#disable-an-unwanted-sync-rule) in plaats van te verwijderen. Een regel is verwijderd, wordt opnieuw gemaakt tijdens een upgrade.
* Naar [wijzigen van een regel voor out-of-box](#change-an-out-of-box-rule), moet u een kopie van de oorspronkelijke regel en de out-of-box-regel uitschakelen. De synchronisatie Rule Editor wordt gevraagd en helpt u bij.
* Exporteer uw aangepaste synchronisatieregels met behulp van de Synchronization Rules Editor. De editor biedt u een PowerShell-script die kunt u ze gemakkelijk opnieuw maken in een noodherstelscenario.

> [!WARNING]
> De out-of-box-synchronisatieregels hebben een vingerafdruk. Als u een wijziging in deze regels aanbrengt, wordt de vingerafdruk van het niet meer overeen. U kunt problemen ondervinden in de toekomst wanneer u probeert toe te passen van een nieuwe versie van Azure AD Connect. Alleen wijzigingen aanbrengen de manier waarop die wordt beschreven in dit artikel.

### <a name="disable-an-unwanted-sync-rule"></a>Een ongewenste Synchronisatieregel uitschakelen
Een out-of-box synchronisatieregel niet verwijderen. Er wordt opnieuw gemaakt tijdens de volgende upgrade.

In sommige gevallen heeft een configuratie die niet voor uw topologie werkt worden geproduceerd door de installatiewizard. Bijvoorbeeld, als u een topologie met account-resource forest hebt, maar u het schema in het forest met de Exchange-schema hebt uitgebreid, worden klikt u vervolgens regels voor het uitwisselen van gemaakt voor het forest en de bron-forest. In dit geval moet u het uitschakelen van de Synchronisatieregel voor Exchange.

![Uitgeschakelde synchronisatieregel](./media/how-to-connect-sync-best-practices-changing-default-configuration/exchangedisabledrule.png)

De installatiewizard heeft een oude Exchange 2003-schema gevonden in het forest in de bovenstaande afbeelding. Dit schema-uitbreiding is toegevoegd voordat de bron-forest is geïntroduceerd in de omgeving van Fabrikam. Om ervoor te zorgen dat er zijn geen kenmerken van de oude Exchange-implementatie worden gesynchroniseerd, kan de synchronisatieregel voor moet worden uitgeschakeld zoals wordt weergegeven.

### <a name="change-an-out-of-box-rule"></a>Een out-of-box-regel wijzigen
De enige keer dat u een out-of-box-regel moet wijzigen, is wanneer u nodig hebt om te wijzigen van de join-regel. Als u een kenmerkstroom wijzigen wilt, moet u een synchronisatieregel maken met een hogere prioriteit dan de out-of-box-regels. De enige regel u vrijwel wilt klonen is de regel **In uit Active Directory - gebruiker toevoegen**. U kunt alle andere regels met een hogere prioriteit regel onderdrukken.

Als u wijzigingen aanbrengen in een regel voor out-of-box moet, moet u een kopie van de out-of-box-regel en de oorspronkelijke regel uitschakelen. Breng de wijzigingen aan de gekloonde regel. De synchronisatie-regeleditor helpt u met deze stappen. Wanneer u een regel voor out-of-box opent, krijgt u dit dialoogvenster:  
![Waarschuwing buiten het vak regel](./media/how-to-connect-sync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Selecteer **Ja** om een kopie van de regel te maken. De gekloonde regel wordt geopend.  
![Gekloonde regel](./media/how-to-connect-sync-best-practices-changing-default-configuration/clonedrule.png)

Controleer op deze regel gekloonde eventueel benodigde wijzigingen bereik, join en transformaties.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
