---
title: Azure AD Connect en Federatie | Microsoft Docs
description: Deze pagina is de centrale locatie voor alle documentatie over AD FS-bewerkingen die gebruikmaken van Azure AD Connect.
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: mtillman
editor: 
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: anandy
ms.openlocfilehash: 04516e38e72405ca797a0d748d9ed825ae452966
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect en federatie
Azure Active Directory (Azure AD) Connect kunt configureren van Federatie met lokale Active Directory Federation Services (AD FS) en Azure AD. Met federation aanmelden kunt u gebruikers aan te melden bij Azure AD-services op basis van hun on-premises wachtwoorden-- en klik op het bedrijfsnetwerk, zonder te hoeven hun wachtwoord opnieuw invoeren. Met de optie Federatie met AD FS, kunt u een nieuwe installatie van AD FS implementeren of u kunt een bestaande installatie opgeven in een farm met Windows Server 2012 R2.

Dit onderwerp is de startpagina voor informatie over de federation-gerelateerde functies voor Azure AD Connect. Hiermee geeft u koppelingen naar alle verwante onderwerpen. Zie voor koppelingen naar Azure AD Connect [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).

## <a name="azure-ad-connect-federation-topics"></a>Azure AD Connect: federation-onderwerpen
| Onderwerp | Er wordt aangegeven en wanneer lezen |
|:--- |:--- |
| **Azure AD Connect gebruiker aanmeldingsopties** | |
| [Opties aanmelden gebruiker begrijpen](active-directory-aadconnect-user-signin.md) |Meer informatie over de verschillende aanmelden gebruikersopties en hoe ze invloed hebben op de gebruikerservaring voor Azure aanmelden. |
| **AD FS installeren met behulp van Azure AD Connect** | |
| [Vereisten](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) |Zie de vereisten voor een geslaagde installatie van AD FS via Azure AD Connect. |
| [Een AD FS-farm configureren](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) |Installeer een nieuwe AD FS-farm met behulp van Azure AD Connect. |
| [Gefedereerd met Azure AD met behulp van alternatieve aanmeldings-ID](active-directory-aadconnect-federation-management.md#alternateid) | Federatie met behulp van alternatieve aanmeldings-ID configureren  |
| **De AD FS-configuratie wijzigen** | |
| [Herstellen van de vertrouwensrelatie](active-directory-aadconnect-federation-management.md#repairthetrust) |Herstel de vertrouwensrelatie van de huidige tussen lokale AD FS en Office 365/Azure. |
| [Een nieuwe AD FS-server toevoegen](active-directory-aadconnect-federation-management.md#addadfsserver) |Vouw een AD FS-farm met een extra AD FS-server na de eerste installatie. |
| [Een nieuwe AD FS WAP-server toevoegen](active-directory-aadconnect-federation-management.md#addwapserver) |Vouw een AD FS-farm met een extra Webtoepassingsproxy (WAP)-server na de eerste installatie. |
| [Een nieuwe federatieve domein toevoegen](active-directory-aadconnect-federation-management.md#addfeddomain) |Toevoegen van een ander domein dat gefedereerd met Azure AD. |
| [Het SSL-certificaat bijwerken](active-directory-aadconnectfed-ssl-update.md)| Werk het SSL-certificaat voor AD FS-farm. |
| [Federatiecertificaten vernieuwen voor Office 365 en Azure AD](active-directory-aadconnect-o365-certs.md)|Uw O365 certificaat vernieuwen met Azure AD.|
| **Andere federatieconfiguratie** | |
| [Meerdere exemplaren van Azure AD federeren met één exemplaar van AD FS](active-directory-aadconnectfed-single-adfs-multitenant-federation.md) | Federeren van meerdere Azure AD met één AD FS-farm| 
| [Een aangepaste bedrijfsportal logo/afbeelding toevoegen](active-directory-aadconnect-federation-management.md#customlogo) |De aanmeldingservaring wijzigen door op te geven van het aangepaste logo dat wordt weergegeven op de aanmeldingspagina van AD FS. |
| [Een beschrijving van de aanmeldingspagina toevoegen](active-directory-aadconnect-federation-management.md#addsignindescription) |Wijzig de omschrijving aanmelden op de aanmeldingspagina van AD FS. |
| [Wijzigen van de claimregels van AD FS](active-directory-aadconnect-federation-management.md#modclaims) |Wijzig of claimregels in AD FS die overeenkomen met het toevoegen aan Azure AD Connect sync-configuratie. |


## <a name="additional-resources"></a>Aanvullende bronnen
* [Federatie twee Azure AD met één AD FS](active-directory-aadconnectfed-single-adfs-multitenant-federation.md)
* [AD FS-implementatie in Azure](active-directory-aadconnect-azure-adfs.md)
* [Hoge beschikbaarheid cross-geografische AD FS-implementatie in Azure met Azure Traffic Manager](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)
