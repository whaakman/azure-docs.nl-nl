---
title: 'Azure AD Connect-synchronisatie: inzicht in en synchronisatie aanpassen | Microsoft Docs'
description: Legt uit hoe Azure AD Connect synchroniseren werkt en hoe u om aan te passen.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: ee4bf802-045b-4da0-986e-90aba2de58d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 247b7bb5b5dbce94c8f8339a6f06c8ae5dab75d9
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919362"
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect-synchronisatie: inzicht in en synchronisatie aanpassen
De Azure Active Directory Connect-Synchronisatieservices (Azure AD Connect-synchronisatie) is een belangrijkste onderdeel van Azure AD Connect. Dit zorgt dat alle bewerkingen die zijn gerelateerd aan het synchroniseren van identiteitsgegevens tussen uw on-premises omgeving en Azure AD. Azure AD Connect-synchronisatie is de opvolger van DirSync, Azure AD Sync en Forefront Identity Manager met de Azure Active Directory-Connector geconfigureerd.

In dit onderwerp is de thuisbasis voor **Azure AD Connect-synchronisatie** (ook wel genoemd **synchronisatie-engine**) en een lijst met koppelingen naar alle andere onderwerpen met betrekking tot het. Zie voor koppelingen naar Azure AD Connect [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).

De synchronisatieservice bestaat uit twee onderdelen: de on-premises **Azure AD Connect-synchronisatie** onderdeel en de kant van de service in Azure AD met de naam **Azure AD Connect-synchronisatieservice**.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect sync-onderwerpen
| Onderwerp | Er wordt aangegeven en bij het lezen |
| --- | --- |
| **Grondbeginselen van Azure AD Connect-synchronisatie** | |
| [Inzicht in de architectuur](active-directory-aadconnectsync-understanding-architecture.md) |Voor het geval u die niet bekend bent met de synchronisatie-engine en meer informatie over de architectuur en de termen die worden gebruikt. |
| [Technische concepten](active-directory-aadconnectsync-technical-concepts.md) |Een verkorte versie van het onderwerp van de architectuur en kort wordt uitgelegd dat de termen die worden gebruikt. |
| [Topologieën voor Azure AD Connect](active-directory-aadconnect-topologies.md) |Beschrijft de verschillende topologieën en scenario's die biedt ondersteuning voor de synchronisatie-engine. |
| **Aangepaste configuratie** | |
| [De installatiewizard opnieuw uit te voeren](active-directory-aadconnectsync-installation-wizard.md) |Wordt uitgelegd wat u opties beschikbaar zijn wanneer u de Azure AD Connect-installatiewizard opnieuw uitvoert. |
| [Informatie over declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md) |Beschrijving van het configuratiemodel met de naam declaratieve inrichting. |
| [Inzicht in verklarende Inrichtingsexpressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |Beschrijving van de syntaxis voor de expressietaal die wordt gebruikt in de declaratieve inrichting. |
| [Inzicht in de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md) |Beschrijving van de out-of-box-regels en de standaardconfiguratie. Beschrijft ook hoe de regels samenwerken voor de out-of-box-scenario's om te werken. |
| [Inzicht krijgen in gebruikers en contactpersonen](active-directory-aadconnectsync-understanding-users-and-contacts.md) |Blijft op het vorige onderwerp en wordt beschreven hoe de configuratie voor gebruikers en contactpersonen werkt samen met name in een omgeving met meerdere forests. |
| [Hoe u een wijziging in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md) |Helpt u bij het maken van een algemene configuratie kenmerkstromen wijzigen. |
| [Aanbevolen procedures voor het wijzigen van de standaardconfiguratie](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) |Beperkingen voor de ondersteuning en voor het aanbrengen van wijzigingen aan de out-of-box-configuratie. |
| [Filteren configureren](active-directory-aadconnectsync-configure-filtering.md) |Beschrijft de verschillende opties voor hoe u wilt beperken welke objecten worden gesynchroniseerd met Azure AD en stapsgewijs hoe u deze opties configureert. |
| **Functies en scenario 's** | |
| [Onopzettelijke verwijderingen voorkomen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) |Beschrijft de *onopzettelijke verwijderingen voorkomen* functie en over hoe dit moet worden geconfigureerd. |
| [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) |Beschrijving van de ingebouwde scheduler, is importeren en synchroniseren en exporteren van gegevens. |
| [Wachtwoord-hashsynchronisatie implementeren](active-directory-aadconnectsync-implement-password-hash-synchronization.md) |Beschrijft hoe werkt Wachtwoordsynchronisatie, hoe u wilt implementeren, en om te werken en op te lossen. |
| [Write-back van apparaat](active-directory-aadconnect-feature-device-writeback.md) |Hierin wordt beschreven hoe apparaat terugschrijven werkt in Azure AD Connect. |
| [Uitbreidingen van de directory](active-directory-aadconnectsync-feature-directory-extensions.md) |Beschrijft hoe u de Azure AD-schema met uw eigen aangepaste kenmerken uit te breiden. |
| [Office 365 PreferredDataLocation](active-directory-aadconnectsync-feature-preferreddatalocation.md) |Beschrijft hoe u Office 365-resources van de gebruiker in dezelfde regio als de gebruiker geplaatst. |
| **Sync-Service** | |
| [Functies van Azure AD Connect sync-service](active-directory-aadconnectsyncservice-features.md) |Beschrijving van de synchronisatie-servicezijde en het synchronisatie-instellingen wijzigen in Azure AD. |
| [Tolerantie van dubbele kenmerken](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) |Beschrijft hoe u wilt inschakelen en gebruiken **userPrincipalName** en **proxyAddresses** tolerantie van dubbel kenmerk waarden. |
| **Bewerkingen en -gebruikersinterface** | |
| [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md) |Beschrijving van de Synchronization Service Manager-UI, met inbegrip van [Operations](active-directory-aadconnectsync-service-manager-ui-operations.md), [Connectors](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse-ontwerper](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md), en [Metaverse zoeken](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) tabbladen. |
| [Operationele taken en overwegingen](active-directory-aadconnectsync-operations.md) |Beschrijving van operationele problemen, zoals herstel na noodgevallen. |
| **Procedures...** | |
| [Opnieuw instellen van de Azure AD-account](active-directory-aadconnectsync-howto-azureadaccount.md) |Het opnieuw instellen van de referenties van de serviceaccount waarmee verbinding wordt gemaakt van Azure AD Connect-synchronisatie met Azure AD. |
| **Meer informatie en verwijzingen** | |
| [Poorten](active-directory-aadconnect-ports.md) |Een lijst met welke poorten moet u openen tussen de synchronisatie-engine en uw on-premises directory's en Azure AD. |
| [Kenmerken gesynchroniseerd naar Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) |Een lijst met alle kenmerken worden gesynchroniseerd tussen on-premises AD en Azure AD. |
| [Functieverwijzing](active-directory-aadconnectsync-functions-reference.md) |Geeft een lijst van alle functies die beschikbaar zijn in de declaratieve inrichting. |

## <a name="additional-resources"></a>Aanvullende resources
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
