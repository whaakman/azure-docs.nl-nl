---
title: Azure Active Directory hybride identiteit ontwerpoverwegingen - strategie voor hybride identity lifecycle ingebruikname bepalen | Microsoft Docs
description: "Helpt bij het definiëren van de beheertaken voor hybride identiteit volgens de beschikbare opties voor elke fase van de levenscyclus."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 420b6046-bd9b-4fce-83b0-72625878ae71
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 18b40486a66d8e092a8af299460145989a1ab99d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Hybride identity lifecycle acceptatie strategie bepalen
In deze taak definieert u de strategie voor het beheer van de identiteit voor uw oplossing voor hybride identiteit om te voldoen aan de bedrijfsvereisten die u hebt gedefinieerd in [beheertaken voor hybride identiteit bepalen](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).

Als u de beheertaken voor hybride identiteit volgens de levenscyclus van de identiteit van de end-to-end gepresenteerd eerder in deze stap definieert, kunt u moet rekening houden met de beschikbare opties voor elke fase van de levenscyclus.

## <a name="access-management-and-provisioning"></a>Toegang tot beheer en inrichting
Met een beheersysteem goede account toegang tot kunt uw organisatie bijhouden nauwkeurig die toegang tot welke gegevens in de hele organisatie heeft.

Toegangsbeheer is een kritieke functie van een gecentraliseerde, één punt inrichting systeem. Naast het beveiligen van vertrouwelijke gegevens zichtbaar toegangsbeheer bestaande accounts met niet-goedgekeurde autorisaties of zijn niet meer nodig. Verouderd accounts, het systeem koppelingen samen Inrichtingsgegevens met gezaghebbende informatie over de gebruikers die eigenaar van de accounts beheren. Identiteitsgegevens van gemachtigde gebruikers worden meestal onderhouden in de databases en mappen van human resources.

Accounts in geavanceerde IT-bedrijven honderden parameters waarmee de instanties bevatten en deze gegevens kunnen worden beheerd door uw provisioning-systeem. Nieuwe gebruikers kunnen worden geïdentificeerd met de gegevens die u van de gezaghebbende bron opgeeft. De mogelijkheid tot aanvraag goedkeuring initieert de processen die resource voor ze ingericht goedkeuren (of weigeren).

| Fase van de levenscyclus | On-premises | Cloud | Hybride |
| --- | --- | --- | --- |
| Accountbeheer en inrichting |Met behulp van de serverfunctie Active Directory® Domain Services (AD DS), kunt u een schaalbare, veilige en beheerbare infrastructuur maken voor gebruikers- en resourcebeheer, en ondersteuning bieden voor Active directory-toepassingen zoals Microsoft® Exchange Server. <br><br> [U kunt groepen in AD DS, via een Identity manager inrichten](https://technet.microsoft.com/library/ff686261.aspx) <br>[U kunt gebruikers in AD DS inrichten](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Beheerders kunnen toegangsbeheer gebruiken voor het beheren van de gebruikerstoegang tot gedeelde bronnen om beveiligingsredenen. In Active Directory en toegangsbeheer wordt beheerd op het objectniveau van het door het instellen van verschillende niveaus van toegang of machtigingen voor objecten, zoals volledig beheer, schrijven, lezen of geen toegang. Toegangsbeheer in Active Directory wordt gedefinieerd hoe verschillende gebruikers Active Directory-objecten kunt gebruiken. Machtigingen voor objecten in Active Directory zijn standaard ingesteld op de veiligste instelling. |U moet een account voor elke gebruiker die toegang hebben tot een Microsoft-cloudservice maken. U kunt ook gebruikersaccounts wijzigen of verwijderen wanneer ze niet meer nodig zijn. Gebruikers hebben geen administrator-machtigingen, maar u kunt deze desgewenst toewijzen. Zie voor meer informatie [gebruikers beheren in Azure AD](active-directory-create-users.md). <br><br> Een van de belangrijkste functies is de mogelijkheid voor het beheren van toegang tot bronnen in Azure Active Directory. Deze resources kunnen deel uitmaken van de map in het geval van machtigingen voor het beheren van objecten door middel van rollen in de directory, of de bronnen die extern zijn voor de map, zoals SaaS-toepassingen, de Azure-services, en de SharePoint-sites of de lokale bronnen. <br><br> Toegang voor de center van Azure Active Directory is beheeroplossing de beveiligingsgroep. De resource-eigenaar (of de beheerder van de directory) kan een groep voor een bepaalde toegang tot de resources die ze eigenaar worden toegewezen. De toegang wordt aangeboden door de leden van de groep en de resource-eigenaar kan het recht voor het beheren van de lijst met leden van een groep naar iemand anders – zoals een afdeling of een sitebeheerder helpdesk delegeren<br> <br> De groepen beheren in Azure AD-onderwerp vindt u meer informatie over het beheren van toegang tot en met groepen. |Active Directory-identiteiten in de cloud via synchronisatie en Federatie uitbreiden |

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
Op rollen gebaseerde toegang beheren (RBAC) maakt gebruik van functies en beleidsregels te evalueren, testen en afdwingen van uw bedrijfsprocessen en regels voor het verlenen van toegang voor gebruikers. Sleutel beheerders beleidsregels voor de inrichting en maken en gebruikers toewijzen aan rollen die sets met rechten aan resources voor deze rollen definiëren. RBAC breidt de oplossing voor identiteitsbeheer als u wilt gebruiken op basis van software-processen en handmatige tussenkomst van de gebruiker in het inrichtingsproces te verminderen.
Azure AD-RBAC beschikt het bedrijf te beperken van de hoeveelheid bewerkingen die een persoon uitvoeren kunt nadat hij toegang tot Azure-beheerportal heeft. Met RBAC toegang tot de portal kunnen IT-beheerders ca toegang delegeren met behulp van de volgende methoden voor access management:

* **Op basis van een groep roltoewijzing**: U kunt toegang toewijzen aan Azure AD-groepen die kunnen worden gesynchroniseerd vanuit uw lokale Active Directory. Hiermee kunt u gebruikmaken van bestaande investeringen in uw organisatie heeft doorgevoerd in tooling en processen voor het beheren van groepen. U kunt ook de functie voor het beheer van gedelegeerd group van Azure AD Premium.
* **Gebruik de ingebouwde rollen in Azure**: U kunt drie rollen: eigenaar, bijdrager en Reader om ervoor te zorgen dat gebruikers en groepen gemachtigd bent alleen de taken die ze nodig hebben om hun werk te doen.
* **Gedetailleerde toegang tot bronnen**: U kunt rollen toewijzen aan gebruikers en groepen voor een bepaald abonnement, resourcegroep of een afzonderlijke Azure resource, zoals een website of de database. Op deze manier kunt u ervoor zorgen dat gebruikers hebben toegang tot alle bronnen die ze nodig hebben en geen toegang tot bronnen die ze niet hoeven te beheren.

## <a name="provisioning-and-other-customization-options"></a>Inrichting en andere aanpassingsopties
Uw team kunt bedrijfsplannen en vereisten gebruiken om te bepalen hoeveel voor het aanpassen van de oplossing voor identiteit. Een grote onderneming mogelijk bijvoorbeeld een gefaseerde implementatieschema voor werkstromen en aangepaste adapters die is gebaseerd op een tijdlijn voor toepassingen die veel worden gebruikt op locaties incrementeel-inrichting. Een andere aanpassing planning overwegen voor twee of meer toepassingen moeten worden ingericht in de hele organisatie, na een geslaagde test leveren. Interactie van de toepassing van de gebruiker kan worden aangepast en procedures voor het inrichten van resources mogelijk worden gewijzigd zodat de geautomatiseerde inrichting.

U kunt inrichting ervan ongedaan maakt om een service of het onderdeel te verwijderen. Een account opheffen van inrichting betekent bijvoorbeeld dat het account van een resource wordt verwijderd.

De hybride-model voor het leveren van bronnen combineert aanvraag en methoden op basis van rollen, die beide worden ondersteund door Azure AD. Voor een subset van werknemers of beheerde systemen, een bedrijf wil mogelijk automatisch aanmelden met toewijzing op basis van rollen. Een bedrijf mogelijk ook andere aanvragen of uitzonderingen door een model op basis van aanvraag verwerkt. Sommige bedrijven kunnen beginnen met handmatige toewijzing en ontwikkelen voor een hybride-model, met een doel van een volledig op rollen gebaseerde implementatie op een later tijdstip.

Andere bedrijven mogelijk vindt het niet praktisch voor zakelijke redenen als u de volledige inrichting op basis van rollen en een benadering hybride als een gewenste doelstelling als doel. Nog andere bedrijven kunnen worden voldaan met alleen op aanvraag gebaseerde inrichting en niet wilt investeren extra moeite om te definiëren en beheren op basis van functies, geautomatiseerde beleidsregels voor de inrichting.

## <a name="license-management"></a>Licentiebeheer
Licentiebeheer in Azure AD op basis van een groep kan beheerders gebruikers toewijzen aan een beveiligingsgroep en licenties in Azure AD automatisch worden toegewezen aan de leden van de groep. Als een gebruiker wordt vervolgens toegevoegd aan of verwijderd uit de groep, een licentie worden automatisch toegewezen aan of verwijderd afhankelijk van wat geschikt.

U kunt groepen die u vanuit synchroniseert on-premises AD of beheren in Azure AD. Deze koppeling met Azure AD premium groepsbeheer met Self-Service kunt u eenvoudig delegeren licentietoewijzing naar de juiste besluitvormers. U kunt erop vertrouwen problemen zoals licentie conflicten en ontbrekende locatiegegevens automatisch worden gesorteerd uit.

## <a name="self-regulating-user-administration"></a>Zelf marktregulerende Gebruikersbeheer
Wanneer uw organisatie wordt gestart voor het leveren van resources voor alle interne organisaties, kunt u de functionaliteit voor beheer van zelf marktregulerende gebruikers implementeren. U kunt de voordelen en de voordelen van inrichting gebruikers beseft buiten de grenzen van de organisatie. In deze omgeving een wijziging in de status van een gebruiker automatisch doorgevoerd in de toegangsrechten op organisatie grenzen en locaties. U kunt inrichting kosten te verlagen en de toegangs- en -processen te stroomlijnen. De implementatie realiseert de mogelijkheden van de implementatie van op rollen gebaseerde toegangsbeheer voor end-to-end-toegangsbeheer in uw organisatie. U kunt beheerdersrechten verlagen door geautomatiseerde procedures voor de betreffende gebruikers inrichten. U kunt de beveiliging verbeteren door de beveiliging wordt afgedwongen, automatiseren en stroomlijnen en levenscyclusbeheer van gebruiker en resources worden ingericht voor grote gebruiker populaties centraliseren.

> [!NOTE]
> Zie voor meer informatie, instellen van Azure AD voor toepassingsbeheer voor toegang tot selfservice
> 
> 

Azure AD (recht gebaseerd) op basis van een licentie services werk door een abonnement in uw Azure AD-directory-service-tenant activeren. Zodra het abonnement actief is kunnen u de servicemogelijkheden worden beheerd door beheerders van directory-service en die wordt gebruikt door gebruikers met een licentie. Zie voor meer informatie, hoe biedt Azure AD licensing work?
Integratie met andere leveranciers 3e

Azure Active Directory biedt eenmalige aanmelding op en verbeterde beveiliging van de toepassing toegang tot duizenden SaaS-toepassingen en on-premises webtoepassingen. Zie voor een gedetailleerde lijst met Azure Active Directory-toepassingsgalerie voor ondersteunde SaaS-toepassingen, Azure Active Directory federatiecompatibiliteitslijst: onafhankelijke id-providers die kunnen worden gebruikt voor het implementeren van eenmalige aanmelding

## <a name="define-synchronization-management"></a>Synchronisatie management definiëren
Wanneer u uw on-premises directory's integreert met Azure AD, worden uw gebruikers productiever omdat zij één identiteit hebben voor toegang tot zowel resources in de cloud als on-premises. Met deze integratie kunnen gebruikers en organisaties profiteren van de volgende opties:

* Organisaties kunnen gebruikers met een algemene hybride identiteit bieden voor on-premises of cloud-gebaseerde services gebruik van Windows Server Active Directory en vervolgens te verbinden met Azure Active Directory.
* Beheerders kunnen voorwaardelijke toegang op basis van de bron van toepassing, apparaat en gebruikers-id, netwerklocatie en multi-factor authentication-server opgeven.
* Gebruikers kunnen gebruikmaken van hun gemeenschappelijke identiteit via accounts in Azure AD bij Office 365, Intune, SaaS-apps en toepassingen van derden.
* Ontwikkelaars kunnen ontwikkelen van toepassingen die gebruikmaken van het algemene identiteitsmodel, integratie van toepassingen in Active Directory on-premises of Azure voor cloud-gebaseerde toepassingen

De volgende afbeelding bevat een voorbeeld van een weergave op hoog niveau van het synchronisatieproces identiteit.

![](./media/hybrid-id-design-considerations/identitysync.png)

Synchronisatieproces van identiteit

Controleer de volgende tabel om te vergelijken van de synchronisatie-opties:

| Optie voor synchronisatie | Voordelen | Nadelen |
| --- | --- | --- |
| Synchronisatie gebaseerde (via DirSync of AADConnect) |Gebruikers en groepen die zijn gesynchroniseerd vanaf de on-premises en cloud <br>  **Beleidsbeheer**: accountbeleid kunnen worden ingesteld via Active Directory, waarmee de beheerder de mogelijkheid voor het beheren van beleid voor wachtwoorden, werkstation, beperkingen, uitsluiting besturingselementen, en meer, zonder dat aanvullende taken in de cloud.  <br>  **Toegangsbeheer**: toegang beperken met de cloudservice, zodat de services kunnen worden geopend via de bedrijfsomgeving via online servers of beide. <br>  Verminderd ondersteuningsoproepen: als gebruikers minder wachtwoorden te onthouden hebben, zijn deze minder waarschijnlijk deze vergeet. <br>  Beveiliging: Identiteit van gebruikers en informatie zijn beveiligd, omdat alle servers en services die worden gebruikt in eenmalige aanmelding onder de knie en lokale beheerd. <br>  Ondersteuning voor sterke verificatie: U sterke verificatie (ook wel tweeledige verificatie genoemd) kunt gebruiken met de cloudservice. Echter, als u sterke verificatie gebruikt, moet u eenmalige aanmelding. | |
| Op basis van Federation (via AD FS) |Ingeschakeld door de beveiligingstokenservice (STS). Wanneer u een STS eenmalige aanmelding om toegang te bieden met een Microsoft-cloudservice configureert, maakt u een federatieve vertrouwensrelatie tussen uw lokale STS en het federatieve domein dat u hebt opgegeven in uw Azure AD-tenant. <br> Eindgebuikers kunnen dezelfde set referenties gebruiken voor toegang tot meerdere resources <br>eindgebruikers kunnen geen onderhouden meerdere sets met referenties. Nog, de gebruikers moeten hun referenties aan elk van de deelnemende resources., B2B en B2C scenario's ondersteund. |Vereist speciale personeel voor implementatie en het onderhoud van de specifieke on-premises AD FS-servers. Er gelden beperkingen voor het gebruik van sterke verificatie als u van plan bent met AD FS voor uw STS. Zie voor meer informatie [geavanceerde opties configureren voor AD FS 2.0](http://go.microsoft.com/fwlink/?linkid=235649). |

> [!NOTE]
> Zie voor meer informatie, [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).
> 
> 

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)

