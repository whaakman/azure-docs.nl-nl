---
title: InSpec gebruiken voor het automatiseren van de naleving van uw Azure-infrastructuur
description: Informatie over het gebruik van InSpec om problemen te detecteren in uw Azure-implementaties
keywords: Azure, chef, devops, virtuele machines, overzicht, automatiseren, inspectie mogelijk
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: a5de2ca04a193f97a2a65a043f744abb8e0ea758
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359218"
---
# <a name="use-inspec-for-compliance-automation-of-your-azure-infrastructure"></a>InSpec gebruiken voor het automatiseren van de naleving van uw Azure-infrastructuur
[Inspectie mogelijk](https://www.chef.io/inspec/) Chef van open source-taal voor het beschrijven van regels voor beveiliging en naleving die kunnen worden gedeeld tussen software-engineers, bewerkingen en beveiliging engineers is. InSpec werkt door het vergelijken van de huidige status van uw infrastructuur met de gewenste status die u snelle in gemakkelijk te lezen en eenvoudig te schrijven inspectie mogelijk code. InSpec schendingen detecteert en worden resultaten weergegeven in de vorm van een rapport, maar staat u controle over herstel.

U kunt InSpec gebruiken voor het valideren van de status van resources en resourcegroepen binnen een abonnement, met inbegrip van virtuele machines, configuraties en instellingen van Azure Active Directory.

Dit artikel beschrijft de voordelen van het gebruik van InSpec beveiliging en naleving om gemakkelijker te maken op Azure.

## <a name="make-compliance-easy-to-understand-and-assess"></a>Controleer naleving eenvoudig te begrijpen en beoordelen
Naleving-documentatie die zijn geschreven in spreadsheets of Word-documenten laat vereisten met interpretatie geopend. Met InSpec, moet u de vereisten in de code is samengesteld, uitvoerbare, leesbare transformeren. Code vervangt gesprekken over wat moet worden beoordeeld en vervangen door concrete tests met de bedoeling duidelijk.

## <a name="detect-fleet-wide-issues-and-prioritize-their-remediation"></a>Hele vloot problemen detecteren en prioriteren van hun herstel
InSpec de zonder agent detecteren modus kunt u snel beoordelen - geschaalde - niveau van uw blootstelling. Ingebouwde metagegevens voor het scoren van impact/ernst kunt u bepalen welke gebieden kunnen concentreren op voor herstel. U kunt ook snel regels schrijven in reactie op nieuwe beveiligingsproblemen of regelgeving en ze onmiddellijk uitgerold.

## <a name="satisfy-audits"></a>Voldoen aan controles
U kunt reageren met InSpec, om te controleren van vragen op elk gewenst moment - niet alleen met een vooraf vastgestelde intervallen zoals per kwartaal of per jaar. Continu inspectie mogelijk tests uitvoert, u een controlecyclus van een weten de exacte naleving kunnen verbeteren en de geschiedenis, in plaats van wordt verbaasd door een revisor van de bevindingen.

## <a name="next-steps"></a>Volgende stappen

* Probeer inspectie mogelijk in de Azure Cloudshell [ ![Open Cloudshell](https://shell.azure.com/images/launchcloudshell.png "Open Cloudshell")](https://shell.azure.com)
