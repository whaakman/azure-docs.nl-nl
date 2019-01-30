---
title: Een Deep Learning virtuele Machine voor Datatechnologie maken
titleSuffix: Azure
description: Configureren en een Deep Learning Data Science Virtual Machine in Azure maken voor analyse en machine learning.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 53ddea5426d2adfa7b0ddfcbda3375efae8d0859
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250803"
---
# <a name="provision-a-deep-learning-virtual-machine-on-azure"></a>Een Deep Learning virtuele Machine op Azure inrichten 

De Deep Learning Virtual Machine (DLVM) is een speciaal geconfigureerd variant van de populaire [Data Science Virtual Machine](https://aka.ms/dsvm) (DSVM) zodat u gemakkelijk te gebruiken op basis van GPU VM-exemplaren snel deep learning-modellen te trainen. Het wordt ondersteund met Windows 2016, of de Ubuntu-DSVM als basis. De DLVM deelt de dezelfde core VM-installatiekopieën en kan daarom alle de uitgebreide verzameling hulpprogramma's die beschikbaar is op DSVM. 

De DLVM bevat verschillende hulpprogramma's voor AI, met inbegrip van GPU-edities van populaire deep learning-frameworks, zoals Microsoft Cognitive Toolkit, TensorFlow, Keras, Caffe2, Chainer; hulpprogramma's aan te schaffen en voorverwerking afbeelding, tekstgegevens, hulpprogramma's voor data science modelleren en ontwikkeling, zoals Microsoft R Server Developer Edition, Anaconda Python, Jupyter-notebooks voor Python en R IDE's voor Python / R, SQL-databases en nog vele andere Data science en hulpprogramma's voor ML. 

## <a name="create-your-deep-learning-virtual-machine"></a>Uw Deep Learning virtuele Machine maken
Hier volgen de stappen voor het maken van een exemplaar van de Deep Learning Virtual Machine: 

1. Navigeer naar de virtuele machine weergeven op [Azure-portal](https://portal.azure.com/#create/microsoft-ads.dsvm-deep-learningtoolkit
).
2. Selecteer de **maken** onder in de wizard een moeten worden genomen.![ Maak dlvm](./media/dlvm-provision-wizard.PNG)
3. De wizard die wordt gebruikt voor het maken van de DLVM vereist **invoer** voor elk van de **vier stappen** geïnventariseerd aan de rechterkant van de volgende afbeelding. Hier volgen de invoer die nodig zijn voor elk van deze stappen configureren:
   
   1. **Basisinstellingen**
      
      1. **Naam**: De naam van de data science-server die u maakt.
      2. **Type besturingssysteem selecteren voor de Deep Learning VM**: Kies Windows of Linux (voor Windows 2016 en base Ubuntu Linux-DSVM)
      2. **Gebruikersnaam**: Admin-account aanmeldings-id.
      3. **Wachtwoord**: Wachtwoord voor beheerdersaccount.
      4. **Abonnement**: Als u meer dan één abonnement hebt, selecteert u een waar de machine zich moet worden gemaakt en worden kosten in rekening gebracht.
      5. **Resourcegroep**: U kunt een nieuwe groep maken of gebruik een **leeg** bestaande Azure-resourcegroep in uw abonnement.
      6. **Locatie**: Selecteer het datacenter die het meest geschikt is. Dit is meestal het datacenter dat de meeste van uw gegevens of zich het dichtst bij uw fysieke locatie voor de snelste toegang tot het netwerk. 
      
> [!NOTE]
> De DLVM biedt ondersteuning voor alle NC- en ND-serie GPU VM-exemplaren. Bij het inrichten van de DLVM, moet u een van de locaties in Azure met GPU's kiezen. Controleer de [Azure-producten per regio pagina](https://azure.microsoft.com/regions/services/) pagina voor de beschikbare locaties en zoek naar **NC-serie**, **NCv2-serie**, **uit de NCv3-serie** , of **ND-serie** onder **Compute**. 

   2. **Instellingen voor**: Selecteer een van de NC-serie (NC, NCv2, NCv3) of de ND-serie GPU VM-grootten die voldoet aan de vereisten van het functionele en kostenbeperkingen. Maak een opslagaccount voor uw virtuele machine.  ![dlvm-instellingen](./media/dlvm-provision-step-2.PNG)
   
   3. **Samenvatting**: Controleer of alle informatie die u hebt ingevoerd juist is.
   5. **Kopen**: Klik op **kopen** om te beginnen met het inrichten. Er wordt een koppeling gegeven naar de voorwaarden van de transactie. De virtuele machine heeft geen eventuele extra kosten buiten de rekenkracht voor de servergrootte van de die u hebt gekozen in de **grootte** stap. 

> [!NOTE]
> De inrichting duurt ongeveer 10-20 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.
> 


## <a name="how-to-access-the-deep-learning-virtual-machine"></a>Toegang tot de Deep Learning Virtual Machine

### <a name="windows-edition"></a>Windows-editie
Nadat de virtuele machine is gemaakt, kunt u extern bureaublad in met behulp van de accountreferenties van de beheerder die u hebt geconfigureerd in de voorgaande **basisbeginselen** sectie. 

### <a name="linux-edition"></a>Linux-editie

Nadat de virtuele machine is gemaakt, kunt u zich aanmelden toe met behulp van SSH. Gebruik de accountreferenties op die u hebt gemaakt in de **basisbeginselen** sectie van stap 3 voor de tekst shell-interface. Op de client aWindows, kunt u een SSH-clienthulpprogramma, zoals downloaden [Putty](http://www.putty.org). Als u liever een grafische desktop (X Windows-systeem), kunt u X11 doorsturen op Putty gebruiken of de X2Go-client installeren.

> [!NOTE]
> De client X2Go beter dan X11 doorsturen bij onze tests uitgevoerd. Het is raadzaam om met behulp van de client X2Go voor een grafische interface voor het bureaublad.
> 
> 

#### <a name="installing-and-configuring-x2go-client"></a>Installeren en configureren van X2Go client
De Linux DLVM is al ingericht met X2Go-server en gereed voor clientverbindingen accepteren. Voor verbinding met het bureaublad van de Linux-VM-grafische, voer de volgende procedure op de client:

1. Download en installeer de client X2Go voor uw clientplatform van [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. De X2Go-client wordt uitgevoerd, en selecteer **nieuwe sessie**. Er verschijnt een configuratievenster met meerdere tabbladen. Voer de volgende configuratieparameters:
   * **Tabblad sessie**:
     * **Host**: De hostnaam of IP-adres van uw Linux Data Science VM.
     * **Aanmelding**: De naam van de gebruiker op de Linux-VM.
     * **SSH poort**: Laat op de standaardwaarde 22.
     * **Sessietype**: Wijzig de waarde in **XFCE**. De Linux-DSVM ondersteunt momenteel alleen XFCE desktop.
   * **Tabblad Media**: U kunt geluid ondersteuning en afdrukken als u niet nodig hebt om ze te gebruiken-client uitschakelen.
   * **Gedeelde mappen**: Als u mappen van uw clientmachines die gekoppeld is op de Linux-VM wilt, voegt u de mappen op de client computer die u wilt delen met de virtuele machine op dit tabblad.

Nadat u zich aanmeldt bij de virtuele machine met behulp van de SSH-client of de XFCE grafische bureaublad via de client X2Go, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de virtuele machine. Op XFCE ziet u snelkoppelingen voor toepassingen in het menu en pictogrammen op het bureaublad voor veel van de hulpprogramma's.

Als uw virtuele machine is gemaakt en ingericht, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op deze. Zijn er tegels van het menu start en pictogrammen op het bureaublad voor veel van de hulpprogramma's. 
