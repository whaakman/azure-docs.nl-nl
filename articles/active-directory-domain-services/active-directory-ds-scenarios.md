---
title: "Azure Active Directory Domain Services: Implementatiescenario's voor | Microsoft Docs"
description: Implementatiescenario's voor Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: maheshu
ms.openlocfilehash: be8d565ea16849302b743e5619afd3f21703bd1e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36218683"
---
# <a name="deployment-scenarios-and-use-cases"></a>Implementatiescenario's en gebruik
In deze sectie bekijken we enkele scenario's en use cases die van Azure Active Directory (AD) Domain Services profiteren.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Veilig en eenvoudig beheer van virtuele machines in Azure
Azure Active Directory Domain Services kunt u uw virtuele Azure-machines op een gestroomlijnde manier beheren. Azure virtuele machines kunnen worden gekoppeld aan het beheerde domein, zodat u uw zakelijke AD-referenties gebruiken om aan te melden. Deze aanpak voorkomt referentie management problemen zoals het beheren van lokale administrator-accounts op elk van uw virtuele machines in Azure.

Server virtuele machines die zijn gekoppeld aan het beheerde domein kan ook worden beheerd en beveiligd met behulp van Groepsbeleid. U kunt de mate van beveiligingsbasislijnen toepassen op uw virtuele machines in Azure en vergrendelen overeenkomstig de richtlijnen voor zakelijke beveiliging. Bijvoorbeeld, kunt u mogelijkheden voor groepsbeheer beleid om te beperken welke typen toepassingen die op deze virtuele machines kunnen worden gestart.

![Gestroomlijnd beheer van virtuele machines in Azure](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Als de servers en andere infrastructuur bereikt einde van de levenscyclus, verplaatst Contoso veel toepassingen momenteel gehost op een lokaal naar de cloud. De huidige IT-standaard vereist dat de servers die als host fungeert voor zakelijke toepassingen zijn moeten lid van een domein en beheerd via Groepsbeleid. Contoso domein join virtuele machines zijn geïmplementeerd in Azure, IT-beheerder liever om beheer te vereenvoudigen. Hierdoor kunnen kunnen beheerders en gebruikers aanmelden met hun bedrijfsreferenties. Op hetzelfde moment kunnen machines worden geconfigureerd om te voldoen aan vereiste basislijnen met Groepsbeleid. Contoso liever niet te implementeren, bewaken en beheren van domeincontrollers in Azure voor het beveiligen van virtuele machines in Azure. Azure AD Domain Services is daarom een geweldige geschikt is voor deze gebruiksvoorbeeld.

**Opmerkingen bij de implementatie**

Houd rekening met de volgende belangrijke punten voor dit implementatiescenario:

* Beheerde domeinen die worden geleverd door Azure AD Domain Services bieden een enkele platte eenheidstructuur van organisatie-(organisatie-eenheid) standaard. Alle domein-machines zich bevinden in één OE platte. U kunt echter aangepaste OE's maken.
* Azure AD Domain Services biedt ondersteuning voor eenvoudige Groepsbeleid in de vorm van een ingebouwde GPO elke voor gebruikers en computers containers. U kunt aangepaste groepsbeleidsobjecten te maken en deze aangepaste organisatie-eenheden.
* Het AD computer object basisschema biedt ondersteuning voor Azure AD Domain Services. U kunt het computerobject schema niet uitbreiden.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Lift-en-shift een on-premises toepassing die gebruikmaakt van verificatie van LDAP-binding voor Azure-infrastructuurservices
![LDAP-binding](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso heeft een on-premises toepassing die is aangeschaft via een ISV lang geleden. De toepassing is op dit moment in de onderhoudsmodus door de ISV en wijzigingen aanvragen op de toepassing is beperkend dure voor Contoso. Deze toepassing is een webgebaseerde frontend die met behulp van een webformulier gebruikersreferenties verzamelt en vervolgens gebruikers worden geverifieerd door het uitvoeren van een LDAP-binding aan de zakelijk Active Directory. Contoso wil deze toepassing om Azure Infrastructure Services te migreren. Is het wenselijk dat de toepassing werkt zoals is zonder wijzigingen. Daarnaast gebruikers moet verifiëren met hun bestaande bedrijfsreferenties en zonder opnieuw trainen gebruikers anders dingen doen. Met andere woorden, eindgebruikers moet oblivious van waar de toepassing wordt uitgevoerd en de migratie moet transparante toe.

**Opmerkingen bij de implementatie**

Houd rekening met de volgende belangrijke punten voor dit implementatiescenario:

* Zorg ervoor dat de toepassing hoeft niet te wijzigen/schrijven naar de map. LDAP-schrijftoegang tot de beheerde domeinen die worden geleverd door Azure AD Domain Services wordt niet ondersteund.
* U kunt wachtwoorden rechtstreeks met het beheerde domein niet wijzigen. Eindgebruikers kunnen ofwel hun wachtwoord wijzigen met behulp van Azure AD-mechanisme voor selfservice voor wachtwoordherstel wijzigen of op basis van de on-premises directory. Deze wijzigingen worden automatisch gesynchroniseerd en beschikbaar zijn in het beheerde domein.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Lift en shift een on-premises toepassing die gebruikmaakt van LDAP lezen voor toegang tot de map voor Azure-infrastructuurservices
Contoso heeft een on-premises line-of-business (LOB)-toepassing die is ontwikkeld bijna een tien jaar geleden. Deze toepassing is op de hoogte-map en is ontworpen voor gebruik met Windows Server AD. De toepassing gebruikt LDAP (Lightweight Directory Access Protocol) om te lezen van gegevens/kenmerken over gebruikers uit Active Directory. De toepassing niet kenmerken wijzigen of anders naar de map schrijven. Contoso wil deze toepassing om Azure Infrastructure Services te migreren en de veroudering lokale hardware momenteel host deze toepassing buiten gebruik stellen. De toepassing kan niet worden herschreven voor het gebruik van moderne directory API's zoals de REST gebaseerde Azure AD Graph API. Daarom is een optie lift en shift gewenste waarbij de toepassing om te worden uitgevoerd in de cloud, zonder de code te wijzigen of het herschrijven van de toepassing kan worden gemigreerd.

**Opmerkingen bij de implementatie**

Houd rekening met de volgende belangrijke punten voor dit implementatiescenario:

* Zorg ervoor dat de toepassing hoeft niet te wijzigen/schrijven naar de map. LDAP-schrijftoegang tot de beheerde domeinen die worden geleverd door Azure AD Domain Services wordt niet ondersteund.
* Zorg ervoor dat de toepassing niet een aangepaste/uitgebreide Active Directory-schema hoeft. Schema-uitbreidingen worden niet ondersteund in Azure AD Domain Services.

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Migreren van een on-premises service of -daemon-toepassing voor Azure-infrastructuurservices
Sommige toepassingen bestaan uit meerdere lagen, waarbij een van de lagen nodig zijn voor geverifieerde aanroepen in een back-end-laag, zoals een databaselaag. Active Directory-service-accounts worden meestal gebruikt voor deze gebruiksvoorbeelden. U kunt lift en shift dergelijke toepassingen op Azure Infrastructure Services en het gebruik van Azure AD Domain Services voor de behoeften van de identiteit van deze toepassingen. U kunt hetzelfde serviceaccount die worden gesynchroniseerd vanuit uw on-premises directory naar Azure AD gebruiken. U kunt ook eerst een aangepaste organisatie-eenheid maken en maak vervolgens een afzonderlijk serviceaccount in organisatie-eenheid, om deze toepassingen te implementeren.

![Met behulp van WIA-serviceaccount](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso heeft een op maat gemaakte kluis softwaretoepassing die bestaat uit een webfront-end, een SQL-server en een back-voor FTP-endserver. Geïntegreerde Windows-verificatie van serviceaccounts wordt gebruikt voor verificatie van de webfront-end voor de FTP-server. De webfront-end is uit te voeren als een service-account ingesteld. De back-endserver is geconfigureerd voor het verlenen van toegang van het serviceaccount voor de web-front-end. Contoso liever niet hebben een domain controller virtuele machine in de cloud te verplaatsen van deze toepassing om Azure Infrastructure Services te implementeren. Contoso IT-beheerder de servers die als host fungeert voor de web-front-end, SQL server en de FTP-server op Azure virtuele machines kunt implementeren. Deze machines worden vervolgens toegevoegd aan een beheerd domein van Azure AD Domain Services. Ze kunnen vervolgens dezelfde serviceaccount gebruiken in hun on-premises directory voor verificatie van de app. Deze serviceaccount is gesynchroniseerd met het beheerde domein van Azure AD Domain Services en is beschikbaar voor gebruik.

**Opmerkingen bij de implementatie**

Houd rekening met de volgende belangrijke punten voor dit implementatiescenario:

* Zorg ervoor dat de toepassing gebruikmaakt van gebruikersnaam en wachtwoord voor verificatie. Certificaat/op basis van smartcardverificatie wordt niet ondersteund door Azure AD Domain Services.
* U kunt wachtwoorden rechtstreeks met het beheerde domein niet wijzigen. Eindgebruikers kunnen ofwel hun wachtwoord wijzigen met behulp van Azure AD-mechanisme voor selfservice voor wachtwoordherstel wijzigen of op basis van de on-premises directory. Deze wijzigingen worden automatisch gesynchroniseerd en beschikbaar zijn in het beheerde domein.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Implementaties in Azure voor Windows Server extern bureaublad-services
Azure AD Domain Services kunt u beheerde AD domain services bieden aan uw extern bureaublad-servers geïmplementeerd in Azure.

Zie voor meer informatie over dit implementatiescenario hoe [Azure AD Domain Services integreren in uw implementatie RDS](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).


## <a name="domain-joined-hdinsight-clusters-preview"></a>HDInsight-clusters domein (Preview)
U kunt een Azure HDInsight-cluster die is gekoppeld aan een beheerd domein van Azure AD Domain Services met Apache Zwerver ingeschakeld instellen. Maken en Hive beleidsregels via Apache Zwerver en toestaan dat gebruikers (bijvoorbeeld gegevenswetenschappers) verbinding maken met Hive ODBC-hulpprogramma's, zoals Excel, Tableau enzovoort met. Microsoft werkt over het toevoegen van andere werkbelastingen verwerken, zoals HBase, Spark en Storm, aan het domein HDInsight snel.

Zie voor meer informatie over dit implementatiescenario hoe [HDInsight-clusters domein configureren](../hdinsight/domain-joined/apache-domain-joined-configure.md)
