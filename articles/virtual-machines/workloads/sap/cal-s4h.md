---
title: SAP S/4HANA of BW/4HANA op een virtuele machine van Azure implementeren | Microsoft Docs
description: SAP S/4HANA of BW/4HANA op een Azure-VM implementeren
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: c59fcf43cb4767f1d95d769dfce4d5c8755e45ee
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57990480"
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a>SAP S/4HANA of BW/4HANA op Azure implementeren
In dit artikel wordt beschreven hoe u S/4HANA op Azure implementeren met behulp van de SAP Cloud Appliance Library (SAP CAL) 3.0. Volg dezelfde stappen voor het implementeren van andere oplossingen op basis van SAP HANA, zoals BW/4HANA.

> [!NOTE]
> Voor meer informatie over de SAP CAL, gaat u naar de [SAP Cloud Appliance Library](https://cal.sap.com/) website. SAP heeft ook een blog over de [SAP Cloud Appliance Library 3.0](https://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).
> 
> [!NOTE]
> Vanaf 29 mei 2017, kunt u het Azure Resource Manager-implementatiemodel naast het minder voorkeur klassieke implementatiemodel gebruiken voor het implementeren van de SAP CAL. U wordt aangeraden dat u de nieuwe Resource Manager-implementatiemodel gebruiken en geen rekening gehouden met het klassieke implementatiemodel.

## <a name="step-by-step-process-to-deploy-the-solution"></a>Stapsgewijze procedure om de oplossing te implementeren

De volgende reeks schermafdrukken ziet u hoe S/4HANA op Azure implementeren met behulp van de SAP CAL. Het proces werkt voor andere oplossingen, zoals BW/4HANA op dezelfde manier.

De **oplossingen** pagina ziet u enkele van de oplossingen op basis van een SAP CAL HANA op Azure. **SAP S/4HANA 1610 FPS01, Fully-Activated toestel** is in de middelste rij:

![SAP CAL-oplossingen](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-the-sap-cal"></a>Een account maken in de SAP CAL
1. Voor de eerste keer aanmelden bij de SAP CAL, gebruikt u uw SAP-S-gebruiker of een andere gebruiker geregistreerd met SAP. Definieer vervolgens een SAP CAL-account dat wordt gebruikt door de SAP CAL voor apparaten in Azure implementeren. In het definitie van de account moet u naar:

    a. Selecteer het implementatiemodel in Azure (Resource Manager of klassiek).

    b. Voer uw Azure-abonnement. Een SAP CAL-account kan worden toegewezen aan slechts één abonnement. Als u meer dan één abonnement nodig hebt, moet u een andere SAP CAL-account maken.

    c. Hiermee geeft u de SAP CAL-machtiging voor het implementeren in uw Azure-abonnement.

   > [!NOTE]
   >  De volgende stappen laten zien hoe een SAP CAL-account voor Resource Manager-implementaties maken. Als u al een SAP CAL-account dat is gekoppeld aan het klassieke implementatiemodel, hebt u *moet* om deze stappen om een nieuwe SAP CAL-account te maken. De nieuwe SAP CAL-account moet implementeren in het Resource Manager-model.

1. Maak een nieuwe SAP CAL-account. De **Accounts** pagina ziet u drie opties voor Azure: 

    a. **Microsoft Azure (klassiek)** is van het klassieke implementatiemodel en niet langer voorkeur.

    b. **Microsoft Azure** is de nieuwe Resource Manager-implementatiemodel.

    c. **Windows Azure uitgevoerd door 21Vianet** is een optie in China die gebruikmaakt van het klassieke implementatiemodel.

    Als u wilt implementeren in het Resource Manager-model, selecteer **Microsoft Azure**.

    ![Details van SAP CAL-Account](./media/cal-s4h/s4h-pic-2a.png)

1. Voer de Azure **abonnements-ID** die kan worden gevonden op de Azure-portal.

   ![SAP CAL-Accounts](./media/cal-s4h/s4h-pic3c.png)

1. Als u wilt toestaan dat de SAP CAL om te implementeren in de Azure-abonnement u hebt gedefinieerd, klikt u op **autoriseren**. De volgende pagina wordt weergegeven op het browsertabblad:

   ![Internet Explorer cloud services-aanmelding](./media/cal-s4h/s4h-pic4c.png)

1. Als meer dan één gebruiker wordt weergegeven, kiest u de Microsoft-account dat is gekoppeld, worden de CO-beheerder van het Azure-abonnement dat u hebt geselecteerd. De volgende pagina wordt weergegeven op het browsertabblad:

   ![Bevestiging van Internet Explorer cloud services](./media/cal-s4h/s4h-pic5a.png)

1. Klik op **accepteren**. Als de autorisatie geslaagd is, wordt de definitie van de SAP CAL-account opnieuw wordt weergegeven. Na korte tijd bevestigt een bericht dat het autorisatieproces geslaagd is.

1. Als u wilt de zojuist gemaakte SAP CAL-account aan uw gebruikers toewijst, Voer uw **gebruikers-ID** in het tekstvak aan de rechterkant en klikt u op **toevoegen**.

   ![Account voor koppeling met gebruiker](./media/cal-s4h/s4h-pic8a.png)

1. Als u wilt uw account te koppelen aan de gebruiker die u gebruiken om aan te melden bij de SAP CAL, klikt u op **revisie**. 
 
1. Klik op de koppeling tussen uw gebruikersnaam en het zojuist gemaakte SAP CAL-account om **maken**.

   ![SAP CAL-account koppelingen van gebruikers aan](./media/cal-s4h/s4h-pic9b.png)

U een SAP CAL-account dat kan worden gemaakt:

- Het Resource Manager-implementatiemodel gebruiken.
- Implementeer SAP-systemen in uw Azure-abonnement.

U kunt nu starten voor het implementeren van S/4HANA in uw gebruikersabonnement in Azure.

> [!NOTE]
> Voordat u doorgaat, moet u bepalen of u hebt de Azure-vCPU-quota voor Azure uit de H-serie VM's. Op dit moment gebruikt de SAP CAL H-serie VM's van Azure enkele van de SAP HANA-oplossingen implementeren. Uw Azure-abonnement mogelijk geen eventuele H-serie vCPU-quota voor de H-serie. Als dit het geval is, moet u mogelijk contact opnemen met ondersteuning van Azure om op te halen van een quotum van ten minste 16 vcpu's voor uit de H-serie.
> 
> [!NOTE]
> Wanneer u een oplossing op Azure in de SAP CAL implementeert, kunt u wellicht dat u slechts één Azure-regio kunt. Als u wilt implementeren in Azure-regio's dan de regio die wordt voorgesteld door de SAP CAL, moet u een abonnement te kopen CAL van SAP. Ook is het mogelijk een bericht openen met SAP hebt ingeschakeld om te leveren in de Azure-regio's dan degene die in eerste instantie aanbevolen CAL-account.

### <a name="deploy-a-solution"></a>Een oplossing implementeren

Laten we het implementeren van een oplossing uit de **oplossingen** pagina van de SAP CAL. De SAP CAL heeft twee reeksen te implementeren:

- Een eenvoudige reeks die gebruikmaakt van één pagina voor het definiëren van het systeem om te worden geïmplementeerd
- Een geavanceerde reeks waarmee u kunt bepaalde opties voor VM-grootten 

Het eenvoudige pad naar implementatie Hier ziet.

1. Op de **accountdetails** pagina, moet u naar:

    a. Selecteer een SAP CAL-account. (Gebruik een account dat is gekoppeld om te implementeren met het implementatiemodel van Resource Manager.)

    b. Geef het exemplaar van een **naam**.

    c. Selecteer een Azure **regio**. De SAP CAL stelt een regio. Als u een andere Azure-regio en u een SAP CAL-abonnement hebt, moet u een abonnement CAL met SAP bestellen.

    d. Voer een master **wachtwoord** voor de oplossing van acht of negen tekens. Het wachtwoord wordt gebruikt voor de beheerders van de verschillende onderdelen.

   ![SAP CAL Basic modus: Instantie maken](./media/cal-s4h/s4h-pic10a.png)

1. Klik op **maken**, en in het vak van het bericht dat wordt weergegeven, klikt u op **OK**.

   ![SAP CAL ondersteunde VM-grootten](./media/cal-s4h/s4h-pic10b.png)

1. In de **persoonlijke sleutel** in het dialoogvenster, klikt u op **Store** voor het opslaan van de persoonlijke sleutel in de SAP CAL. Voor het gebruik van beveiliging met een wachtwoord voor de persoonlijke sleutel, klikt u op **downloaden**. 

   ![SAP CAL persoonlijke sleutel](./media/cal-s4h/s4h-pic10c.png)

1. Lees de SAP CAL **waarschuwing** bericht en klikt u op **OK**.

   ![Waarschuwing voor SAP CAL](./media/cal-s4h/s4h-pic10d.png)

    Nu de implementatie plaatsvindt. Na enige tijd, afhankelijk van de grootte en complexiteit van de oplossing (de SAP CAL biedt een schatting te maken), wordt de status weergegeven als actief en klaar voor gebruik.

1. Als u de virtuele machines die zijn verzameld met de bijbehorende bronnen in één resourcegroep zoekt, gaat u naar de Azure-portal: 

   ![SAP CAL-objecten die zijn geïmplementeerd in de nieuwe portal](./media/cal-s4h/sapcaldeplyment_portalview.png)

1. Op de SAP CAL-portal, de status wordt weergegeven als **Active**. Voor verbinding met de oplossing, klikt u op **Connect**. Verschillende opties om te verbinden met de verschillende onderdelen worden in deze oplossing geïmplementeerd.

   ![Instanties van SAP CAL](./media/cal-s4h/active_solution.png)

1. Voordat u een van de opties voor verbinding met de geïmplementeerde systemen gebruiken kunt, klikt u op **Getting Started Guide**. 

   ![Verbinding maken met het exemplaar](./media/cal-s4h/connect_to_solution.png)

    De documentatie van de naam van de gebruikers voor elk van de methoden connectiviteit. De wachtwoorden voor gebruikers die zijn ingesteld op de master wachtwoord dat u hebt gedefinieerd aan het begin van het implementatieproces. Andere meer functionaliteit gebruikers worden weergegeven in de documentatie met hun wachtwoorden, die u gebruiken kunt voor aanmelding bij het geïmplementeerde systeem. 

    Bijvoorbeeld, als u de SAP-gebruikersinterface die vooraf geïnstalleerd op de extern bureaublad van Windows-machine, het s/4-systeem als volgt uitzien:

   ![SM50 in de vooraf geïnstalleerde SAP-gebruikersinterface](./media/cal-s4h/gui_sm50.png)

    Of als u de DBACockpit, het exemplaar als volgt uitzien:

   ![SM50 in de gebruikersinterface van de SAP DBACockpit](./media/cal-s4h/dbacockpit.png)

Binnen een paar uur is een gezonde SAP s/4-apparaat geïmplementeerd in Azure.

Als u een SAP CAL-abonnement hebt gekocht, biedt implementaties via de SAP CAL volledige ondersteuning voor SAP op Azure. De wachtrij is BC-VCM-CAL.







