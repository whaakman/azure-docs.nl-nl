---
title: "Azure Active Directory Domain Services: Implementatiescenario's | Microsoft Docs"
description: Implementatiescenario's voor Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/21/2017
ms.author: ergreenl
ms.openlocfilehash: 0659586512b36c51c5058271fa5e1bdb46efbc3b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193281"
---
# <a name="deployment-scenarios-and-use-cases"></a>Implementatiescenario's en gebruik
In deze sectie kijken we enkele scenario's en use cases die baat bij Azure Active Directory (AD) Domain Services hebben.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Veilig en eenvoudig beheer van virtuele machines van Azure
U kunt Azure Active Directory Domain Services gebruiken voor het beheren van uw Azure virtual machines op een gestroomlijnde manier. Virtuele machines van Azure kunnen worden toegevoegd aan het beheerde domein, zodat u uw zakelijke AD-referenties gebruiken om aan te melden. Deze aanpak helpt te voorkomen dat referentie management gedoe zoals het beheren van lokale administrator-accounts op elk van uw Azure virtual machines.

Server virtuele machines die zijn gekoppeld aan het beheerde domein kunnen ook worden beheerd en beveiligd met behulp van Groepsbeleid. U kunt de vereiste basislijnen toepassen op uw Azure virtual machines en vergrendelen in overeenstemming met de richtlijnen voor zakelijke beveiliging. Bijvoorbeeld, kunt u mogelijkheden voor groepsbeheer beleid om te beperken de typen toepassingen die kunnen worden gestart op deze virtuele machines.

![Gestroomlijnd beheer van virtuele machines van Azure](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Zoals servers en andere infrastructuur einde van de levenscyclus is bereikt, is Contoso verplaatsen van veel toepassingen die momenteel on-premises naar de cloud worden gehost. De huidige IT-standaard mandaten dat de servers die als host fungeert voor zakelijke toepassingen zijn moeten domein en beheerd via Groepsbeleid. Contoso domein lid worden van virtuele machines die zijn geïmplementeerd in Azure, IT-beheerder liever beheer te vereenvoudigen. Als gevolg hiervan, beheerders en gebruikers kunnen zich aanmelden met hun bedrijfsreferenties. Machines op hetzelfde moment, kunnen worden geconfigureerd om te voldoen aan de vereiste basislijnen met behulp van Groepsbeleid. Contoso zou liever niet hoeft te implementeren, bewaken en beheren van domeincontrollers in Azure voor het beveiligen van virtuele machines van Azure. Azure AD Domain Services is daarom uitstek geschikt voor deze use case.

**Opmerkingen bij de implementatie**

Houd rekening met de volgende belangrijke punten voor deze implementatiescenario:

* Beheerde domeinen die worden geleverd door Azure AD Domain Services bieden een enkele platte OU (organisatie-eenheid) structuur standaard. Alle domein-machines zich bevinden in een enkele platte organisatie-eenheid. U kunt echter aangepaste OE's maken.
* Azure AD Domain Services biedt ondersteuning voor eenvoudige Groepsbeleid in de vorm van een ingebouwde GPO elke voor de gebruikers en computers containers. U kunt aangepaste GPO's maken en toepast op aangepaste organisatie-eenheden.
* Azure AD Domain Services biedt ondersteuning voor de basis AD computer-object-schema. U kunt het computerobject schema niet uitbreiden.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Lift-and-shift een on-premises toepassing die gebruikmaakt van LDAP-binding voor verificatie met Azure Infrastructure Services
![LDAP-binding](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso heeft een on-premises toepassing die is aangeschaft via een ISV jaren geleden. De toepassing is op dit moment in de onderhoudsmodus bevindt door de ISV en aanvragen van wijzigingen in de toepassing is beperkend duur voor Contoso. Deze toepassing is een webgebaseerde frontend die worden verzameld met behulp van een webformulier gebruikersreferenties en vervolgens gebruikers worden geverifieerd door het uitvoeren van een LDAP-binding aan de zakelijke Active Directory. Contoso wilt migreren van deze toepassing met Azure Infrastructure Services. Is het wenselijk dat de toepassing is, zonder wijzigingen werkt. Daarnaast gebruikers kunt verifiëren met hun bestaande bedrijfsreferenties moet hoeven gebruikers handelingen anders trainen. Met andere woorden, eindgebruikers moet oblivious van waar de toepassing wordt uitgevoerd en de migratie moet worden transparant voor hen.

**Opmerkingen bij de implementatie**

Houd rekening met de volgende belangrijke punten voor deze implementatiescenario:

* Zorg ervoor dat de toepassing hoeft niet te wijzigen/schrijven naar de map. LDAP-schrijftoegang tot de beheerde domeinen die worden geleverd door Azure AD Domain Services wordt niet ondersteund.
* U kunt wachtwoorden rechtstreeks aan de hand van het beheerde domein niet wijzigen. Eindgebruikers kunnen hun wachtwoord kunt wijzigen met behulp van Azure AD-mechanisme voor self-service voor wachtwoord wijzigen of op basis van de on-premises directory. Deze wijzigingen worden automatisch gesynchroniseerd en beschikbaar zijn in het beheerde domein.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Lift-and-shift een on-premises toepassing die gebruikmaakt van LDAP lezen voor toegang tot de map met Azure Infrastructure Services
Contoso heeft een on-premises line-of-business (LOB)-toepassing die bijna een decennium geleden is ontwikkeld. Deze toepassing is op de hoogte-map en is ontworpen voor gebruik met Windows Server AD. De toepassing gebruikt LDAP (Lightweight Directory Access Protocol) te lezen van gegevens/kenmerken voor gebruikers uit Active Directory. De toepassing niet wijzigen van kenmerken of anderszins in de map schrijven. Contoso wilt migreren van deze toepassing met Azure Infrastructure Services en de ouderdom on-premises hardware momenteel die als host fungeert van deze toepassing buiten gebruik stellen. De toepassing kan niet worden herschreven voor het gebruik van moderne directory API's, zoals de Azure AD Graph REST gebaseerde API. Daarom is een optie voor lift-and-shift gewenste waarbij de toepassing kan worden gemigreerd om uit te voeren in de cloud, zonder code te wijzigen of het herschrijven van de toepassing.

**Opmerkingen bij de implementatie**

Houd rekening met de volgende belangrijke punten voor deze implementatiescenario:

* Zorg ervoor dat de toepassing hoeft niet te wijzigen/schrijven naar de map. LDAP-schrijftoegang tot de beheerde domeinen die worden geleverd door Azure AD Domain Services wordt niet ondersteund.
* Zorg ervoor dat de toepassing hoeft niet een aangepaste/uitgebreide Active Directory-schema. Schema-uitbreidingen worden niet ondersteund in Azure AD Domain Services.

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Een on-premises service of -daemon-toepassing met Azure Infrastructure Services migreren
Sommige toepassingen bestaan uit meerdere lagen, waar een van de lagen moet zijn om uit te voeren geverifieerde aanroepen naar een back endlaag, zoals een database-laag. Active Directory-service-accounts worden vaak gebruikt voor deze use cases. U kunt lift-and-shift dergelijke toepassingen op Azure Infrastructure Services en het gebruik van Azure AD Domain Services voor de behoeften van de identiteit van deze toepassingen. U kunt dezelfde serviceaccount die is gesynchroniseerd vanuit uw on-premises directory met Azure AD gebruiken. U kunt ook eerst een aangepaste organisatie-eenheid maken en maak vervolgens een afzonderlijk serviceaccount in die organisatie-eenheid, om deze toepassingen te implementeren.

![Met behulp van WIA-serviceaccount](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso heeft een op maat gemaakte software kluis-toepassing die bestaat uit een webfront-end, een SQL-server en een back-FTP-endserver. Geïntegreerde Windows-verificatie van service-accounts wordt gebruikt voor verificatie van de web-front-end met de FTP-server. De web-front-end is ingesteld om uit te voeren als een service-account. De back-endserver is geconfigureerd voor het toestaan van toegang van het serviceaccount voor de web-front-end. Contoso liever niet te implementeren van een domein netwerkcontroller-VM in de cloud om deze toepassing met Azure Infrastructure Services te verplaatsen. Het Contoso IT-beheerder de servers die als host fungeert voor de web-front-end, SQL server en de FTP-server op virtuele machines van Azure kunt implementeren. Deze machines zijn vervolgens gekoppeld aan een beheerd domein van Azure AD Domain Services. Vervolgens kunnen ze hetzelfde serviceaccount gebruiken in hun on-premises directory voor verificatie van de app. Deze serviceaccount wordt gesynchroniseerd met de Azure AD Domain Services beheerde domein en is beschikbaar voor gebruik.

**Opmerkingen bij de implementatie**

Houd rekening met de volgende belangrijke punten voor deze implementatiescenario:

* Zorg ervoor dat de toepassing maakt gebruik van gebruikersnaam en wachtwoord voor verificatie. Certificaat/Smartcard op basis van verificatie wordt niet ondersteund door Azure AD Domain Services.
* U kunt wachtwoorden rechtstreeks aan de hand van het beheerde domein niet wijzigen. Eindgebruikers kunnen hun wachtwoord kunt wijzigen met behulp van Azure AD-mechanisme voor self-service voor wachtwoord wijzigen of op basis van de on-premises directory. Deze wijzigingen worden automatisch gesynchroniseerd en beschikbaar zijn in het beheerde domein.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Windows Server extern bureaublad-services-implementaties in Azure
U kunt Azure AD Domain Services gebruiken voor beheerde AD domain services voor uw extern bureaublad-servers geïmplementeerd in Azure.

Zie voor meer informatie over dit implementatiescenario, hoe u [Azure AD Domain Services integreren met uw implementatie van extern bureaublad-services](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).


## <a name="domain-joined-hdinsight-clusters-preview"></a>Aan domein gekoppelde HDInsight-clusters (Preview)
U kunt een Azure HDInsight-cluster dat is gekoppeld aan een beheerd domein van Azure AD Domain Services met Apache Ranger ingeschakeld instellen. Maken en toepassen via Apache Ranger Hive-beleid en toestaan dat gebruikers (bijvoorbeeld gegevenswetenschappers) verbinding maken met Hive ODBC-hulpprogramma's, zoals Excel, Tableau enzovoort gebruiken. Microsoft is bezig met toevoegen van andere werkbelastingen, zoals HBase en Spark, Storm, aan Domain-joined HDInsight binnenkort.

Zie voor meer informatie over dit implementatiescenario, hoe u [aan domein gekoppelde HDInsight-clusters configureren](../hdinsight/domain-joined/apache-domain-joined-configure.md)
