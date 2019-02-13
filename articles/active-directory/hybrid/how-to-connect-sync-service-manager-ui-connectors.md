---
title: Connectors in de Azure AD Synchronization Service Manager-gebruikersinterface | Microsoft-Docs
description: Meer informatie over het tabblad Connectors is in de Synchronization Service Manager voor Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a0d42ff70bfaebf4d544fd81e6767592198ccef
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205593"
---
# <a name="using-connectors-with-the-azure-ad-connect-sync-service-manager"></a>Met behulp van connectors met de Azure AD Connect Sync Service Manager

![Synchronisatie van Service Manager](./media/how-to-connect-sync-service-manager-ui-connectors/connectors.png)

Het tabblad Connectors is gebruikt voor het beheren van alle systemen die de synchronisatie-engine met is verbonden.

## <a name="connector-actions"></a>Connectoracties
| Bewerking | Opmerking |
| --- | --- |
| Maken |Gebruik geen. Voor het verbinden met aanvullende AD-forests, gebruik van de wizard. |
| Properties |Gebruikt voor het domein en OE filteren. |
| [Verwijderen](#delete) |Gebruikt om de gegevens in het connectorgebied verwijderen of verwijderen van verbinding met een forest. |
| [Profielen uitvoeren configureren](#configure-run-profiles) |Met uitzondering van domein filteren, en er kan niets worden hier configureert. Deze actie kunt u al geconfigureerde uitvoeringsprofielen zien. |
| Voer |Gebruikt voor het starten van een eenmalig uitvoeren van een profiel. |
| Stoppen |Hiermee stopt u een Connector die momenteel wordt uitgevoerd een profiel. |
| Exporteren-Connector |Gebruik geen. |
| Import-Connector |Gebruik geen. |
| Connector bijwerken |Gebruik geen. |
| Schema vernieuwen |Hiermee vernieuwt u het schema in de cache. Het is voorkeur aan in plaats daarvan gebruikt u de optie in de installatiewizard sinds die updates ook regels synchroniseren. |
| [Connectorgebied zoeken](#search-connector-space) |Gebruikt om objecten te zoeken en te volgen van een object en de bijbehorende gegevens via het systeem. |

### <a name="delete"></a>Verwijderen
De verwijderingsactie wordt gebruikt voor twee verschillende dingen.  
![Synchronisatie van Service Manager](./media/how-to-connect-sync-service-manager-ui-connectors/connectordelete.png)

De optie **connectorgebied alleen verwijderen** alle gegevens worden verwijderd, maar houd de configuratie.

De optie **Connector verwijderen en connector space** Hiermee verwijdert u de gegevens en de configuratie. Deze optie wordt gebruikt wanneer u niet wilt dat deze niet meer verbinding maken met een forest.

Beide opties voor alle objecten synchroniseren en bijwerken van de metaverse-objecten. Deze actie is een langdurige bewerking.

### <a name="configure-run-profiles"></a>Profielen uitvoeren configureren
Deze optie kunt u om te zien van de uitvoerprofielen geconfigureerd voor een Connector.

![Synchronisatie van Service Manager](./media/how-to-connect-sync-service-manager-ui-connectors/configurerunprofiles.png)

### <a name="search-connector-space"></a>Connectorgebied zoeken
De zoekactie voor de connector-ruimte is handig om objecten te vinden en oplossen van problemen met gegevens.

![Synchronisatie van Service Manager](./media/how-to-connect-sync-service-manager-ui-connectors/cssearch.png)

Selecteer eerst een **bereik**. U kunt zoeken op basis van gegevens (RDN, DN-naam, anker, subboomstructuur), of de status van het object (alle andere opties).  
![Sync Service Manager](./media/how-to-connect-sync-service-manager-ui-connectors/cssearchscope.png)  
Als u bijvoorbeeld een zoekopdracht subboomstructuur doet, krijgt u alle objecten in een organisatie-eenheid.  
![Sync Service Manager](./media/how-to-connect-sync-service-manager-ui-connectors/cssearchsubtree.png)  
Vanaf dit raster kunt u een object selecteert, selecteert u **eigenschappen**, en [volgen door](tshoot-connect-object-not-syncing.md) van het connectorgebied bron via de metaverse en het doel-connectorgebied.

### <a name="changing-the-ad-ds-account-password"></a>Het wachtwoord van het AD DS-account wijzigen
Als u een wachtwoord voor het account wijzigt, de synchronisatieservice wordt niet langer te importeren/exporteren wijzigingen naar on-premises AD.   U kunt het volgende te zien krijgen:

- De import/export-stap voor de AD-connector is mislukt met fout "geen-start-referenties".
- In Windows-Logboeken bevat het logboek voor toepassingsgebeurtenissen een fout met 6000 van gebeurtenis-ID en het bericht "de beheeragent 'contoso.com' kan niet worden uitgevoerd omdat de referenties ongeldig zijn."

Los het probleem, update voor de gebruikersaccount in AD DS met het volgende:


1. Start de Synchronization Service Manager (→-synchronisatieservice START).
</br>![Sync Service Manager](./media/how-to-connect-sync-service-manager-ui-connectors/startmenu.png)
2. Ga naar de **Connectors** tabblad.
3. Selecteer de AD-Connector die is geconfigureerd voor gebruik van de AD DS-account.
4. Selecteer onder acties **eigenschappen**.
5. Selecteer in het pop-updialoogvenster, verbinding maken met Active Directory-Forest:
6. De naam van het Forest geeft aan dat de bijbehorende on-premises AD.
7. Naam van de gebruiker geeft aan dat de AD DS-account voor synchronisatie te gebruiken.
8. Voer het nieuwe wachtwoord van het AD DS-account in het tekstvak wachtwoord ![Azure AD Connect Sync versleuteling sleutel hulpprogramma](./media/how-to-connect-sync-service-manager-ui-connectors/key6.png)
9. Klik op OK om het nieuwe wachtwoord opslaan en opnieuw opstarten van de Synchronization Service als u wilt verwijderen van het oude wachtwoord uit geheugencache.



## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](whatis-hybrid-identity.md).
