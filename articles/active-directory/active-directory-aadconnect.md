---
title: 'Azure AD Connect: uw on-premises identiteiten integreren met Azure Active Directory. | Microsoft Docs'
description: "Azure AD Connect integreert uw on-premises adreslijsten met Azure Active Directory. Hiermee kunt u een algemene identiteit bieden voor Office 365-, Azure- en SaaS-toepassingen die zijn geïntegreerd met Azure AD."
keywords: inleiding tot Azure AD Connect, overzicht Azure AD Connect, wat is Azure AD Connect, Active Directory installeren
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/04/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: eedb788b2a174d01a2ef661cf4093ff938649bce


---
# <a name="integrating-your-onpremises-identities-with-azure-active-directory"></a>Uw on-premises identiteiten integreren met Azure Active Directory
Azure AD Connect integreert uw on-premises adreslijsten met Azure Active Directory. Hiermee kunt u uw gebruikers een algemene identiteit bieden voor Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD. In dit onderwerp worden de planning, implementatie en bewerking besproken. Het is een verzameling van koppelingen naar de onderwerpen die betrekking hebben op dit gebied.

> [!IMPORTANT]
> [Azure AD Connect is de beste manier om uw on-premises adreslijst te verbinden met Azure AD en Office 365. Dit is het perfecte moment om te upgraden van Windows Azure Active Directory-synchronisatie (DirSync) of Azure AD Sync naar Azure AD Connect omdat deze hulpprogramma's zijn afgeschaft en vanaf 13 april 2017 niet meer worden ondersteund.](active-directory-aadconnect-dirsync-deprecated.md)
> 
> 

![Wat is Azure AD Connect?](./media/active-directory-aadconnect/arch.png)

## <a name="why-use-azure-ad-connect"></a>Waarom Azure AD Connect gebruiken?
Wanneer u uw on-premises adreslijsten integreert met Azure AD, worden uw gebruikers productiever omdat zij één identiteit hebben voor toegang tot zowel resources in de cloud als on-premises. Gebruikers en organisaties kunnen profiteren van het volgende:

* Gebruikers kunnen één identiteit gebruiken voor toegang tot on-premises toepassingen en cloudservices zoals Office 365.
* Eén hulpprogramma voor gemakkelijke implementatie voor synchronisatie en aanmelden.
* Biedt de nieuwste mogelijkheden voor uw scenario's. Azure AD Connect vervangt oudere versies van hulpprogramma's voor identiteitsintegratie zoals DirSync en Azure AD Sync. Zie voor meer informatie [Vergelijking hybride hulpprogramma’s voor identiteitsdirecory-integratie ](active-directory-hybrid-identity-design-considerations-tools-comparison.md).

### <a name="how-azure-ad-connect-works"></a>Hoe Azure AD werkt
Azure Active Directory Connect bestaat uit drie primaire onderdelen: de synchronisatieservices, het optionele Active Directory Federation Services-onderdeel en het bewakingsonderdeel [Azure AD Connect Health](active-directory-aadconnect-health.md).

<center>![Azure AD Connect Stack](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png)
</center>

* Synchronisatie : met dit onderdeel worden gebruikers, groepen en andere objecten aangemaakt. Het onderdeel kan ook worden gebruikt om te controleren of de identiteitsgegevens voor uw on-premises gebruikers en groepen overeenkomen met de gegevens in de cloud.
* AD FS - federatie is een optioneel onderdeel van Azure AD Connect en kan worden gebruikt om een hybride omgeving te configureren met een lokale AD FS-infrastructuur. Organisaties kunnen zo complexe implementaties uitvoeren, zoals SSO voor domeinlidmaatschap, afdwingen van een AD-aanmeldingsbeleid en MFA via smartcards of een externe provider.
* Statusbewaking - Azure AD Connect Health kan goede bewaking en een centrale locatie in Azure Portal bieden om deze activiteit weer te geven. Zie voor meer informatie [Azure Active Directory Connect Health](active-directory-aadconnect-health.md).

## <a name="install-azure-ad-connect"></a>Azure AD Connect installeren
U vindt de download voor Azure AD Connect in het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=615771).

| Oplossing | Scenario |
| --- | --- |
| Voordat u begint - [hardware- en vereisten](active-directory-aadconnect-prerequisites.md) |<li>Stappen die moeten worden voltooid voordat u Azure AD Connect installeert.</li> |
| [Snelle instellingen](connect/active-directory-aadconnect-get-started-express.md) |<li>Als u één AD-forest hebt, dan is dit de aanbevolen optie.</li> <li>Aanmelden gebruiker met hetzelfde wachtwoord tijdens wachtwoordsynchronisatie.</li> |
| [Aangepaste instellingen](connect/active-directory-aadconnect-get-started-custom.md) |<li>Wordt gebruikt wanneer u meerdere forests hebt. Ondersteunt vele on-premises [topologieën](active-directory-aadconnect-topologies.md).</li> <li>Pas uw aanmeldingsoptie aan zoals ADFS voor federatie of gebruik een externe identiteitsprovider.</li> <li>Synchronisatiefuncties, zoals filteren en terugschrijven, aanpassen.</li> |
| [Upgraden van DirSync](connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Wordt gebruikt wanneer u een bestaande DirSync-server hebt die al actief is.</li> |
| [Upgraden van Azure AD Sync of Azure AD Connect](active-directory-aadconnect-upgrade-previous-version.md) |<li>Er zijn verschillende methoden, afhankelijk van uw voorkeur.</li> |

[Na de installatie](active-directory-aadconnect-whats-next.md) moet u controleren of het werkt zoals verwacht en licenties toewijzen aan de gebruikers.

### <a name="next-steps-to-install-azure-ad-connect"></a>De volgende stappen om Azure AD Connect te installeren
| Onderwerp |
| --- | --- |
| Azure AD Connect downloaden |
| Installeren met behulp van snelle instellingen |
| Installeren met behulp van aangepaste instellingen |
| Upgraden van DirSync |
| Na installatie |

### <a name="learn-more-about-install-azure-ad-connect"></a>Meer informatie over het installeren van Azure AD Connect
U wilt u ook voorbereiden op [operationele](active-directory-aadconnectsync-operations.md) problemen. U wilt mogelijk een stand-by-server hebben, waar u eenvoudig op terug kunt vallen in geval van een [noodgeval](active-directory-aadconnectsync-operations.md#disaster-recovery). Als u van plan bent frequente configuratiewijzigingen aan te brengen, moet u een [faseringsmodus](active-directory-aadconnectsync-operations.md#staging-mode)-server gebruiken.

| Onderwerp |
| --- | --- |
| Ondersteunde topologieën |
| Ontwerpconcepten |
| Accounts die worden gebruikt voor installatie |
| Operationele planning |
| Opties aanmelden gebruiker |

## <a name="configure-sync-features"></a>Synchronisatiefuncties configureren
Azure AD Connect wordt geleverd met verschillende functies die u in of uit kunt schakelen of die standaard zijn ingesteld. Sommige functies vereisen mogelijk meer configuratie in bepaalde scenario's en topologieën.

[Filteren](active-directory-aadconnectsync-configure-filtering.md) wordt gebruikt wanneer u wilt beperken welke objecten worden gesynchroniseerd naar Azure AD. Alle gebruikers, contactpersonen, groepen en Windows 10-computers worden standaard gesynchroniseerd. U kunt de filtering wijzigen op basis van domeinen, OE’s of kenmerken.

[Wachtwoordsynchronisatie](active-directory-aadconnectsync-implement-password-synchronization.md) synchroniseert de wachtwoordhash in Active Directory naar Azure AD. De eindgebruiker kan hetzelfde wachtwoord zowel on-premises als in de cloud gebruiken. Beheer is maar op slechts één locatie mogelijk. Aangezien uw on-premises Active Directory als de certificeringsinstantie wordt gebruikt voor het wachtwoord, kunt u ook uw eigen wachtwoordbeleid gebruiken.

Met [Wachtwoord terugschrijven](active-directory-passwords-getting-started.md) kunnen uw gebruikers hun wachtwoorden wijzigen en resetten in de cloud en uw lokale wachtwoordbeleid toepassen.

Met [Apparaat terugschrijven](active-directory-aadconnect-feature-device-writeback.md) kan een apparaat dat is geregistreerd in Azure AD worden teruggeschreven naar on-premises Active Directory zodat deze kan worden gebruikt voor voorwaardelijke toegang.

De functie [Onopzettelijk verwijderen voorkomen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) is standaard ingeschakeld en beschermt uw cloudgebaseerde adreslijsten tegen meerdere verwijderingen op hetzelfde moment. Er kunnen standaard 500 verwijderingen per keer gedaan worden. U kunt deze instelling wijzigen, afhankelijk van de grootte van uw organisatie.

[Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) is standaard ingeschakeld voor installaties met snelle instellingen en zorgt ervoor dat u altijd beschikt over de nieuwste versie van Azure AD Connect.

### <a name="next-steps-to-configure-sync-features"></a>De volgende stappen om synchronisatiefuncties te configureren
| Onderwerp |
| --- | --- |
| Filtering configureren |
| Wachtwoordsynchronisatie |
| Wachtwoord terugschrijven |
| Apparaat terugschrijven |
| Onopzettelijke verwijderingen voorkomen |
| Automatische upgrade |

## <a name="customize-azure-ad-connect-sync"></a>Azure AD Connect-synchronisatie aanpassen
Azure AD Connect-synchronisatie wordt geleverd met een standaardconfiguratie waarmee de meeste klanten en topologieën kunnen werken. Maar er zijn altijd situaties waarin de standaardconfiguratie niet werkt en aangepast moet worden. Er kunnen wijzigingen gedaan worden zoals beschreven in deze sectie en gekoppelde onderwerpen.

Als u niet eerder met een synchronisatie-topologie hebt gewerkt, zorg er dan voor dat u eerst bekend raakt met de beginselen en de voorwaarden zoals beschreven in de [technische concepten](active-directory-aadconnectsync-technical-concepts.md). Azure AD Connect is de ontwikkeling van MIIS2003, ILM2007 en FIM 2010. Zelfs als er dingen identiek zijn, is er ook veel gewijzigd.

De [standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md) gaat ervan uit dat er meer dan één forest in de configuratie kan zijn. In deze topologieën kan een gebruikersobject worden weergegeven als een contactpersoon in een andere forest. De gebruiker heeft mogelijk ook een gekoppeld postvak in een andere bron-forest. Het gedrag van de standaardconfiguratie wordt beschreven in [gebruikers en contactpersonen](active-directory-aadconnectsync-understanding-users-and-contacts.md).

Het gesynchroniseerde configuratiemodel heet [declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md). De geavanceerde kenmerkstromen gebruiken [functies](active-directory-aadconnectsync-functions-reference.md) om kenmerktransformaties weer te geven. U kunt de volledige configuratie zien en controleren met de hulpprogramma's die worden geleverd met Azure AD Connect. Als u configuratiewijzigingen aan moet brengen, controleert u of u de [aanbevolen procedures](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) volgt zodat het eenvoudiger is nieuwe releases toe te passen.

### <a name="next-steps-to-customize-azure-ad-connect-sync"></a>De volgende stappen om Azure AD Connect-synchronisatie aan te passen
| Onderwerp |
| --- | --- |
| Alle artikelen over Azure AD Connect-synchronisatie |
| Technische concepten |
| Inzicht in de standaardconfiguratie |
| Inzicht krijgen in gebruikers en contactpersonen |
| Declaratieve inrichting |
| De standaardconfiguratie wijzigen |

## <a name="configure-federation-features"></a>Federatiekenmerken configureren
ADFS kan worden geconfigureerd om [meerdere domeinen](active-directory-aadconnect-multiple-domains.md) te ondersteunen. Zo zijn er mogelijk meerdere populaire domeinen die u wilt gebruiken voor federatie.

Als uw ADFS-server niet is geconfigureerd voor het automatisch bijwerken van certificaten van Azure AD of als u een niet-ADFS-oplossing gebruikt, krijgt u een melding wanneer u [certificaten moet bijwerken](active-directory-aadconnect-o365-certs.md).

### <a name="next-steps-to-configure-federation-features"></a>De volgende stappen om federatie-functies te configureren
| Onderwerp |
| --- | --- |
| Alle AD FS-artikelen |
| ADFS configureren met subdomeinen |
| AD FS-farm beheren |
| Handmatig bijwerken van de federatiecertificaten |

## <a name="more-information-and-references"></a>Meer informatie en verwijzingen
| Onderwerp |
| --- | --- |
| Versiegeschiedenis |
| Vergelijk DirSync Azure, ADSync en Azure AD Connect |
| Niet-ADFS compatibiliteitslijst voor Azure AD |
| Gesynchroniseerde kenmerken |
| Bewaken met behulp van Azure AD Connect Health |
| Veelgestelde vragen |

**Aanvullende resources**

Ignite 2015-presentatie over het uitbreiden van uw on-premises adreslijsten in de cloud.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3862/player]
> 
> 




<!--HONumber=Nov16_HO2-->


