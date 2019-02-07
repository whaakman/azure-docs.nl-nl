---
title: Problemen met Azure AD Connect installeren | Microsoft-Docs
description: Dit onderwerp bevat stappen voor het oplossen van problemen met Azure AD Connect installeert.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 56fd4be85fabebcdde5ca271630e1c5b83c398b9
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55773341"
---
# <a name="troubleshoot-azure-ad-connect-install-issues"></a>Problemen oplossen: Problemen met Azure AD Connect installeren

## <a name="recommended-steps"></a>**Aanbevolen stappen**
Controleer welke [installatietype voor Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-select-installation) is geschikt voor u. Als u voldoet aan de criteria van snelle installatie, klikt u vervolgens raden wij u aan de slag met de snelle installatie. De snelle installatie biedt u de minimale opties die nodig zijn om de installatie te voltooien, dus er minder kans op problemen. 

Echter, als u niet voldoen aan de criteria voor snelle installatie en de aangepaste installatie uitvoeren moet en vervolgens hier enkele aanbevolen procedures volgen kunt u volgen om veelvoorkomende problemen te voorkomen. Om het eenvoudig te alleen selectief opties zijn die hier worden vermeld:

* Zorg ervoor dat u bent een beheerder op de computer waarop u AAD Connect installeert. Meld u aan de machine met dezelfde beheerdersreferenties op.

* Laat de opties worden standaard op de volgende pagina, met uitzondering van 'Gebruik een bestaande SQL Server', als u wilt gebruiken van bestaande SQL-Server. Hier vindt u [meer details](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-custom) over het gebruik van aangepaste installatie-opties. 

    ![Bestaande SQL Server gebruiken](media/tshoot-connect-install-issues/tshoot-connect-install-issues/useexistingsqlserver.png)

* Kies op de volgende pagina optie 'Maken nieuwe AD-account', om te voorkomen dat een andere machtiging problemen met bestaande account.

    ![AD-Forest Account](media/tshoot-connect-install-issues/tshoot-connect-install-issues/createnewaccount.png)

### <a name="common-issues"></a>**Algemene problemen**

* [Problemen met de netwerkverbinding met on-premises Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-adconnectivitytools).

* [Problemen met de netwerkverbinding met de online Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-connectivity).

* [Problemen met de machtiging met de on-premises Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-configure-ad-ds-connector-account).

## <a name="recommended-documents"></a>**Aanbevolen documenten**
* [Vereisten voor Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-prerequisites)
* [Het installatietype voor Azure AD Connect selecteren](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-select-installation)
* [Aan de slag met Azure AD Connect met Express-instellingen](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express)
* [Aangepaste installatie van Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-custom)
* [Azure AD Connect: Upgraden van een vorige naar de meest recente versie](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-upgrade-previous-version)
* [Azure AD Connect: Wat is een faseringsserver?](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-topologies#staging-server)
* [Wat is de PowerShell-module ADConnectivityTool?](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-adconnectivitytools)

## <a name="next-steps"></a>Volgende stappen
- [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md).
- [Wat is hybride identiteit? ](whatis-hybrid-identity.md).





