---
title: 'Azure AD Connect: Bronanker problemen oplossen tijdens de installatie | Microsoft Docs'
description: Dit onderwerp bevat stappen voor het oplossen van problemen met het bronanker tijdens de installatie.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/19/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: fac33a01afc2efc1ab06c4783c11f7a089bb6208
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62114151"
---
# <a name="troubleshooting-source-anchor-issues-during-installation"></a>Het oplossen van problemen met het anker van bron tijdens de installatie
In dit artikel worden de verschillende bronanker problemen die zich tijdens de installatie en aanbiedingen manieren voordoen kunnen om deze problemen oplossen.

## <a name="invalid-source-anchor-in-azure-active-directory"></a>Ongeldige Bronanker in Azure Active Directory

### <a name="custom-installation"></a>Aangepaste installatie

Tijdens de installatie van aangepaste leest Azure AD Connect het anker bronbeleid uit Azure Active Directory. Als het beleid in Azure Active Directory bestaat, wordt in het Azure AD Connect hetzelfde beleid van toepassing, tenzij deze is overschreven door de klant. De wizard geeft aan welk kenmerk is gelezen. Bovendien wordt de wizard gewaarschuwd als u probeert om het anker bronbeleid negeren.

Tijdens deze bewerking lezen is het mogelijk dat het beleid voor het anker van bron in Azure Active Directory onverwacht is. In dit geval Azure AD Connect niet weet wat het bronanker te gebruiken en moet handmatig overschrijven.</br>
![unexpected](media/tshoot-connect-source-anchor/source1.png)

U lost dit probleem, kunt u handmatig het bronanker overschrijven door te selecteren van een specifiek kenmerk. Doorgaan met deze optie alleen als u er zeker van welk kenmerk te selecteren. Als u niet zeker weet, neem dan contact op met [Microsoft ondersteuning](https://support.microsoft.com/contactus/) voor hulp. Als u het beleid voor het anker van bron wijzigt, kan zij de koppeling tussen uw on-premises gebruikers en hun bijbehorende Azure-resources worden verbroken.</br>
![unexpected](media/tshoot-connect-source-anchor/source2.png)

### <a name="express-installation"></a>Snelle installatie
Tijdens de snelle installatie van leest Azure AD Connect het anker bronbeleid uit Azure Active Directory. Als het beleid in Azure Active Directory bestaat, is Azure AD Connect hetzelfde beleid van toepassing. Er is geen optie om handmatig overschrijven.

Tijdens deze bewerking lezen is het mogelijk dat het beleid voor het anker van bron in Azure Active Directory onverwacht is. Azure AD Connect biedt in dit geval niet weet wat het bronanker moet zijn.</br>
![unexpected](media/tshoot-connect-source-anchor/source3.png)

U lost dit probleem, moet u opnieuw installeren met behulp van de aangepaste modus en het bronanker handmatig overschrijven door te selecteren van een specifiek kenmerk. Doorgaan met deze optie alleen als u er zeker van welk kenmerk te selecteren. Als u niet zeker weet, neem dan contact op met [Microsoft ondersteuning](https://support.microsoft.com/contactus/) voor hulp. Als u het beleid voor het anker van bron wijzigt, kan zij de koppeling tussen uw on-premises gebruikers en hun bijbehorende Azure-resources worden verbroken.

### <a name="invalid-source-anchor-in-sync-engine"></a>Ongeldige Bronanker in synchronisatie-Engine
Tijdens de installatie, is het mogelijk Azure AD dat verbindingspogingen de synchronisatie-engine met behulp van een ongeldige bronanker configureren. Met deze bewerking is waarschijnlijk een probleem met een product en mislukt de installatie van Azure AD Connect. Neem contact op met [Microsoft ondersteuning](https://support.microsoft.com/contactus/) als u in voor dit probleem uitvoert.</br>
![unexpected](media/tshoot-connect-source-anchor/source4.png)


## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).