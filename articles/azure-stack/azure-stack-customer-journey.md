---
title: Overzicht van Azure Stack datacenter-integratie | Microsoft Docs
description: Informatie over wat u kunt verwachten bij een geslaagde on-site implementatie van Azure Stack in uw datacenter.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: jeffgilb
ms.reviewer: asganesh
ms.openlocfilehash: db33deaa9bb0bf9a4c3f766fbae883df4075c431
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323368"
---
# <a name="azure-stack-customer-journey"></a>Azure Stack-klanttraject

Dit artikel beschrijft de gebruikerservaring van end-to-end Azure Stack bij de aanschaf van een geïntegreerde oplossing via voor een succesvolle on-site implementatie door een solution provider. Deze informatie gebruiken om uw reis vereenvoudigen en om u te helpen bij het instellen van verwachtingen op wat u, als een klant van Azure Stack moeten verwachten bij het integreren van Azure Stack in uw datacenter.

Als een Azure Stack-klant bent, moet u verwacht dat de volgende fasen van de datacenter-integratie:

|     |Planningsfase|Volgorde-proces|Vóór de implementatie|Factory-proces|Hardware-levering|Implementatie van op locatie|
|-----|-----|-----|-----|-----|-----|-----|
|**Microsoft**|Neem contact op met de partner om presale ondersteuning te bieden.|Bereid softwarelicenties en opdrachten naar behoefte.|Geef vereiste hulpprogramma's voor het verzamelen van vereisten voor integratie van datacenter- en documentatie aan de klant.|De meest recente builds van de basislijn en het hulpprogramma keten updates bieden een maandelijkse uitgebracht.|N/A|Ondersteuningstechnici van Microsoft ondersteuning bij implementatieproblemen met.|
|**Partner**|Aanbevolen oplossingsopties op basis van de eisen van klanten.<br><br>Voorgesteld proof-of-concept (POC), indien nodig.<br><br>Tot stand brengen zakelijke relatie.<br><br>Bepaal op niveau van ondersteuning.|Voorbereiden die nodig zijn contracten met de klant.<br><br>Inkooporder klant maken.<br><br>Bepaal de tijdlijn van de levering van.<br><br>Verbinding met Microsoft customer indien nodig.|De klant geven met het trainen van die nodig zijn om ervoor te zorgen begrip van alle vereisten voor implementatie en datacenter integratie-opties.<br><br>De klant helpt bij de validatie van de verzamelde gegevens om ervoor te zorgen en volledigheid.|De laatste versie van de gevalideerde basislijn toepassen.<br><br>De vereiste Microsoft deployment toolkit van toepassing.|Hardware aan klant verzenden.|De implementatie is verwerkt door een technicus op locatie.<br><br>Rack en de stack.<br><br>Implementatie van de levenscyclus host (HLH) hardware.<br><br>Azure Stack-implementatie.<br><br>Hand uit aan de klant.|
|**De klant**|Beschrijf beoogde use-cases en vereisten opgeven.|Bepaal het facturering model te gebruiken, controleren en goedkeuren van opdrachten.|Het werkblad voor implementatie voltooid en controleer of dat alle vereisten voor implementatie zijn voldaan en gereed voor implementatie.|N/A|Datacenter voorbereiden door ervoor te zorgen dat alle vereiste voeding en koeling, border-connectiviteit en andere vereiste datacenter-integratie eis is voldaan.|Beschikbaar tijdens de implementatie in een abonnement referenties en ondersteunen als er vragen op de opgegeven gegevens zijn.|
| | | | | | | |


## <a name="planning-phase"></a>Planningsfase
De planningsfase is bij Microsoft of de partner van de oplossing Azure Stack wordt samen met u te evalueren en uw behoeften om te bepalen of Azure Stack de juiste oplossing voor u te begrijpen:

Ze kunt u bepalen op het volgende:

-   Is Azure Stack op de juiste oplossing voor uw organisatie?

-   De oplossing van welke grootte moet u?

-   Welk type model voor facturering en licentieverlening werkt voor uw organisatie?

-   Wat zijn de voeding en koeling vereisten?

Om ervoor te zorgen dat de hardwareoplossing beste wordt aan uw behoeften voldoen, de [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) kan worden gebruikt om te helpen bij pre-Purchase plannen om te bepalen van de juiste capaciteit en de configuratie voor uw Azure Stack-hardware oplossing.

Het werkblad is *niet* bedoeld om te worden gebruikt ter vervanging van uw eigen onderzoek en analyse van de hardwareoplossingen die het beste passen aan uw behoeften. Bij het plannen van een Azure Stack-implementatie, moet u ook controleren de [algemene datacenter-Integratieoverwegingen](azure-stack-datacenter-integration.md) voor Azure Stack-geïntegreerde systemen.

## <a name="order-process-phase"></a>Volgorde proces fase
In deze fase wordt zou veel van uw vragen met betrekking tot haalbaarheid zijn beantwoord. Nu dat u klaar bent om op het aanschaffen van Azure Stack en na de ondertekening van de vereiste opdrachten doorvoeren en inkooporders, wordt u gevraagd om te zorgen dat de gegevens van de vereisten voor integratie met uw oplossingsprovider.

## <a name="pre-deployment-phase"></a>De fase vóór de implementatie
Tijdens deze fase moet u bepalen hoe u Azure Stack integreren in uw datacenter. Plaats samen een sjabloon waarmee u kunt de benodigde informatie om te plannen voor de implementatie van een geïntegreerd systeem binnen uw omgeving verzamelen om dit proces, Microsoft, in samenwerking met de leveranciers van oplossingen.

De [algemene datacenter-Integratieoverwegingen](azure-stack-datacenter-integration.md) artikel bevat informatie die u bij het voltooien van de sjabloon helpt, ook wel het werkblad voor implementatie. 

> [!IMPORTANT]
> Tijdens deze fase is het belangrijk dat alle vereiste informatie is onderzocht en besloten op voordat u de oplossing te bestellen. Let erop dat deze stap tijd in beslag nemen en coördinatie en verzamelen van gegevens bij meerdere disciplines binnen uw organisatie vereist. 

In de fase vóór de implementatie moet u beslissen over het volgende:

- **Azure Stack-model en de identiteit verbindingsprovider**. U kunt kiezen om een Azure Stack implementeren [verbonden met internet (en naar Azure) of verbroken](azure-stack-connection-models.md). Voor het ophalen van het meeste voordeel uit Azure Stack, hybride scenario's, zoals wilt u implementeren met Azure bent verbonden. Kiezen voor Active Directory Federation Services (AD FS) of Azure Active Directory (Azure AD) is een eenmalige beslissing die u tijdens de implementatie moet aanbrengen. **U dit later niet wijzigen zonder het hele systeem opnieuw te implementeren**.

- **Licentiemodel**. De licentie model-opties voor u om de verkeersbelasting, is afhankelijk van het type van de implementatie hebt u. Uw keuze van id-provider heeft geen gevolgen voor virtuele machines van tenants of de identiteitssysteem en accounts die ze gebruiken.
    - Klanten die zich in een [verbroken implementatie](azure-stack-disconnected-deployment.md) slechts één optie: facturering op basis van capaciteit.

    - Klanten die zich in een [verbonden implementatie](azure-stack-connected-deployment.md) kunt kiezen tussen facturering op basis van capaciteit en betalen als u-gebruik. Facturering op basis van capaciteit moet een Azure-abonnement voor Enterprise Agreement (EA) voor de registratie. Dit is nodig voor inschrijving, waarmee de beschikbaarheid van de items in de Marketplace via een Azure-abonnement.

- **Netwerkintegratie**. [Netwerkintegratie](azure-stack-network.md) is van cruciaal belang voor implementatie, bewerking en beheer van Azure Stack-systemen. Er zijn verschillende overwegingen die ervoor te zorgen dat de oplossing Azure Stack is tegen en beschikt over een maximaal beschikbare fysieke infrastructuur ter ondersteuning van bewerkingen.

- **Firewall-integratie**. Het is raadzaam dat u [een firewall gebruikt](azure-stack-firewall.md) voor de beveiligde Azure-Stack. Firewalls kunnen helpen voorkomen DDOS-aanvallen, inbraakdetectie en inhoudsinspectie. Echter, moet worden opgemerkt dat deze een doorvoer voor Azure storage-services knelpunt kan.


- **Vereisten voor certificaten**. Het is essentieel dat alle [vereiste certificaten](azure-stack-pki-certs.md) zijn beschikbaar *voorafgaande* naar een on-site engineer die binnenkomen in uw datacenter voor implementatie.

Nadat de vereiste gegevens worden verzameld via het werkblad voor implementatie, wordt de fabriek wordt uitgevoerd op basis van de gegevens die zijn verzameld om te controleren of een geslaagde integratie van Azure Stack in uw datacenter de solution provider gestart.

## <a name="hardware-delivery-phase"></a>Hardware levering fase
Uw solution provider wordt samen met u plannen wanneer de oplossing wordt naar uw opslagruimte binnenkomen. Zodra de ontvangen en opstellen, moet u de tijd met de solution provider hebben een technicus afkomstig zijn van uw bedrijfslocatie worden gebruikt om uit te voeren van de Azure Stack-implementatie plannen.

Het is **cruciale** dat alle vereiste gegevens is vergrendeld en kan *voordat de on-site engineer binnenkomt voor het implementeren van de oplossing*.

-   Alle certificaten moeten worden aangeschaft en gereed.

-   Domeinnaam moet worden vastgesteld.

-   Alle parameters van de netwerk-integratie zijn voltooid en komt overeen met wat u met uw solution provider hebt gedeeld.

> [!TIP]
> Als een van deze informatie is gewijzigd, zorg ervoor dat de wijziging communiceren met de solution provider voordat u de werkelijke implementatie plannen.

## <a name="onsite-deployment-phase"></a>Implementatiefase op locatie
Als u wilt implementeren in Azure Stack, moet een technicus op locatie van uw hardware solution provider aanwezig zijn om te worden gehouden, trappen af van de implementatie. Zorg ervoor dat alle opgegeven informatie die door de implementatie-werkblad niet is gewijzigd zodat een geslaagde implementatie. 

Hier volgt wat u mag verwachten van de technicus op locatie tijdens de implementatie-ervaring:

- Controleer alle kabels en connectiviteit van de rand om te controleren of de oplossing wordt goed samengesteld en voldoet aan uw vereisten
- De oplossing HLH (Hardware Lifecycle Host) configureren
- Controleer of dat alle BMC-, BIOS-en netwerkinstellingen juist zijn.
- Zorg ervoor dat de firmware van alle onderdelen is de meest recente goedgekeurde versie door de oplossing
- De implementatie starten

> [!NOTE]
> Een implementatie van procedure door de engineering op locatie kan over een week van zakelijke duren om uit te voeren.

## <a name="post-integration-phase"></a>Na integratie fase
Verschillende stappen moeten worden uitgevoerd door de partner voordat de oplossing wordt doorgegeven aan de klant in de fase na integratie. In deze fase is validatie belangrijk om te controleren of dat het systeem is geïmplementeerd en de prestaties goed. 

Acties die moeten worden genomen door de OEM-Partner zijn:

-   [Test-azurestack uitvoeren](azure-stack-diagnostic-test.md#run-validation-tool-to-test-system-readiness-before-installing-update-or-hotfix)

-   [Registratie bij Azure](azure-stack-registration.md)

-   [Marketplace-syndicatie](azure-stack-download-azure-marketplace-item.md#use-the-marketplace-syndication-tool-to-download-marketplace-items)

-   Back-Switch-configuratiebestanden

-   DVM verwijderen

-   Een klant overzicht voorbereiden voor implementatie

-   [Controleren op updates te controleren of dat de oplossingsoftware wordt bijgewerkt naar de nieuwste versie](azure-stack-updates.md)

Er zijn verschillende stappen die vereist of optioneel zijn, afhankelijk van het installatietype zijn.

-   Als de implementatie is voltooid met behulp van [AD FS](azure-stack-integrate-identity.md), klikt u vervolgens de Azure Stack stempel moet worden geïntegreerd met de klant de eigen AD FS.

  > [!NOTE]
  > Deze stap is de verantwoordelijkheid van de klant, hoewel de partner kan desgewenst bieden services om dit te doen.

-   Integratie met een bestaande bewakingssysteem van de respectieve partner.

    -   [Integratie van System Center Operations Manager](azure-stack-integrate-monitor.md) biedt ook ondersteuning voor vloot beheermogelijkheden.

    -   [Nagios-integratie](azure-stack-integrate-monitor.md#integrate-with-nagios)

## <a name="overall-timeline"></a>Algemene tijdlijn

![](./media/azure-stack-datacenter-integration-walkthrough/image1.png)

## <a name="support"></a>Ondersteuning
Azure Stack kunt een Azure-consistente, een geïntegreerde ondersteuningservaring die de levenscyclus van het volledige systeem. Ter ondersteuning van volledig geïntegreerde Azure Stack-systemen, moeten klanten twee ondersteuningscontracten; een met Microsoft (of hun Cloud Solution Provider) voor ondersteuning van Azure-services en één met de hardwareprovider voor systeemondersteuning. De geïntegreerde ondersteuningservaring biedt gecoördineerd escalatie- en resolutie, zodat klanten genieten van een consistente ondersteuningservaring, ongeacht wie ze eerst aanroepen. Voor klanten die al Premier hebben, Azure - Standard / ProDirect- of -Partner ondersteuning met Microsoft ondersteuning voor Azure Stack-software is opgenomen.

De geïntegreerde ondersteuningservaring wordt gebruik gemaakt van een Exchange geval mechanisme voor de overdracht van de richtingen van kwesties en case updates tussen Microsoft en de hardware-partner. Microsoft Azure Stack volgt de [moderne Lifecycle-beleid](https://support.microsoft.com/help/30881).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [algemene datacenter-Integratieoverwegingen](azure-stack-datacenter-integration.md).
