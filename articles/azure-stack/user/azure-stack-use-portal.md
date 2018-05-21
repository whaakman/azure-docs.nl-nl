---
title: Met de Stack van Azure portal | Microsoft Docs
description: Informatie over het openen en gebruiken van de gebruikersportal in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: mabrigg
ms.reviewer: efemmano
ms.openlocfilehash: 2ebafc4249f8455bdbe45a07a5bf88aa8984d67a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="using-the-azure-stack-portal"></a>Met behulp van de Stack van Azure-portal

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt de Stack van Azure-portal gebruiken om u te abonneren op openbare voorstellen en gebruiken van de services die deze aanbiedingen voorzien. Als u de globale Azure-portal hebt gebruikt, bent u al bekend bent met de werking van de site.

## <a name="access-the-portal"></a>Toegang tot de portal

Uw Azure-Stack-operator (een serviceprovider of een beheerder in uw organisatie), kunt u weet dat de juiste URL toegang tot de portal.

- Voor een geïntegreerde, de URL varieert op basis van de operator regio en externe domeinnaam en bevindt zich in de indeling https://portal.&lt; *regio*&gt;.&lt; *FQDN*&gt;.
- Als u de Azure-Stack Development Kit, het adres van de portal is https://portal.local.azurestack.external.

![Schermafbeelding van de gebruikersportal van Azure-Stack](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Het dashboard aanpassen

Het dashboard bevat een aantal tegels. Klikt u op **bewerken dashboard** de standaarddashboard wijzigen of klik op **nieuw dashboard** voor het maken van een aangepast dashboard. U kunt een dashboard eenvoudig aanpassen door het toevoegen of verwijderen van tegels. Bijvoorbeeld een tegel Compute toevoegen, klikt u op **nieuw**. Met de rechtermuisknop op **berekenen**, en klik vervolgens op **Pin dashboard**.

## <a name="create-subscription-and-browse-available-resources"></a>Abonnement maken en beschikbare resources bladeren
 
Als u nog een abonnement hebt, is het eerste wat dat u moet doen zich abonneert op een aanbieding. Daarna kunt u de beschikbare middelen. Bladeren en resources maken, een van de volgende methoden gebruiken:

- Klik op de **Marketplace** tegel op het dashboard.
- Op de **alle resources** tegel, klikt u op **resources maken**.
- Klik in het navigatiedeelvenster links op **nieuw**.

## <a name="learn-how-to-use-available-services"></a>Informatie over het gebruik van de beschikbare services

Als u de richtlijnen voor het gebruik van de beschikbare services moet, mogelijk zijn er verschillende opties voor u beschikbaar.

- Hun eigen documentatie, meestal het geval is als zij aangepaste services of toepassingen bieden kan bepalen door uw organisatie of serviceprovider.
- Apps van derden hebben hun eigen documentatie.
- Voor consistente Azure services, wordt aangeraden dat u eerst de documentatie van Azure-Stack doornemen. Klik op het pictogram Help voor toegang tot de Azure-Stack gebruikersdocumentatie, en klik vervolgens op **Help + ondersteuning**.
 
    ![Schermafbeelding van de optie Help en ondersteuning in de gebruikersinterface](media/azure-stack-use-portal/HelpAndSupport.png)

    We raden met name de volgende artikelen aan de slag te controleren:

    - [Belangrijke overwegingen: met behulp van services of het ontwikkelen van apps voor Azure-Stack](azure-stack-considerations.md)
    - In de **services gebruiken** sectie van de documentatie is een artikel overwegingen voor elke service. De pagina overwegingen beschrijft de verschillen tussen de aangeboden in Azure-service en dezelfde service aangeboden in Azure-Stack. Zie voor een voorbeeld [VM overwegingen](azure-stack-vm-considerations.md). Er kunnen ook andere informatie in de **services gebruiken** sectie die uniek is voor Azure-Stack.
     
      U kunt de documentatie van Azure gebruiken als algemene referentie voor een service, maar u moet rekening houden met deze verschillen. Begrijpen dat de documentatie van koppelingen op de **Quick Start-zelfstudies** tegel wijs Azure-documentatie.

## <a name="get-support"></a>Ondersteuning krijgen

Als u ondersteuning nodig hebt, moet u contact op met uw organisatie of service provider voor hulp.

Als u de Azure-Stack Development Kit de [Azure Stack-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) is de enige bron van ondersteuning.

## <a name="next-steps"></a>Volgende stappen

[Belangrijke overwegingen: met behulp van services of het ontwikkelen van apps voor Azure-Stack](azure-stack-considerations.md)
