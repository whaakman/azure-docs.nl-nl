---
title: Werkblad voor capaciteitsplanning voor Azure Stack | Microsoft Docs
description: Meer informatie over de capaciteitsplanning spreadsheet voor Azure Stack-implementaties.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2018
ms.author: jeffgilb
ms.reviewer: prchint
ms.lastreviewed: 12/11/2018
ms.openlocfilehash: 459398f5f40ae5fb28dfa4ac73b94aecca7304eb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58099413"
---
# <a name="azure-stack-capacity-planner"></a>Azure Stack Capacity Planner
De Azure Stack Capacity Planner is een werkblad gebruikt voor de capaciteitsplanning voor Azure Stack-resource. De Capaciteitsplanner biedt u de mogelijkheid om verschillende toewijzingen van computerbronnen ontwerpen en zien hoe deze zou passen in een selectie van hardware-aanbiedingen. Hieronder vindt u gedetailleerde instructies voor het gebruik van de Azure Stack-Calculator.

## <a name="worksheet-descriptions"></a>Werkblad beschrijvingen
Hieronder volgt een kort overzicht van de werkbladen die deel uitmaken van het Azure Stack Capacity Planner-werkblad dat kan worden gedownload vanaf [ https://aka.ms/azstackcapacityplanner ](https://aka.ms/azstackcapacityplanner):

|Tabbladnaam|Description|
|-----|-----|
|Version-Disclaimer|Kort overzicht van het doel van de Rekenmachine, versienummer en vrijgegeven datum.|
|Instructies|Geeft gedetailleerde instructies voor het gebruik van de Azure Stack Capacity Planner.|
|DefinedSolutionSKUs|Tabel met meerdere kolommen dat maximaal vijf hardware definities bevat. De vermeldingen in dit werkblad zijn voorbeelden. De bedoeling is dat de gebruiker de details voor systeemconfiguraties wordt overwogen voor gebruiken of kopen zullen veranderen.|
|DefineByVMFootprint|Zoek de juiste hardware-SKU door het maken van een verzameling van verschillende grootten en aantallen van virtuele machines.|
|DefineByWorkloadFootprint|Zoek de juiste hardware-SKU door het maken van een verzameling van Azure Stack-werkbelastingen.|
|  |  |

## <a name="definedsolutionskus-instructions"></a>DefinedSolutionSKUs instructies
Dit werkblad bevat voorbeelden van de definitie van maximaal vijf hardware. Details zodat deze overeenkomen met de configuratie wordt overwogen voor gebruik wijzigen of kopen.

### <a name="hardware-selections-provided-by-authorized-hardware-partners"></a>Hardware-selecties aangeboden door hardwarepartners geautoriseerd
Azure Stack wordt geleverd als een geïntegreerd systeem waarop software is geïnstalleerd door oplossingspartners. Deze voor oplossingspartners hebben hun eigen, gezaghebbende versies van Azure Stack-hulpprogramma's en deze hulpprogramma's voor capaciteitsplanning moet worden gebruikt voor het voltooien van de discussies van een oplossing capaciteit.

### <a name="multiple-ways-to-model-computing-resources"></a>Meerdere manieren om modellen te maken over Resources voor Computing
De elementaire bouwstenen voor resource modelleren in de Azure Stack-planner gebruikt, zijn de verschillende grootten van virtuele machines (VM's) voor Azure Stack. Het bereik van deze VM's de grootte van de kleinste waarde, "Basic 0", tot aan de huidige grootste VM "Standard_Fsv2." Op basis van uw behoeften, worden verschillende hoeveelheden van verschillende virtuele machines gebruikt om te maken van computing resourcetoewijzingen met dit hulpprogramma op twee verschillende manieren.

1. De planner-gebruiker selecteert de aanbieding van een specifieke hardware en krijgt te zien welke combinaties van verschillende resources binnen deze hardwarebronnen passen. 

2. De planner-gebruiker een specifieke combinatie van toewijzingen van virtuele machine maakt en vervolgens kunt de Azure-Resource-Calculator ziet u welke beschikbare hardware-SKU's geschikt voor het zijn ondersteunen van deze VM-configuratie.

Dit hulpprogramma biedt twee methoden voor het toewijzen van resources van de virtuele machine. hetzij als één enkele verzameling resourcetoewijzingen VM of als een verzameling van maximaal zes verschillende configuraties in de Workload. Elke Workload-configuratie kan een andere toewijzing van beschikbare VM-resources bevatten. Hieronder vindt u gedetailleerde informatie over het maken en gebruiken van elk van deze toewijzing-modellen. Alleen de waarden die zijn opgenomen in niet-achtergrond gearceerde cellen of binnen SKU keuzemenu lijsten in dit werkblad moeten worden gewijzigd. Wijzigingen in de gearceerde cellen werken mogelijk niet meer resource berekeningen.


## <a name="definebyvmfootprint-instructions"></a>DefineByVMFootprint instructies
Selecteer het tabblad 'DefineByVMFootprint' voor het maken van een model met één verzameling van verschillende grootten en aantallen van virtuele machines, en volgt u deze reeks stappen.

1. In de rechterbovenhoek van dit werkblad voor het gebruik van besturingselementen van het opgegeven pull-keuzelijst om een initiële nummer te selecteren van servers (tussen 4 en 16) u wilt installeren in elk hardwaresysteem (SKU). Dit aantal servers kan worden gewijzigd op elk gewenst moment tijdens het modelleringsproces om te zien hoe dit van invloed is op de algemene beschikbare resources voor uw model voor brontoewijzing.
2. Als u wilt dat als model voor verschillende VM-resource-toewijzingen op basis van een specifieke hardwareconfiguratie, vindt u in het blauw pull-keuzelijst direct onder het label 'Huidige SKU' in de rechterbovenhoek van de pagina. Haal deze keuzelijst met invoervak en selecteer de gewenste hardware-SKU.
3. U bent nu klaar om te beginnen met het toevoegen van verschillende grootte VM's aan uw model. Als u wilt opnemen in een bepaald type van de virtuele machine, moet u een hoeveelheidswaarde invoeren in het blauwe vak wordt beschreven aan de linkerkant van de vermelding in die virtuele machine.

   > [!NOTE]
   > Totaal aantal VM-opslag verwijst naar de totale capaciteit van de gegevensschijf van de virtuele machine (aantal ondersteunde schijven * de maximale capaciteit van één schijf (1 TB)). Op basis van de configuratie-indicatoren hebben we de tabel beschikbaar opslagconfiguraties ingevuld, zodat u ervoor uw gewenste niveau van de resource voor opslag voor elke virtuele machine van Azure Stack kiezen kunt. Het is echter belangrijk te weten dat u kan toevoegen of wijzigen van de tabel beschikbaar opslagconfiguraties indien nodig.<br><br>Elke virtuele machine wordt gestart met een lokale tijdelijke opslag in eerste instantie wordt toegewezen. Aanleiding van de thin provisioning van tijdelijke opslag kan het nummer van de lokale tijdelijke iets dat in de vervolgkeuzelijst met inbegrip van de maximale toegestane tijdelijke opslagruimte worden gewijzigd.

4. Als u virtuele machines toevoegt, ziet u de grafieken met beschikbare SKU resources wijzigen. Hiermee kunt u zien wat de effecten van verschillende grootten en hoeveelheden van virtuele machines toe te voegen tijdens het modelleringsproces. Er is een andere manier om het effect van wijzigingen weer te geven om te bekijken van de getallen verbruikt en nog steeds beschikbaar direct onder de lijst met beschikbare virtuele machines. Deze getallen zijn geschatte waarden op basis van de geselecteerde SKU-hardware.
5. Als u de set van virtuele machines hebt gemaakt, kunt u de voorgestelde hardware SKU kunt vinden door te klikken op de knop 'Voorgestelde SKU' in de rechterbovenhoek van de pagina, direct onder het label 'Huidige SKU'. Met deze knop kunt u vervolgens de configuraties van uw virtuele machine wijzigen en zien welke hardware ondersteunt elke configuratie.


## <a name="definebyworkloadfootprint-instructions"></a>DefineByWorkloadFootprint instructions
Selecteer het tabblad 'DefineByWorkloadFootprint' voor het maken van een model met behulp van een verzameling van Azure Stack-werkbelasting, en volgt u deze reeks stappen. Azure Stack-Workloads worden gemaakt met behulp van de beschikbare VM-resources.   

> [!TIP]
> Als u wilt wijzigen van de grootte van de opgegeven voor een virtuele machine van Azure Stack, Zie de opmerking in stap 3 in de vorige sectie.

1. Gebruik van besturingselementen van het opgegeven pull-keuzelijst in de rechterbovenhoek van deze pagina om een initiële nummer te selecteren van servers (tussen 4 en 16) u wilt installeren in elk hardwaresysteem (SKU).
2. Als u wilt dat als model voor verschillende VM-resource-toewijzingen op basis van een specifieke hardwareconfiguratie, vindt u in het blauw pull-keuzelijst direct onder het label 'Huidige SKU' in de rechterbovenhoek van de pagina. Haal deze keuzelijst met invoervak en selecteer de gewenste hardware-SKU.
3. Selecteer de grootte van de juiste voor elk van de gewenste Azure Stack-VM's op de pagina DefineByVMFootprint zoals hierboven is beschreven in stap 3 van DefineByVMFootprint instructies. De opslagruimte per virtuele machine is gedefinieerd in het werkblad DefineByVMFootprint.
4. Vanaf de linkerbovenhoek van de pagina DefineByWorkloadFootprint, configuraties voor maximaal zes verschillende workloadtypen maken door te voeren van de hoeveelheid van elke VM-type dat is opgenomen in die werkbelasting. Dit wordt gedaan door het plaatsen van numerieke waarden in de kolom direct onder de naam van de Workload. Namen van de werkbelasting kunnen worden gewijzigd om het type van de werkbelastingen die worden ondersteund door deze bepaalde configuratie weer te geven.
5. Door een waarde aan de onderkant van die kolom direct onder het label "Hoeveelheid", mag u een bepaalde hoeveelheid van elk type werkbelasting opnemen.
6. Zodra de typen werkbelasting en aantallen zijn gemaakt, te klikken op de "voorgesteld SKU" knop gevonden in de rechterbovenhoek van de pagina, direct onder het label "Huidige SKU" zorgt ervoor dat de kleinste SKU met voldoende resources voor ondersteuning van deze algemene configuratie van Workloads die worden weergegeven.
7. Verder modelleren kan worden gedaan door het wijzigen van het aantal servers die zijn geselecteerd voor een hardware-SKU, of het wijzigen van de toewijzingen van virtuele machine of hoeveelheden binnen de configuraties van uw Workload. De bijbehorende grafieken wordt onmiddellijk feedback van de invloed van uw wijzigingen op het totale Resourcegebruik weergegeven.
8. Als u tevreden met uw wijzigingen bent, wordt opnieuw te klikken op de knop 'Voorgestelde SKU' de SKU voorgesteld voor de nieuwe configuratie weergegeven.


## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [datacenter-integratie-overwegingen voor Azure Stack](azure-stack-datacenter-integration.md)
