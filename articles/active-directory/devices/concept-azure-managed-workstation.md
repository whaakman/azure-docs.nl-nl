---
title: Informatie over veilige, beheerde Azure-werkstations - Azure Active Directory
description: Meer informatie over veilige, beheerde Azure-werkstations en te begrijpen waarom ze belangrijk zijn.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02a6ddef294c4872f2d7e50e8940ecbb4b4b7bc4
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491598"
---
# <a name="understand-secure-azure-managed-workstations"></a>Veilige, beheerde Azure-werkstations begrijpen

Beveiligde, afgeschermde werkstations zijn uitermate belangrijk voor de beveiliging van gevoelige functies, zoals beheerders, ontwikkelaars en kritieke-operators. Als beveiliging van client-werkstation is geknoeid, kunnen veel besturingselementen voor de beveiliging en zekerheid mislukken of ongeschikt zijn.

Dit document wordt uitgelegd wat u nodig hebt voor het bouwen van een beveiligd werkstation, beter bekend als een privileged access workstation (PAW). Het artikel bevat ook uitgebreide instructies voor het instellen van besturingselementen voor de initiële beveiliging. Hierin wordt beschreven hoe u cloudgebaseerde technologie kunt beheren van de service. Afhankelijk van beveiligingsfuncties die zijn geïntroduceerd in Windows 10RS5, Microsoft Defender Advanced Threat Protection (ATP), Azure Active Directory en Intune.

> [!NOTE]
> In dit artikel wordt uitgelegd dat het concept van een beveiligd werkstation en het belang. Als u al bekend met het concept bent en wilt doorgaan met de implementatie, gaat u naar [implementeren van een werkstation Secure](https://docs.microsoft.com/azure/active-directory/devices/howto-azure-managed-workstation).

## <a name="why-secure-workstation-access-is-important"></a>Waarom beveiligd werkstation toegang belangrijk is

De snelle acceptatie van cloudservices en de mogelijkheid om te werken vanaf elke locatie heeft een nieuwe methode voor het misbruik worden gemaakt. Door misbruik van zwakke beveiligingsmaatregelen op apparaten waar beheerders werkt, kunnen aanvallers toegang krijgen tot beschermde bronnen.

Misbruik van bevoegdheden en supply chain aanvallen zijn tussen de bovenste vijf methoden die aanvallers gebruiken tot een inbreuk op organisaties. Ze zijn ook de tweede meestal tactieken gedetecteerd in incidenten die zijn gerapporteerd in 2018 volgens de [Verizon Threat rapport](https://enterprise.verizon.com/resources/reports/dbir/), en de [Security Intelligence Report](https://aka.ms/sir).

De meeste aanvallers als volgt te werk:

1. Reconnaissance op een manier vinden, vaak specifiek is voor een industrie.
1. Analyse van gegevens kunt verzamelen en identificeren van de beste manier om een werkstation dat wordt beschouwd als lage waarde infiltreren.
1. Persistentie om te zoeken naar een manier om te verplaatsen [zich lateraal](https://en.wikipedia.org/wiki/Network_Lateral_Movement).
1. Exfiltration van vertrouwelijke of gevoelige gegevens.

Tijdens de reconnaissance infiltreren aanvallers vaak apparaten die met een laag risico lijken of ondergewaardeerd. Ze gebruiken deze kwetsbare apparaten te vinden van een verkoopkans voor zijdelingse verplaatsing en zoeken naar gebruikers met beheerdersrechten en -apparaten. Nadat ze toegang tot bevoorrechte rollen krijgen, aanvallers waardevolle gegevens en met succes gegevens te exfiltreren die gegevens identificeren.

![Typische inbreuk patroon](./media/concept-azure-managed-workstation/typical-timeline.png)

Dit document beschrijft een oplossing die uw apparaten kunt beveiligen tegen dergelijke aanvallen laterale. De oplossing worden geïsoleerd management en services van minder belangrijke productiviteit apparaten, de keten belangrijke voordat het apparaat dat toegang tot gevoelige cloudresources heeft kan worden longinfiltraat. De oplossing maakt gebruik van systeemeigen Azure-services die deel van de Microsoft 365 Enterprise-stack uitmaken:

* Intune voor het beheer van apparaten en een lijst met veilige van toepassingen en URL 's
* AutoPilot voor de installatie van apparaat-, implementatie- en vernieuwen
* Azure AD voor beheer van gebruikers, voorwaardelijke toegang en verificatie met meerdere factoren
* Windows 10 (huidige versie) voor de health attestation en gebruikerservaring apparaat
* Defender ATP voor de cloud beheerde endpoint protection, detectie en reactie
* Azure AD PIM voor het beheren van autorisatie en just-in-time (JIT) gemachtigde toegang tot resources

## <a name="who-benefits-from-a-secure-workstation"></a>Die profiteren van een beveiligd werkstation?

Alle gebruikers en operators voordelen bij het gebruik van een beveiligd werkstation. Een aanvaller die wordt aangetast van een computer of apparaat accounts alle in de cache kan worden geïmiteerd. Wanneer u bent aangemeld op het apparaat, kunnen ze ook referenties en tokens gebruikt. Dit risico is het belangrijk om het beveiligen van apparaten die worden gebruikt voor bevoorrechte rollen, met inbegrip van beheerdersrechten. Apparaten met bevoegde accounts zijn doelen voor het type lateral movement en privilege escalatieaanvallen. Deze accounts kunnen worden gebruikt voor een verscheidenheid aan activa, zoals:

* Beheerder van de on-premises of cloud-gebaseerde systemen
* Developer-werkstation voor kritieke systemen
* De accountbeheerder sociale media met een hoge blootstelling
* Uiterst gevoelige werkstation, zoals een terminal SWIFT betaling
* Werkstation verwerking handelsgeheimen

Als risico wilt voorkomen, moet u besturingselementen voor verhoogde beveiliging implementeren voor privileged werkstations die van deze accounts gebruikmaken. Zie voor meer informatie de [Implementatiehandleiding voor Azure Active Directory-functie](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2), [Office 365-roadmap](https://aka.ms/o365secroadmap), en [beveiligingsschema voor bevoegde toegang beveiligen](https://aka.ms/sparoadmap)).

## <a name="why-use-dedicated-workstations"></a>Waarom speciale werkstations gebruiken?

Hoewel het mogelijk om te beveiliging wordt toegevoegd aan een bestaand apparaat, is het beter om te beginnen met een veilige basis. Als uw organisatie in de beste positie voor het onderhouden van een hoog beveiligingsniveau, beginnen met een apparaat dat u weet dat is beveiligd en implementeren van een reeks bekende beveiligingsmechanismen.

Een groeiend aantal aanvalsvectoren via e-mail en surfen op het web maakt het moeilijk om ervoor te zorgen dat een apparaat kan worden vertrouwd. Deze handleiding wordt ervan uitgegaan dat een toegewezen werkstation geïsoleerd van standard productiviteit is, surfen en e-mailbericht. Het verwijderen van productiviteit, het web te surfen en e-mailbericht van een apparaat kan een negatieve invloed hebben op de productiviteit. Deze beveiliging is echter meestal geschikt voor scenario's waarbij de taken niet expliciet vereisen en risico's van een beveiligingsincident is hoog.

> [!NOTE]
> Het web te surfen hier verwijst naar algemene toegang tot willekeurige websites die de activiteit van een hoog risico kan zijn. Dergelijke bladeren verschilt wachtwoorddelen afzonderlijk van een webbrowser gebruiken voor toegang tot een klein aantal bekende beheerwebsites voor services zoals Azure, Office 365, andere cloudproviders en SaaS-toepassingen.

Strategieën voor insluiting de beveiliging door het verhogen van het aantal en type van de besturingselementen die een aanvaller ontmoedigen krijgen toegang tot gevoelige activa. Het model dat wordt beschreven in dit artikel maakt gebruik van een gelaagde bevoegdheden-ontwerp en administratorbevoegdheden beperkt tot specifieke apparaten.

## <a name="supply-chain-management"></a>Toeleveringsbeheer

Essentieel is voor een beveiligd werkstation is een oplossing voor uw toeleveringsketen waarin u een vertrouwde werkstation met de naam 'root van de vertrouwensrelatie'. Voor deze oplossing gebruikmaakt van de hoofdmap van de vertrouwensrelatie [Microsoft Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-autopilot) technologie. Als u wilt beveiligen op een werkstation, kunt Autopilot u gebruikmaken van Microsoft, OEM-geoptimaliseerde Windows 10-apparaten. Deze apparaten worden geleverd met een bekende goede status van de fabrikant. In plaats van de installatiekopie werd teruggezet op een mogelijk onveilig zijn apparaat, Autopilot's kunt gebruiken om een Windows-apparaat in een status 'bedrijfsklare'. Het is van toepassing van instellingen en beleidsregels, apps installeert en zelfs de editie van Windows 10 wordt gewijzigd. Autopilot kan bijvoorbeeld van een apparaat Windows-installatie van Windows 10 Pro naar Windows 10 Enterprise wijzigen zodat deze geavanceerde functies kan gebruiken.

![Beveiligd werkstation niveaus](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Apparaat-functies en -profielen

Deze handleiding verwijst naar verschillende beveiligingsprofielen en functies waarmee u kunnen beter beveiligde oplossingen voor gebruikers, ontwikkelaars en IT-personeel maken. Deze profielen balans tussen bruikbaarheid en risico's voor algemene gebruikers die van een werkstation met verbeterde of beveiligd profiteren kunnen. De instellingen voor configuraties die hier beschikbaar zijn op basis van aanvaarde industrienormen. Deze handleiding laat zien hoe Windows 10 versterken en het risico op inbreuk op apparaat of gebruiker is gekoppeld. Dit gebeurt met behulp van beleid en de technologie voor het beheer van beveiligingsfuncties en -risico's.
![Beveiligd werkstation niveaus](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Lage beveiliging** : een werkstation met beheerde-, standard biedt een goed uitgangspunt voor de meeste thuis en in kleine bedrijven. Deze apparaten zijn geregistreerd bij Azure AD en beheerd met Intune. Dit profiel kan gebruikers toepassingen uitvoeren en elke website bladeren. Een anti-malware-oplossing zoals [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) moet worden ingeschakeld.

* **Verbeterde beveiliging** – deze oplossing op instapniveau, beveiligde is geschikt voor thuisgebruikers, kleine zakelijke gebruikers en algemene ontwikkelaars.

   Het verbeterde werkstation is een manier op basis van beleid om verhoogt de beveiliging van de lage beveiligingsprofiel. Het biedt een veilige manier om te werken met gegevens van de klant, terwijl ook met behulp van hulpprogramma's voor productiviteit, zoals e-mail en het web te surfen. U kunt beleidsregels en Intune gebruiken voor het bewaken van een verbeterde werkstation voor gebruik voor het gedrag en het profiel van gebruiker. U implementeert het profiel verbeterde werkstation met een script voor Windows10 (1809) en het profiteert van het gebruik van geavanceerde schadelijke software protection [geavanceerde Threat Protection (ATP)](https://docs.microsoft.com/office365/securitycompliance/office-365-atp).

* **Hoge beveiliging** – de meest efficiënte manier om te beperken van de kwetsbaarheid voor aanvallen van een werkstation is de mogelijkheid om het werkstation self-administer verwijderen. Lokale beheerdersrechten verwijderen is een stap die verbetert de beveiliging, maar dit kan gevolgen hebben voor productiviteit als onjuist geïmplementeerd. Het profiel hoge beveiliging is gebaseerd op het profiel van de verbeterde beveiliging met een aanzienlijke hoeveelheid wijzigen: het verwijderen van de lokale beheerder. Dit profiel is bedoeld voor profielgebruikers van hoge: leidinggevenden, salaris en gebruikers van gevoelige gegevens, fiatteurs voor services en -processen.

   De gebruiker hoge beveiliging vereist een meer gecontroleerde omgeving terwijl u nog steeds kunt doen, activiteiten, zoals e-mail en browsen in de ervaring van een eenvoudig te gebruiken op het web. De gebruikers verwachten dat functies, zoals cookies, Favorieten en andere snelkoppelingen om te werken. Deze gebruikers kunnen echter niet vereist voor de mogelijkheid om te wijzigen of foutopsporing van hun apparaat. Ze hoeft ook te installeren stuurprogramma's. Het profiel hoge beveiliging is geïmplementeerd met behulp van de hoge beveiliging - script voor Windows10 (1809).

* **Gespecialiseerde** : aanvallers richten ontwikkelaars en IT-beheerders, omdat ze systemen die interessant is voor de aanvallers kunnen wijzigen. Het gespecialiseerde werkstation borduurt voort op het beleid van het werkstation hoge beveiliging door het beheer van lokale toepassingen en websites te beperken. Ook daardoor wordt beperkt met een hoog risico productiviteit-mogelijkheden, zoals ActiveX-, Java, browser-invoegtoepassingen en andere Windows-besturingselementen. U implementeert dit profiel met de DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline script.

* **Beveiligd** : een aanvaller die een Administrator-account wordt aangetast aanzienlijke zakelijke schade door diefstal van gegevens, gegevens wijziging of onderbreking van de service kan veroorzaken. In deze beveiligde status kunt het werkstation alle beveiligingsmaatregelen en beleidsregels die directe controle van beheer van lokale toepassingen beperken. Een beveiligd werkstation heeft geen hulpprogramma's voor productiviteit zodat het moeilijker om inbreuk te apparaat. De meest voorkomende vector voor phishing-aanvallen worden geblokkeerd: e-mail en sociale media.  De beveiligd werkstation kan worden geïmplementeerd met het werkstation beveiligen - Windows10 (1809) SecurityBaseline script.

   ![Beveiligd werkstation](./media/concept-azure-managed-workstation/secure-workstation.png)

   Een beveiligd werkstation biedt een beheerder met een beperkt werkstation dat duidelijk Toepassingsbeheer en application guard is. Het werkstation gebruikt credential guard en device guard exploit guard om te beveiligen van de host van schadelijk gedrag. Alle lokale schijven worden ook versleuteld met BitLocker.

* **Geïsoleerde** : in dit scenario aangepaste, offline vertegenwoordigt het uiteinde van het spectrum krijgen. Geen installatiescripts vindt u in dit geval. U moet mogelijk voor het beheren van een essentiële functie die een niet-ondersteunde of niet-gepatchte verouderde besturingssysteem vereist. Bijvoorbeeld, een hoge waarde productielijn of een levensduur--ondersteuning van system. Omdat beveiliging is van essentieel belang en cloudservices niet beschikbaar zijn, kunt u deze kunt beheren en bijwerken van deze computers handmatig of met een geïsoleerd architectuur Active Directory-forest, zoals de verbeterde beveiliging Admin omgeving (ESAE). In deze omstandigheden, houd rekening met alle toegang, behalve basis Intune en ATP statuscontroles verwijderen.

  * [Vereiste communicatie Intune](https://docs.microsoft.com/intune/network-bandwidth-use)
  * [Vereiste communicatie ATP](https://docs.microsoft.com/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Volgende stappen

[Implementeren van een beveiligde Azure-beheerd werkstation](howto-azure-managed-workstation.md).
