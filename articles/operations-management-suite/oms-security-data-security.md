<properties
   pageTitle="Gegevensbeveiliging in de oplossing Beveiliging en controle in Operations Management Suite | Microsoft Azure"
   description="In dit document wordt uitgelegd hoe gegevens worden beheerd en bewaakt in de oplossing Beveiliging en controle in Operations Management Suite."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>


# Gegevensbeveiliging in de oplossing Beveiliging en controle in Operations Management Suite

Om klanten te helpen bedreigingen te voorkomen, ze te detecteren en erop te reageren, verzamelt en verwerkt de [oplossing Beveiliging en controle in Operations Management Suite (OMS)](operations-management-suite-overview.md) gegevens over uw resources waaronder:

- Beveiligingslogboek
- ETW-gebeurtenissen (Event Tracing for Windows)
- Controlegebeurtenissen AppLocker
- Windows Firewall-logboek
- Advanced Threat Analytics-gebeurtenissen
- Resultaten van de evaluatie van de basislijn
- Resultaten van de antimalware-evaluatie
- Resultaten van de evaluatie van updates/patching
- Syslogs-streams die expliciet zijn ingeschakeld op de agent

We doen er alles aan om de privacy van gegevens te beschermen en deze gegevens te beveiligen. Microsoft voldoet aan strikte nalevings- en beveiligingsrichtlijnen - van het schrijven van code tot de uitvoering van een service.
In dit artikel wordt uitgelegd hoe gegevens worden beheerd en beveiligd in Beveiliging en controle in OMS.

## Gegevensbronnen

Met de oplossing Beveiliging en controle in OMS worden gegevens geanalyseerd die afkomstig zijn van uw virtuele machines en fysieke computers waarop OMS Agent is geïnstalleerd. Met de oplossing Beveiliging en controle in OMS worden configuratiegegevens over beveiligingsgebeurtenissen verzameld, zoals Windows-gebeurtenissen, controlelogboeken, IIS-logboeken en syslog-berichten. Voorbeelden van dergelijke gegevens zijn: besturingssysteemtype en -versie, actieve processen, computernaam, IP-adressen, aangemelde gebruiker en tenant-id.  

## Gegevensbeveiliging

**Scheiding van gegevens**: gegevens worden op een logische manier apart van elkaar gehouden, in elk onderdeel van de service. Alle gegevens worden gemarkeerd per organisatie. Deze markering blijft aanwezig gedurende de levenscyclus van de gegevens en deze wordt afgedwongen op elke laag van de service. 

**Gegevenstoegang**: om aanbevelingen voor beveiliging te bieden en mogelijke bedreigingen te onderzoeken, kunnen medewerkers van Microsoft toegang hebben tot gegevens die door services worden verzameld of geanalyseerd. We voldoen aan de [voorwaarden voor Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) en de [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), die stellen dat Microsoft niet de klantgegevens gebruikt of gegevens ervan afleidt voor reclame- of vergelijkbare commerciële doeleinden. Om aanbevelingen voor beveiliging te bieden en mogelijke bedreigingen te onderzoeken, kunnen medewerkers van Microsoft toegang hebben tot gegevens die door services worden verzameld of geanalyseerd. We gebruiken klantgegevens alleen indien nodig om u Azure-services te bieden, met inbegrip van doeleinden die compatibel zijn met het leveren van die services. U behoudt alle rechten op uw eigen gegevens.

**Gegevensgebruik**: Microsoft gebruikt informatie over patronen en bedreigingen die worden gezien tussen meerdere tenants voor het verbeteren van onze mogelijkheden voor voorkoming en detectie; wij doen dit in overeenstemming met de privacyverplichtingen beschreven in onze [Privacyverklaring](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [AZURE.NOTE] Gegevenslocatie is geconfigureerd op het niveau van de OMS-werkruimte, tijdens het maken van de werkruimte, dat deel uitmaakt van het eerste configuratieproces voor Beveiliging en controle in OMS.

## Zie ook

In dit document hebt u geleerd hoe gegevens worden beheerd en bewaakt in OMS. Voor meer informatie over de oplossing Beveiliging en controle in OMS, zie:

- [Overzicht van Operations Management Suite (OMS)](operations-management-suite-overview.md)
- [Monitoring and Responding to Security Alerts in Operations Management Suite Security and Audit Solution (Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren)](oms-security-responding-alerts.md)
- [Monitoring Resources in Operations Management Suite Security and Audit Solution (Beveiligingswaarschuwingen in de oplossing Beveiliging en controle van Operations Management Suite bewaken en erop reageren)](oms-security-monitoring-resources.md)




<!--HONumber=Sep16_HO3-->


