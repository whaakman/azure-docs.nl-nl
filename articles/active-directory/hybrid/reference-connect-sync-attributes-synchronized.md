---
title: Kenmerken gesynchroniseerd met Azure AD Connect | Microsoft Docs
description: Een lijst met kenmerken die zijn gesynchroniseerd met Azure Active Directory.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: c2bb36e0-5205-454c-b9b6-f4990bcedf51
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 08633b52e6b7cfef28635925ad9fcf34e065e9bf
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251795"
---
# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect-synchronisatie: kenmerken gesynchroniseerd naar Azure Active Directory
In dit onderwerp worden de kenmerken die door Azure AD Connect-synchronisatie worden gesynchroniseerd.  
De kenmerken zijn gegroepeerd op de bijbehorende Azure AD-app.

## <a name="attributes-to-synchronize"></a>Kenmerken worden gesynchroniseerd
Een algemene vraag is *wat is de lijst met kenmerken synchroniseren*. De standaardinstellingen en de aanbevolen aanpak is om de standaardkenmerken houden, zodat een volledige GAL (globale adreslijst) kan worden gemaakt in de cloud en om op te halen van alle functies in Office 365-werkbelastingen. In sommige gevallen, er zijn enkele kenmerken die uw organisatie wil niet gesynchroniseerd naar de cloud omdat deze kenmerken bevat gevoelige of PII (persoonlijk identificeerbare informatie) gegevens, net zoals in dit voorbeeld:  
![ongeldige kenmerken](./media/reference-connect-sync-attributes-synchronized/badextensionattribute.png)

In dit geval beginnen met de lijst van kenmerken in dit onderwerp en identificeren van de kenmerken die gevoelige of persoonlijke gegevens bevatten zou en kunnen niet worden gesynchroniseerd. Schakelt u deze kenmerken tijdens de installatie met [Azure AD-app en kenmerkfilters](how-to-connect-install-custom.md#azure-ad-app-and-attribute-filtering).

> [!WARNING]
> Wanneer als kenmerken uitschakelt, moet u voorzichtig zijn en schakel alleen de kenmerken die helemaal niet mogelijk om te synchroniseren. Selectie opheffen van andere kenmerken, mogelijk een nadelige invloed hebben op functies.
>
>

## <a name="office-365-proplus"></a>Office 365 ProPlus
| Naam kenmerk | Gebruiker | Opmerking |
| --- |:---:| --- |
| accountEnabled |X |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| algemene naam |X | |
| displayName |X | |
| objectSID |X |mechanische eigenschap. AD gebruikers-id gebruikt voor het onderhouden van de synchronisatie tussen Azure AD en AD. |
| pwdLastSet |X |mechanische eigenschap. Gebruikt om te weten wanneer u al uitgegeven tokens ongeldig te maken. Gebruikt door de synchronisatie van wachtwoordhashes, pass-through-verificatie en Federatie. |
| sourceAnchor |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| usageLocation |X |mechanische eigenschap. Land van de gebruiker. Gebruikt voor het toewijzen van licenties. |
| userPrincipalName |X |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="exchange-online"></a>Exchange Online
| Naam kenmerk | Gebruiker | Contactpersoon | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| assistent |X |X | | |
| altRecipient |X | | |Azure AD Connect build 1.1.552.0 vereist of na. |
| authOrig |X |X |X | |
| c |X |X | | |
| algemene naam |X | |X | |
| CO |X |X | | |
| Bedrijf |X |X | | |
| countryCode |X |X | | |
| Afdeling |X |X | | |
| description |X |X |X | |
| displayName |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| extensionAttribute10 |X |X |X | |
| extensionAttribute11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| extensionAttribute13 |X |X |X | |
| extensionAttribute14 |X |X |X | |
| extensionAttribute15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| homePhone |X |X | | |
| informatie |X |X |X |Dit kenmerk is momenteel niet gebruikt voor groepen. |
| Intialen |X |X | | |
| l |X |X | | |
| legacyExchangeDN |X |X |X | |
| mailNickname |X |X |X | |
| beheerdDoor | | |X | |
| beheerder |X |X | | |
| lid | | |X | |
| mobiele |X |X | | |
| msDS-HABSeniorityIndex |X |X |X | |
| msDS-PhoneticDisplayName |X |X |X | |
| msExchArchiveGUID |X | | | |
| msExchArchiveName |X | | | |
| msExchAssistantName |X |X | | |
| msExchAuditAdmin |X | | | |
| msExchAuditDelegate |X | | | |
| msExchAuditDelegateAdmin |X | | | |
| msExchAuditOwner |X | | | |
| msExchBlockedSendersHash |X |X | | |
| msExchBypassAudit |X | | | |
| msExchBypassModerationLink | | |X |Beschikbaar in Azure AD Connect versie 1.1.524.0 en hoger |
| msExchCoManagedByLink | | |X | |
| msExchDelegateListLink |X | | | |
| msExchELCExpirySuspensionEnd |X | | | |
| msExchELCExpirySuspensionStart |X | | | |
| msExchELCMailboxFlags |X | | | |
| msExchEnableModeration |X | |X | |
| msExchExtensionCustomAttribute1 |X |X |X |Dit kenmerk is momenteel niet gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute2 |X |X |X |Dit kenmerk is momenteel niet gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute3 |X |X |X |Dit kenmerk is momenteel niet gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute4 |X |X |X |Dit kenmerk is momenteel niet gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute5 |X |X |X |Dit kenmerk is momenteel niet gebruikt door Exchange Online. |
| msExchHideFromAddressLists |X |X |X | |
| msExchImmutableID |X | | | |
| msExchLitigationHoldDate |X |X |X | |
| msExchLitigationHoldOwner |X |X |X | |
| msExchMailboxAuditEnable |X | | | |
| msExchMailboxAuditLogAgeLimit |X | | | |
| msExchMailboxGuid |X | | | |
| msExchModeratedByLink |X |X |X | |
| msExchModerationFlags |X |X |X | |
| msExchRecipientDisplayType |X |X |X | |
| msExchRecipientTypeDetails |X |X |X | |
| msExchRemoteRecipientType |X | | | |
| msExchRequireAuthToSendTo |X |X |X | |
| msExchResourceCapacity |X | | | |
| msExchResourceDisplay |X | | | |
| msExchResourceMetaData |X | | | |
| msExchResourceSearchProperties |X | | | |
| msExchRetentionComment |X |X |X | |
| msExchRetentionURL |X |X |X | |
| msExchSafeRecipientsHash |X |X | | |
| msExchSafeSendersHash |X |X | | |
| msExchSenderHintTranslations |X |X |X | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxOwners |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| msExchUserHoldPolicies |X | | | |
| msOrg IsOrganizational | | |X | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt voor het onderhouden van de synchronisatie tussen Azure AD en AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherTelephone |X |X | | |
| Pager |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postcode |X |X | | |
| proxyAddresses |X |X |X | |
| publicDelegates |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer u al uitgegeven tokens ongeldig te maken. Gebruikt door zowel Wachtwoordsynchronisatie en Federatie. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| securityEnabled | | |X |Afgeleid van groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| St |X |X | | |
| streetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| titel |X |X | | |
| unauthOrig |X |X |X | |
| usageLocation |X | | |mechanische eigenschap. Land van de gebruiker. Gebruikt voor het toewijzen van licenties. |
| userCertificate |X |X | | |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |
| userSMIMECertificates |X |X | | |
| wWWHomePage |X |X | | |

## <a name="sharepoint-online"></a>SharePoint Online
| Naam kenmerk | Gebruiker | Contactpersoon | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| authOrig |X |X |X | |
| c |X |X | | |
| algemene naam |X | |X | |
| CO |X |X | | |
| Bedrijf |X |X | | |
| countryCode |X |X | | |
| Afdeling |X |X | | |
| description |X |X |X | |
| displayName |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| extensionAttribute10 |X |X |X | |
| extensionAttribute11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| extensionAttribute13 |X |X |X | |
| extensionAttribute14 |X |X |X | |
| extensionAttribute15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| hideDLMembership | | |X | |
| homephone |X |X | | |
| informatie |X |X |X | |
| De initialen |X |X | | |
| ipPhone |X |X | | |
| l |X |X | | |
| mail |X |X |X | |
| mailnickname |X |X |X | |
| beheerdDoor | | |X | |
| beheerder |X |X | | |
| lid | | |X | |
| Afzonderlijk |X |X | | |
| mobiele |X |X | | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxOwners |X | | | |
| msExchTeamMailboxSharePointLinkedBy |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt voor het onderhouden van de synchronisatie tussen Azure AD en AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherIpPhone |X |X | | |
| otherMobile |X |X | | |
| otherPager |X |X | | |
| otherTelephone |X |X | | |
| Pager |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postcode |X |X | | |
| postOfficeBox |X |X | |Dit kenmerk is momenteel niet gebruikt door SharePoint Online. |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer u al uitgegeven tokens ongeldig te maken. Gebruikt door de synchronisatie van wachtwoordhashes, pass-through-verificatie en Federatie. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| securityEnabled | | |X |Afgeleid van groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| St |X |X | | |
| streetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| titel |X |X | | |
| unauthOrig |X |X |X | |
| url |X |X | | |
| usageLocation |X | | |mechanische eigenschap. Land van de gebruiker. Gebruikt voor het toewijzen van licenties. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |
| wWWHomePage |X |X | | |

## <a name="lync-online-subsequently-known-as-skype-for-business"></a>Lync Online (vervolgens bekend als Skype voor bedrijven)
| Naam kenmerk | Gebruiker | Contactpersoon | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| c |X |X | | |
| algemene naam |X | |X | |
| CO |X |X | | |
| Bedrijf |X |X | | |
| Afdeling |X |X | | |
| description |X |X |X | |
| displayName |X |X |X | |
| facsimiletelephonenumber |X |X |X | |
| givenName |X |X | | |
| homephone |X |X | | |
| ipPhone |X |X | | |
| l |X |X | | |
| mail |X |X |X | |
| mailNickname |X |X |X | |
| beheerdDoor | | |X | |
| beheerder |X |X | | |
| lid | | |X | |
| mobiele |X |X | | |
| msExchHideFromAddressLists |X |X |X | |
| msRTCSIP-ApplicationOptions |X | | | |
| msRTCSIP-DeploymentLocator |X |X | | |
| msRTCSIP-Line |X |X | | |
| msRTCSIP-OptionFlags |X |X | | |
| msRTCSIP-OwnerUrn |X | | | |
| msRTCSIP-PrimaryUserAddress |X |X | | |
| msRTCSIP-UserEnabled |X |X | | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt voor het onderhouden van de synchronisatie tussen Azure AD en AD. |
| otherTelephone |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postcode |X |X | | |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer u al uitgegeven tokens ongeldig te maken. Gebruikt door de synchronisatie van wachtwoordhashes, pass-through-verificatie en Federatie. |
| securityEnabled | | |X |Afgeleid van groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| St |X |X | | |
| streetAddress |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| titel |X |X | | |
| usageLocation |X | | |mechanische eigenschap. Land van de gebruiker. Gebruikt voor het toewijzen van licenties. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |
| wWWHomePage |X |X | | |

## <a name="azure-rms"></a>Azure RMS
| Naam kenmerk | Gebruiker | Contactpersoon | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| algemene naam |X | |X |Algemene naam of alias. Meestal is dit het voorvoegsel van de waarde [e]. |
| displayName |X |X |X |Een tekenreeks met de naam die vaak worden weergegeven als de beschrijvende naam (voornaam achternaam). |
| mail |X |X |X |volledige e-mailadres. |
| lid | | |X | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt voor het onderhouden van de synchronisatie tussen Azure AD en AD. |
| proxyAddresses |X |X |X |mechanische eigenschap. Gebruikt door Azure AD. Bevat alle secundaire e-mailadressen van de gebruiker. |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer u al uitgegeven tokens ongeldig te maken. |
| securityEnabled | | |X |GroupType hiervan is afgeleid. |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| usageLocation |X | | |mechanische eigenschap. Land van de gebruiker. Gebruikt voor het toewijzen van licenties. |
| userPrincipalName |X | | |De UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="intune"></a>Intune
| Naam kenmerk | Gebruiker | Contactpersoon | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| c |X |X | | |
| algemene naam |X | |X | |
| description |X |X |X | |
| displayName |X |X |X | |
| mail |X |X |X | |
| mailnickname |X |X |X | |
| lid | | |X | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt voor het onderhouden van de synchronisatie tussen Azure AD en AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer u al uitgegeven tokens ongeldig te maken. Gebruikt door de synchronisatie van wachtwoordhashes, pass-through-verificatie en Federatie. |
| securityEnabled | | |X |Afgeleid van groupType |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| usageLocation |X | | |mechanische eigenschap. Land van de gebruiker. Gebruikt voor het toewijzen van licenties. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="dynamics-crm"></a>Dynamics CRM
| Naam kenmerk | Gebruiker | Contactpersoon | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| c |X |X | | |
| algemene naam |X | |X | |
| CO |X |X | | |
| Bedrijf |X |X | | |
| countryCode |X |X | | |
| description |X |X |X | |
| displayName |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| l |X |X | | |
| beheerdDoor | | |X | |
| beheerder |X |X | | |
| lid | | |X | |
| mobiele |X |X | | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt voor het onderhouden van de synchronisatie tussen Azure AD en AD. |
| physicalDeliveryOfficeName |X |X | | |
| Postcode |X |X | | |
| preferredLanguage |X | | | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer u al uitgegeven tokens ongeldig te maken. Gebruikt door de synchronisatie van wachtwoordhashes, pass-through-verificatie en Federatie. |
| securityEnabled | | |X |Afgeleid van groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| St |X |X | | |
| streetAddress |X |X | | |
| telephoneNumber |X |X | | |
| titel |X |X | | |
| usageLocation |X | | |mechanische eigenschap. Land van de gebruiker. Gebruikt voor het toewijzen van licenties. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="3rd-party-applications"></a>3e toepassingen van derden
Deze groep is een set kenmerken die worden gebruikt als de minimale kenmerken die nodig zijn voor een algemene workload of toepassing. Het kan worden gebruikt voor een werkbelasting niet wordt vermeld in een andere sectie of voor een niet-Microsoft-app. Dit expliciet wordt gebruikt voor het volgende:

* Yammer (alleen gebruiker wordt gebruikt)
* [Hybride Business-to-Business (B2B) organisatieoverstijgende samenwerking scenario's die worden aangeboden door resources, zoals SharePoint](https://go.microsoft.com/fwlink/?LinkId=747036)

Deze groep is een set kenmerken die kunnen worden gebruikt als de Azure AD-directory niet gebruikt wordt ter ondersteuning van Office 365, Dynamics of Intune. Er is een klein aantal belangrijke kenmerken.

| Naam kenmerk | Gebruiker | Contactpersoon | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| algemene naam |X | |X | |
| displayName |X |X |X | |
| givenName |X |X | | |
| mail |X | |X | |
| beheerdDoor | | |X | |
| mailNickName |X |X |X | |
| lid | | |X | |
| objectSID |X | | |mechanische eigenschap. AD gebruikers-id gebruikt voor het onderhouden van de synchronisatie tussen Azure AD en AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer u al uitgegeven tokens ongeldig te maken. Gebruikt door de synchronisatie van wachtwoordhashes, pass-through-verificatie en Federatie. |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| usageLocation |X | | |mechanische eigenschap. Land van de gebruiker. Gebruikt voor het toewijzen van licenties. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="windows-10"></a>Windows 10
Een Windows 10 domein computer(device) worden enkele kenmerken naar Azure AD gesynchroniseerd. Zie voor meer informatie over de scenario's [domein apparaten verbinden met Azure AD voor Windows 10-ervaringen](../active-directory-azureadjoin-devices-group-policy.md). Deze kenmerken altijd te synchroniseren en Windows 10 wordt niet weergegeven als een app die u de selectie. Een computer met Windows 10 domein wordt geïdentificeerd door het kenmerk userCertificate ingevuld.

| Naam kenmerk | Apparaat | Opmerking |
| --- |:---:| --- |
| accountEnabled |X | |
| deviceTrustType |X |De waarde van de vastgelegd voor domeincomputers. |
| displayName |X | |
| ms-DS-CreatorSID |X |Ook wel registeredOwnerReference genoemd. |
| objectGUID |X |Ook wel de apparaat-id genoemd. |
| objectSID |X |Ook wel onPremisesSecurityIdentifier genoemd. |
| het besturingssysteem |X |Ook wel deviceOSType genoemd. |
| operatingSystemVersion |X |Ook wel deviceOSVersion genoemd. |
| userCertificate |X | |

Deze kenmerken voor **gebruiker** worden naast de andere apps die u hebt geselecteerd.  

| Naam kenmerk | Gebruiker | Opmerking |
| --- |:---:| --- |
| domainFQDN |X |Ook wel een DNS-domeinnaam genoemd. Bijvoorbeeld contoso.com. |
| domainNetBios |X |Ook wel een NetBIOS-naam genoemd. Bijvoorbeeld: CONTOSO. |
| msDS-KeyCredentialLink |X |Zodra de gebruiker is ingeschreven in Windows Hello voor bedrijven. | 

## <a name="exchange-hybrid-writeback"></a>Exchange hybrid Write-back
Deze kenmerken worden teruggeschreven van Azure AD naar on-premises Active Directory wanneer u selecteert om in te schakelen **Exchange hybride**. Afhankelijk van uw versie van Exchange mogelijk minder kenmerken worden gesynchroniseerd.

| De naam van het kenmerk (verbinding maken met de gebruikersinterface) |De naam van het kenmerk (On-premises AD) | Gebruiker | Contactpersoon | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |---|
| msDS-ExternalDirectoryObjectID| ms-DS-External-Directory-Object-Id |X | | |Afgeleid van de waarde voor cloudAnchor in Azure AD. Dit kenmerk is er nieuw in Exchange 2016 en Windows Server 2016 AD. |
| msExchArchiveStatus| ms-Exch-ArchiveStatus |X | | |Online archief: Kunnen klanten voor het archiveren van e-mail. |
| msExchBlockedSendersHash| ms-Exch-BlockedSendersHash |X | | |Filteren: Schrijft terug on-premises filteren en online veilige en geblokkeerde afzender gegevens van clients. |
| msExchSafeRecipientsHash| ms-Exch-SafeRecipientsHash  |X | | |Filteren: Schrijft terug on-premises filteren en online veilige en geblokkeerde afzender gegevens van clients. |
| msExchSafeSendersHash| ms-Exch-SafeSendersHash  |X | | |Filteren: Schrijft terug on-premises filteren en online veilige en geblokkeerde afzender gegevens van clients. |
| msExchUCVoiceMailSettings| ms-Exch-UCVoiceMailSettings |X | | |Inschakelen van Unified Messaging (UM) - Online voicemail: gebruikt door Microsoft Lync Server integratie om aan te geven voor Lync Server ter plaatse dat de gebruiker voicemail in de online services heeft. |
| msExchUserHoldPolicies| ms-Exc-hUserHoldPolicies |X | | |Rechtszaak wachtstand: Hiermee cloudservices om te bepalen welke gebruikers zijn onder gerechtelijke procedures bevatten. |
| proxyAddresses| proxyAddresses |X |X |X |Alleen de x500 adres van Exchange Online wordt ingevoegd. |
| publicDelegates| ms-Exch-Public-Delegates  |X | | |Hiermee kunt een Exchange Online-postbus SendOnBehalfTo rechten voor gebruikers met een on-premises Exchange-postvak worden verleend. Azure AD Connect build 1.1.552.0 vereist of na. |

## <a name="exchange-mail-public-folder"></a>Openbare map voor Exchange-e-Mail
Deze kenmerken worden gesynchroniseerd vanuit Active Directory on-premises naar Azure AD wanneer u selecteert om in te schakelen **Exchange Mail openbare map**.

| Naam kenmerk | PublicFolder | Opmerking |
| --- | :---:| --- |
| displayName | X |  |
| mail | X |  |
| msExchRecipientTypeDetails | X |  |
| objectGUID | X |  |
| proxyAddresses | X |  |
| targetAddress | X |  |

## <a name="device-writeback"></a>Apparaat terugschrijven
Apparaatobjecten zijn gemaakt in Active Directory. Deze objecten kunnen worden toegevoegd aan Azure AD. apparaten of computers met Windows 10 domein.

| Naam kenmerk | Apparaat | Opmerking |
| --- |:---:| --- |
| altSecurityIdentities |X | |
| displayName |X | |
| DN-naam |X | |
| msDS-CloudAnchor |X | |
| msDS-apparaat-id |X | |
| msDS-DeviceObjectVersion |X | |
| msDS-DeviceOSType |X | |
| msDS-DeviceOSVersion |X | |
| msDS-DevicePhysicalIDs |X | |
| msDS-KeyCredentialLink |X |Alleen met Windows Server 2016 AD-schema |
| msDS-IsCompliant |X | |
| msDS-IsEnabled |X | |
| msDS-IsManaged |X | |
| msDS-RegisteredOwner |X | |

## <a name="notes"></a>Opmerkingen
* Wanneer u een alternatieve ID gebruikt, wordt de on-premises kenmerk userPrincipalName is gesynchroniseerd met de Azure AD-kenmerk onPremisesUserPrincipalName. Het kenmerk alternatieve ID voor een voorbeeld van de e-mail wordt gesynchroniseerd met de Azure AD-kenmerk userPrincipalName.
* In de lijsten boven het objecttype **gebruiker** ook van toepassing op het objecttype **iNetOrgPerson**.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](whatis-hybrid-identity.md).
