---
title: Connectors in de Azure AD Synchronization Service Manager-gebruikersinterface | Microsoft-Docs
description: Het tabblad Connectors Synchronization Service Manager begrijpen voor Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3bbbe5d0d7a7ed7065133b4bc6e5fc2dba39bf7d
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="using-connectors-with-the-azure-ad-connect-sync-service-manager"></a>Met behulp van connectors met de Azure AD Connect Sync Service Manager

![Sync-Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Het tabblad Connectors wordt gebruikt voor het beheren van alle systemen die met de synchronisatie-engine is verbonden.

## <a name="connector-actions"></a>Connector-acties
| Bewerking | Opmerking |
| --- | --- |
| Maken |Gebruik geen. Voor het verbinden met extra AD-forests, gebruikt u de installatiewizard. |
| Eigenschappen |Gebruikt voor het domein en OE filteren. |
| [Verwijderen](#delete) |Gebruikt om de gegevens in het connectorgebied overgebracht ofwel verwijderen of te verwijderen van de verbinding met een forest. |
| [Profielen uitvoeren configureren](#configure-run-profiles) |Met uitzondering van domein filteren, niets hier configureren. Deze actie kunt u al geconfigureerde uitvoeringsprofielen zien. |
| Voer |Gebruikt een eenmalige uitvoering van een profiel te starten. |
| Stoppen |Een Connector die momenteel wordt uitgevoerd een profiel stopt. |
| Exporteren van de Connector |Gebruik geen. |
| Connector importeren |Gebruik geen. |
| Update-Connector |Gebruik geen. |
| Schema vernieuwen |Hiermee vernieuwt u het schema in de cache. Het verdient de voorkeur voor gebruik in plaats daarvan de optie in de installatiewizard sinds die updates ook regels synchroniseren. |
| [Connectorgebied zoeken](#search-connector-space) |Gebruikt om objecten te vinden en [volgen op een object en de gegevens via het systeem](#follow-an-object-and-its-data-through-the-system). |

### <a name="delete"></a>Verwijderen
De verwijderactie wordt gebruikt voor twee verschillende dingen.  
![Sync-Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

De optie **alleen connectorruimte verwijderen** alle gegevens worden verwijderd, maar de configuratie te behouden.

De optie **Connector verwijderen en connector ruimte** verwijdert u de gegevens en de configuratie. Deze optie wordt gebruikt wanneer u niet wilt dat deze niet meer verbinding maken met een forest.

Beide opties synchroniseren van alle objecten en de metaverse-objecten bijwerken. Deze actie is een langdurige bewerking.

### <a name="configure-run-profiles"></a>Profielen uitvoeren configureren
Deze optie kunt u zien van de uitvoeringsprofielen geconfigureerd voor een Connector.

![Sync-Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Connectorgebied zoeken
De zoekactie voor de connector-ruimte is handig voor het vinden van objecten en gegevensproblemen oplossen.

![Sync-Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Begin met het selecteren een **bereik**. U kunt zoeken op basis van gegevens (RDN, DN-naam, anker substructuur), of de status van het object (voor alle opties).  
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Als u bijvoorbeeld substructuur zoeken wilt, krijgt u alle objecten in een organisatie-eenheid.  
![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)  
Vanaf dit raster kunt u een object selecteert, selecteert u **eigenschappen**, en [hieraan](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) vanuit de bron-connectorruimte, via de metaverse en naar het doel connectorgebied overgebracht.

### <a name="changing-the-ad-ds-account-password"></a>Het wachtwoord voor de AD DS-account wijzigen
Als u het wachtwoord wijzigt, de synchronisatieservice wordt niet langer te importeren/exporteren wijzigingen met on-premises AD.   U ziet het volgende:

- De stap voor importeren/exporteren voor de AD-connector is mislukt met fout 'geen start referenties'.
- Onder Windows Logboeken bevat het logboek voor toepassingsgebeurtenissen een fout met 6000 van gebeurtenis-ID en het bericht "de beheeragent 'contoso.com' kan niet worden uitgevoerd omdat de referenties ongeldig zijn."

U lost het probleem update voor de gebruikersaccount in AD DS met het volgende:


1. Synchronization Service Manager (START → Synchronization-Service) starten.
</br>![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)
2. Ga naar de **Connectors** tabblad.
3. Selecteer de AD-Connector die is geconfigureerd voor gebruik van de AD DS-account.
4. Selecteer onder acties **eigenschappen**.
5. Selecteer in het pop-updialoogvenster verbinding maken met Active Directory-Forest:
6. De naam van het Forest geeft aan dat de bijbehorende on-premises AD.
7. Naam van de gebruiker geeft aan dat de AD DS-account voor synchronisatie te gebruiken.
8. Voer het nieuwe wachtwoord van het AD DS-account in het tekstvak wachtwoord ![Azure AD Connect-synchronisatie versleuteling sleutel hulpprogramma](media/active-directory-aadconnectsync-encryption-key/key6.png)
9. Klik op OK om het nieuwe wachtwoord opslaan en opnieuw opstarten van de synchronisatieservice om te verwijderen van het oude wachtwoord uit cachegeheugen.



## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
