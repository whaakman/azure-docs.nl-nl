---
title: Waarom beveiligde werkstations belangrijk zijn - Azure Active Directory
description: Ontdek waarom organisaties beveiligde Azure beheerde werkstations moeten maken
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
ms.openlocfilehash: 05d21910d1b3601346fbd038cbc25f8f2be61f99
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110705"
---
# <a name="building-secure-workstations"></a>Het bouwen van veilige werkstations

Beveiligde geïsoleerde werkstations zijn uitermate belangrijk voor de beveiliging van gevoelige functies, zoals beheerders, ontwikkelaars en exploitanten van kritieke services. Veel andere besturingselementen voor de beveiliging en zekerheid mislukt of hebben geen effect als de onderliggende beveiliging van de client-werkstation is aangetast.

Dit document wordt uitgelegd wat u nodig hebt om te maken van een beveiligde client-werkstation met gedetailleerde stapsgewijze instructies over het instellen van het starten van beveiligingsmaatregelen zoals. Dit type werkstations soms is een werkstation met bevoegde toegang (PAW), dat deze referentie wordt gebruikt en die zijn gebouwd met genoemd. De richtlijnen echter uiterlijk voor cloud-gebaseerde technologie voor het beheren van de service en beveiligingsmogelijkheden die is geïntroduceerd in Windows 10RS5, Microsoft Defender ATP, Azure Active Directory en Intune vanaf introduceert.

> [!NOTE]
> In dit artikel wordt uitgelegd dat het concept van beveiligde werkstation en het belang. Als u al bekend met het concept bent en wilt overslaan voor distributie, Lees [over het implementeren van een werkstation Secure](https://docs.microsoft.com/azure/active-directory/devices/howto-azure-managed-workstation).

## <a name="why-securing-workstation-access-is-important"></a>Waarom werkstation toegang beveiligen is belangrijk

De snelle acceptatie van cloudservices en de mogelijkheid om te werken vanaf elke locatie heeft een nieuwe methode voor misbruik worden gemaakt. Aanvallers misbruik van zwakke beveiligingsmaatregelen op apparaten waar beheerders werken en toegang te krijgen tot beschermde resources kunnen maken.

Zoals beschreven in de [Verizon Threat rapport](https://enterprise.verizon.com/resources/reports/dbir/), en [Security Intelligence Report](https://aka.ms/sir) misbruik van bevoegdheden en supply chain aanvallen zijn een van de top vijf mechanismen waarmee organisaties, inbreuk en de meestal tweede tactiek gedetecteerd in incidenten die zijn gerapporteerd in 2018.

De meeste aanvallers Volg de onderstaande locatie:

* Beginnen met reconnaissance, vaak specifiek is voor een industrie, in een manier vinden
* Analyseren van verzamelde gegevens voor het identificeren van de beste manier toegang krijgen (binnendringen) van een werkstation waargenomen lage waarde
* Persistentie en kijken om te verplaatsen [zich lateraal](https://en.wikipedia.org/wiki/Network_Lateral_Movement)
* Gegevens te Exfiltreren vertrouwelijke of gevoelige gegevens

Aanvallers infiltreren vaak apparaten die met een laag risico lijken of ondergewaardeerd voor reconnaissance. Deze kwetsbare apparaten worden vervolgens gebruikt voor het vinden van een verkoopkans voor zijdelingse verplaatsing, administratieve gebruikers en apparaten zoeken en identificeren van hoge waardevolle gegevens, is gegevens te exfiltreren informatie wanneer ze deze rollen bevoegde gebruiker krijgen.

![Typische inbreuk patroon](./media/concept-azure-managed-workstation/typical-timeline.png)

Dit document biedt een oplossing voor uw apparaten beschermen door het isoleren van beheer en -services te beschermen tegen zijdelingse verplaatsing of aanvallen vanaf apparaten met minder kostbare productiviteit. Het ontwerp vermindert de mogelijkheid om uit te voeren is er inbreuk wordt gepleegd door het verbreken van de keten voorafgaand aan het binnendringen van het apparaat gebruikt om te beheren of toegang krijgt tot gevoelige cloudresources. De oplossing wordt beschreven, maken gebruik van systeemeigen Azure-services die deel van de Microsoft 365 Enterprise stack, waaronder uitmaken:

* Intune voor het beheer van apparaten, met inbegrip van de toepassing en de URL in een whitelist opnemen
* AutoPilot voor configuratie van het apparaat en de implementatie en vernieuwen 
* Azure AD voor beheer van gebruikers, voorwaardelijke toegang en verificatie met meerdere factoren
* Windows 10 (huidige versie) voor de health attestation en gebruikerservaring apparaat
* Microsoft Defender Advanced Threat Protection (ATP) voor endpoint protection, detectie en reactie met cloudbeheer
* Azure AD PIM voor het beheren van autorisatie, waaronder alleen In tijd (JIT) gemachtigde toegang tot resources

## <a name="who-benefit-from-using-a-secure-workstation"></a>Die baat hebben bij het gebruik van een beveiligd werkstation

Alle gebruikers en operators voordeel hebben van een beveiligd werkstation. Een aanvaller inbreuk een PC of het apparaat kunt doen imiteren verschillende dingen, met inbegrip van alle accounts in de cache, en referenties gebruiken en tokens die worden gebruikt op het apparaat, terwijl ze zijn aangemeld. Dit risico kunt u de apparaten die worden gebruikt voor alle bevoorrechte rollen, met inbegrip van beheerdersrechten zo belangrijk als apparaten waarbij een bevoegd account wordt gebruikt voor het type lateral movement en privilege escalatieaanvallen te beveiligen. Deze accounts kunnen worden gebruikt voor tal van assets zoals:

* Beheerders van on-premises en cloud-gebaseerde systemen
* Werkstations van ontwikkelaars voor kritieke systemen
* Beheerder van sociale media-accounts met hoge blootstelling
* Uiterst gevoelige werkstations, zoals SWIFT betaling terminals
* Werkstations handelsgeheimen verwerken

Microsoft raadt het implementeren van besturingselementen voor verhoogde beveiliging voor bevoegde werkstations waar deze accounts worden gebruikt om risico te beperken. Meer informatie vindt u de [Implementatiehandleiding voor Azure Active Directory-functie](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2), [Office 365-roadmap](https://aka.ms/o365secroadmap), en [beveiligingsschema voor bevoegde toegang beveiligen](https://aka.ms/sparoadmap)).

## <a name="why-dedicated-workstations"></a>Waarom toegewezen werkstations

Hoewel het mogelijk om te beveiliging wordt toegevoegd aan een bestaand apparaat, is het beter om te beginnen met een veilige basis. Beginnen met een bekende goede apparaat en een set van bekende beveiligingsrisico besturingselementen worden uw organisatie in de gewenste positie te onderhouden die verhoogd niveau van beveiliging. Met de steeds groeiende aantal aanvalsvectoren toegestaan door de alledaagse e-mail en browsen op het web, is het moeilijk om te controleren of dat een apparaat kan worden vertrouwd. Deze handleiding werkt in de veronderstelling een toegewezen werkstation gescheiden van de standaard productiviteit, bladeren, en de e-taken zijn voltooid. Het verwijderen van productiviteit, het web te surfen en e-mailbericht van een apparaat kan een negatieve invloed hebben op de productiviteit, maar deze beveiliging is meestal geschikt voor scenario's waarbij de taken niet expliciet vereisen en risico's van een beveiligingsincident is hoog.

> [!NOTE]
> Het web te surfen hier verwijst naar algemene toegang tot willekeurige websites, dit is een hoog risico wachtwoorddelen afzonderlijk af van een webbrowser gebruiken voor toegang tot een klein aantal bekende beheerwebsites voor services zoals Azure, Office 365, andere cloudproviders en SaaS toepassingen.

Strategieën voor insluiting bieden betere beveiliging garanties door het verhogen van het aantal en type van de besturingselementen die een kwaadwillende persoon heeft te strijden tegen toegang tot gevoelige activa. Het model ontwikkeld hier biedt containment van beheerdersrechten voor specifieke apparaten met behulp van een gelaagde bevoegdheden model.

## <a name="supply-chain-management"></a>Toeleveringsbeheer

Essentieel is voor een beveiligd werkstation is een oplossing voor uw toeleveringsketen waarbij het werkstation dat u vertrouwde, een 'vertrouwensbasis'. Deze oplossing heeft betrekking op de hoofdmap van het gebruik van de vertrouwensrelatie van de [Microsoft Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-autopilot) technologie. Voor een beveiligd werkstation biedt Microsoft Autopilot gebruikmaken van Microsoft OEM-geoptimaliseerde Windows 10-apparaten met een bekende goede status van de fabrikant. In plaats van de installatiekopie werd teruggezet op een apparaat dat niet vertrouwd worden kan, Microsoft Autopilot kunt gebruiken om een Windows-apparaat in een status 'bedrijfsklare' toepassen van instellingen en beleidsregels, installeren van apps, en zelfs wijzigen van de editie van Windows 10 worden gebruikt (bijvoorbeeld van Windows 10 Pro naar Windows 10 Enterprise, ter ondersteuning van geavanceerde functies).

![Beveiligd werkstation niveaus](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Apparaat-functies en -profielen

In het advies meerdere beveiligingsprofielen voor en rollen voor het bereiken van een veiligere oplossing voor gebruikers, ontwikkelaars en IT-personeel verholpen. Deze profielen hebben ter ondersteuning van gewone gebruikers in organisaties die van een werkstation met verbeterde of beveiligd profiteren kunnen terwijl gebruiksvriendelijkheid en risico's zijn uitgelijnd. Configuratie van de instellingen op basis van aanvaarde industrienormen vindt u de instructies. In deze richtlijnen wordt gebruikt ter illustratie van een methode in Windows 10 beperken en de risico's die zijn gekoppeld aan een apparaat of gebruiker inbreuk op met behulp van beleid en de technologie voor het beheer van beveiligingsfuncties en -risico's te beperken.
![Beveiligd werkstation niveaus](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Lage beveiliging** : een werkstation voor beheerde standard biedt een goed uitgangspunt voor gebruik met de meeste thuis, en voor kleine bedrijven. Deze apparaten zijn geregistreerd bij Azure AD en Intune beheerd. Het profiel kan gebruikers toepassingen uitvoeren en elke website bladeren. Een antimalwareoplossing als [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) moet worden ingeschakeld.
* **Verbeterde beveiliging** – Is een oplossing op instapniveau beveiligd goed voor thuisgebruikers, kleine bedrijven, evenals algemene ontwikkelaars.
   * Het werkstation uitgebreid biedt een op basis van beleid voor een betere beveiliging van het beveiligingsprofiel van laag betekent. Dit profiel kunt u een veilige manier om te werken met gegevens van de klant en productiviteitsprogramma's zoals het controleren van e-mail en surfen op het web gebruiken. Een werkstation met uitgebreid kan worden gebruikt voor controle van gebruikersgedrag, en het gebruik van profiel van een werkstation aan controlebeleid inschakelen en registreren in Intune. In dit profiel is, het werkstation kunt beveiligingsmaatregelen en beleidsregels die worden beschreven in de inhoud en geïmplementeerd in het verbeterde werkstation - Windows10 (1809) script. De implementatie maakt ook gebruik van het gebruik van geavanceerde schadelijke software protection [geavanceerde Threat Protection (ATP)](https://docs.microsoft.com/office365/securitycompliance/office-365-atp)
* **Hoge beveiliging** – de meest efficiënte manier om te beperken van de kwetsbaarheid voor aanvallen van een werkstation is de mogelijkheid om het werkstation self-administer verwijderen. Lokale beheerdersrechten verwijderen is een stap die verbetert de beveiliging en kan invloed hebben op productiviteit als onjuist geïmplementeerd. Het profiel hoge beveiliging is gebaseerd op het profiel van de verbeterde beveiliging met een aanzienlijke hoeveelheid wijziging, het verwijderen van de lokale beheerder. Dit profiel is ontworpen om u te helpen bij gebruikers die mogelijk een hoge profiel, zoals een manager of gebruikers die mogelijk contact met gevoelige gegevens, zoals salarisadministratie, of de goedkeuring van services en processen.
   * Het gebruikersprofiel hoge beveiliging vereist een hogere gecontroleerde omgeving terwijl u nog steeds kunnen uitvoeren, hun productiviteit-activiteit, zoals e-mail en browsen behoud van een eenvoudige ervaring gebruiken op het web. De gebruikers verwachten dat functies, zoals cookies, Favorieten en andere snelkoppelingen die beschikbaar zijn om te werken. Maar deze gebruikers niet vereisen dat de mogelijkheid om te wijzigen of hun apparaat foutopsporing en hoeft niet te installeren van stuurprogramma's. Het profiel hoge beveiliging is geïmplementeerd met behulp van de hoge beveiliging - script voor Windows10 (1809).
* **Gespecialiseerde** -ontwikkelaars en IT-beheerders een aantrekkelijke doelwit voor aanvallen als deze rollen en systemen van belang zijn voor de aanvallers wijzigen kunnen. Het gespecialiseerde werkstation neemt de moeite geïmplementeerd in de hoge beveiliging werkstation en verdere emphases de beveiliging door het beheren van lokale toepassingen, websites internet te beperken en beperken van de productiviteit van mogelijkheden die hoge zijn risico's, zoals ActiveX- Java, browserinvoegtoepassing en verschillende andere bekende hoog risico besturingselementen op een Windows-apparaat. In dit profiel is, het werkstation kunt beveiligingsmaatregelen en beleidsregels die worden beschreven in de inhoud en geïmplementeerd in de DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline script.
* **Beveiligd** : een aanvaller die een Administrator-account in gevaar kan brengen meestal tot gevolg dat belangrijke zakelijke schade door diefstal van gegevens, wijziging van de gegevens of service wordt onderbroken. In deze beveiligde status, het werkstation kunnen alle beveiligingsmaatregelen en beleidsregels die directe controle van beheer van lokale toepassingen beperken en hulpprogramma's voor productiviteit worden verwijderd. Als gevolg hiervan moeilijker als e-mailbericht inbreuk op het apparaat wordt gemaakt en sociale media die passen bij de meest voorkomende manier phishing-aanvallen kunnen slagen worden geblokkeerd.  De beveiligd werkstation kan worden geïmplementeerd met het werkstation beveiligen - Windows10 (1809) SecurityBaseline script.

   ![Beveiligd werkstation](./media/concept-azure-managed-workstation/secure-workstation.png)

   Een beveiligd werkstation biedt een beheerder een beperkt werkstation dat duidelijk Toepassingsbeheer en application guard is. Het werkstation worden referenties, apparaat en exploit guard gebruiken ter bescherming van de host van schadelijk gedrag. Bovendien alle lokale schijven worden versleuteld met Bitlocker-versleuteling.

* **Geïsoleerde** – dit aangepaste offline scenario vertegenwoordigt het uiteinde van het spectrum (geen scripts voor installatie zijn opgegeven in dit geval). Organisaties mogelijk voor het beheren van een geïsoleerde zakelijke kritieke functie, zoals een productielijn hoge waarde of de levensduur van ondersteuning voor systemen die oudere besturingssystemen zonder de niet-ondersteunde/patch vereist. Omdat beveiliging is van essentieel belang en cloudservices niet beschikbaar zijn, organisaties mogelijk handmatig deze computers beheren/bijwerken of een geïsoleerde architectuur Active Directory-forest (zoals de verbeterde beveiliging Admin omgeving (ESAE)) gebruiken om deze te beheren. In deze geval verwijderen van alle toegang, behalve basis Intune en ATP moet status controleren worden overwogen.
   * [Vereiste communicatie Intune](https://docs.microsoft.com/intune/network-bandwidth-use)
   * [Vereiste communicatie ATP](https://docs.microsoft.com/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Volgende stappen

[Implementatie van een beveiligde Azure-beheerd werkstation](howto-azure-managed-workstation.md)
