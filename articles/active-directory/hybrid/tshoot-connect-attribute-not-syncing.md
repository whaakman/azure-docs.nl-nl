---
title: Een kenmerk dat niet kan worden gesynchroniseerd in Azure AD Connect oplossen | Microsoft-Docs
description: Dit onderwerp bevat stappen voor het oplossen van problemen met synchronisatie van kenmerk met de taak voor het oplossen van problemen.
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: a639b14c9313179816f6376aa0c5642a645ea344
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56180822"
---
# <a name="troubleshoot-an-attribute-not-synchronizing-in-azure-ad-connect"></a>Een kenmerk dat niet kan worden gesynchroniseerd in Azure AD Connect oplossen

## <a name="recommended-steps"></a>**Aanbevolen stappen**

Voordat u wilt onderzoeken kenmerk problemen worden gesynchroniseerd, moeten we eerst begrijpen de **Azure AD Connect** proces synchroniseren:

  ![Azure AD Connect-synchronisatie](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminologie**

* **CS:** Connectorgebied, een tabel in de database.
* **MV:** Metaverse, een tabel in de database.
* **AD:** Active Directory
* **AAD:** Azure Active Directory

### <a name="synchronization-steps"></a>**Stappen voor synchronisatie**

* Importeren uit Active Directory: Active Directory-objecten worden in AD CS gebracht.

* Importeren uit AAD: Azure Active Directory-objecten worden in AAD CS gebracht.

* Synchronisatie: **Synchronisatie van regels voor binnenkomende verbindingen** en **uitgaande synchronisatieregels** vanaf worden uitgevoerd in volgorde van prioriteit nummer lager op hoger. Als u wilt de synchronisatieregels weergeven, gaat u naar **Synchronization Rules Editor** vanuit de bureaublad-toepassingen. De **regels voor binnenkomende synchronisatie** worden in de gegevens uit de CS te MV. De **uitgaande synchronisatieregels** gegevens verplaatst van MV naar CS.

* Exporteren naar AD: Nadat synchronisatie is uitgevoerd, objecten zijn geëxporteerd uit AD CS **Active Directory**.

* Exporteren naar AAD: Nadat synchronisatie is uitgevoerd, objecten zijn geëxporteerd uit AAD CS **Azure Active Directory**.

### <a name="step-by-step-investigation"></a>**Stap voor stap onderzoek**

* Gaan we onze zoeken vanaf de **Metaverse** en bekijkt u de kenmerktoewijzing van bron naar doel.

* Start **Synchronization Service Manager** vanuit de bureaublad-toepassingen, zoals hieronder wordt weergegeven:

  ![Open Synchronization Service Manager](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/startmenu.png)

* Op de **Synchronization Service Manager**, selecteer de **Metaverse zoeken**, selecteer **bereik op objecttype**, selecteert u het object met behulp van een kenmerk en klikt u op **Zoeken** knop.

  ![Metaverse zoeken](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvsearch.png)

* Dubbelklik op het object dat is gevonden in de **Metaverse** zoeken om de kenmerken ervan weer te geven. U kunt klikken op de **Connectors** tabblad om te kijken naar overeenkomstige object in alle de **Connectorspaces**.

  ![Metaverse-Object-Connectors](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvattributes.png)

* Dubbelklik op de **Active Directory-Connector** om weer te geven de **Connectorgebied** kenmerken. Klik op de **Preview** knop op het volgende dialoogvenster Klik op de **genereren Preview** knop.

  ![Connector Space-kenmerken](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/csattributes.png)

* Klik nu op de **importeren Kenmerkstroom**, dit wordt de stroom van de kenmerken van weergegeven **Active Directory-Connectorgebied** naar de **Metaverse**. **Regel voor synchroniseren** kolom geeft aan welke **Synchronisatieregel** hebben bijgedragen aan dit kenmerk. **Gegevensbron** kolom ziet u de kenmerken van de **Connectorgebied**. **Metaverse-kenmerk** kolom ziet u de kenmerken in de **Metaverse**. U kunt zoeken voor het kenmerk niet hier worden gesynchroniseerd. Als u niet kunt, het kenmerk vinden wordt dit niet is toegewezen en u moet maken van nieuwe aangepaste **Synchronisatieregel** om toe te wijzen van het kenmerk.

  ![Connector Space-kenmerken](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/cstomvattributeflow.png)

* Klik op de **Kenmerkstroom exporteren** in het linkerdeelvenster om weer te geven van de kenmerkstroom van **Metaverse** terug naar **Active Directory-Connectorgebied** met behulp van  **Regels voor uitgaande synchronisatie**.

  ![Connector Space-kenmerken](media/tshoot-connect-attribute-not-syncing/tshoot-connect-attribute-not-syncing/mvtocsattributeflow.png)

* Op deze manier kunt u bekijken de **Azure Active Directory-Connectorgebied** object en kan genereren de **Preview** om weer te geven van de kenmerkstroom van **Metaverse** naar de **Connectorgebied** en vice versa, op deze manier kunt u onderzoeken waarom een kenmerk niet wordt gesynchroniseerd.

## <a name="recommended-documents"></a>**Aanbevolen documenten**
* [Azure AD Connect-synchronisatie: Technische concepten](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-technical-concepts)
* [Azure AD Connect-synchronisatie: Inzicht in de architectuur](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
* [Azure AD Connect-synchronisatie: Inzicht in declaratieve inrichting](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning)
* [Azure AD Connect-synchronisatie: Inzicht in verklarende inrichtingsexpressies](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-declarative-provisioning-expressions)
* [Azure AD Connect-synchronisatie: Inzicht in de standaardconfiguratie](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-default-configuration)
* [Azure AD Connect-synchronisatie: Inzicht in gebruikers, groepen en contactpersonen](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-user-and-contacts)
* [Azure AD Connect-synchronisatie: Schaduwkenmerken](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-syncservice-shadow-attributes)

## <a name="next-steps"></a>Volgende stappen

- [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md).
- [Wat is hybride identiteit? ](whatis-hybrid-identity.md).
