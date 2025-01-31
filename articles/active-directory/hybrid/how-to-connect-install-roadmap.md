---
title: Installatieprocedure voor Azure AD Connect en Azure AD Connect Health. | Microsoft Docs
description: Dit document bevat een overzicht van de installatieopties en -paden die beschikbaar zijn voor het installeren van Azure AD Connect en Connect Health.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/18/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d6cce859aa26935f0b6007fe8e5ff2c2239b210
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109455"
---
# <a name="azure-ad-connect-and-azure-ad-connect-health-installation-roadmap"></a>Installatieprocedure voor Azure AD Connect en Azure AD Connect Health

## <a name="install-azure-ad-connect"></a>Azure AD Connect installeren

> [!IMPORTANT]
> Microsoft biedt geen ondersteuning voor het wijzigen of uitvoeren van Azure AD Connect-synchronisatie anders dan op de manier die officieel is gedocumenteerd. Alle andere acties kunnen resulteren in een inconsistente of niet-ondersteunde status van de Azure AD Connect-synchronisatie. Daarom biedt Microsoft geen technische ondersteuning voor dergelijke implementaties.

U vindt de download voor Azure AD Connect op [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=615771).

| Oplossing | Scenario |
| --- | --- |
| Voordat u begint - [hardware- en vereisten](how-to-connect-install-prerequisites.md) |<li>Stappen die voltooid moeten worden voordat u Azure AD Connect installeert.</li> |
| [Snelle instellingen](how-to-connect-install-express.md) |<li>Als u één AD-forest hebt, dan is dit de aanbevolen optie.</li> <li>Aanmelden gebruiker met hetzelfde wachtwoord tijdens wachtwoordsynchronisatie.</li> |
| [Aangepaste instellingen](how-to-connect-install-custom.md) |<li>Wordt gebruikt wanneer u meerdere forests hebt. Ondersteunt vele on-premises [topologieën](plan-connect-topologies.md).</li> <li>Pas uw aanmeldingsoptie, zoals pass-through-verificatie, of ADFS voor federatie aan of gebruik een externe id-provider.</li> <li>Synchronisatiefuncties, zoals filteren en terugschrijven, aanpassen.</li> |
| [Upgraden van DirSync](how-to-dirsync-upgrade-get-started.md) |<li>Wordt gebruikt wanneer u een bestaande DirSync-server hebt die al actief is.</li> |
| [Upgraden van Azure AD Sync of Azure AD Connect](how-to-upgrade-previous-version.md) |<li>Er zijn verschillende methoden, afhankelijk van uw voorkeur.</li> |

[Na de installatie](how-to-connect-post-installation.md) moet u controleren of het werkt zoals verwacht en licenties toewijzen aan de gebruikers.

### <a name="next-steps-to-install-azure-ad-connect"></a>De volgende stappen om Azure AD Connect te installeren
|Onderwerp |Koppeling|  
| --- | --- |
|Azure AD Connect downloaden | [Azure AD Connect downloaden](https://go.microsoft.com/fwlink/?LinkId=615771)|
|Installeren met behulp van snelle instellingen | [Snelle installatie van Azure AD Connect](./how-to-connect-install-express.md)|
|Installeren met behulp van aangepaste instellingen | [Aangepaste installatie van Azure AD Connect](./how-to-connect-install-custom.md)|
|Upgraden van DirSync | [Upgraden van Azure AD-synchronisatiehulpprogramma (DirSync)](./how-to-dirsync-upgrade-get-started.md)|
|Na installatie | [Controleer of de installatie en licenties toewijzen](how-to-connect-post-installation.md)|

### <a name="learn-more-about-install-azure-ad-connect"></a>Meer informatie over het installeren van Azure AD Connect
U wilt u ook voorbereiden op [operationele](how-to-connect-sync-operations.md) problemen. U wilt mogelijk een stand-by-server hebben, waarop u eenvoudig failover-overschakeling kunt uitvoeren in geval van een [noodgeval](how-to-connect-sync-staging-server.md#disaster-recovery). Als u van plan bent frequente configuratiewijzigingen aan te brengen, moet u een [faseringsmodus](how-to-connect-sync-staging-server.md)-server gebruiken.

|Onderwerp |Koppeling|  
| --- | --- |
|Ondersteunde topologieën | [Topologieën voor Azure AD Connect](plan-connect-topologies.md)|
|Ontwerpconcepten | [Ontwerpconcepten van Azure AD Connect](plan-connect-design-concepts.md)|
|Accounts die worden gebruikt voor installatie | [Meer informatie over referenties en machtigingen van Azure AD Connect](reference-connect-accounts-permissions.md)|
|Operationele planning | [Azure AD Connect Sync: Operationele taken en overwegingen](how-to-connect-sync-operations.md)|
|Opties aanmelden gebruiker | [Opties van Azure AD Connect voor het aanmelden van gebruikers](plan-connect-user-signin.md)|

## <a name="configure-sync-features"></a>Synchronisatie-functies configureren
Azure AD Connect wordt geleverd met verschillende functies die u in of uit kunt schakelen of die standaard zijn ingesteld. Sommige functies vereisen mogelijk meer configuratie in bepaalde scenario's en topologieën.

[Filteren](how-to-connect-sync-configure-filtering.md) wordt gebruikt wanneer u wilt beperken welke objecten worden gesynchroniseerd naar Azure AD. Alle gebruikers, contactpersonen, groepen en Windows 10-computers worden standaard gesynchroniseerd. U kunt de filtering wijzigen op basis van domeinen, OE’s of kenmerken.

[Synchronisatie van wachtwoord-hash](how-to-connect-password-hash-synchronization.md) synchroniseert de wachtwoord-hash in Active Directory naar Azure AD. De eindgebruiker kan hetzelfde wachtwoord zowel on-premises als in de cloud gebruiken, maar deze slechts op één locatie beheren. Aangezien het wachtwoord gebruikmaakt van uw on-premises Active Directory als de instantie, kunt u ook uw eigen wachtwoordbeleid gebruiken.

Met [Wachtwoord terugschrijven](../authentication/quickstart-sspr.md) kunnen uw gebruikers hun wachtwoorden wijzigen en resetten in de cloud en uw lokale wachtwoordbeleid toepassen.

[Apparaat terugschrijven](how-to-connect-device-writeback.md) kan een apparaat dat is geregistreerd bij Azure AD worden teruggeschreven naar on-premises Active Directory, zodat deze kan worden gebruikt voor voorwaardelijke toegang.

De functie [Onopzettelijk verwijderen voorkomen](how-to-connect-sync-feature-prevent-accidental-deletes.md) is standaard ingeschakeld en beschermt uw clouddirectory tegen meerdere verwijderingen op hetzelfde moment. Er kunnen standaard 500 verwijderingen per keer gedaan worden. U kunt deze instelling wijzigen, afhankelijk van de grootte van uw organisatie.

[Automatische upgrade](how-to-connect-install-automatic-upgrade.md) is standaard ingeschakeld voor installaties met snelle instellingen en zorgt ervoor dat u altijd beschikt over de nieuwste versie van Azure AD Connect.

### <a name="next-steps-to-configure-sync-features"></a>De volgende stappen om synchronisatie-functies te configureren
|Onderwerp |Koppeling|  
| --- | --- |
|Filtering configureren | [Azure AD Connect Sync: Filteren configureren](how-to-connect-sync-configure-filtering.md)|
|Synchronisatie van wachtwoord-hashes | [Synchronisatie van wachtwoord-hashes](how-to-connect-password-hash-synchronization.md)|
|Pass-through-verificatie | [Pass-through-verificatie](how-to-connect-pta.md)
|Wachtwoord terugschrijven | [Aan de slag met wachtwoordbeheer](../authentication/quickstart-sspr.md)|
|Apparaat terugschrijven | [Apparaat terugschrijven inschakelen in Azure AD Connect](how-to-connect-device-writeback.md)|
|Onopzettelijke verwijderingen voorkomen | [Azure AD Connect Sync: Onopzettelijke verwijderingen voorkomen](how-to-connect-sync-feature-prevent-accidental-deletes.md)|
|Automatische upgrade | [Azure AD Connect: Automatische upgrade](how-to-connect-install-automatic-upgrade.md)|

## <a name="customize-azure-ad-connect-sync"></a>Azure AD Connect-synchronisatie aanpassen
Azure AD Connect-synchronisatie wordt geleverd met een standaardconfiguratie waarmee de meeste klanten en topologieën kunnen werken. Maar er zijn altijd situaties waarin de standaardconfiguratie niet werkt en aangepast moet worden. Er kunnen wijzigingen gedaan worden zoals beschreven in deze sectie en gekoppelde onderwerpen.

Als u niet eerder met een synchronisatie-topologie hebt gewerkt, zorg er dan voor dat u eerst bekend raakt met de beginselen en de voorwaarden zoals beschreven in de [technische concepten](how-to-connect-sync-technical-concepts.md). Azure AD Connect is de ontwikkeling van MIIS2003, ILM2007 en FIM 2010. Zelfs als er dingen identiek zijn, is er ook veel gewijzigd.

De [standaardconfiguratie](concept-azure-ad-connect-sync-default-configuration.md) gaat ervan uit dat er meer dan één forest in de configuratie kan zijn. In deze topologieën kan een gebruikersobject worden weergegeven als een contactpersoon in een andere forest. De gebruiker heeft mogelijk ook een gekoppeld postvak in een andere bron-forest. Het gedrag van de standaardconfiguratie wordt beschreven in [gebruikers en contactpersonen](concept-azure-ad-connect-sync-user-and-contacts.md).

Het gesynchroniseerde configuratiemodel heet [declaratieve inrichting](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md). De geavanceerde kenmerkstromen gebruiken [functies](reference-connect-sync-functions-reference.md) om kenmerktransformaties weer te geven. U kunt de volledige configuratie zien en controleren met de hulpprogramma's die worden geleverd met Azure AD Connect. Als u configuratiewijzigingen aan moet brengen, controleert u of u de [aanbevolen procedures](how-to-connect-sync-best-practices-changing-default-configuration.md) volgt zodat het eenvoudiger is nieuwe releases toe te passen.

### <a name="next-steps-to-customize-azure-ad-connect-sync"></a>De volgende stappen om Azure AD Connect-synchronisatie aan te passen
|Onderwerp |Koppeling|  
| --- | --- |
|Alle artikelen over Azure AD Connect-synchronisatie | [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md)|
|Technische concepten | [Azure AD Connect Sync: Technische concepten](how-to-connect-sync-technical-concepts.md)|
|Inzicht in de standaardconfiguratie | [Azure AD Connect Sync: Inzicht in de standaardconfiguratie](concept-azure-ad-connect-sync-default-configuration.md)|
|Inzicht krijgen in gebruikers en contactpersonen | [Azure AD Connect Sync: Inzicht krijgen in gebruikers en contactpersonen](concept-azure-ad-connect-sync-user-and-contacts.md)|
|Declaratieve inrichting | [Azure AD Connect Sync: Inzicht in verklarende inrichtingsexpressies](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)|
|De standaardconfiguratie wijzigen | [Aanbevolen procedures voor het wijzigen van de standaardconfiguratie](how-to-connect-sync-best-practices-changing-default-configuration.md)|

## <a name="configure-federation-features"></a>Federatiekenmerken configureren

Azure AD Connect biedt verschillende functies die een federatie met Azure AD met behulp van AD FS en het beheren van een federatieve vertrouwensrelatie vereenvoudigen. Azure AD Connect ondersteunt AD FS in Windows Server 2012 R2 of later.

[Werk het SSL-certificaat van AD FS-farm bij](how-to-connect-fed-ssl-update.md) zelfs als u Azure AD Connect niet gebruikt om de federatieve vertrouwensrelatie te beheren.

[Voeg een AD FS-server](how-to-connect-fed-management.md#addadfsserver) aan de farm toe om deze uit te breiden zoals vereist.

[Herstel de vertrouwensrelatie](how-to-connect-fed-management.md#repairthetrust) met Azure AD door een paar keer te klikken.

ADFS kan worden geconfigureerd om [meerdere domeinen](how-to-connect-install-multiple-domains.md) te ondersteunen. Zo zijn er mogelijk meerdere populaire domeinen die u wilt gebruiken voor federatie.

Als uw ADFS-server niet is geconfigureerd voor het automatisch bijwerken van certificaten van Azure AD of als u een niet-ADFS-oplossing gebruikt, krijgt u een melding wanneer u [certificaten moet bijwerken](how-to-connect-fed-o365-certs.md).

### <a name="next-steps-to-configure-federation-features"></a>De volgende stappen om federatie-functies te configureren
|Onderwerp |Koppeling|  
| --- | --- |
|Alle AD FS-artikelen | [Azure AD Connect en federatie](how-to-connect-fed-whatis.md)|
|ADFS configureren met subdomeinen | [Ondersteuning voor meerdere domeinen voor federatie met Azure AD](how-to-connect-install-multiple-domains.md)|
|AD FS-farm beheren | [AD FS-beheer en aanpassingen met Azure AD Connect](how-to-connect-fed-management.md)|
|Handmatig bijwerken van de federatiecertificaten | [Federatiecertificaten vernieuwen voor Office 365 en Azure AD](how-to-connect-fed-o365-certs.md)|


## <a name="get-started-with-azure-ad-connect-health"></a>Aan de slag met Azure AD Connect Health
Volg de volgende stappen om aan de slag te gaan met Azure AD Connect Health:

1. [Installeer Azure AD Premium](../fundamentals/active-directory-get-started-premium.md) of [gebruik een proefversie](https://azure.microsoft.com/trial/get-started-active-directory/).
2. [Download en installeer Azure AD Connect Health-agents](#download-and-install-azure-ad-connect-health-agent) op uw servers voor identiteiten.
3. Bekijk het Azure AD Connect Health-dashboard op [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth).

> [!NOTE]
> Houd er rekening mee dat u de Azure AD Connect Health-agents moet installeren op uw doelservers om gegevens te zien in uw Azure AD Connect Health-dashboard.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>De Azure AD Connect Health-agent downloaden en installeren
* Zorg dat u [voldoet aan de vereisten](how-to-connect-health-agent-install.md#requirements) voor Azure AD Connect Health.
* Aan de slag met Azure AD Connect Health voor AD FS
    * [Download de Azure AD Connect Health-agent voor AD FS.](https://go.microsoft.com/fwlink/?LinkID=518973)
    * [Zie de installatie-instructies](how-to-connect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs).
* Aan de slag met Azure AD Connect Health for Sync
    * [Download en installeer de nieuwste versie van Azure AD Connect](https://go.microsoft.com/fwlink/?linkid=615771). De Health for Sync-agent wordt geïnstalleerd als onderdeel van de Azure AD Connect-installatie (versie 1.0.9125.0 of hoger).
* Aan de slag met Azure AD Connect Health voor AD DS
    * [Download de Azure AD Connect Health-agent voor AD DS](https://go.microsoft.com/fwlink/?LinkID=820540).
    * [Zie de installatie-instructies](how-to-connect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-ds).


## <a name="azure-ad-connect-health-portal"></a>Portal voor Azure AD Connect Health
In de portal voor Azure AD Connect Health kunt u waarschuwingen weergeven, de prestaties controleren en het gebruik analyseren. Via de URL https://aka.ms/aadconnecthealth gaat u naar de hoofdblade van Azure AD Connect Health. Een blade kunt u zien als een venster. Op de hoofdblade ziet u **Snel starten**, services in Azure AD Connect Health en extra configuratie-opties. Zie de volgende schermafbeelding en de korte uitleg die daarna wordt gegeven. Nadat u de agents hebt geïmplementeerd, identificeert de Health-service automatisch de services die met Azure AD Connect Health worden bewaakt.

> [!NOTE]
> Raadpleeg de [veelgestelde vragen over Azure AD Connect Health](reference-connect-health-faq.md) of de [Azure AD-pagina met prijzen](https://aka.ms/aadpricing) voor licentie-informatie.
    
![Portal voor Azure AD Connect Health](./media/whatis-hybrid-identity-health/portalsidebar.png)

* **Snel starten**: wanneer u deze optie selecteert, wordt de blade **Snel starten** geopend. U kunt de Azure AD Connect Health-agent downloaden door **Hulpprogramma's downloaden** te selecteren. U kunt ook documentatie bekijken en feedback geven.
* **Azure Active Directory Connect (Sync)** : deze optie laat uw Azure AD Connect-servers zien die momenteel door Azure AD Connect Health worden bewaakt. Onder de vermelding **Synchronisatiefouten** ziet u basissynchronisatiefouten van uw eerste geïmplementeerde synchronisatieservice per categorie. Wanneer u de vermelding **Synchronisatieservices** selecteert, vindt u op de blade die wordt geopend informatie over uw Azure AD Connect-servers. Zie [Azure AD Connect Health for Sync gebruiken](how-to-connect-health-sync.md) voor meer informatie over de mogelijkheden.
* **Active Directory Federation Services**: deze optie laat alle AD FS-services zien die momenteel door Azure AD Connect Health worden bewaakt. Wanneer u een exemplaar selecteert, staat er op de blade die wordt geopend informatie over dat service-exemplaar. Deze informatie omvat een overzicht, eigenschappen, waarschuwingen, controle en gebruiksanalyses. Zie [Azure AD Connect Health gebruiken met AD FS](how-to-connect-health-adfs.md) voor meer informatie over de mogelijkheden.
* **Active Directory Domain Services**: deze optie laat alle AD DS-forests zien die momenteel door Azure AD Connect Health worden bewaakt. Wanneer u een forest selecteert, staat er op de blade die wordt geopend informatie over die forest. Deze informatie bevat een overzicht van essentiële gegevens, een dashboard voor domeincontrollers, een dashboard voor replicatiestatus, waarschuwingen en bewaking. Zie [Azure AD Connect Health gebruiken met AD DS](how-to-connect-health-adds.md) voor meer informatie over de mogelijkheden.
* **Configureren**: dit gedeelte bevat opties voor het uit- of inschakelen van de volgende zaken:

  - De vermelding **Instellingen** bevat basisconfiguraties van de agents. Met de instelling voor automatische upgrade wordt de Azure AD Connect Health-agent automatisch bijgewerkt naar de nieuwste versie: Er wordt automatisch bijgewerkt naar de nieuwste versies van de Azure AD Connect Health-agent zodra deze beschikbaar komen. Deze optie is standaard ingeschakeld. Microsoft alleen toegang geven tot de statusgegevens van uw Azure AD-directory voor het oplossen van problemen: Als deze optie is ingeschakeld, kan Microsoft dezelfde gegevens zien die u ziet. Dit kan handig zijn bij het oplossen van problemen en het verlenen van hulp. Deze optie is standaard uitgeschakeld.
* **Op rollen gebaseerde toegangsbeheer (IAM)** is het gedeelte voor het beheren van de toegang tot Connect Health-gegevens op rolbasis. 

## <a name="next-steps"></a>Volgende stappen

- [Hardware en vereisten](how-to-connect-install-prerequisites.md) 
- [Snelle instellingen](how-to-connect-install-express.md)
- [Aangepaste instellingen](how-to-connect-install-custom.md)
- [Synchronisatie van wachtwoord-hashes](how-to-connect-password-hash-synchronization.md)|
- [Pass-through-verificatie](how-to-connect-pta.md)
- [Azure AD Connect en federatie](how-to-connect-fed-whatis.md)
- [Azure AD Connect Health-agents installeren](how-to-connect-health-agent-install.md) 
- [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md)
