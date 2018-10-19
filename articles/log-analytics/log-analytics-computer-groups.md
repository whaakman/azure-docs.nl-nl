---
title: Computergroepen in Azure Log Analytics-Logboeken | Microsoft Docs
description: Computergroepen in Log Analytics kunnen u zoekopdrachten bereik voor een bepaalde set computers.  Dit artikel beschrijft de verschillende methoden die u gebruiken kunt om te maken van computergroepen en hoe u deze kunt gebruiken in een zoeken in Logboeken.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 81eacf464c90d56c4ebeae1d1cefbd6f2f0fdab8
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49408537"
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Computergroepen in Log Analytics-zoekopdrachten

Computergroepen in Log Analytics kunnen u aan het bereik [zoekopdrachten](log-analytics-log-search-new.md) aan een bepaalde set computers.  Elke groep wordt gevuld met computers met behulp van een query die u definieert of door het importeren van groepen van verschillende bronnen.  Wanneer de groep is opgenomen in een zoeken in Logboeken, zijn de resultaten beperkt tot de records die voldoen aan de computers in de groep.

## <a name="creating-a-computer-group"></a>Het maken van een computergroep
U kunt een computergroep maken in Log Analytics met behulp van een van de methoden in de volgende tabel.  In de onderstaande secties vindt u meer informatie over elke methode. 

| Methode | Beschrijving |
|:--- |:--- |
| Zoekopdrachten in logboeken |Maak een logboekzoekopdracht die als resultaat een lijst van computers geeft. |
| API voor zoeken in logboeken |De Log Search-API gebruiken om via een programma maakt een computergroep op basis van de resultaten van een logboekzoekopdracht. |
| Active Directory |Automatisch scannen op het groepslidmaatschap van de agentcomputers die lid zijn van een Active Directory-domein en een groep in Log Analytics maken voor elke beveiligingsgroep. |
| Configuratiebeheer | Verzamelingen importeren uit System Center Configuration Manager en maak een groep in Log Analytics voor elk. |
| Windows Server updateservices |Automatisch scannen van WSUS-servers of clients die zijn gericht op groepen en voor elk een groep maken in Log Analytics. |

### <a name="log-search"></a>Zoekopdrachten in logboeken
Gemaakt op basis van een Logboekzoekopdracht computergroepen bevatten alle van de computers die zijn geretourneerd door een query die u definieert.  Deze query wordt uitgevoerd telkens als groep op de computer wordt gebruikt, zodat alle wijzigingen die sinds de groep is gemaakt is doorgevoerd.  

U kunt een query voor een computergroep, maar er moet een afzonderlijke set computers geretourneerd met behulp van `distinct Computer`.  Hieronder volgt een typisch voorbeeld zoeken die u voor als een computergroep gebruiken kunt.

    Heartbeat | where Computer contains "srv" | distinct Computer

De volgende tabel beschrijft de eigenschappen die een computergroep definiëren.

| Eigenschap | Beschrijving |
|:---|:---|
| Weergavenaam   | Naam van de zoekopdracht wilt weergeven in de portal. |
| Categorie       | De categorie te organiseren van de zoekopdrachten in de portal. |
| Query’s uitvoeren          | De query voor de groep van de computer. |
| Functiealias | Een unieke alias die wordt gebruikt voor het identificeren van de groep van de computer in een query. |

Gebruik de volgende procedure een computergroep maken vanuit een zoeken in Logboeken in Azure portal.

2. Open **zoeken in logboeken** en klik vervolgens op **opgeslagen zoekopdrachten** aan de bovenkant van het scherm.
3. Klik op **toevoegen** en geef waarden op voor elke eigenschap voor de groep van de computer.
4. Selecteer **deze query opslaan als een computergroep** en klikt u op **OK**.



### <a name="active-directory"></a>Active Directory
Als u Log Analytics configureren om Active Directory-groepslidmaatschappen importeren, wordt het groepslidmaatschap van computers met de Log Analytics-agent toegevoegd aan een domein geanalyseerd.  Een computergroep in Log Analytics is gemaakt voor elke beveiligingsgroep in Active Directory en elke computer wordt toegevoegd aan de computergroepen die overeenkomt met de beveiligingsgroepen die ze lid van zijn.  Dit lidmaatschap wordt voortdurend bijgewerkt voor elke 4 uur.  

Configureren van Log Analytics voor het importeren van Active Directory-beveiligingsgroepen van Log Analytics **geavanceerde instellingen** in Azure portal.  Selecteer **computergroepen**, **Active Directory**, en vervolgens **importeren Active Directory-groepslidmaatschappen van computers**.  Er is geen verdere configuratie nodig.

![Computergroepen uit Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Wanneer groepen zijn geïmporteerd, wordt het menu geeft het aantal computers met groepslidmaatschappen gedetecteerd en het aantal groepen geïmporteerd.  U kunt klikken op een van deze koppelingen om terug te keren de **ComputerGroup** records met deze informatie.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Als u Log Analytics configureren om WSUS-groepslidmaatschappen importeren, wordt het lidmaatschap van de toewijzing van alle computers met de Log Analytics-agent geanalyseerd.  Als u van client-side gebruikmaakt heeft doelen, elke computer die is verbonden met Log Analytics en maakt deel uit van een WSUS het groepslidmaatschap geïmporteerd naar Log Analytics die gericht is op groepen. Als u van server-side gebruikmaakt moet doelen, de Log Analytics agent worden geïnstalleerd op de WSUS-server om de gegevens van het groepslidmaatschap naar Log Analytics worden geïmporteerd.  Dit lidmaatschap wordt voortdurend bijgewerkt voor elke 4 uur. 

Configureren van Log Analytics voor het importeren van WSUS-groepen van Log Analytics **geavanceerde instellingen** in Azure portal.  Selecteer **computergroepen**, **WSUS**, en vervolgens **importeren WSUS-groepslidmaatschappen**.  Er is geen verdere configuratie nodig.

![Computergroepen van WSUS](media/log-analytics-computer-groups/configure-wsus.png)

Wanneer groepen zijn geïmporteerd, wordt het menu geeft het aantal computers met groepslidmaatschappen gedetecteerd en het aantal groepen geïmporteerd.  U kunt klikken op een van deze koppelingen om terug te keren de **ComputerGroup** records met deze informatie.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Als u Log Analytics voor het importeren van Configuration Manager-verzamelingslidmaatschappen configureert, maakt u een computergroep voor elke verzameling.  De lidmaatschapsgegevens van de verzameling wordt elke drie uur zodat de computergroepen huidige opgehaald. 

Voordat u Configuration Manager-verzamelingen importeren kunt, moet u [Configuration Manager koppelen aan Log Analytics](log-analytics-sccm.md).  Vervolgens kunt u het importeren van Log Analytics configureren **geavanceerde instellingen** in Azure portal.  Selecteer **computergroepen**, **SCCM**, en vervolgens **Import Configuration Manager-verzamelingslidmaatschappen**.  Er is geen verdere configuratie nodig.

![Computergroepen uit SCCM](media/log-analytics-computer-groups/configure-sccm.png)

Wanneer verzamelingen zijn geïmporteerd, wordt het menu geeft het aantal computers met groepslidmaatschappen gedetecteerd en het aantal groepen geïmporteerd.  U kunt klikken op een van deze koppelingen om terug te keren de **ComputerGroup** records met deze informatie.

## <a name="managing-computer-groups"></a>Computergroepen beheren
U kunt computergroepen die zijn gemaakt op basis van een zoeken in Logboeken of de Log Search-API van Log Analytics weergeven **geavanceerde instellingen** in Azure portal.  Selecteer **computergroepen** en vervolgens **opgeslagen groepen**.  

Klik op de **x** in de **verwijderen** kolom te verwijderen van de groep op de computer.  Klik op de **leden weergeven** pictogram voor een groep voor het uitvoeren van zoeken in Logboeken van de groep die de leden als resultaat geeft.  U een computergroep niet wijzigen, maar in plaats daarvan moet verwijderen en vervolgens opnieuw te maken met de gewijzigde instellingen.

![Opgeslagen computergroepen](media/log-analytics-computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-search"></a>Met behulp van de groep van een computer in een zoeken in Logboeken
U een computergroep gemaakt op basis van een logboekzoekopdracht in een query door de alias te behandelen als een functie, meestal met de volgende syntaxis gebruiken:

  `Table | where Computer in (ComputerGroup)`

U kunt bijvoorbeeld het volgende om terug te keren UpdateSummary records voor alleen computers in een computergroep mycomputergroup genoemd.
 
  `UpdateSummary | where Computer in (mycomputergroup)`


Geïmporteerde computergroepen en hun opgenomen computers worden opgeslagen in de **ComputerGroup** tabel.  Bijvoorbeeld, retourneert de volgende query een lijst van computers in de groep Domeincomputers uit Active Directory. 

  `ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer`

De volgende query retourneert UpdateSummary records voor alleen computers in domeincomputers.

  ```
  let ADComputers = ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer;
  UpdateSummary | where Computer in (ADComputers)
  ```




## <a name="computer-group-records"></a>Computer groeperen van records
Een record wordt gemaakt in de werkruimte voor logboekanalyse voor elke computer groepslidmaatschap gemaakt op basis van Active Directory of WSUS.  Deze records zijn een type **ComputerGroup** en hebben de eigenschappen in de volgende tabel.  Records worden niet gemaakt voor computergroepen op basis van zoekopdrachten in Logboeken.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Computer |De naam van de computer die lid is. |
| Groep |Naam van de groep. |
| GroupFullName |Het volledige pad naar de groep met inbegrip van de bron en de naam van de gegevensbron. |
| GroupSource |Bron van die groep is verzameld uit. <br><br>ActiveDirectory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName |Naam van de bron die is verzameld, de groep.  Dit is de domeinnaam voor Active Directory. |
| ManagementGroupName |Naam van de beheergroep voor SCOM-agents.  Voor andere agents is dit AOI -\<werkruimte-ID\> |
| TimeGenerated |Datum en tijd groep op de computer is gemaakt of bijgewerkt. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [zoekopdrachten](log-analytics-log-searches.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.  

