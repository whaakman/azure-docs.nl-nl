---
title: Verplichte artefacten opgeven voor uw Azure DevTest Labs | Microsoft Docs
description: Informatie over het opgeven van de verplichte artefacten die nodig hebt geïnstalleerd voordat u een gebruiker geselecteerde artefacten installeert op virtuele machines (VM's) in het lab.
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: spelluru
ms.openlocfilehash: 090236ec3647c7c3e38eb862780a615f854e952b
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905797"
---
# <a name="specify-mandatory-artifacts-for-your-lab-in-azure-devtest-labs"></a>Geef op verplichte artefacten voor uw lab in Azure DevTest Labs
Als een eigenaar van een lab, kunt u verplicht artefacten die worden toegepast op elke machine die is gemaakt in het lab. Stel een scenario waar u elke computer in uw testomgeving moet worden verbonden met uw bedrijfsnetwerk. In dit geval moet elke gebruiker lab een domain-join-artefact toevoegen tijdens het maken van virtuele machine om ervoor te zorgen dat hun computer is verbonden met het bedrijfsnetwerk domein. Labgebruikers hoeft met andere woorden, in feite een virtuele machine opnieuw maken in het geval ze vergeten om toe te passen verplichte artefacten op hun computer. Als lab-eigenaar kunt maken u het artefact domain-join als een verplichte artefact in uw testomgeving. Deze stap zorgt ervoor dat elke computer is verbonden met het bedrijfsnetwerk en het opslaan van de tijd en moeite voor uw labgebruikers.
 
Andere verplichte artefacten kunnen bevatten een veelgebruikt hulpprogramma, die gebruikmaakt van uw team of een platform met betrekking tot beveiliging pack dat nodig is voor elke machine hebben standaard enzovoort. Kort gezegd, wordt de algemene software die elke computer in uw testomgeving moet hebben een verplichte artefact. Als u een aangepaste installatiekopie maken van een virtuele machine waarvoor verplichte artefacten die zijn toegepast op deze en maak vervolgens een nieuwe machine van die installatiekopie, zijn de verplichte artefacten tijdens het maken worden toegepast op de machine. Dit gedrag betekent ook dat zelfs als de aangepaste installatiekopie oud is is, telkens wanneer u een virtuele machine uit de meest recente versie van de verplichte artefacten worden toegepast op deze tijdens de stroom maken. 
 
Alleen de artefacten die geen parameters hebben worden als verplichte die ondersteund. Uw lab-gebruiker hoeft niet te extra parameters opgeven tijdens het maken van een lab en waardoor het proces voor het maken van VM eenvoudige. 

## <a name="specify-mandatory-artifacts"></a>Verplichte artefacten opgeven
U kunt de verplichte artefacten voor Windows en Linux-machines afzonderlijk selecteren. U kunt ook de volgorde wijzigen voor deze artefacten, afhankelijk van de volgorde waarin u ze dat wilt toegepast. 

1. Selecteer op de startpagina van uw praktijksessie **configuratie en het beleid** onder **instellingen**. 
3. Selecteer **verplichte artefacten** onder **externe bronnen**. 
4. Selecteer **bewerken** in de **Windows** sectie of de **Linux** sectie. In dit voorbeeld wordt de **Windows** optie. 

    ![Verplichte artefacten pagina - knop bewerken](media/devtest-lab-mandatory-artifacts/mandatory-artifacts-edit-button.png)
4. Selecteer een artefact. In dit voorbeeld wordt **7-Zip** optie. 
5. Op de **toevoegen artefact** weergeeft, schakelt **toevoegen**. 

    ![Verplichte artefacten pagina - 7-zip toevoegen](media/devtest-lab-mandatory-artifacts/add-seven-zip.png)
6. Als u wilt een andere artefacten toevoegen, selecteert u het artikel en selecteer **toevoegen**. In dit voorbeeld wordt **Chrome** als de tweede verplichte artefact.

    ![Verplichte artefacten pagina - Chrome toevoegen](media/devtest-lab-mandatory-artifacts/add-chrome.png)
7. Op de **verplichte artefacten** pagina, ziet u een bericht dat Hiermee geeft u het nummer van de artefacten die zijn geselecteerd. Als u het bericht klikt, ziet u de artefacten die u hebt geselecteerd. Selecteer **opslaan** om op te slaan. 

    ![Verplichte artefacten pagina - opslaan artefacten](media/devtest-lab-mandatory-artifacts/save-artifacts.png)
8. Herhaal de stappen voor het verplichte artefacten voor Linux-VM's opgeven. 
    
    ![Verplichte artefacten pagina - artefacten voor Windows en Linux](media/devtest-lab-mandatory-artifacts/windows-linux-artifacts.png)
9. Naar **verwijderen** een artefact in de lijst, selecteer **... (ellips)**  aan het einde van de rij en selecteer **verwijderen**. 
10. Naar **opnieuw rangschikken** artefacten in de lijst, plaats de muisaanwijzer Beweeg de muis over het artefact, schakel **... (ellips)**  die wordt weergegeven aan het begin van de rij en sleept u het item naar de nieuwe positie. 
11. Selecteren om op te slaan verplichte artefacten in het lab, **opslaan**. 

    ![Verplichte artefacten pagina - opslaan artefacten in het lab](media/devtest-lab-mandatory-artifacts/save-to-lab.png)
12. Sluit de **configuratie en het beleid** pagina (Selecteer **X** in de rechterbovenhoek) terug te gaan naar de startpagina van uw testomgeving.  

## <a name="delete-a-mandatory-artifact"></a>Een verplichte artefact verwijderen
Als een verplichte artefact verwijderen uit een lab, moet u de volgende acties uitvoeren: 

1. Selecteer **configuratie en het beleid** onder **instellingen**. 
2. Selecteer **verplichte artefacten** onder **externe bronnen**. 
3. Selecteer **bewerken** in de **Windows** sectie of de **Linux** sectie. In dit voorbeeld wordt de **Windows** optie. 
4. Selecteer het bericht met het aantal verplichte artefacten aan de bovenkant. 

    ![Verplichte artefacten pagina: Selecteer het bericht](media/devtest-lab-mandatory-artifacts/select-message-artifacts.png)
5. Op de **geselecteerd artefacten** weergeeft, schakelt **... (ellips)**  voor het artefact dat moet worden verwijderd, en selecteer **verwijderen**. 
    
    ![Verplichte artefacten pagina - artefacten verwijderen](media/devtest-lab-mandatory-artifacts/remove-artifact.png)
6. Selecteer **OK** sluiten de **geselecteerd artefacten** pagina. 
7. Selecteer **opslaan** op de **verplichte artefacten** pagina.
8. Herhaal de stappen voor **Linux** afbeeldingen indien nodig. 
9. Selecteer **opslaan** alle wijzigingen in het lab wilt opslaan. 

## <a name="view-mandatory-artifacts-when-creating-a-vm"></a>Verplichte artefacten weergeven bij het maken van een virtuele machine
Nu als een lab-gebruiker kunt u de lijst met verplichte artefacten weergeven tijdens het maken van een virtuele machine in het lab. U niet bewerken of verwijderen van de verplichte artefacten in het lab instellen door de eigenaar van het lab.

1. Selecteer op de startpagina van uw lab, **overzicht** in het menu.
2. Selecteer een virtuele machine toevoegen aan het lab, **+ toevoegen**. 
3. Selecteer een **basisinstallatiekopie**. In dit voorbeeld wordt **Windows Server, versie 1709**.
4. U ziet dat er een bericht voor **artefacten** met het aantal verplichte artefacten die zijn geselecteerd. 
5. Selecteer **artefacten**. 
6. Controleer of de **verplichte artefacten** u hebt opgegeven in de configuratie en het beleid van het lab. 

    ![Een virtuele machine - verplichte artefacten maken](media/devtest-lab-mandatory-artifacts/create-vm-artifacts.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [een Git-opslagplaats voor artefacten toevoegen aan een lab](devtest-lab-add-artifact-repo.md).

