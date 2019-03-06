---
title: Computergroepen in Azure Monitor query's bijgehouden | Microsoft Docs
description: Computergroepen in Azure Monitor kunnen u bereik logboeken-query's naar een bepaalde set computers.  Dit artikel beschrijft de verschillende methoden die u gebruiken kunt om computergroepen en het gebruik ervan in een logboekquery te maken.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/05/2019
ms.author: bwren
ms.openlocfilehash: 0a29e453c723ecc9ac378ee337365525587aaef2
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57444120"
---
# <a name="computer-groups-in-azure-monitor-log-queries"></a>Computergroepen in Logboeken-query's van Azure Monitor
Computergroepen in Azure Monitor kunnen u aan het bereik [query's bijgehouden](../log-query/log-query-overview.md) aan een bepaalde set computers.  Elke groep wordt gevuld met computers met behulp van een query die u definieert of door het importeren van groepen van verschillende bronnen.  Wanneer de groep is opgenomen in een logboekquery, worden de resultaten beperkt tot de records die voldoen aan de computers in de groep.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="creating-a-computer-group"></a>Het maken van een computergroep
U kunt een computergroep maken in Azure Monitor met een van de methoden in de volgende tabel.  In de onderstaande secties vindt u meer informatie over elke methode. 

| Methode | Description |
|:--- |:--- |
| Logboekquery |Maak een logboekquery een lijst van computers retourneert. |
| API voor zoeken in logboeken |De Log Search-API gebruiken om via een programma maakt een computergroep op basis van de resultaten van een query voor. |
| Active Directory |Automatisch scannen op het groepslidmaatschap van de agentcomputers die lid zijn van een Active Directory-domein en een groep maken in Azure Monitor voor elke beveiligingsgroep. (Alleen Windows-machines)|
| Configuratiebeheer | Verzamelingen importeren uit System Center Configuration Manager en maak een groep in Azure Monitor voor elk. |
| Windows Server updateservices |Automatisch scannen van WSUS-servers of clients die zijn gericht op groepen en een groep maken in Azure Monitor voor elk. |

### <a name="log-query"></a>Logboekquery
Gemaakt op basis van een query voor computergroepen bevatten alle van de computers die zijn geretourneerd door een query die u definieert.  Deze query wordt uitgevoerd telkens als groep op de computer wordt gebruikt, zodat alle wijzigingen die sinds de groep is gemaakt is doorgevoerd.  

U kunt een query voor een computergroep, maar er moet een afzonderlijke set computers geretourneerd met behulp van `distinct Computer`.  Hieronder volgt een typisch voorbeeld-query die u voor als een computergroep gebruiken kunt.

    Heartbeat | where Computer contains "srv" | distinct Computer

Gebruik de volgende procedure een computergroep maken vanuit een zoeken in Logboeken in Azure portal.

1. Klik op **logboeken** in de **Azure Monitor** in het menu in de Azure-portal.
1. Maken en uitvoeren van een query die wordt geretourneerd van de computers die u wilt in de groep.
1. Klik op **opslaan** aan de bovenkant van het scherm.
1. Wijziging **opslaan als** naar **functie** en selecteer **deze query opslaan als een computergroep**.
1. Geef waarden op voor elke eigenschap voor de computergroep die worden beschreven in de tabel en klik op **opslaan**.

De volgende tabel beschrijft de eigenschappen die een computergroep definiëren.

| Eigenschap | Description |
|:---|:---|
| Name   | Naam van de query om weer te geven in de portal. |
| Functiealias | Een unieke alias die wordt gebruikt voor het identificeren van de groep van de computer in een query. |
| Categorie       | De categorie te organiseren van de query's in de portal. |


### <a name="active-directory"></a>Active Directory
Wanneer u Azure-Monitor voor het Active Directory-groepslidmaatschappen importeren configureert, wordt het groepslidmaatschap van alle computers Windows toegevoegd aan een domein met de Log Analytics-agent geanalyseerd.  Een computergroep is gemaakt in Azure Monitor voor elke beveiligingsgroep in Active Directory en elke Windows-computer wordt toegevoegd aan de overeenkomt met de beveiligingsgroepen die ze lid van zijn computergroepen.  Dit lidmaatschap wordt voortdurend bijgewerkt voor elke 4 uur.  

> [!NOTE]
> Geïmporteerde Active Directory-groepen bevatten alleen Windows-machines.

Configureren van Azure Monitor voor het importeren van Active Directory-beveiligingsgroepen van **geavanceerde instellingen** in uw Log Analytics-werkruimte in de Azure-portal.  Selecteer **computergroepen**, **Active Directory**, en vervolgens **importeren Active Directory-groepslidmaatschappen van computers**.  Er is geen verdere configuratie nodig.

![Computergroepen uit Active Directory](media/computer-groups/configure-activedirectory.png)

Wanneer groepen zijn geïmporteerd, wordt het menu geeft het aantal computers met groepslidmaatschappen gedetecteerd en het aantal groepen geïmporteerd.  U kunt klikken op een van deze koppelingen om terug te keren de **ComputerGroup** records met deze informatie.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Wanneer u Azure Monitor op de WSUS-groepslidmaatschappen importeren configureert, wordt het lidmaatschap van de toewijzing van alle computers met de Log Analytics-agent geanalyseerd.  Als u van client-side gebruikmaakt heeft doelen, elke computer die is verbonden met Azure Monitor en deel uitmaakt van een WSUS het groepslidmaatschap geïmporteerd naar Azure Monitor die gericht is op groepen. Als u van server-side gebruikmaakt moet doelen, de Log Analytics agent worden geïnstalleerd op de WSUS-server om de gegevens van het groepslidmaatschap worden geïmporteerd naar Azure Monitor.  Dit lidmaatschap wordt voortdurend bijgewerkt voor elke 4 uur. 

Configureren van Azure Monitor voor het importeren van de WSUS-groepen van **geavanceerde instellingen** in uw Log Analytics-werkruimte in de Azure-portal.  Selecteer **computergroepen**, **WSUS**, en vervolgens **importeren WSUS-groepslidmaatschappen**.  Er is geen verdere configuratie nodig.

![Computergroepen van WSUS](media/computer-groups/configure-wsus.png)

Wanneer groepen zijn geïmporteerd, wordt het menu geeft het aantal computers met groepslidmaatschappen gedetecteerd en het aantal groepen geïmporteerd.  U kunt klikken op een van deze koppelingen om terug te keren de **ComputerGroup** records met deze informatie.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Wanneer u Azure-Monitor voor het importeren van Configuration Manager-verzamelingslidmaatschappen configureert, maakt u een computergroep voor elke verzameling.  De lidmaatschapsgegevens van de verzameling wordt elke drie uur zodat de computergroepen huidige opgehaald. 

Voordat u Configuration Manager-verzamelingen importeren kunt, moet u [Configuration Manager verbinden met Azure Monitor](collect-sccm.md).  Vervolgens kunt u het importeren van configureren **geavanceerde instellingen** in uw Log Analytics-werkruimte in de Azure-portal.  Selecteer **computergroepen**, **SCCM**, en vervolgens **Import Configuration Manager-verzamelingslidmaatschappen**.  Er is geen verdere configuratie nodig.

![Computergroepen uit SCCM](media/computer-groups/configure-sccm.png)

Wanneer verzamelingen zijn geïmporteerd, wordt het menu geeft het aantal computers met groepslidmaatschappen gedetecteerd en het aantal groepen geïmporteerd.  U kunt klikken op een van deze koppelingen om terug te keren de **ComputerGroup** records met deze informatie.

## <a name="managing-computer-groups"></a>Computergroepen beheren
U kunt computergroepen die zijn gemaakt op basis van een logboekquery of de Log Search-API van weergeven **geavanceerde instellingen** in uw Log Analytics-werkruimte in de Azure-portal.  Selecteer **computergroepen** en vervolgens **opgeslagen groepen**.  

Klik op de **x** in de **verwijderen** kolom te verwijderen van de groep op de computer.  Klik op de **leden weergeven** pictogram voor een groep voor het uitvoeren van zoeken in Logboeken van de groep die de leden als resultaat geeft.  U een computergroep niet wijzigen, maar in plaats daarvan moet verwijderen en vervolgens opnieuw te maken met de gewijzigde instellingen.

![Opgeslagen computergroepen](media/computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-query"></a>Met behulp van de groep van een computer in een logboekquery
U een computergroep gemaakt op basis van een logboekquery in een query door de alias te behandelen als een functie, meestal met de volgende syntaxis gebruiken:

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
Een record wordt gemaakt in de werkruimte voor logboekanalyse voor elke computer groepslidmaatschap gemaakt op basis van Active Directory of WSUS.  Deze records zijn een type **ComputerGroup** en hebben de eigenschappen in de volgende tabel.  Records worden niet gemaakt voor computergroepen op basis van Logboeken-query's.

| Eigenschap | Description |
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
* Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.  

