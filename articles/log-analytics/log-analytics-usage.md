<properties
    pageTitle="Gegevensgebruik analyseren in Log Analytics | Microsoft Azure"
    description="U kunt de Gebruikspagina in Log Analytics gebruiken om na te gaan hoeveel gegevens er naar de OMS-service worden verzonden."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>


# Gegevensgebruik analyseren in Log Analytics

Met Log Analytics in de OMS (Operations Management Suite) worden periodiek gegevens verzameld en verzonden naar de OMS-service.  U kunt de **Gebruikspagina** gebruiken om na te gaan hoeveel gegevens er naar de OMS-service worden verzonden. Op de **Gebruikspagina** wordt weergegeven hoeveel gegevens er dagelijks op basis van de oplossingen worden verzonden en hoe vaak er via uw servers gegevens worden verzonden.

>[AZURE.NOTE] Als u een gratis account hebt gemaakt met behulp van de [OMS-website](http://www.microsoft.com/oms), kunt u dagelijks maximaal 500 MB aan gegevens naar de OMS-service verzenden. Als de dagelijkse limiet is bereikt, wordt het analyseren van gegevens gestopt en aan het begin van de volgende dag hervat. U moet ook alle gegevens opnieuw verzenden die niet zijn geaccepteerd of verwerkt via OMS.

U kunt uw gebruik bekijken via de **Gebruikstegel** op het dashboard **Overzicht** in OMS.

![gebruikstegel](./media/log-analytics-usage/usage-tile.png)

Als u de dagelijkse gebruikslimiet hebt overschreden of als u deze limiet nadert, kunt u ervoor kiezen om een oplossing te verwijderen om de hoeveelheid gegevens die naar de OMS-service wordt verzonden, te verminderen. Zie [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md) (Log Analytics-oplossingen toevoegen uit de oplossingengalerie) voor meer informatie over het verwijderen van oplossingen.

![gebruiksdashboard](./media/log-analytics-usage/usage-dashboard.png)

Deze **Gebruikspagina** geeft de volgende informatie weer:

- Gemiddeld gebruik per dag
- Gegevensgebruik voor elke oplossing in de afgelopen 30 dagen
- Hoeveel gegevens er in de afgelopen 30 dagen via de servers in uw omgeving naar de OMS-service zijn verzonden
- De prijscategorie van uw gegevensplan en de geschatte kosten
- Informatie over de SLA (Service Level Agreement), inclusief de hoeveelheid tijd die nodig is om uw gegevens via OMS te verwerken

## Werken met gebruiksgegevens

1. Klik op de **Overzichtspagina** op de **Gebruikstegel**.
2. Bekijk op de **Gebruikspagina** de gebruikscategorieën waarop de gebieden worden weergegeven waarover u zich zorgen maakt.
3. Als u een oplossing hebt die te veel van uw dagelijkse quota verbruikt, kunt u overwegen om deze oplossing te verwijderen.

## De geschatte kosten en factureringsgegevens weergeven
1. Klik op de **Overzichtspagina** op de **Gebruikstegel**.
2. Klik op de **Gebruikspagina** onder **Gebruik** op de punthaak (**>**) naast **Geschatte kosten**.
3. In het uitgevouwen detailvenster **Uw gegevensplan** kunt u de geschatte maandkosten zien.  
    ![Uw gegevensplan](./media/log-analytics-usage/usage-data-plan.png)
4. Als u uw factureringsgegevens wilt bekijken, klikt u op **Mijn factuur weergeven** om uw abonnementsgegevens weer te geven.
    - Klik op de pagina met abonnementen op uw abonnement om details en een lijst met regelitems voor verbruik weer te geven.  
        ![abonnement](./media/log-analytics-usage/usage-sub01.png)
    - Op de Overzichtspagina voor uw abonnement kunt u diverse taken uitvoeren om meer details van het abonnement te beheren en te bekijken.  
        ![abonnementsdetails](./media/log-analytics-usage/usage-sub02.png)

## Gegevensbatches voor uw SLA weergeven
1. Klik op de **Overzichtspagina** op de **Gebruikstegel**.
2. Klik onder **Serviceovereenkomst** op **SLA-gegevens downloaden**.
3. Er wordt ter controle een Excel XLSX-bestand gedownload.  
    ![SLA-gegevens](./media/log-analytics-usage/usage-sla-details.png)

## Volgende stappen

- Zie [Log searches in Log Analytics](log-analytics-log-searches.md) (Zoekopdrachten naar logboeken in Log Analytics) om gedetailleerde informatie te bekijken die is verzameld via oplossingen.



<!--HONumber=Sep16_HO3-->


