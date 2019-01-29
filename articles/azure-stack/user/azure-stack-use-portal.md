---
title: Met behulp van de Azure Stack-portal | Microsoft Docs
description: Informatie over het openen en gebruiken van de gebruikersportal in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: mabrigg
ms.reviewer: efemmano
ms.openlocfilehash: 0bf725a20a7c030b0a835439c0f97f23b3cbef71
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55097867"
---
# <a name="use-the-azure-stack-portal"></a>De Azure Stack-portal gebruiken

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt de Azure Stack-portal gebruiken om u te abonneren op openbare aanbiedingen en gebruik van de services die deze aanbiedingen bieden. Als u de globale Azure-portal hebt gebruikt, bent u al bekend bent met de werking van de site.

## <a name="access-the-portal"></a>Toegang tot de portal

Uw Azure Stack-operators (een serviceprovider of een beheerder in uw organisatie), kunt u de juiste URL voor toegang tot de portal.

- Voor een geïntegreerd systeem, de URL is afhankelijk van op de regio en het externe domeinnaam van de operator en bevindt zich in de indeling https://portal.&lt; *regio*&gt;.&lt; *FQDN*&gt;.
- Als u de Azure Stack Development Kit, het adres van de portal is https://portal.local.azurestack.external.
- De standaardtijdzone voor alle Azure Stack-implementaties is ingesteld op Coordinated Universal Time (UTC). U kunt een tijdzone selecteren bij het installeren van Azure Stack, maar dan automatisch wordt overgeschakeld naar UTC als standaardwaarde tijdens de installatie.

## <a name="customize-the-dashboard"></a>Het dashboard aanpassen

Het dashboard bevat een set tegels. U kunt selecteren **dashboard bewerken** wijzigen van het standaarddashboard, of selecteer **nieuw dashboard** te maken van een aangepast dashboard. U kunt eenvoudig een dashboard aanpassen door het toevoegen of verwijderen van tegels. Selecteer bijvoorbeeld het volgende als u wilt toevoegen een Compute-tegel, **+ een resource maken**. Met de rechtermuisknop op **Compute**, en selecteer vervolgens **vastmaken aan dashboard**.

![Schermopname van de gebruikersportal van Azure Stack](media/azure-stack-use-portal/userportal.png)

## <a name="create-subscription-and-browse-available-resources"></a>Abonnement maken en beschikbare resources bladeren

Als u nog een abonnement hebt, is het eerste wat dat u moet doen zich abonneren op een aanbieding. Hierna kunt u de beschikbare resources zoeken. Als u wilt zoeken en resources maken, moet u een van de volgende methoden gebruiken:

- Selecteer de **Marketplace** tegel op het dashboard.
- Op de **alle resources** tegel, selecteer **resources maken**.
- Selecteer in het navigatiedeelvenster links **+ een resource maken**.

## <a name="learn-how-to-use-available-services"></a>Informatie over het gebruik van de beschikbare services

Als u hulp voor het gebruik van de beschikbare services nodig hebt, kunnen er verschillende opties beschikbaar voor u.

- Uw organisatie of serviceprovider kan hun eigen documentatie meestal het geval is als deze verbindingen bieden een aangepaste services of apps bepalen.
- Apps van derden hebben hun eigen documentatie.
- Voor Azure-consistente services, wordt aangeraden dat u eerst de Azure Stack-documentatie bekijken. Selecteer het pictogram Help voor toegang tot de gebruikersdocumentatie voor Azure Stack, en selecteer vervolgens **Help en ondersteuning**.

    ![Help en ondersteuning voor optie in de gebruikersinterface](media/azure-stack-use-portal/HelpAndSupport.png)

    In het bijzonder het is raadzaam dat u de volgende artikelen aan de slag bekijken:

    - [Belangrijke overwegingen: Met behulp van services of het bouwen van apps voor Azure Stack](azure-stack-considerations.md)
    - In de **aanwenden** sectie van de documentatie, er is een artikel overwegingen voor elke service. De overwegingen met betrekking tot pagina Beschrijving van de verschillen tussen de service die wordt aangeboden in Azure en dezelfde service aangeboden in Azure Stack. Zie voor een voorbeeld [overwegingen omtrent virtuele machines](azure-stack-vm-considerations.md). Mogelijk zijn er andere informatie in de **aanwenden** sectie die uniek is voor Azure Stack.

      U kunt de Azure-documentatie gebruiken als een algemene verwijzing voor een service, maar u moet rekening houden met deze verschillen. Begrijpt dat de documentatie van koppelingen op de **zelfstudies** tegel punt Azure-documentatie.

## <a name="get-support"></a>Ondersteuning krijgen

Als u ondersteuning nodig hebt, neem dan contact op met uw organisatie of de service-provider voor hulp.

Als u de Azure Stack Development Kit, de [Azure Stack-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) is de enige bron van de ondersteuning.

## <a name="next-steps"></a>Volgende stappen

[Belangrijke overwegingen: Met behulp van services of het bouwen van apps voor Azure Stack](azure-stack-considerations.md)
