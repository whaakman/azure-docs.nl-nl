---
title: Computergroepen in Azure-logboekanalyse Meld zoekopdrachten | Microsoft Docs
description: Computergroepen in Log Analytics kunnen u bereik logboek zoekopdrachten aan een bepaalde set van computers.  In dit artikel beschrijft de verschillende methoden die u gebruiken kunt om computergroepen en hoe ze te gebruiken in een zoekopdracht logboek te maken.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: bwren
ms.openlocfilehash: 4d6a80082711f09e9c189d53fb4fda00a7d73c29
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Meld u zoekopdrachten computergroepen in Log Analytics

Computergroepen in Log Analytics kunnen u bereik [Meld zoekopdrachten](log-analytics-log-search-new.md) aan een bepaalde set van computers.  Elke groep wordt gevuld met computers met behulp van een query die u definieert of groepen importeren uit verschillende bronnen.  Wanneer de groep is opgenomen in een zoekopdracht in de logboekbestanden, zijn de resultaten beperkt tot records die overeenkomen met de computers in de groep.

## <a name="creating-a-computer-group"></a>Een computergroep maken
U kunt een computergroep maken in een van de methoden in de volgende tabel met logboekanalyse.  In de onderstaande secties vindt u meer informatie over elke methode. 

| Methode | Beschrijving |
|:--- |:--- |
| Zoekopdrachten in logboeken |Maak een logboek zoekquery waarmee een lijst met computers. |
| API voor zoeken in logboeken |Het logboek zoeken-API maken op een computergroep op basis van de resultaten van een zoekopdracht logboek gebruiken. |
| Active Directory |Automatisch scannen van het groepslidmaatschap van de agentcomputers die lid zijn van een Active Directory-domein en een groep maken in Log Analytics voor elke beveiligingsgroep. |
| Configuratiebeheer | Verzamelingen importeren uit System Center Configuration Manager en maak een groep in Log Analytics voor elk. |
| Windows Server updateservices |Automatisch scannen van WSUS-servers of clients gebruiken om groepen en maakt u een groep in Log Analytics voor elk. |

### <a name="log-search"></a>Zoekopdrachten in logboeken
Gemaakt op basis van een zoekopdracht logboek computergroepen bevatten alle computers geretourneerd door een query die u definieert.  Deze query wordt uitgevoerd telkens wanneer de computergroep wordt gebruikt zodat alle wijzigingen die sinds de groep is gemaakt wordt weergegeven.  

U kunt een query voor de groep van een computer, maar er moet een unieke set computers geretourneerd met behulp van `distinct Computer`.  Hier volgt een typisch voorbeeld zoeken die u voor als een computergroep gebruiken kunt.

    Heartbeat | where Computer contains "srv" | distinct Computer

De volgende tabel beschrijft de eigenschappen die een computergroep definiëren.

| Eigenschap | Beschrijving |
|:---|:---|
| Weergavenaam   | De naam van de zoekopdracht wilt weergeven in de portal. |
| Category       | De categorie te organiseren de zoekopdrachten die in de portal. |
| Query’s uitvoeren          | De query voor de computergroep. |
| Functie alias | Een unieke alias die wordt gebruikt voor het identificeren van de groep van de computer in een query. |

Gebruik de volgende procedure een computergroep maken van een zoekopdracht logboek in de Azure portal.

2. Open **logboek zoeken** en klik vervolgens op **opgeslagen zoekacties** aan de bovenkant van het scherm.
3. Klik op **toevoegen** en geef waarden op voor elke eigenschap voor de computergroep.
4. Selecteer **deze query opslaan als een computergroep** en klik op **OK**.


Gebruik de volgende procedure een computergroep maken van een zoekopdracht logboek in de OMS-portal.

1. Open **logboek zoeken** en maak de zoekopdracht logboek voor de computergroep.  
2. Klik op de **opslaan** knop aan de bovenkant van het scherm.
3. Selecteer **Ja** naar **deze query opslaan als een computergroep**.
5. Geef waarden op voor elke eigenschap voor de computergroep. 


>[!NOTE]
> Als uw werkruimte is nog steeds de [verouderde logboekanalyse querytaal](log-analytics-log-search-upgrade.md) en u dezelfde procedure voor het maken van een computergroep gebruiken, maar u moet de syntaxis van de verouderde querytaal gebruiken.


### <a name="log-search-api"></a>Meld u API van zoekservice
Computergroepen die zijn gemaakt met de Search-API van het logboek zijn hetzelfde als de zoekopdrachten die zijn gemaakt met een zoekopdracht logboek.  Zie voor meer informatie over het maken van de groep van een computer met de Search-API van het logboek [computergroepen in logboek logboekanalyse zoeken REST-API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory
Wanneer u Log Analytics Active Directory-groepslidmaatschappen importeren configureert, wordt het groepslidmaatschap van computers met de OMS-agent lid van domein geanalyseerd.  Een computergroep in logboekanalyse gemaakt voor elke beveiligingsgroep in Active Directory en elke computer is toegevoegd aan de overeenkomt met de beveiligingsgroepen waarvan die ze lid van zijn computergroepen.  Dit lidmaatschap wordt voortdurend bijgewerkt voor elke 4 uur.  

Configureren van logboekanalyse voor het importeren van Active Directory-beveiligingsgroepen uit logboekanalyse **geavanceerde instellingen** in de Azure portal.  Selecteer **computergroepen**, **Active Directory**, en vervolgens **importeren Active Directory-groepslidmaatschappen van computers**.  Er is geen verdere configuratie nodig.

![Computergroepen uit Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Wanneer u groepen zijn geïmporteerd, worden in het menu het aantal computers met groepslidmaatschappen gedetecteerd en het aantal groepen geïmporteerd worden.  U kunt klikken op een van deze koppelingen om terug te keren de **ComputerGroup** records met deze informatie.

### <a name="windows-server-update-service"></a>Windows Server updateservice
Wanneer u Log Analytics WSUS-groepslidmaatschappen importeren configureert, wordt het lidmaatschap van de toewijzing van alle computers met de OMS-agent geanalyseerd.  Als u aan de clientzijde heeft ontwikkelt, elke computer die is verbonden met logboekanalyse en maakt deel uit van een WSUS het groepslidmaatschap geïmporteerd met logboekanalyse groepen als doel. Als u van server side gebruikmaakt moet ontwikkelt, de OMS agent worden geïnstalleerd op de WSUS-server om de gegevens moeten worden geïmporteerd met logboekanalyse.  Dit lidmaatschap wordt voortdurend bijgewerkt voor elke 4 uur. 

Configureren van logboekanalyse voor het importeren van de WSUS-groepen van logboekanalyse **geavanceerde instellingen** in de Azure portal.  Selecteer **computergroepen**, **WSUS**, en vervolgens **importeren WSUS-groepslidmaatschappen**.  Er is geen verdere configuratie nodig.

![Computergroepen van WSUS](media/log-analytics-computer-groups/configure-wsus.png)

Wanneer u groepen zijn geïmporteerd, worden in het menu het aantal computers met groepslidmaatschappen gedetecteerd en het aantal groepen geïmporteerd worden.  U kunt klikken op een van deze koppelingen om terug te keren de **ComputerGroup** records met deze informatie.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Wanneer u logboekanalyse voor het importeren van Configuration Manager-verzamelinglidmaatschappen configureert, maakt u een computergroep voor elke verzameling.  De lidmaatschapsgegevens van de verzameling wordt elke drie uur zodat de computergroepen huidige opgehaald. 

Voordat u Configuration Manager-verzamelingen importeren kunt, moet u [Configuration Manager verbinden met logboekanalyse](log-analytics-sccm.md).  Vervolgens kunt u het importeren van logboekanalyse **geavanceerde instellingen** in de Azure portal.  Selecteer **computergroepen**, **SCCM**, en vervolgens **Import Configuration Manager-verzamelinglidmaatschappen**.  Er is geen verdere configuratie nodig.

![Computergroepen uit SCCM](media/log-analytics-computer-groups/configure-sccm.png)

Wanneer u verzamelingen zijn geïmporteerd, worden in het menu het aantal computers met groepslidmaatschappen gedetecteerd en het aantal groepen geïmporteerd worden.  U kunt klikken op een van deze koppelingen om terug te keren de **ComputerGroup** records met deze informatie.

## <a name="managing-computer-groups"></a>Computergroepen beheren
U kunt computergroepen die zijn gemaakt van een zoekopdracht logboek of de Search-API van het logboek van logboekanalyse weergeven **geavanceerde instellingen** in de Azure portal.  Selecteer **computergroepen** en vervolgens **opgeslagen groepen**.  

Klik op de **x** in de **verwijderen** kolom te verwijderen van de computergroep.  Klik op de **leden weergeven** pictogram voor een groep voor het uitvoeren van de groep logboek zoekquery waarmee de leden ervan.  Een computergroep niet wijzigen, maar in plaats daarvan moet verwijderen en vervolgens opnieuw te maken met de gewijzigde instellingen.

![Opgeslagen computergroepen](media/log-analytics-computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-search"></a>Met behulp van de groep van een computer in een logboek zoekopdracht
U een computergroep gemaakt op basis van een zoekopdracht logboek in een query door de alias behandelen als een functie, meestal met de volgende syntaxis gebruiken:

  `Table | where Computer in (ComputerGroup)`

U kunt bijvoorbeeld het volgende om terug te keren UpdateSummary records voor alleen computers in een computergroep mycomputergroup aangeroepen.
 
  `UpdateSummary | where Computer in (mycomputergroup)`


Geïmporteerde computergroepen en hun computers opgenomen worden opgeslagen in de **ComputerGroup** tabel.  De volgende query zou bijvoorbeeld een lijst met computers in de groep Computers in het domein van Active Directory. 

  `ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer`

De volgende query zou UpdateSummary records voor alleen computers in Domeincomputers retourneren.

  ```
  let ADComputers = ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer;
  UpdateSummary | where Computer in (ADComputers)
  ```



  

>[!NOTE]
> Als uw werkruimte is nog steeds de [verouderde logboekanalyse querytaal](log-analytics-log-search-upgrade.md)>, u de volgende syntaxis gebruiken om te verwijzen naar een computergroep in een logboek zoekopdracht.  Geven de **categorie** > is optioneel en is alleen vereist als er computergroepen met dezelfde naam in verschillende categorieën. 
>
>    `$ComputerGroups[Category: Name]`
>
>Computergroepen worden doorgaans gebruikt voor de **IN** -component in de zoekopdracht logboek zoals in het volgende voorbeeld:
>
>    `Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]`



## <a name="computer-group-records"></a>Groeperen van computerrecords
Een record gemaakt in de werkruimte voor logboekanalyse voor elke computer groepslidmaatschap gemaakt op basis van Active Directory of WSUS.  Deze records zijn een type **ComputerGroup** en de eigenschappen in de volgende tabel hebben.  Records worden niet gemaakt voor computergroepen op basis van het logboek zoekopdrachten.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Computer |Naam van de computer die lid is. |
| Groep |De naam van de groep. |
| GroupFullName |Volledig pad naar de groep met inbegrip van de bron en de naam van de gegevensbron. |
| GroupSource |Bron van die groep die is verzameld van is. <br><br>Active Directory<br>WSUS<br>WSUSClientTargeting |
| GroupSourceName |De naam van de bron waarvan de groep is verzameld.  Dit is de domeinnaam voor Active Directory. |
| ManagementGroupName |Naam van de beheergroep voor SCOM-agents.  Dit is voor andere agents AOI -\<werkruimte-ID\> |
| TimeGenerated |Datum en tijd van de computergroep is gemaakt of bijgewerkt. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren.  

