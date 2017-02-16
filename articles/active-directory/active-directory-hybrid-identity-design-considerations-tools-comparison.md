---
title: "Hybride identiteit: vergelijking van hulpprogramma’s voor directory-integratie | Microsoft Docs"
description: Deze pagina bevat een uitgebreide tabel waarin de verschillende hulpprogramma&quot;s voor directory-integratie worden vergeleken.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/14/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: facdbff229eaf5ee20d98214c62c8b2b46acc0fd
ms.openlocfilehash: 80617b203bf5efa4cd6bed590ea937b2e68db9e1


---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Vergelijking van hulpprogramma’s voor directory-integratie voor hybride identiteit
In de afgelopen jaren hebben de hulpprogramma's voor directory-integratie zich enorm ontwikkeld.  Dit document geeft een overzicht van deze hulpprogramma's en hierin worden de beschikbare functies in elk hulpprogramma vergeleken.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

> [!NOTE]
> Azure AD Connect bevat de onderdelen en functionaliteit die eerder zijn uitgebracht als Dirsync en AAD Sync. Deze hulpprogramma's worden niet meer afzonderlijk uitgebracht en alle toekomstige verbeteringen worden opgenomen in updates voor Azure AD Connect, zodat u altijd weet waar u de meest recente functionaliteit kunt ophalen.
> 
> DirSync en Azure AD Sync zijn afgeschaft. Meer informatie vindt u [hier](active-directory-aadconnect-dirsync-deprecated.md).
> 
> 

Gebruik de volgende sleutel voor elk van de tabellen.

● = Nu beschikbaar  
FR = Toekomstige release  
PP = Openbare preview  

## <a name="on-premises-to-cloud-synchronization"></a>Synchronisatie van on-premises naar cloud
| Functie | Azure Active Directory Connect | Azure Active Directory-synchronisatieservices (AAD Sync) | Hulpprogramma voor Azure Active Directory-synchronisatie (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Verbinding maken met één on-premises AD-forest |● |● |● |● |● |
| Verbinding maken met meerdere on-premises AD-forests |● |● | |● |● |
| Verbinding maken met meerdere on-premises Exchange-organisaties |● | | | | |
| Verbinding maken met één on-premises LDAP-adreslijst |FR | | |● |● |
| Verbinding maken met meerdere on-premises LDAP-adreslijsten |FR | | |● |● |
| Verbinding maken met on-premises AD- en on-premises LDAP-adreslijsten |FR | | |● |● |
| Verbinding maken met aangepaste systemen (SQL, Oracle, MySQL enzovoort) |FR | | |● |● |
| Door de klant gedefinieerde kenmerken synchroniseren (directory-extensies) |● | | | | |
| Verbinding maken met on-premises HR (SAP, Oracle eBusiness, PeopleSoft) |FR | | |● |● |
| Biedt ondersteuning voor FIM-synchronisatieregels en -connectors voor de inrichting van on-premises systemen. | | | |● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Synchronisatie van cloud naar on-premises
| Functie | Azure Active Directory Connect | Azure Active Directory-synchronisatieservices | Hulpprogramma voor Azure Active Directory-synchronisatie (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Write-back van apparaten |● | |● | | |
| Write-back van kenmerken (voor de hybride implementatie voor Exchange) |● |● |● |● |● |
| Write-back van groepsobjecten |● | | | | |
| Write-back van wachtwoorden (van de selfservice voor wachtwoordherstel (SSPR) en het wijzigen van wachtwoorden) |● |● | | | |

## <a name="authentication-feature-support"></a>Ondersteuning van verificatiefuncties
| Functie | Azure Active Directory Connect | Azure Active Directory-synchronisatieservices | Hulpprogramma voor Azure Active Directory-synchronisatie (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Wachtwoordsynchronisatie voor één on-premises AD-forest |● |● |● | | |
| Wachtwoordsynchronisatie voor meerdere on-premises AD-forests |● |● | | | |
| Eenmalige aanmelding met federatie |● |● |● |● |● |
| Write-back van wachtwoorden (van SSPR en het wijzigen van wachtwoorden) |● |● | | | |

## <a name="set-up-and-installation"></a>Instellingen en installatie
| Functie | Azure Active Directory Connect | Azure Active Directory-synchronisatieservices | Hulpprogramma voor Azure Active Directory-synchronisatie (DirSync) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|
| Biedt ondersteuning voor installatie op een domeincontroller |● |● |● | |
| Biedt ondersteuning voor installatie met SQL Express |● |● |● | |
| Eenvoudig upgraden van DirSync |● | | | |
| Lokalisatie van beheerder-UX naar Windows Server-talen |● |● |● | |
| Lokalisatie van eindgebruiker-UX naar Windows Server-talen | | | |● |
| Ondersteuning voor Windows Server 2008 en Windows Server 2008 R2 |● voor synchronisatie, niet voor federatie |● |● |● |
| Ondersteuning voor Windows Server 2012 en Windows Server 2012 R2 |● |● |● |● |

## <a name="filtering-and-configuration"></a>Filteren en configuratie
| Functie | Azure Active Directory Connect | Azure Active Directory-synchronisatieservices | Hulpprogramma voor Azure Active Directory-synchronisatie (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Filteren op domeinen en organisatie-eenheden |● |● |● |● |● |
| Filteren op kenmerkwaarden van objecten |● |● |● |● |● |
| Toestaan dat minimale set kenmerken worden gesynchroniseerd (MinSync) |● |● | | | |
| Toestaan dat verschillende servicesjablonen worden toegepast voor kenmerkstromen |● |● | | | |
| Toestaan dat het verwijderen van kenmerken van AD naar Azure AD stroomt |● |● | | | |
| Geavanceerd aanpassen voor kenmerkstromen toestaan |● |● | |● |● |

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).




<!--HONumber=Jan17_HO3-->


