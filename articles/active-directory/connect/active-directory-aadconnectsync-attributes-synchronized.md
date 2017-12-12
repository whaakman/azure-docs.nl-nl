---
title: Kenmerken die zijn gesynchroniseerd met Azure AD Connect | Microsoft Docs
description: Hier worden de kenmerken die zijn gesynchroniseerd met Azure Active Directory.
services: active-directory
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: c2bb36e0-5205-454c-b9b6-f4990bcedf51
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: 1fb5772f58511b33d6927c3d0ff155980ed756ad
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect-synchronisatie: kenmerken gesynchroniseerd naar Azure Active Directory
Dit onderwerp worden de kenmerken die worden gesynchroniseerd door Azure AD Connect-synchronisatie.  
De kenmerken zijn gegroepeerd op de bijbehorende Azure AD-app.

## <a name="attributes-to-synchronize"></a>Kenmerken synchroniseren
Een algemene vraag is *wat is de lijst met kenmerken synchroniseren*. De standaard- en aanbevolen aanpak is dat de standaardkenmerken zodat een volledige GAL (globale adreslijst) kan worden gemaakt in de cloud en ophalen van alle functies in Office 365-werkbelastingen. In sommige gevallen, er zijn enkele kenmerken die uw organisatie wil niet gesynchroniseerd met de cloud omdat deze kenmerken bevatten gevoelige of persoonlijke (persoonsgegevens) gegevens, zoals in dit voorbeeld:  
![ongeldige kenmerken](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

In dit geval beginnen met de lijst met kenmerken in dit onderwerp en identificeren van de kenmerken die gevoelige of persoonlijke gegevens bevatten zou en kunnen niet worden gesynchroniseerd. Deselecteer tijdens de installatie met deze kenmerken [Azure AD-app en kenmerkfilters](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

> [!WARNING]
> Wanneer kenmerken uitschakelt, moet u voorzichtig zijn en alleen Hef de selectie die kenmerken absoluut niet mogelijk om te synchroniseren. Selectie opheffen van andere kenmerken, kan een nadelige invloed op onderdelen hebben.
>
>

## <a name="office-365-proplus"></a>Office 365 ProPlus
| Kenmerknaam | Gebruiker | Opmerking |
| --- |:---:| --- |
| accountEnabled |X |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| algemene naam |X | |
| Weergavenaam |X | |
| objectSID |X |mechanische eigenschap. AD gebruikers-id gebruikt om te onderhouden synchronisatie tussen Azure AD en AD. |
| pwdLastSet |X |mechanische eigenschap. Gebruikt om te weten wanneer al uitgegeven tokens ongeldig maken. Door Wachtwoordsynchronisatie en Federatie gebruikt. |
| sourceAnchor |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| usageLocation |X |mechanische eigenschap. Het land van de gebruiker. Gebruikt voor de licentietoewijzing. |
| userPrincipalName |X |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="exchange-online"></a>Exchange Online
| Kenmerknaam | Gebruiker | Contact | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| assistent |X |X | | |
| altRecipient |X | | |Azure AD Connect build 1.1.552.0 vereist of na. |
| authOrig |X |X |X | |
| C |X |X | | |
| algemene naam |X | |X | |
| CO |X |X | | |
| Bedrijf |X |X | | |
| CountryCode |X |X | | |
| Afdeling |X |X | | |
| description |X |X |X | |
| Weergavenaam |X |X |X | |
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
| Voornaam |X |X | | |
| HomePhone |X |X | | |
| Info |X |X |X |Dit kenmerk is momenteel niet worden gebruikt voor groepen. |
| Intialen |X |X | | |
| L |X |X | | |
| LegacyExchangeDN |X |X |X | |
| mailNickname |X |X |X | |
| Door | | |X | |
| Manager |X |X | | |
| Lid | | |X | |
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
| msExchBypassModerationLink | | |X |Beschikbaar in Azure AD Connect versie 1.1.524.0 |
| msExchCoManagedByLink | | |X | |
| msExchDelegateListLink |X | | | |
| msExchELCExpirySuspensionEnd |X | | | |
| msExchELCExpirySuspensionStart |X | | | |
| msExchELCMailboxFlags |X | | | |
| msExchEnableModeration |X | |X | |
| msExchExtensionCustomAttribute1 |X |X |X |Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute2 |X |X |X |Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute3 |X |X |X |Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute4 |X |X |X |Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
| msExchExtensionCustomAttribute5 |X |X |X |Dit kenmerk is momenteel niet worden gebruikt door Exchange Online. |
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
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt om te onderhouden synchronisatie tussen Azure AD en AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherTelephone |X |X | | |
| Pager |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postcode |X |X | | |
| proxyAddresses |X |X |X | |
| publicDelegates |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer al uitgegeven tokens ongeldig maken. Door Wachtwoordsynchronisatie en Federatie gebruikt. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| securityenabled moet | | |X |Afgeleid van groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| St |X |X | | |
| StreetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| titel |X |X | | |
| unauthOrig |X |X |X | |
| usageLocation |X | | |mechanische eigenschap. Het land van de gebruiker. Gebruikt voor de licentietoewijzing. |
| userCertificate |X |X | | |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |
| userSMIMECertificates |X |X | | |
| wWWHomePage |X |X | | |

## <a name="sharepoint-online"></a>SharePoint Online
| Kenmerknaam | Gebruiker | Contact | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| authOrig |X |X |X | |
| C |X |X | | |
| algemene naam |X | |X | |
| CO |X |X | | |
| Bedrijf |X |X | | |
| CountryCode |X |X | | |
| Afdeling |X |X | | |
| description |X |X |X | |
| Weergavenaam |X |X |X | |
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
| Voornaam |X |X | | |
| hideDLMembership | | |X | |
| homephone |X |X | | |
| Info |X |X |X | |
| initialen |X |X | | |
| ipPhone |X |X | | |
| L |X |X | | |
| E-mail |X |X |X | |
| mailnickname |X |X |X | |
| Door | | |X | |
| Manager |X |X | | |
| Lid | | |X | |
| Afzonderlijk |X |X | | |
| mobiele |X |X | | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxOwners |X | | | |
| msExchTeamMailboxSharePointLinkedBy |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt om te onderhouden synchronisatie tussen Azure AD en AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherIpPhone |X |X | | |
| OtherMobile |X |X | | |
| otherPager |X |X | | |
| otherTelephone |X |X | | |
| Pager |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postcode |X |X | | |
| postOfficeBox |X |X | |Dit kenmerk is momenteel niet worden gebruikt door SharePoint Online. |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer al uitgegeven tokens ongeldig maken. Door Wachtwoordsynchronisatie en Federatie gebruikt. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| securityenabled moet | | |X |Afgeleid van groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| St |X |X | | |
| StreetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| titel |X |X | | |
| unauthOrig |X |X |X | |
| url |X |X | | |
| usageLocation |X | | |mechanische eigenschap. Het land van de gebruiker. Gebruikt voor de licentietoewijzing. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |
| wWWHomePage |X |X | | |

## <a name="lync-online-subsequently-known-as-skype-for-business"></a>Lync Online (vervolgens bekend als Skype voor bedrijven)
| Kenmerknaam | Gebruiker | Contact | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| C |X |X | | |
| algemene naam |X | |X | |
| CO |X |X | | |
| Bedrijf |X |X | | |
| Afdeling |X |X | | |
| description |X |X |X | |
| Weergavenaam |X |X |X | |
| facsimiletelephonenumber |X |X |X | |
| Voornaam |X |X | | |
| homephone |X |X | | |
| ipPhone |X |X | | |
| L |X |X | | |
| E-mail |X |X |X | |
| mailNickname |X |X |X | |
| Door | | |X | |
| Manager |X |X | | |
| Lid | | |X | |
| mobiele |X |X | | |
| msExchHideFromAddressLists |X |X |X | |
| msRTCSIP-ApplicationOptions |X | | | |
| msRTCSIP-DeploymentLocator |X |X | | |
| msRTCSIP-Line |X |X | | |
| msRTCSIP-OptionFlags |X |X | | |
| msRTCSIP-OwnerUrn |X | | | |
| msRTCSIP-PrimaryUserAddress |X |X | | |
| msRTCSIP-UserEnabled |X |X | | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt om te onderhouden synchronisatie tussen Azure AD en AD. |
| otherTelephone |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postcode |X |X | | |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer al uitgegeven tokens ongeldig maken. Door Wachtwoordsynchronisatie en Federatie gebruikt. |
| securityenabled moet | | |X |Afgeleid van groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| St |X |X | | |
| StreetAddress |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| titel |X |X | | |
| usageLocation |X | | |mechanische eigenschap. Het land van de gebruiker. Gebruikt voor de licentietoewijzing. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |
| wWWHomePage |X |X | | |

## <a name="azure-rms"></a>Azure RMS
| Kenmerknaam | Gebruiker | Contact | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| algemene naam |X | |X |Algemene naam of alias. Meestal het voorvoegsel van de waarde [e]. |
| Weergavenaam |X |X |X |Een tekenreeks met de naam die vaak worden weergegeven als de beschrijvende naam (voornaam achternaam). |
| E-mail |X |X |X |volledige e-mailadres. |
| Lid | | |X | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt om te onderhouden synchronisatie tussen Azure AD en AD. |
| proxyAddresses |X |X |X |mechanische eigenschap. Gebruikt door Azure AD. Bevat alle secundaire e-mailadressen op voor de gebruiker. |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer al uitgegeven tokens ongeldig maken. |
| securityenabled moet | | |X |GroupType is afgeleid. |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| usageLocation |X | | |mechanische eigenschap. Het land van de gebruiker. Gebruikt voor de licentietoewijzing. |
| userPrincipalName |X | | |De UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="intune"></a>Intune
| Kenmerknaam | Gebruiker | Contact | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| C |X |X | | |
| algemene naam |X | |X | |
| description |X |X |X | |
| Weergavenaam |X |X |X | |
| E-mail |X |X |X | |
| mailnickname |X |X |X | |
| Lid | | |X | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt om te onderhouden synchronisatie tussen Azure AD en AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer al uitgegeven tokens ongeldig maken. Door Wachtwoordsynchronisatie en Federatie gebruikt. |
| securityenabled moet | | |X |Afgeleid van groupType |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| usageLocation |X | | |mechanische eigenschap. Het land van de gebruiker. Gebruikt voor de licentietoewijzing. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="dynamics-crm"></a>Dynamics CRM
| Kenmerknaam | Gebruiker | Contact | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| C |X |X | | |
| algemene naam |X | |X | |
| CO |X |X | | |
| Bedrijf |X |X | | |
| CountryCode |X |X | | |
| description |X |X |X | |
| Weergavenaam |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| Voornaam |X |X | | |
| L |X |X | | |
| Door | | |X | |
| Manager |X |X | | |
| Lid | | |X | |
| mobiele |X |X | | |
| objectSID |X | |X |mechanische eigenschap. AD gebruikers-id gebruikt om te onderhouden synchronisatie tussen Azure AD en AD. |
| physicalDeliveryOfficeName |X |X | | |
| Postcode |X |X | | |
| preferredLanguage |X | | | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer al uitgegeven tokens ongeldig maken. Door Wachtwoordsynchronisatie en Federatie gebruikt. |
| securityenabled moet | | |X |Afgeleid van groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| St |X |X | | |
| StreetAddress |X |X | | |
| telephoneNumber |X |X | | |
| titel |X |X | | |
| usageLocation |X | | |mechanische eigenschap. Het land van de gebruiker. Gebruikt voor de licentietoewijzing. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="3rd-party-applications"></a>3e toepassingen van derden
Deze groep is een set kenmerken die worden gebruikt als de minimale kenmerken die nodig zijn voor een algemene werkbelasting of toepassing. Het kan worden gebruikt voor een werkbelasting die niet wordt vermeld in een andere sectie of voor een niet-Microsoft-app. Expliciet wordt deze gebruikt voor het volgende:

* Yammer (alleen gebruiker wordt gebruikt)
* [Hybride Business-to-Business (B2B) cross-org samenwerking scenario's die worden aangeboden door bronnen zoals SharePoint](http://go.microsoft.com/fwlink/?LinkId=747036)

Deze groep is een set kenmerken die kunnen worden gebruikt als de Azure AD-directory niet gebruikt wordt ter ondersteuning van Office 365, Dynamics of Intune. Er is een klein aantal belangrijke kenmerken.

| Kenmerknaam | Gebruiker | Contact | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Hiermee worden gedefinieerd als een account is ingeschakeld. |
| algemene naam |X | |X | |
| Weergavenaam |X |X |X | |
| Voornaam |X |X | | |
| E-mail |X | |X | |
| Door | | |X | |
| mailNickName |X |X |X | |
| Lid | | |X | |
| objectSID |X | | |mechanische eigenschap. AD gebruikers-id gebruikt om te onderhouden synchronisatie tussen Azure AD en AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mechanische eigenschap. Gebruikt om te weten wanneer al uitgegeven tokens ongeldig maken. Door Wachtwoordsynchronisatie en Federatie gebruikt. |
| SN |X |X | | |
| sourceAnchor |X |X |X |mechanische eigenschap. Onveranderbare id voor het onderhouden van de relatie tussen ADDS en Azure AD. |
| usageLocation |X | | |mechanische eigenschap. Het land van de gebruiker. Gebruikt voor de licentietoewijzing. |
| userPrincipalName |X | | |UPN is de aanmeldings-ID voor de gebruiker. Meestal als [e] dezelfde waarde. |

## <a name="windows-10"></a>Windows 10
Een Windows 10 domein computer(device) worden enkele kenmerken naar Azure AD gesynchroniseerd. Zie voor meer informatie over de scenario's [domein apparaten verbinden met Azure AD voor Windows 10 optreedt](../active-directory-azureadjoin-devices-group-policy.md). Deze kenmerken altijd te synchroniseren en Windows 10 wordt niet weergegeven als een app die kunt u het vakje. Een computer voor het domein van Windows 10 wordt geïdentificeerd door met het kenmerk userCertificate ingevuld.

| Kenmerknaam | Apparaat | Opmerking |
| --- |:---:| --- |
| accountEnabled |X | |
| deviceTrustType |X |De waarde van de hardcoded voor domeincomputers. |
| Weergavenaam |X | |
| DS-MS-CreatorSID |X |Ook wel registeredOwnerReference genoemd. |
| objectGUID |X |Ook wel de apparaat-id genoemd. |
| objectSID |X |Ook wel onPremisesSecurityIdentifier genoemd. |
| operatingSystem |X |Ook wel deviceOSType genoemd. |
| operatingSystemVersion |X |Ook wel deviceOSVersion genoemd. |
| userCertificate |X | |

Deze kenmerken voor **gebruiker** worden naast de andere apps die u hebt geselecteerd.  

| Kenmerknaam | Gebruiker | Opmerking |
| --- |:---:| --- |
| domainFQDN |X |Ook wel een DNS-domeinnaam genoemd. Bijvoorbeeld contoso.com. |
| domainNetBios |X |Ook wel netBiosName genoemd. Bijvoorbeeld CONTOSO. |

## <a name="exchange-hybrid-writeback"></a>Exchange hybride Write-back
Deze kenmerken worden teruggeschreven van Azure AD naar lokale Active Directory wanneer u selecteert om in te schakelen **Exchange hybride**. Afhankelijk van uw versie van Exchange mogelijk minder kenmerken worden gesynchroniseerd.

| Kenmerknaam | Gebruiker | Contact | Groep | Opmerking |
| --- |:---:|:---:|:---:| --- |
| msDS-ExternalDirectoryObjectID |X | | |Afgeleid van cloudAnchor in Azure AD. Dit kenmerk is nieuw in Exchange 2016 en Windows Server 2016 AD. |
| msExchArchiveStatus |X | | |Online archief: Kunnen klanten bij de archivering van e-mail. |
| msExchBlockedSendersHash |X | | |Filteren: Schrijft terug on-premises filteren en online veilige en geblokkeerde afzender gegevens van clients. |
| msExchSafeRecipientsHash |X | | |Filteren: Schrijft terug on-premises filteren en online veilige en geblokkeerde afzender gegevens van clients. |
| msExchSafeSendersHash |X | | |Filteren: Schrijft terug on-premises filteren en online veilige en geblokkeerde afzender gegevens van clients. |
| msExchUCVoiceMailSettings |X | | |Unified Messaging (UM) - Online voicemail inschakelen: gebruikt door Microsoft Lync Server integratie om aan te geven voor Lync Server lokale dat de gebruiker voicemail in onlineservices heeft. |
| msExchUserHoldPolicies |X | | |Geschil wachtstand: Hiermee cloudservices om te bepalen welke gebruikers zijn onder geschil houdt. |
| proxyAddresses |X |X |X |Alleen de x500-adres van Exchange Online wordt ingevoegd. |
| publicDelegates |X | | |Hiermee kunt een Exchange Online-postvak om te worden toegekend SendOnBehalfTo rechten aan gebruikers met lokale Exchange-postvak. Azure AD Connect build 1.1.552.0 vereist of na. |

## <a name="exchange-mail-public-folder"></a>Exchange Mail openbare map
Deze kenmerken worden gesynchroniseerd vanuit de lokale Active Directory naar Azure AD wanneer u selecteert om in te schakelen **Exchange Mail openbare map**.

| Kenmerknaam | PublicFolder | Opmerking |
| --- | :---:| --- |
| Weergavenaam | X |  |
| E-mail | X |  |
| msExchRecipientTypeDetails | X |  |
| objectGUID | X |  |
| proxyAddresses | X |  |
| targetAddress | X |  |

## <a name="device-writeback"></a>Apparaat terugschrijven
Apparaatobjecten zijn gemaakt in Active Directory. Deze objecten kunnen worden apparaten die zijn gekoppeld aan Azure AD of domein Windows 10-computers.

| Kenmerknaam | Apparaat | Opmerking |
| --- |:---:| --- |
| altSecurityIdentities |X | |
| Weergavenaam |X | |
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
* Wanneer u een alternatieve ID, wordt het lokale kenmerk userPrincipalName is gesynchroniseerd met de Azure AD-kenmerk onPremisesUserPrincipalName. Het kenmerk alternatieve ID van de voorbeeld-mailserver is gesynchroniseerd met het kenmerk userPrincipalName van Azure AD.
* In de lijsten boven het objecttype **gebruiker** geldt ook voor het objecttype **iNetOrgPerson**.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
