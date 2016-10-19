<properties
   pageTitle="Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center | Microsoft Azure"
   description="Dit document bevat informatie over het gebruik van de mogelijkheden van Azure Security Center om beveiligingswaarschuwingen te beheren en hierop te reageren."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/19/2016"
   ms.author="yurid"/>


# Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center
Dit document bevat informatie over het gebruik van Azure Security Center om beveiligingswaarschuwingen te beheren en hierop te reageren.

> [AZURE.NOTE] Als u geavanceerde detectie wilt inschakelen, voert u een upgrade uit naar Azure Security Center Standard. Er is een gratis proefversie voor 90 dagen beschikbaar. Als u een upgrade wilt uitvoeren, selecteert u de prijscategorie in het [beveiligingsbeleid](security-center-policies.md). Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.


## Wat zijn beveiligingswaarschuwingen?
Security Center verzamelt, analyseert en integreert automatisch logboekgegevens van uw Azure-resources, het netwerk en verbonden partneroplossingen, zoals firewall- en eindpuntbeveiligingsoplossingen om werkelijke dreigingen te detecteren en fout-positieven te reduceren. In Security Center wordt een lijst met beveiligingswaarschuwingen met prioriteiten weergegeven samen met de informatie die u nodig hebt om snel onderzoek te doen naar het probleem en aanbevelingen voor het herstellen van een aanval. Azure Security Center voegt soortgelijke waarschuwingen ook samen om kettingpatronen onder te brengen in [incidenten](security-center-incident.md). 

> [AZURE.NOTE] Lees [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md) voor meer informatie over de werking van de detectiemogelijkheden van Azure Security Center.


## Beveiligingswaarschuwingen beheren

U kunt uw huidige waarschuwingen controleren met de tegel **Beveiligingswaarschuwingen**. Open Azure Portal en voer de volgende stappen uit om meer informatie over elke waarschuwing weer te geven:

1. Op het Security Center-dashboard ziet u de tegel **Beveiligingswaarschuwingen**.

    ![De tegel Beveiligingswaarschuwingen in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2.  Klik op de tegel om de blade **Beveiligingswaarschuwingen** met meer informatie over de waarschuwingen te openen (zie hieronder).

    ![De blade Beveiligingswaarschuwingen in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

In het onderste gedeelte van deze blade vindt u de details voor elke waarschuwing. Als u wilt sorteren, klikt u op de kolom waarop u wilt sorteren. Hieronder volgt de definitie voor elke kolom:

- **Waarschuwing**: een korte beschrijving van de waarschuwing.
- **Aantal**: een lijst met alle waarschuwingen van dit specifieke type die zijn gedetecteerd op een specifieke dag.
- **Gedetecteerd door**: de service die verantwoordelijk is voor activering van de waarschuwing.
- **Datum**: de datum waarop de gebeurtenis heeft plaatsgevonden.
- **Status**: de huidige status voor deze waarschuwing. Er zijn twee soorten statussen:
    - **Actief**: de beveiligingswaarschuwing is gedetecteerd.
    - **Gesloten**: de beveiligingswaarschuwing is door de gebruiker gesloten. Deze status wordt meestal gebruikt voor waarschuwingen die zijn onderzocht, en die zijn verholpen of waarvan is vastgesteld dat ze eigenlijk geen aanval zijn.

- **Ernst**: de ernst kan hoog, gemiddeld of laag zijn.

### Waarschuwingen filteren

U kunt waarschuwingen filteren op basis van datum, status en ernst. Het filteren van waarschuwingen kan nuttig zijn wanneer u minder beveiligingswaarschuwingen wilt weergeven. U kunt u bijvoorbeeld concentreren op de beveiligingswaarschuwingen van de afgelopen 24 uur, omdat u een mogelijke inbreuk in het systeem onderzoekt.

1. Klik op **Filter** op de blade **Beveiligingswaarschuwingen**. De blade **Filter** wordt geopend en u selecteert de gewenste waarden voor datum, status en ernst.

    ![Waarschuwingen filteren in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-ga.png)

2.  Nadat u een beveiligingswaarschuwing hebt onderzocht, kunt u tot de conclusie komen dat het om een fout-positief voor uw omgeving gaat of dat deze duidt op een verwachte werking voor een bepaalde resource. Als u om wat voor reden dan ook vaststelt dat een beveiligingswaarschuwing niet van toepassing is, kunt u de waarschuwing sluiten en vervolgens uit uw weergave filteren. Er zijn twee manieren om een beveiligingswaarschuwing te sluiten. Klik met de rechtermuisknop op een waarschuwing en selecteer **Sluiten** of beweeg de muisaanwijzer over een item, klik op de drie punten die worden weergegeven aan de rechterkant en selecteer **Sluiten**. U kunt de beveiligingswaarschuwingen die u hebt gesloten, weergeven door op **Filter** te klikken en **Gesloten** te selecteren.

    ![Waarschuwingen sluiten in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig4-ga.png)

### Reageren op beveiligingswaarschuwingen

Selecteer een beveiligingswaarschuwing voor meer informatie over de gebeurtenis(sen) waarmee de waarschuwing is geactiveerd en welke stappen u zo nodig moet uitvoeren om een aanval te verhelpen. Beveiligingswaarschuwingen zijn gegroepeerd op type en datum. Als u op een beveiligingswaarschuwing klikt, wordt een blade met een lijst gegroepeerde waarschuwingen geopend.

![Op beveiligingswaarschuwingen reageren in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

In dit geval verwijzen de geactiveerde waarschuwingen naar verdachte RDP-activiteiten (Remote Desktop Protocol). In de eerste kolom ziet u welke resources zijn aangevallen, de tweede kolom toont hoe vaak de resource is aangevallen, de derde kolom bevat de tijd waarop de aanval heeft gevonden; de vierde kolom bevat de status van de waarschuwing en in de vijfde kolom wordt de ernst van de aanval weergegeven. Bekijk deze informatie en klik op de resource die is aangevallen. Er wordt dan een nieuwe blade geopend.

![Suggesties voor wat u kunt doen bij beveiligingswaarschuwingen in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

U vindt in het veld **Omschrijving** van deze blade meer informatie over deze gebeurtenis. Deze aanvullende informatie biedt inzicht in wat de beveiligingswaarschuwing heeft geactiveerd, de doelresource, eventueel het bron-IP-adres en aanbevelingen voor het herstellen.  In sommige gevallen zal het bron-IP-adres leeg zijn (niet beschikbaar), omdat niet alle Windows-logboeken voor beveiligingsgebeurtenissen het IP-adres bevatten.

Het herstel dat door Security Center wordt voorgesteld, is afhankelijk van de beveiligingswaarschuwing. In sommige gevallen moet u wellicht andere Azure-mogelijkheden gebruiken om het aanbevolen herstel te implementeren. Als herstel voor deze aanval moet bijvoorbeeld het IP-adres op een zwarte lijst worden gezet dat door de aanval is gegenereerd. Hiervoor is de regel van een [netwerk-ACL](../virtual-network/virtual-networks-acl.md) of een [netwerkbeveiligingsgroep](../virtual-network/virtual-networks-nsg.md) nodig.

> [AZURE.NOTE] Lees voor meer informatie over de verschillende typen waarschuwingen [Beveiligingswaarschuwingen per type in Azure Security Center](security-center-alerts-type.md).

## Zie ook

In dit document hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende onderwerpen voor meer informatie over Security Center:

- [Beveiligingsincidenten afhandelen in Azure Security Center](security-center-incident.md)
- [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md)
- [Plannings- en bedieningsgids voor het Azure Beveiligingscentrum](security-center-planning-and-operations-guide.md)
- [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
- [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.



<!--HONumber=Sep16_HO3-->


