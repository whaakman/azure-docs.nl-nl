---
title: Met de Stack van Azure portal | Microsoft Docs
description: Informatie over het openen en gebruiken van de gebruikersportal in Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 5aa00123-5b87-45e0-a671-4165e66bfbc6
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 7c34d7a225be63da95f664525b0366ff89b28838
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="using-the-azure-stack-portal"></a>Met behulp van de Stack van Azure-portal

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als een gebruiker van de Stack van het Azure-services, kunt u de Stack van Azure-portal gebruiken om u te abonneren op openbare voorstellen en gebruikmaken van de services die via deze aanbiedingen beschikbaar worden gesteld. Als u de Azure-portal voordat hebt gebruikt, bent u al bekend bent met de gebruikersinterface.

## <a name="access-the-portal"></a>Toegang tot de portal

Uw Azure-Stack-operator (een serviceprovider of een beheerder in uw organisatie), kunt u weet dat de juiste URL toegang tot de portal. 

- De URL varieert op basis van de operator regio en externe domeinnaam en bevindt zich in de indeling https://portal voor een geïntegreerd systeem. &lt; *regio*&gt;.&lt; *FQDN*&gt;.
- Als u de Azure-Stack Development Kit gebruikt, is het adres van de portal https://portal.local.azurestack.external.

![Schermafbeelding van de gebruikersportal van Azure-Stack](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Het dashboard aanpassen

Het dashboard bevat een reeks standaardtegels. U kunt klikken op **bewerken dashboard** standaarddashboard wijzigen of klik op **nieuwe dashboard** toevoegen van aangepaste dashboards. U kunt eenvoudig tegels toevoegen aan het dashboard. U kunt bijvoorbeeld klikken op **nieuw**, met de rechtermuisknop op **Compute**, en klik vervolgens op **vastmaken aan dashboard**.

## <a name="create-subscription-and-browse-available-resources"></a>Abonnement maken en beschikbare resources bladeren
 
Als u nog een abonnement hebt, is het eerste wat dat u moet doen zich abonneert op een aanbieding. Daarna kunt u bladeren welke resources voor u beschikbaar zijn. Bladeren en resources maken, een van de volgende handelingen uit:

- Klik op de **Marketplace** tegel op het dashboard. 
- Op de **alle resources** tegel, klikt u op **resources maken**.
- Klik in het navigatiedeelvenster links op **nieuw**.

## <a name="learn-how-to-use-available-services"></a>Informatie over het gebruik van de beschikbare services

Als u de richtlijnen voor het gebruik van de beschikbare services moet, mogelijk zijn er verschillende opties voor u beschikbaar.

- Uw organisatie of serviceprovider kan bieden hun eigen documentatie. Dit geldt vooral als ze aangepaste services of toepassingen aanbieden.
- Apps van derden hebben hun eigen documentatie.
- Voor consistente Azure services, wordt aangeraden dat u eerst de documentatie van Azure-Stack doornemen. Klik op het pictogram Help voor toegang tot de Azure-Stack gebruikersdocumentatie, en klik vervolgens op **Help + ondersteuning**.
 
    ![Schermafbeelding van de optie Help en ondersteuning in de gebruikersinterface](media/azure-stack-use-portal/HelpAndSupport.png)

    We raden met name de volgende artikelen aan de slag te controleren:

    - [Belangrijke overwegingen: met behulp van services of het ontwikkelen van apps voor Azure-Stack](azure-stack-considerations.md)
    - In de sectie "Services gebruiken" van de documentatie ziet u elke Azure-consistent service die wordt weergegeven. Er is een onderwerp 'overwegingen' voor elke service, die de delta's tussen de service die wordt aangeboden op Azure, en dezelfde service aangeboden in Azure-Stack beschrijft. Zie voor een voorbeeld [VM overwegingen](azure-stack-vm-considerations.md). Mogelijk zijn er andere informatie in de sectie "Services gebruiken", die uniek is voor Azure-Stack. 
     
      U kunt de documentatie van Azure gebruiken als algemene referentie voor een service, maar u moet rekening houden met deze verschillen. Begrijpen dat de documentatie van koppelingen op de **Quick Start-zelfstudies** tegel wijs Azure-documentatie.

## <a name="get-support"></a>Ondersteuning krijgen

Als u aanvullende ondersteuning nodig hebt, neem voor assistentie contact op met uw organisatie of service provider. 

Als u de Azure-Stack Development Kit de [Azure Stack-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) is de enige methode van ondersteuning.

## <a name="next-steps"></a>Volgende stappen

[Belangrijke overwegingen: met behulp van services of het ontwikkelen van apps voor Azure-Stack](azure-stack-considerations.md)
