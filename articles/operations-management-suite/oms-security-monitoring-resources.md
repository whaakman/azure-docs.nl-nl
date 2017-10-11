---
title: Bewaking van Resources in de beveiliging van Operations Management Suite en Audit oplossing | Microsoft Docs
description: Dit document helpt u met OMS-beveiliging en controle-mogelijkheden voor het controleren van uw resources en beveiligingsproblemen te identificeren.
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: d6752120-821f-4aa7-a049-25bf5a653b95
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 2f73266b65a4eda6c8751a2d56bc3f11bf4e6a57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Bewaking van bronnen voor beveiliging van Operations Management Suite en Audit-oplossing
Dit document helpt u OMS beveiligings- en Audit mogelijkheden gebruiken voor het controleren van uw resources en beveiligingsproblemen te identificeren.

## <a name="what-is-oms"></a>Wat is OMS?
Microsoft Operations Management Suite (OMS) is van Microsoft-cloud-gebaseerde IT-beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur. Lees het artikel [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx) voor meer informatie over OMS.

## <a name="monitoring-resources"></a>Bewaking van resources
Indien mogelijk is, wilt u voorkomen dat beveiligingsincidenten plaatsvinden in de eerste plaats. Het is echter niet mogelijk om te voorkomen dat alle beveiligingsincidenten. Wanneer een beveiligingsincident voordoet, moet u om ervoor te zorgen dat de gevolgen ervan wordt geminimaliseerd.  Er zijn drie essentiële aanbevelingen die kunnen worden gebruikt om te beperken het aantal en de impact van beveiligingsincidenten:

* Evalueer regelmatig beveiligingslekken in uw omgeving.
* Controleer regelmatig alle computersystemen en netwerkapparaten om ervoor te zorgen dat ze alle van de meest recente patches geïnstalleerd hebben.
* Controleer regelmatig alle logboeken en mechanismen voor logboekregistratie, met inbegrip van gebeurtenislogboeken besturingssysteem, toepassingen en inbraakdetectie detectie het systeemlogboek in Logboeken.

OMS beveiligings- en Audit oplossing kunnen IT-voor het bewaken van alle resources die kunnen helpen actief minimaliseert de gevolgen van beveiligingsincidenten. OMS beveiligings- en Audit heeft security-domeinen die kunnen worden gebruikt voor het bewaken van resources. De beveiligingsdomeinen die biedt snelle toegang tot een opties voor beveiligingsbewaking van de volgende domeinen worden behandeld in meer informatie:

* evaluatie van schadelijke software
* Update-evaluatie
* Identiteit en toegang

> [!NOTE]
> Lees voor een overzicht van al deze opties [aan de slag met beveiliging van Operations Management Suite-oplossing van de Audit en](oms-security-getting-started.md).
> 
> 

### <a name="monitoring-system-protection"></a>Bewaking van de systeembeveiliging
In een ingrijpende diepte benadering, elke laag van beveiliging, het is belangrijk voor de algehele beveiligingsstatus van uw asset. Computers met gedetecteerde bedreigingen en computers met onvoldoende beveiliging worden weergegeven in de tegel evaluatie van schadelijke software onder beveiligingsdomeinen. Met behulp van de informatie over de beoordeling van schadelijke software, kunt u een plan voor beveiliging toepassen op de servers waarvoor deze identificeren. Voor toegang tot deze optie de volgende stappen:

1. Klik in het hoofddashboard van **Microsoft Operations Management Suite** op de tegel **Beveiliging en controle**.
   
    ![Beveiliging en controle](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. In de **beveiligings- en Audit** dashboard, klikt u op **Antimalware Assessment** onder **beveiligingsdomeinen**. De **Antimalware Assessment** dashboard wordt weergegeven zoals hieronder wordt weergegeven:

![evaluatie van schadelijke software](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

U kunt de **evaluatie van schadelijke software** dashboard voor het identificeren van de volgende beveiligingsproblemen kijken:

* **Actieve bedreigingen**: computers die in verkeerde handen en actieve bedreigingen in het systeem hebben.
* **Bedreigingen hersteld**: computers waarmee is geknoeid, maar de bedreigingen zijn hersteld.
* **Handtekening is verouderd**: computers waarvoor bescherming tegen schadelijke software is ingeschakeld, maar de handtekening is verouderd.
* **Er is geen real-timebeveiliging**: computers die geen antimalware geïnstalleerd hebben.

### <a name="monitoring-updates"></a>updates controleren
Toepassen van de meest recente beveiligingsupdates is een best practice bij beveiliging en deze moet worden opgenomen in de beheerstrategie voor update. Microsoft Monitoring Agent-service (HealthService.exe) inleest update van bewaakte computers en stuurt vervolgens deze bijgewerkte informatie naar de OMS-service in de cloud voor verwerking. De Microsoft Monitoring Agent-service is geconfigureerd als een automatische-service en deze altijd in de doelcomputer moet worden uitgevoerd.

![updates controleren](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logica wordt toegepast op de updategegevens en registreert de gegevens van de cloudservice. Als ontbrekende updates worden gevonden, worden weergegeven op de **Updates** dashboard. U kunt de **Updates** dashboard voor het werken met ontbrekende updates en ontwikkelen van een plan als u wilt toepassen op de servers die ze nodig hebt. Volg de stappen hieronder om naar de **Updates** dashboard:

1. Klik in het hoofddashboard van **Microsoft Operations Management Suite** op de tegel **Beveiliging en controle**.
2. In de **beveiligings- en Audit** dashboard, klikt u op **Update-evaluatie** onder **beveiligingsdomeinen**. Het dashboard van de Update wordt weergegeven zoals hieronder wordt weergegeven:

![Update-evaluatie](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

In dit dashboard kunt u een update-evaluatie om te begrijpen van de huidige status van uw computers en de meest kritieke bedreigingen kunnen weren uitvoeren. Met behulp van de **essentiële of beveiligingsupdates** tegel, IT-beheerders is het mogelijk om gedetailleerde gegevens over de updates die ontbreken zoals hieronder wordt weergegeven:

![zoekresultaat](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Dit rapport kritieke gegevens bevatten die kan worden gebruikt voor het identificeren van het type van dit systeem kwetsbaar is voor, waaronder de Microsoft KB-artikelen die zijn gekoppeld aan de beveiligingsupdate en de MS Bulletin meer informatie over het beveiligingslek bevat threat.

### <a name="monitoring-identity-and-access"></a>Bewaking van identiteits- en toegangsbeheer
Met gebruikers die overal werken met behulp van verschillende apparaten en toegang tot een enorme hoeveelheid cloud en on-premises apps, is het noodzakelijk dat hun referenties worden beveiligd. Credential theft aanvallen zijn waarbij een aanvaller in eerste instantie toegang tot een gewone gebruiker referenties krijgt voor toegang tot een systeem in het netwerk. In veel gevallen deze initiële aanval is alleen een manier om ze toegang krijgen tot het netwerk, einddoel het toegankelijk is voor het detecteren van accounts met bevoegdheden. 

Aanvallers blijft in het netwerk, met behulp van gratis tooling om referenties uit de sessies van andere accounts aangemelde extraheren. Afhankelijk van de configuratie van het systeem, kunnen deze referenties worden geëxtraheerd in de vorm van hashes, tickets of zelfs ongecodeerde wachtwoorden.  

> [!NOTE]
> machines die rechtstreeks worden blootgesteld aan Internet ziet veel mislukte pogingen die proberen aan te melden met alle soorten bekende gebruikersnamen (bijvoorbeeld Administrator). In de meeste gevallen is het OK als u de bekende gebruikersnamen niet worden gebruikt en als het wachtwoord sterk genoeg is.
> 
> 

Het is mogelijk deze indringers identificeren voordat ze een account met bevoegdheden in gevaar brengen. U kunt gebruikmaken van **OMS beveiligings- en Audit oplossing** monitor identiteit en toegang. Volg de stappen hieronder om naar de **identiteits- en toegangsbeheer** dashboard:

1. In de **Microsoft Operations Management Suite** hoofddashboard klikt u op beveiliging en Audit tegel.
2. In de **beveiligings- en Audit** dashboard, klikt u op **identiteits- en toegangsbeheer** onder **beveiligingsdomeinen**. De **identiteits- en toegangsbeheer** dashboard wordt weergegeven zoals hieronder wordt weergegeven:

![identiteit en toegang](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Als onderdeel van uw strategie voor regelmatige controle, moet u de bewaking van de identiteit opnemen. IT-beheerder moet eruitzien als er een specifieke geldige gebruikersnaam met veel pogingen. Dit kan erop wijzen dat een aanvaller die de echte gebruikersnaam opgehaald en probeer het voor Bruut geweld of een automatische hulpprogramma dat wordt gebruikt vastgelegde wachtwoord die zijn verlopen.

Dit dashboard IT de mogelijkheid geven om snel te identificeren mogelijke bedreigingen die zijn gerelateerd aan de identiteit en toegang tot bedrijfsresources. Is bijzonder belangrijk is ook mogelijk om trends te identificeren, bijvoorbeeld in de tegel aanmeldingen gedurende een periode, ziet u gedurende periode het aantal keren dat een mislukte aanmeldingspoging is uitgevoerd. In dit geval de computer **bestandsserver** 35 aanmeldingspogingen ontvangen. U vindt meer informatie over deze computer door erop te klikken. 

![meer informatie](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Het rapport gegenereerd voor deze computer brengt waardevolle informatie over dit patroon. Opgemerkt dat de **ACCOUNT** kolom geeft het gebruikersaccount dat is gebruikt voor toegang tot het systeem de **TIMEGENERATED** kolom geeft het tijdsinterval waarin de poging is gedaan en de **LOGONTYPENAME** kolom wordt de locatie waar deze poging is gedaan. Als deze pogingen lokaal in het systeem zijn uitgevoerd door een programma, het **proces** kolom de naam van het proces zou worden weergegeven. U hebt al de procesnaam beschikbaar in scenario's waar de aanmeldingspoging afkomstig is van een programma, en nu u kunt verder onderzoek verrichten in het doelsysteem.

## <a name="see-also"></a>Zie ook
In dit document hebt u geleerd hoe OMS-beveiliging gebruiken en controleren van de oplossing voor het bewaken van uw resources. Raadpleeg de volgende artikelen voor meer informatie over OMS Beveiliging:

* [Overzicht van Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Aan de slag met beveiliging van Operations Management Suite en Audit-oplossing](oms-security-getting-started.md)
* [Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren](oms-security-responding-alerts.md)

