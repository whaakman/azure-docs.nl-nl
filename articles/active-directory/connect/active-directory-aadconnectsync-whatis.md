---
title: 'Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen | Microsoft Docs'
description: Legt uit hoe Azure AD Connect synchroniseren werkt en hoe u kunt aanpassen.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: ee4bf802-045b-4da0-986e-90aba2de58d6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.openlocfilehash: 20614aaabc05513e93eb001c1676a73f85003947
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen
De Azure Active Directory Connect-Synchronisatieservices (Azure AD Connect sync) is een belangrijkste onderdeel van Azure AD Connect. Dit zorgt voor alle bewerkingen die betrekking hebben op het synchroniseren van identiteitsgegevens tussen uw on-premises omgeving en Azure AD. Azure AD Connect-synchronisatie is de opvolger van DirSync, Azure AD Sync en Forefront Identity Manager met de Azure Active Directory-Connector geconfigureerd.

Dit onderwerp is de startpagina voor **Azure AD Connect-synchronisatie** (ook wel **synchronisatie-engine**) en vindt u koppelingen naar andere onderwerpen die zijn gekoppeld. Zie voor koppelingen naar Azure AD Connect [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).

De synchronisatieservice bestaat uit twee onderdelen: de on-premises **Azure AD Connect-synchronisatie** onderdeel en de servicezijde in Azure AD aangeroepen **Azure AD Connect-synchronisatieservice**. 

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect sync-onderwerpen
| Onderwerp | Er wordt aangegeven en wanneer lezen |
| --- | --- |
| **Grondbeginselen van Azure AD Connect-synchronisatie** | |
| [Inzicht in de architectuur](active-directory-aadconnectsync-understanding-architecture.md) |Voor mensen die niet bekend bent met de synchronisatie-engine en meer informatie over de architectuur en de gebruikte termen. |
| [Technische concepten](active-directory-aadconnectsync-technical-concepts.md) |Een korte versie van het onderwerp architectuur en een korte verklaart de termen die worden gebruikt. |
| [Topologieën voor Azure AD Connect](active-directory-aadconnect-topologies.md) |Beschrijft de verschillende topologieën en scenario's die biedt ondersteuning voor de synchronisatie-engine. |
| **Aangepaste configuratie** | |
| [De installatiewizard opnieuw uit te voeren](active-directory-aadconnectsync-installation-wizard.md) |Wordt uitgelegd wat u opties beschikbaar zijn wanneer u de Azure AD Connect-installatiewizard opnieuw uitvoert. |
| [Informatie over declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md) |Beschrijft de configuratiemodel aangeroepen declaratieve inrichting. |
| [Inzicht in expressies declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |Beschrijft de syntaxis voor de expressietaal in de declaratieve inrichting gebruikt. |
| [Inzicht in de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md) |Beschrijft de out-of-box-regels en de standaardconfiguratie. Tevens wordt beschreven hoe de regels samenwerken voor de out-of-box-scenario's werken. |
| [Inzicht krijgen in gebruikers en contactpersonen](active-directory-aadconnectsync-understanding-users-and-contacts.md) |Van het vorige onderwerp wordt voortgezet en wordt beschreven hoe de configuratie voor gebruikers en contactpersonen werkt samen met name in een omgeving met meerdere forests. |
| [Hoe een wijziging aanbrengt in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md) |Begeleidt u bij het maken van een algemene configuratie-instellingen wijzigen voor kenmerkstromen. |
| [Aanbevolen procedures voor het wijzigen van de standaardconfiguratie](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) |Beperkingen voor de ondersteuning en voor het aanbrengen van wijzigingen in de out-of-box-configuratie. |
| [Filteren configureren](active-directory-aadconnectsync-configure-filtering.md) |Beschrijft de verschillende opties voor hoe u wilt beperken welke objecten worden gesynchroniseerd naar Azure AD en stapsgewijs hoe u deze opties configureert. |
| **Scenario's en onderdelen** | |
| [Onopzettelijke verwijderingen voorkomen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) |Beschrijft de *onopzettelijk verwijderen voorkomen* functie en over hoe dit moet worden geconfigureerd. |
| [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) |Beschrijving van de ingebouwde scheduler die importeren, synchroniseren en exporteren van gegevens. |
| [Wachtwoordsynchronisatie implementeren](active-directory-aadconnectsync-implement-password-synchronization.md) |Hierin wordt beschreven hoe werkt Wachtwoordsynchronisatie, implementeren en hoe werkt en problemen oplossen. |
| [Write-back van apparaat](active-directory-aadconnect-feature-device-writeback.md) |Hierin wordt beschreven hoe Write-back van apparaat werkt in Azure AD Connect. |
| [Uitbreidingen van de directory](active-directory-aadconnectsync-feature-directory-extensions.md) |Beschrijft hoe de Azure AD-schema met uw eigen aangepaste kenmerken uit te breiden. |
| **Sync Service** | |
| [Azure AD Connect sync-service-functies](active-directory-aadconnectsyncservice-features.md) |Beschrijft de servicezijde synchronisatie en het synchronisatie-instellingen wijzigen in Azure AD. |
| [Dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) |Hierin wordt beschreven hoe inschakelen en gebruiken **userPrincipalName** en **proxyAddresses** dubbel kenmerk waarden tolerantie. |
| **Bewerkingen en gebruikersinterface** | |
| [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md) |Beschrijft de Synchronization Service Manager-UI, met inbegrip van [Operations](active-directory-aadconnectsync-service-manager-ui-operations.md), [Connectors](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md), en [Metaverse zoeken](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) tabbladen. |
| [Operationele taken en overwegingen](active-directory-aadconnectsync-operations.md) |Beschrijving van operationele problemen, zoals herstel na noodgevallen. |
| **Procedures...** | |
| [Opnieuw instellen van de Azure AD-account](active-directory-aadconnectsync-howto-azureadaccount.md) |Het opnieuw instellen van de referenties van het serviceaccount dat wordt gebruikt voor het verbinden van Azure AD Connect-synchronisatie met Azure AD. |
| **Meer informatie en verwijzingen** | |
| [Poorten](active-directory-aadconnect-ports.md) |Een lijst met welke poorten u wilt openen tussen de synchronisatie-engine en uw on-premises adreslijsten en Azure AD. |
| [Kenmerken gesynchroniseerd naar Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) |Hier worden alle kenmerken worden gesynchroniseerd tussen on-premises AD en Azure AD. |
| [Functieverwijzing](active-directory-aadconnectsync-functions-reference.md) |Een lijst met alle beschikbare functies in declaratieve inrichting. |

## <a name="additional-resources"></a>Aanvullende resources
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

