---
title: Informatie over Azure Lab Services | Microsoft Docs
description: Lees hoe u met Lab Services gemakkelijk labs kunt maken, beheren en beveiligen met virtuele machines die kunnen worden gebruikt door ontwikkelaars, testers, docenten, studenten en anderen.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/13/2018
ms.author: spelluru
ms.openlocfilehash: 2f4bc3bc00d1a803ed678e49df23a78e6b063e50
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957790"
---
# <a name="an-introduction-to-azure-lab-services"></a>Een inleiding tot Azure Lab Services

Met Azure Lab Services kunt u snel in de cloud een omgeving inrichten voor uw team (zoals een ontwikkelomgeving, testomgeving of labomgeving voor het leslokaal). De eigenaar van een lab maakt een lab, richt virtuele machines met Windows of Linux in, installeert de benodigde software en hulpprogramma's en maakt deze beschikbaar voor gebruikers van het lab. Labgebruikers maken verbinding met virtuele machines (VM's) in het lab en gebruiken deze voor hun dagelijkse werk, kortlopende projecten of voor het uitvoeren van oefeningen in het leslokaal. Zodra gebruikers aan de slag gaan met resources in het lab, kan een beheerder van het lab kosten en gebruik voor meerdere labs analyseren en overkoepelend beleid instellen om de kosten van uw organisatie of het team te optimaliseren.

> [!IMPORTANT]
> **Azure DevTest Labs** wordt uitgebreid met nieuwe typen labs (Azure Lab Services).
>  
> Met Azure Lab Services kunt u beheerde labs maken, zoals labs voor het leslokaal. De service zelf is verantwoordelijk voor het volledige beheer van de infrastructuur voor een beheerd lab, van het inrichten van VM's tot de afhandeling van fouten, en het schalen van de infrastructuur. De beheerde labs zijn momenteel beschikbaar als preview. Als de preview is afgelopen, komen de nieuwe labtypen en bestaande DevTest Labs beschikbaar onder de nieuwe algemene overkoepelende naam van Azure Lab Services. Hier worden alle labtypen voortdurend verbeterd.

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Azure Lab Services ondersteunt de volgende belangrijke mogelijkheden en-functies:

- **Snelle en flexibele installatie van een lab**. Met Azure Lab Services kunnen labeigenaren snel een lab instellen voor hun behoeften. De service biedt voor beheerde labs de mogelijkheid om alle beheertaken voor de Azure-infrastructuur uit te besteden. Eigenaren van een lab kunnen er echter ook voor kiezen om de infrastructuur zelf te beheren en aan te passen in het abonnement van de labeigenaar. De service biedt ingebouwde schaalbaarheid en tolerantie van de infrastructuur voor labs die de service voor u beheert.
- **Vereenvoudigde ervaring voor labgebruikers**. In een beheerd lab, zoals een lab voor het leslokaal, kunnen labgebruikers zich met een registratiecode registreren bij een lab en het lab vervolgens op elk gewenst moment openen om de resources van het lab te gebruiken. In een lab dat in DevTest Labs is gemaakt, kan de eigenaar van een lab machtigingen verlenen aan labgebruikers voor het maken en gebruiken van virtuele machines, het beheren en het gebruiken van gegevensschijven en het instellen van herbruikbare geheimen.  
- **Optimalisatie van kosten en analyse**. De eigenaar van een lab kan planningen voor het lab instellen om virtuele machines automatisch af te sluiten en op te starten. De eigenaar van het lab kan een planning instellen om de tijdstippen op te geven wanneer de virtuele machines van het lab toegankelijk zijn voor gebruikers. De eigenaar kan bovendien een gebruiksbeleid per gebruiker of per lab instellen om de kosten te optimaliseren, en om gebruiks- en activiteitstrends te analyseren. Voor beheerde labs zoals leslokaal-labs is momenteel een kleinere subset met opties voor kostenoptimalisatie en -analyse beschikbaar.
- **Ingesloten beveiliging**. De eigenaar van een lab kan particuliere virtuele netwerken en een subnet inrichten voor een lab, en een gedeeld openbaar IP-adres inschakelen. Labgebruikers hebben op een veilige manier toegang tot resources met behulp van een virtueel netwerk dat is geconfigureerd met ExpressRoute of site-naar-site-VPN. (momenteel alleen beschikbaar in DevTest Labs)
- **Integratie met uw werkstromen en hulpprogramma's**. Met behulp van Azure Lab Services kunt u de labs integreren met de website en beheersystemen van uw organisatie. U kunt vanuit uw hulpprogramma's voor continue integratie/continue implementatie (CI/CD) automatisch omgevingen inrichten. (momenteel alleen beschikbaar in DevTest Labs)

> [!NOTE]
> Azure Lab Services ondersteunt momenteel alleen VM's die zijn gemaakt op basis van Microsoft Azure Marketplace-installatiekopieën. Als u aangepaste installatiekopieën wilt gebruiken of andere PaaS-resources wilt maken in een testomgeving, gebruikt u DevTest Labs. Zie [Een aangepaste installatiekopie maken in DevTest Labs](devtest-lab-create-custom-image-from-vm-using-portal.md) en [Labomgevingen maken met behulp van Resource Manager-sjablonen maken](devtest-lab-create-environment-from-arm.md) voor meer informatie.

## <a name="scenarios"></a>Scenario's

Hier volgen enkele van de scenario's die worden ondersteund door Azure Lab Services:

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Inrichten van een schaalbaar computerlab in de cloud voor uw leslokaal  

- Maak een beheerd lab voor het leslokaal. U geeft precies aan wat u nodig hebt en de service zorgt er vervolgens voor dat de infrastructuur van het lab wordt gemaakt en beheerd. Hierdoor kunt u zich volledig richten op uw leerlingen of studenten en hoeft u zich niet bezig te houden met de technische details van een lab.
- Bied leerlingen en studenten een lab met virtuele machines die zijn geconfigureerd met exact wat nodig is voor een les of college. Geef elke leerling of student een beperkt aantal uren voor het gebruik van de VM's voor het uitvoeren van hun taken.  
- Breng het fysieke computerlab van uw school over naar de cloud. U kunt het aantal VM's automatisch schalen tot de drempelwaarde voor gebruik en kosten die u instelt voor het lab.
- Verwijder het lab met één muisklik als u klaar bent.

### <a name="use-devtest-labs-for-development-environments"></a>DevTest Labs gebruiken voor ontwikkelomgevingen

Azure DevTest Labs kan worden gebruikt voor het implementeren van allerlei scenario's. Eén van de belangrijkste scenario's is het inzetten van DevTest Labs voor het hosten van ontwikkelmachines voor ontwikkelaars. In dit scenario biedt DevTest Labs deze voordelen:

- Ontwikkelaars kunnen hun ontwikkelmachines snel op aanvraag inrichten.
- U kunt Windows- en Linux-omgevingen inrichten met behulp van herbruikbare sjablonen en artefacten.
- Ontwikkelaars kunnen hun ontwikkelmachines eenvoudig aanpassen als dat nodig is.
- Beheerders kunnen de kosten beheersen door ervoor te zorgen dat ontwikkelaars niet meer VM's krijgen toegewezen dan dat ze nodig hebben voor ontwikkeling en dat VM's worden afgesloten wanneer deze niet in gebruik zijn.

Zie [Use Azure DevTest Labs for developers](devtest-lab-developer-lab.md) (Azure DevTest Labs gebruiken voor ontwikkelaars) voor meer informatie.

### <a name="use-devtest-labs-for-test-environments"></a>DevTest Labs gebruiken voor testomgevingen

U kunt Azure DevTest Labs gebruiken voor het implementeren van allerlei scenario's. Eén van de belangrijkste scenario's is het inzetten van DevTest Labs voor het hosten van machines voor testers. In dit scenario biedt DevTest Labs deze voordelen:

- Testers kunnen de nieuwste versie van hun toepassing testen door Windows- en Linux-omgevingen snel in te richten met behulp van herbruikbare sjablonen en artefacten.
- Testers kunnen hun belastingstests omhoog schalen door meerdere test-agents in te richten.
- Beheerders kunnen de kosten beheersen door ervoor te zorgen dat testers niet meer VM's krijgen toegewezen dan dat ze nodig hebben voor testen en dat VM's worden afgesloten wanneer deze niet in gebruik zijn.

Zie [Use Azure DevTest Labs for testing](devtest-lab-test-env.md) (Azure DevTest Labs gebruiken voor testen) voor meer informatie.

## <a name="user-profiles"></a>Gebruikersprofielen

In dit artikel worden de verschillende gebruikersprofielen in Azure Lab Services beschreven.

### <a name="lab-account-owner"></a>Labeigenaar

Meestal fungeert een IT-beheerder van de cloudresources van de organisatie, die ook eigenaar is van het Azure-abonnement, als de eigenaar van een lab-account. Deze persoon is verantwoordelijk voor de volgende taken:

- Een lab-account instellen voor uw organisatie.
- Beleidsregels beheren en configureren voor alle labs.
- Machtigingen verlenen aan personen binnen de organisatie voor het maken van een lab onder het lab-account.

### <a name="lab-creator"></a>Labmaker

Normaal gesproken kunnen gebruikers zoals de leider of manager van een ontwikkelteam, een leraar of docent, de organisator van een hackathon of een onlinetrainer labs maken onder een lab-account. Een labmaker is verantwoordelijk voor de volgende taken:

- Een lab maken.
- Virtuele machines maken in het lab.
- De juiste software installeren op virtuele machines.
- Instellen wie toegang heeft tot het lab.
- Een koppeling naar het lab verstrekken aan labgebruikers.

### <a name="lab-user"></a>Labgebruiker

Een labgebruiker is verantwoordelijk voor de volgende taken:

- De registratiekoppeling volgen die is verstuurd door een labmaker om zich te registreren bij het lab.
- Verbinding maken met een virtuele machine in het lab en deze gebruiken voor ontwikkeling, testen of school- of studiewerk.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met het installeren van een lab met Azure Lab Services:

- [Een leslokaallab instellen](classroom-labs/tutorial-setup-classroom-lab.md)
- [Een lab instellen](tutorial-create-custom-lab.md)
