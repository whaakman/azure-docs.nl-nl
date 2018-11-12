---
title: Hybride identiteit ontwerp - acceptatiestrategie voor levenscyclus Azure | Microsoft Docs
description: Helpt bij het definiëren van de beheertaken voor hybride identiteit op basis van de beschikbare opties voor elke fase van de levenscyclus.
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: 420b6046-bd9b-4fce-83b0-72625878ae71
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 5590f1fc3716582da090b8429f8bcf4fc7911dbe
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251760"
---
# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Acceptatiestrategie voor voor de levenscyclus van de identiteit van de hybride bepalen
In deze taak definieert u de strategie voor het beheer van identiteit voor uw oplossing voor hybride identiteit om te voldoen aan de bedrijfsvereisten die u hebt gedefinieerd in [beheertaken voor hybride identiteit bepalen](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).

U moet rekening houden met de opties die beschikbaar zijn voor elke fase van de levenscyclus voor het definiëren van de beheertaken voor hybride identiteit op basis van de levenscyclus van de identiteit van de end-to-end die eerder in deze stap worden gepresenteerd.

## <a name="access-management-and-provisioning"></a>Toegangsbeheer en inrichting
Met een beheersysteem goede account toegang tot kunt uw organisatie bijhouden precies wie toegang tot welke gegevens in de hele organisatie heeft.

Toegangsbeheer is een belangrijke functie van een gecentraliseerd, single point inrichting systeem. Naast het beveiligen van vertrouwelijke gegevens blootstellen besturingselementen voor toegang bestaande accounts die zijn niet-goedgekeurde autorisaties of zijn niet meer nodig. Voor het beheren van verouderde accounts, het systeem koppelingen samen Inrichtingsgegevens met gezaghebbende informatie over de gebruikers die eigenaar zijn van de accounts. Informatie over de gemachtigde gebruiker-id meestal onderhouden in de databases en mappen van human resources.

Accounts in geavanceerde IT-bedrijven zijn honderden parameters die de instanties te definiëren en deze gegevens kunnen worden beheerd door uw provisioning-systeem. Nieuwe gebruikers kunnen worden geïdentificeerd met de gegevens die u van de gezaghebbende bron opgeeft. De mogelijkheid tot aanvraag voor goedkeuring initieert de processen die broninrichting voor deze goedkeuren (of afwijzen).

| Levenscyclus van management-fase | On-premises | Cloud | Hybride |
| --- | --- | --- | --- |
| Accountbeheer en inrichting |Met behulp van de serverfunctie Active Directory® Domain Services (AD DS), kunt u een schaalbare, veilige en beheersbare infrastructuur voor gebruikers- en bronbeheer maken en bieden ondersteuning voor Active directory-toepassingen zoals Microsoft® Exchange Server. <br><br> [U kunt groepen in AD DS, via een Identity manager inrichten](https://technet.microsoft.com/library/ff686261.aspx) <br>[ U kunt gebruikers in AD DS inrichten](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Beheerders kunnen toegangsbeheer gebruiken voor het beheren van de gebruikerstoegang tot gedeelde bronnen om veiligheidsredenen. In Active Directory, toegangsbeheer wordt beheerd op het objectniveau door verschillende niveaus instellen van toegang en machtigingen voor objecten, zoals volledig beheer, schrijven, lezen of geen toegang. Toegangsbeheer in Active Directory wordt gedefinieerd hoe verschillende gebruikers Active Directory-objecten kunt gebruiken. Machtigingen voor objecten in Active Directory zijn standaard ingesteld op de veiligste instelling. |U moet maken van een account voor elke gebruiker die toegang een Microsoft-cloudservice tot. U kunt ook gebruikersaccounts wijzigen of te verwijderen wanneer ze niet meer nodig zijn. Gebruikers hebben geen beheerdersmachtigingen standaard, maar u kunt deze desgewenst toewijzen. <br><br> In Azure Active Directory is een van de belangrijkste functies de mogelijkheid voor het beheren van toegang tot bronnen. Deze resources kunnen deel uitmaken van de adreslijst, zoals in het geval van machtigingen voor het beheren van objecten door middel van rollen in de adreslijst, of resources die zich buiten de adreslijst bevinden, zoals SaaS-toepassingen, Azure-services, en SharePoint-sites of on-premises resources. <br><br> Management-oplossing is in het midden van van Azure Active Directory toegang tot de beveiligingsgroep. De resource-eigenaar (of de beheerder van de adreslijst of directory) kan een groep toewijzen om een bepaald toegangsrecht in te stellen voor de resources waarvan ze eigenaar zijn. De toegang wordt geleverd door de leden van de groep en de resource-eigenaar het recht voor het beheren van de lijst met leden van een groep aan iemand anders – zoals de afdelingsmanager van een of helpdesk-beheerder kan overdragen<br> <br> De groepen beheren in Azure AD-sectie vindt u meer informatie over het beheren van toegang tot en met groepen. |Active Directory-identiteiten uitbreiden naar de cloud via synchronisatie en Federatie |

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
Toegang op basis van rollen (RBAC) maakt gebruik van rollen beheren en beleidsregels om te beoordelen, testen en af te dwingen uw zakelijke processen en regels voor het verlenen van toegang voor gebruikers. Belangrijkste beheerders beleidsregels voor de inrichting maken en toewijzen van gebruikers aan rollen en die sets met rechten om resources voor deze rollen te definiëren. RBAC breidt de oplossing voor identiteitsbeheer voor het gebruik van processen op basis van software en handmatige tussenkomst van de gebruiker in het inrichtingsproces te verminderen.
Azure AD RBAC beschikt het bedrijf om te beperken van het aantal bewerkingen die een persoon doen kunt wanneer hij toegang tot de Azure-portal heeft. Met RBAC voor het beheren van toegang tot de portal kunnen IT-beheerders ca toegang delegeren met behulp van de volgende methoden voor access management:

* **Op basis van een groep roltoewijzing**: U kunt toegang toewijzen aan Azure AD-groepen die kunnen worden gesynchroniseerd vanuit uw lokale Active Directory. Hiermee kunt u gebruikmaken van bestaande investeringen die uw organisatie heeft aangebracht in de hulpprogramma's en processen voor het beheren van groepen. U kunt ook de functie voor het beheer van groepen met gemachtigden van Azure AD Premium gebruiken.
* **Maak gebruik van ingebouwde rollen in Azure**: kunt u drie rollen: eigenaar, Inzender en lezer, om ervoor te zorgen dat gebruikers en groepen gemachtigd alleen de taken die ze nodig hebben om hun werk te doen.
* **Gedetailleerde toegang tot resources**: U kunt rollen toewijzen aan gebruikers en groepen voor een bepaald abonnement, resourcegroep of een afzonderlijke Azure-resource, zoals een website of de database. Op deze manier kunt u ervoor zorgen dat gebruikers hebben toegang tot alle bronnen die ze nodig hebben en geen toegang tot resources die ze niet hoeven te beheren.

## <a name="provisioning-and-other-customization-options"></a>Inrichting en andere opties voor het aanpassen
Uw team kunt bedrijfsplannen en vereisten om te bepalen hoeveel om aan te passen van de oplossing voor identiteit. Een grote onderneming mogelijk bijvoorbeeld een gefaseerde uitrol-plan voor werkstromen en aangepaste adapters die is gebaseerd op een tijdlijn voor het inrichten van incrementeel toepassingen die veelvuldig worden gebruikt in verschillende geografische gebieden. Een andere aanpassing plan kan bieden voor twee of meer toepassingen moeten worden ingericht in de hele organisatie, na een geslaagde test. Tussenkomst van de gebruiker-toepassing kan worden aangepast en procedures voor het inrichten van resources mogelijk worden gewijzigd om te voldoen aan de geautomatiseerde inrichting.

U kunt inrichting ongedaan maken om een service of het onderdeel te verwijderen. Een account opheffen van inrichting betekent bijvoorbeeld dat het account wordt verwijderd uit een resource.

De hybride-model van de inrichting van resources combineert aanvraag en op basis van rollen benaderingen, die beide worden ondersteund door Azure AD. Voor een subset van werknemers of beheerde systemen wilt een bedrijf automatiseren toegang met toewijzing op basis van rollen. Een bedrijf kan ook alle andere aanvragen of uitzonderingen via een model op basis van een aanvraag verwerken. Sommige bedrijven kunnen beginnen met handmatige toewijzing en ontwikkelen op een hybride-model, met een doel van een implementatie volledig op rollen gebaseerd op een later tijdstip.

Andere bedrijven mogelijk vindt u het niet praktisch is voor zakelijke redenen om de volledige inrichting op basis van rollen en een hybride benadering als een gewenste doelstelling als doel. Nog steeds andere bedrijven kunnen worden voldaan met alleen op basis van een aanvraag voor inrichting en niet wilt investeren extra moeite om te definiëren en beheren op basis van rollen, geautomatiseerde beleidsregels voor de inrichting.

## <a name="license-management"></a>Licentiebeheer
Licentiebeheer in Azure AD op basis van een groep kan beheerders gebruikers toewijzen aan een beveiligingsgroep en licenties voor Azure AD automatisch toegewezen aan alle leden van de groep. Als een gebruiker wordt vervolgens toegevoegd aan of verwijderd uit de groep, wordt een licentie automatisch toegewezen of verwijderd, zoals toepasselijk.

U kunt groepen die u vanuit synchroniseert on-premises AD of beheren in Azure AD. U kunt eenvoudig een licentietoewijzing aan de juiste besluitvormers delegeren dit om te koppelen met Azure AD premium groepsbeheer met Self-Service. U kunt er zeker van zijn dat problemen zoals licentie conflicten en ontbrekende locatiegegevens automatisch worden gesorteerd uit.

## <a name="self-regulating-user-administration"></a>Zelf marktregulerende Gebruikersbeheer
Wanneer uw organisatie wordt gestart voor het inrichten van resources in alle interne organisaties, kunt u de mogelijkheden zelf marktregulerende beheer implementeren. U kunt de voordelen en de voordelen van inrichting gebruikers realiseren grenzen van de organisatie. In deze omgeving is wordt een wijziging in de status van een gebruiker automatisch weerspiegeld in toegangsrechten voor de grenzen van de organisatie en landen. U kunt inrichten kosten te verlagen en stroomlijnen de processen voor toegang en goedkeuring. De implementatie realiseert het volle potentieel van de implementatie van op rollen gebaseerd toegangsbeheer voor end-to-end-toegangsbeheer in uw organisatie. U kunt de administratieve kosten door geautomatiseerde procedures voor het inrichten van gebruikers inzake verminderen. U kunt de beveiliging verbeteren door het automatiseren van beleidsafdwinging voor beveiliging, en stroomlijnen en beheer van de levenscyclus van gebruikers en de inrichting van resources voor grote gebruiker populaties communicatiemiddelen zijn gecentraliseerd.

> [!NOTE]
> Zie voor meer informatie, instellen van Azure AD voor toegangsbeheer met selfservice-toepassing
> 
> 

Werk (recht-indeling) van Azure AD-licentie op basis van services door een abonnement in uw Azure AD-directory/service-tenant activeren. Zodra het abonnement actief is kunnen de servicemogelijkheden worden beheerd door beheerders van directory-service en gebruikt door gebruikers met een licentie. 

## <a name="integration-with-other-3rd-party-providers"></a>Integratie met andere 3rd party-providers

Azure Active Directory biedt eenmalige aanmelding op en verbeterde beveiliging van de toepassing toegang tot duizenden SaaS-toepassingen en on-premises webtoepassingen. Zie voor meer informatie, [toepassingen integreren met Azure Active Directory](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)

## <a name="define-synchronization-management"></a>Synchronisatie management definiëren
Wanneer u uw on-premises directory's integreert met Azure AD, worden uw gebruikers productiever omdat zij één identiteit hebben voor toegang tot zowel resources in de cloud als on-premises. Met deze integratie kunt gebruikers en organisaties kunnen profiteren van de volgende opties:

* Organisaties kunnen gebruikers met een algemene hybride identiteit bieden voor on-premises of cloud-gebaseerde services gebruik te maken van Windows Server Active Directory en vervolgens verbinding maken met Azure Active Directory.
* Beheerders kunnen bieden voor voorwaardelijke toegang op basis van de toepassingsresource, apparaten en gebruikers-id, netwerklocatie en multi-factor authentication.
* Gebruikers kunnen profiteren van hun algemene identiteit via accounts in Azure AD aan Office 365, Intune, SaaS-apps, en toepassingen van derden.
* Ontwikkelaars kunnen bouwen van toepassingen die gebruikmaken van het algemene identiteitsmodel, integratie van toepassingen in on-premises Active Directory of Azure voor cloud-toepassingen

De volgende afbeelding bevat een voorbeeld van een weergave op hoog niveau van de synchronisatie-proces identiteit.

![Sync](./media/plan-hybrid-identity-design-considerations/identitysync.png)

Synchronisatieproces voor identiteit

Bekijk de volgende tabel als u wilt vergelijken van de synchronisatie-opties:

| Optie voor synchronisatie | Voordelen | Nadelen |
| --- | --- | --- |
| Synchronisatie op basis van (door middel van DirSync of AADConnect) |Gebruikers en groepen die zijn gesynchroniseerd vanaf on-premises en cloud <br>  **Beheer van beleidsregels**: accountbeleid kunnen worden ingesteld via Active Directory, waarmee de beheerder de mogelijkheid voor het beheren van beleid voor wachtwoorden, werkstation, beperkingen, lock-out besturingselementen, en meer, zonder dat extra taken in uit te voeren de cloud.  <br>  **Toegangsbeheer**: toegang beperken met de cloudservice, zodat de services kunnen worden geopend via de bedrijfsomgeving, via de online-servers, of beide. <br>  Minder ondersteuningsaanvragen worden ingediend: als gebruikers minder wachtwoorden te onthouden, ze zijn minder waarschijnlijk, deze vergeet. <br>  Beveiliging: Gebruikers-id's en gegevens worden beveiligd, omdat alle servers en services die worden gebruikt in de eenmalige aanmelding, worden beheerd en on-premises beheerd. <br>  Ondersteuning voor sterke verificatie: kunt u sterke verificatie (ook wel tweeledige verificatie genoemd) met de cloudservice. Echter, als u sterke verificatie, moet u eenmalige aanmelding. | |
| Op basis van Federation (via AD FS) |Ingeschakeld door een beveiligingstokenservice (STS). Wanneer u een STS eenmalige aanmelding in om toegang te bieden met een Microsoft cloud-service configureert, maakt u een federatieve vertrouwensrelatie tussen uw on-premises STS en de federatieve domein dat u hebt opgegeven in uw Azure AD-tenant. <br> Hiermee kunnen eindgebruikers de dezelfde set referenties gebruiken om te verkrijgen van toegang tot meerdere resources <br>eindgebruikers hoeft te onderhouden van meerdere sets met referenties. Nog, de gebruikers hebben hun referenties aan elk van de deelnemende resources., B2B en B2C-scenario's ondersteund. |Gespecialiseerde personeel is vereist voor de implementatie en het onderhoud van toegewezen on-premises AD FS-servers. Er gelden beperkingen voor het gebruik van sterke verificatie als u van plan bent te gebruiken van AD FS voor de STS. Zie voor meer informatie, [geavanceerde opties configureren voor AD FS 2.0](https://go.microsoft.com/fwlink/?linkid=235649). |

> [!NOTE]
> Zie voor meer informatie, [uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md).
> 
> 

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)

