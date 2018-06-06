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
ms.date: 06/01/2018
ms.author: mabrigg
ms.reviewer: efemmano
ms.openlocfilehash: 7ca29ee359349f69c3d5ff21bd9db3f93358206a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724309"
---
# <a name="use-the-azure-stack-portal"></a>De Azure Stack-portal gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt de Stack van Azure-portal gebruiken om u te abonneren op openbare voorstellen en gebruiken van de services die deze aanbiedingen voorzien. Als u de globale Azure-portal hebt gebruikt, bent u al bekend bent met de werking van de site.

## <a name="access-the-portal"></a>Toegang tot de portal

Uw Azure-Stack-operator (een serviceprovider of een beheerder in uw organisatie), kunt u weet dat de juiste URL toegang tot de portal.

- Voor een geïntegreerde, de URL varieert op basis van de operator regio en externe domeinnaam en bevindt zich in de indeling https://portal.&lt; *regio*&gt;.&lt; *FQDN*&gt;.
- Als u de Azure-Stack Development Kit, het adres van de portal is https://portal.local.azurestack.external.

![Schermopname van de gebruikersportal van Azure-Stack](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Het dashboard aanpassen

Het dashboard bevat een aantal tegels. U kunt selecteren **dashboard bewerken** standaarddashboard wijzigen of selecteer **nieuwe dashboard** om een aangepast dashboard te maken. U kunt een dashboard eenvoudig aanpassen door het toevoegen of verwijderen van tegels. Selecteer bijvoorbeeld het volgende om een tegel Compute toe **nieuw**. Met de rechtermuisknop op **Compute**, en selecteer vervolgens **vastmaken aan dashboard**.

## <a name="create-subscription-and-browse-available-resources"></a>Abonnement maken en beschikbare resources bladeren

Als u nog een abonnement hebt, is het eerste wat dat u moet doen zich abonneert op een aanbieding. Daarna kunt u de beschikbare middelen. Bladeren en resources maken, een van de volgende methoden gebruiken:

- Selecteer de **Marketplace** tegel op het dashboard.
- Op de **alle resources** tegel, selecteer **resources maken**.
- Selecteer in het navigatiedeelvenster links **nieuw**.

## <a name="learn-how-to-use-available-services"></a>Informatie over het gebruik van de beschikbare services

Als u de richtlijnen voor het gebruik van de beschikbare services moet, mogelijk zijn er verschillende opties voor u beschikbaar.

- Hun eigen documentatie, meestal het geval is als zij aangepaste services of toepassingen bieden kan bepalen door uw organisatie of serviceprovider.
- Apps van derden hebben hun eigen documentatie.
- Voor consistente Azure services, wordt aangeraden dat u eerst de documentatie van Azure-Stack doornemen. Voor toegang tot de Azure-Stack gebruikersdocumentatie, selecteer het Help-pictogram en selecteer vervolgens **Help + ondersteuning**.

    ![Help en ondersteuning van de optie in de gebruikersinterface](media/azure-stack-use-portal/HelpAndSupport.png)

    We raden met name de volgende artikelen aan de slag te controleren:

    - [Belangrijke overwegingen: met behulp van services of het ontwikkelen van apps voor Azure-Stack](azure-stack-considerations.md)
    - In de **services gebruiken** sectie van de documentatie is een artikel overwegingen voor elke service. De pagina overwegingen beschrijft de verschillen tussen de aangeboden in Azure-service en dezelfde service aangeboden in Azure-Stack. Zie voor een voorbeeld [VM overwegingen](azure-stack-vm-considerations.md). Er kunnen ook andere informatie in de **services gebruiken** sectie die uniek is voor Azure-Stack.

      U kunt de documentatie van Azure gebruiken als algemene referentie voor een service, maar u moet rekening houden met deze verschillen. Begrijpen dat de documentatie van koppelingen op de **Quick Start-zelfstudies** tegel wijs Azure-documentatie.

## <a name="get-support"></a>Ondersteuning krijgen

Als u ondersteuning nodig hebt, moet u contact op met uw organisatie of service provider voor hulp.

Als u de Azure-Stack Development Kit de [Azure Stack-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) is de enige bron van ondersteuning.

## <a name="next-steps"></a>Volgende stappen

[Belangrijke overwegingen: met behulp van services of het ontwikkelen van apps voor Azure-Stack](azure-stack-considerations.md)
