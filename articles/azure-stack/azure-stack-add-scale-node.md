---
title: Azure Stack schaal knooppunten toevoegen | Microsoft Docs
description: Eenheden schalen in Azure Stack-knooppunten toevoegen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: brenduns
ms.reviewer: thoroet
ms.openlocfilehash: 15c46bb0fee4689b614636d8c9e01e36f7f0be3d
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228033"
---
# <a name="add-additional-scale-unit-nodes-in-azure-stack"></a>Extra scale unit knooppunten toevoegen in Azure Stack

Azure Stack-operators kunnen de totale capaciteit van een bestaande schaaleenheid vergroten door het toevoegen van een andere fysieke computer. De fysieke computer is ook een scale unit-knooppunt genoemd. Elke nieuwe scale unit knooppunt die u toevoegt moet homogene in CPU-type, geheugen en schijfnummer & grootte van de knooppunten die al aanwezig zijn in de schaaleenheid.  

> [!NOTE]  
U moet extra scale Azure Stack 1807 of hoger om toe te voegen eenheid knooppunten uitvoeren.

Als u wilt toevoegen een scale unit-knooppunt, fungeren als Azure Stack en hulpprogramma's van uw hardware-leveranciers (OEM) worden uitgevoerd. De OEM-tooling wordt uitgevoerd op de hardware lifecycle-host (HLH) om te controleren of de nieuwe fysieke computer overeenkomt met het niveau van de dezelfde firmware als bestaande knooppunten.

Het volgende stroomdiagram ziet u het algemene proces voor het toevoegen van een scale unit-knooppunt.

![Schaal eenheid stroom toevoegen](media/azure-stack-add-scale-node/add-node-flow.png) &#42; *of de leverancier van de OEM-hardware enacts de plaatsing van de rack fysieke server en de firmware-updates is afhankelijk van uw contract support.*

De bewerking voor het toevoegen van een nieuw knooppunt duurt enkele uren of dagen om te voltooien.

## <a name="add-scale-unit-nodes"></a>Schaal eenheid knooppunten toevoegen 
De volgende stappen zijn een gedetailleerd overzicht van hoe u een knooppunt toevoegt. Geen Volg deze stappen zonder eerste verwijzen naar de documentatie bij uw OEM geleverde capaciteit-uitbreiding.

1. De nieuwe fysieke server in het rek te plaatsen en op de juiste wijze kabel. 
2. Het juiste IP-adres in de BMC (baseboard management controller) configureren en toepassen van alle BIOS-instellingen per uw OEM-documentatie.
3. De huidige basislijn van de firmware van toepassing op alle onderdelen met de hulpprogramma's die worden geleverd door de fabrikant van de hardware die worden uitgevoerd op de HLH.
4. Voer de bewerking van de knooppunt toevoegen in de Azure Stack-beheerportal.
5. Valideren dat de add-knooppunt-bewerking is geslaagd. Om dit te doen, controleert u de [ **Status** van de schaaleenheid](#monitor-add-node-operations). 

## <a name="add-the-node"></a>Het knooppunt toevoegen 
U kunt de beheerconsole of PowerShell gebruiken om toe te voegen nieuwe knooppunten. Het knooppunt toevoegbewerking eerst het nieuwe knooppunt van de schaal-eenheid als beschikbare compute-capaciteit toegevoegd en vervolgens breidt de opslagcapaciteit. De capaciteit wordt automatisch vergroot, omdat Azure Stack een hypergeconvergeerd systeem is waar *compute* en *opslag* samen worden geschaald.

### <a name="use-the-admin-console"></a>Gebruik de-beheerconsole
1. Meld u aan de Azure Stack-beheerportal als Azure Stack-operators.
2. Navigeer naar **regiobeheer** > **schaaleenheden**, en selecteer vervolgens de schaaleenheid die u wilt uitbreiden, de capaciteit voor.  
   ![Schaaleenheid selecteren](media/azure-stack-add-scale-node/select-node1.png)

   ![Details voor schaal eenheid weergeven](media/azure-stack-add-scale-node/select-node2.png)
 
3. De volgende parameters zijn vereist:  
   - **IP-adres van BMC** voor elk schaal eenheid knooppunt dat u toevoegt. (Één IP-adres per regel). ![Knooppunt toevoegen](media/azure-stack-add-scale-node/add-node.png)

### <a name="use-powershell"></a>PowerShell gebruiken

Gebruik de **New-AzsScaleUnitNodeObject** cmdlet voor het toevoegen van een of meer knooppunten.  

Voordat u een van de volgende PowerShell-voorbeeldscripts gebruikt, vervang de waarden *knooppuntnamen* en *IP-adressen* met waarden uit uw Azure Stack-omgeving.

**Voor een enkel knooppunt:**
  ```powershell
  ## Add a single Node 
  $NewNode=New-AzsScaleUnitNodeObject -computername "<name_of_new_node>" -BMCIPv4Address "<BMCIP_address_of_new_node>" 
 
  Add-AzsScaleUnitNode -NodeList $NewNode -ScaleUnit "<name_of_scale_unit_cluster>" 
  ```  

**Meerdere knooppunten toevoegen:**
  ```powershell
  ## Add multiple nodes 
  $NewNode1=New-AzsScaleUnitNodeObject -computername "<name_of_new_node01>"  -BMCIPv4Address "<BMCIP_address_of_new_node01>" 
 
  $NewNode2=New-AzsScaleUnitNodeObject -computername "<name_of_new_node02>" -BMCIPv4Address “<BMCIP_address_of_new_node02>”$ 
 
  Add-AzsScaleUnitNode -NodeList @($NewNode1,$NewNode2) -ScaleUnit "<name_of_scale_unit_cluster>" 
  ```

## <a name="monitor-add-node-operations"></a>Bewaak de bewerkingen van knooppunt toevoegen 
U kunt de admin-portal of PowerShell om op te halen van de status van de add-knooppunt-bewerking. Voeg knooppunt operations kunnen enkele uren duren tot dagen om te voltooien.

### <a name="use-the-admin-console"></a>Gebruik de-beheerconsole 
Voor het controleren van het toevoegen van een nieuw knooppunt in de beheerportal de schaaleenheid controleren of schalen van objecten van unit-knooppunt. Om dit te doen, gaat u naar **regiobeheer** > **schaaleenheden**. Selecteer vervolgens de schaaleenheid of schaal eenheid knooppunt die u wilt bekijken. 

### <a name="use-powershell"></a>PowerShell gebruiken
De status van de schaaleenheid en schaal eenheid knooppunten kan worden opgehaald met behulp van PowerShell als volgt:
  ```powershell
  #Retrieve Status for the Scale Unit
  Get-AzsScaleUnit|select name,state
 
  #Retrieve Status for each Scale Unit Node
  Get-AzsScaleUnitNode |Select Name, ScaleUnitNodeStatus
```

### <a name="status-for-the-add-node-operation"></a>Status voor de bewerking van de knooppunten toevoegen 
**Voor een schaaleenheid:**
|Status               |Beschrijving  |
|---------------------|---------|
|In uitvoering              |Alle knooppunten zijn actief deel aan de schaaleenheid.|
|Gestopt              |Het knooppunt van de eenheid schaal is omlaag of niet bereikbaar is.|
|Uitvouwen            |Een of meer scale unit knooppunten worden momenteel toegevoegd als de rekencapaciteit.|
|Opslag configureren  |De rekencapaciteit is uitgebreid en de configuratie van de opslag wordt uitgevoerd.|
|Herstel is vereist |Een fout is aangetroffen die moet worden een of meer eenheid knooppunten van de schaal die u moet worden hersteld.|


**Voor een scale unit-knooppunt:**
|Status                |Beschrijving  |
|----------------------|---------|
|In uitvoering               |Het knooppunt is actief deelnemer in de schaaleenheid.|
|Gestopt               |Het knooppunt is niet beschikbaar.|
|Toevoegen                |Het knooppunt wordt actief toegevoegd aan de schaaleenheid.|
|Bezig met het herstellen van             |Het knooppunt wordt actief hersteld.|
|Onderhoud           |Het knooppunt wordt onderbroken en geen actieve gebruiker-werkbelasting wordt uitgevoerd. |
|Herstel is vereist  |Een fout is aangetroffen die vereist dat het knooppunt moet worden hersteld.|


## <a name="troubleshooting"></a>Problemen oplossen
De volgende zijn problemen die regelmatig voorkomen bij het toevoegen van een knooppunt. 

**Scenario 1:** mislukt de bewerking toevoegen scale unit-knooppunt maar een of meer knooppunten die zijn opgenomen met de status gestopt.  
- Herstel: Gebruik de herstelbewerking te herstellen van een of meer knooppunten. Alleen een één herstelbewerking kan in één keer worden uitgevoerd.

**Scenario 2:** een of meer scale unit-knooppunten zijn toegevoegd, maar de opslaguitbreiding van de is mislukt. In dit scenario wordt het schaalobject eenheid in het knooppunt rapporteert de status van het uitvoeren, maar het configureren van opslag-taak is niet gestart.  
- Herstel: Gebruik het eindpunt van de bevoegde de opslag om status te controleren door het uitvoeren van de volgende PowerShell-cmdlet:
 
**Scenario 3:** ontvangen van een waarschuwing die aangeeft van de opslag scale-out-taak is mislukt.  
- Herstel: In dit geval de configuratietaak opslag is mislukt. Dit probleem, moet u contact opnemen met ondersteuning.


## <a name="next-steps"></a>Volgende stappen 
Beoordeling [knooppunt acties](azure-stack-node-actions.md) 
