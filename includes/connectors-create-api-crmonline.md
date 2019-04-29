---
ms.openlocfilehash: 53c683dacbb3b94e34bd8662743673c3a0490d94
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119958"
---
#### <a name="prerequisites"></a>Vereisten
* Een Azure-account; u kunt maken een [gratis account](https://azure.microsoft.com/free)
* Een [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) account 

Voordat u uw Dynamics-account in een logische app, verleent u de logische app verbinding maken met uw Online CRM-account. U kunt dit eenvoudig doen in uw logische app in Azure portal. 

Toestaan dat uw logische app verbinding maken met uw CRM Online-account met behulp van de volgende stappen uit:

1. Een logische app maken. Selecteer in de ontwerpfunctie voor logische Apps **beheerde API's van Microsoft weergeven** in de vervolgkeuzelijst en voer vervolgens 'dynamics' in het zoekvak in. Selecteer een van de triggers of acties:  
   ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Als u alle verbindingen met Dynamics nog niet eerder hebt gemaakt, wordt u gevraagd zich aanmelden met uw Dynamics-referenties:  
   ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Selecteer **aanmelden**, en voer uw gebruikersnaam en wachtwoord. Selecteer **aanmelden**. 
   
    Deze referenties worden gebruikt om uw logische app verbinding maken met en toegang tot de gegevens in uw Dynamics-account te autoriseren. 
4. U ziet dat de verbinding is gemaakt. Nu gaat u verder met de overige stappen in uw logische app:  
   ![](./media/connectors-create-api-crmonline/dynamics-properties.png)

