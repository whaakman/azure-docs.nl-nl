---
title: Inrichten van een diepe Learning gegevens wetenschappelijke virtuele Machine in Azure | Microsoft Docs
description: Configureren en een grondige Learning gegevens wetenschappelijke virtuele Machine maken op Azure voor analytics en machine learning.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: e1467c0f-497b-48f7-96a0-7f806a7bec0b
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: db1360fa54d82c50adc04194697d994925338296
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="provision-a-deep-learning-virtual-machine-on-azure"></a>Een virtuele Machine in Azure Learning Deep inrichten 

De grondige leren werken met virtuele Machine (DLVM) is een speciaal geconfigureerde variant van de populaire [gegevens wetenschappelijke virtuele Machine](http://aka.ms/dsvm) (DSVM) om het gemakkelijker om te gebruiken op basis van GPU VM-exemplaren voor het snel training grondige learning-modellen. Wordt ondersteund met Windows 2016 of de Ubuntu DSVM als basis. De DLVM deelt dezelfde core VM afbeeldingen en daarom de uitgebreide toolset die beschikbaar is op DSVM. 

De DLVM bevat verschillende hulpprogramma's voor AI inclusief GPU-edities van populaire grondige learning frameworks, zoals Microsoft cognitieve Toolkit, TensorFlow, Keras, Caffe2 Chainer; hulpprogramma's om toegang te verkrijgen en de installatiekopie van de vooraf verwerken, tekstgegevens, hulpprogramma's voor gegevens wetenschappelijke modelleren en ontwikkeling activiteiten zoals Microsoft R Server Developer Edition, Python, Anaconda, Jupyter-notebooks voor Python en R, IDE voor Python en R, SQL databases en tal van andere gegevenswetenschap en ML-hulpprogramma's. 

## <a name="create-your-deep-learning-virtual-machine"></a>De virtuele Machine Learning Deep maken
Hier volgen de stappen voor het maken van een exemplaar van de grondige Learning virtuele Machine: 

1. Navigeer naar de virtuele machine op aanbieding [Azure-portal](https://portal.azure.com/#create/microsoft-ads.dsvm-deep-learningtoolkit
).
2. Selecteer de **maken** knop onderaan in een wizard moeten worden genomen.![ Maak dlvm](./media/dlvm-provision-wizard.PNG)
3. De wizard gebruikt voor het maken van de DLVM vereist **invoer** voor elk van de **bestaat uit vier stappen** opgesomd aan de rechterkant van de volgende afbeelding. Hier volgen de invoer voor het configureren van deze stappen:
   
   1. **Basisinstellingen**
      
      1. **Naam**: naam van uw gegevens wetenschap-server die u maakt.
      2. **Type besturingssysteem voor de grondige Learning virtuele machine selecteren**: Kies Windows of Linux (voor Windows 2016 en Ubuntu Linux base DSVM)
      2. **Gebruikersnaam**: Admin-account aanmeldings-id.
      3. **Wachtwoord**: wachtwoord van de beheerder.
      4. **Abonnement**: als u meer dan één abonnement hebt, selecteert u een op waarop de machine is gemaakt en kosten in rekening gebracht.
      5. **Resourcegroep**: U kunt een nieuwe maken of gebruik een **leeg** bestaande Azure-resourcegroep in uw abonnement.
      6. **Locatie**: Selecteer het datacenter die het meest geschikt is. Het is doorgaans het datacenter die de meeste van uw gegevens of heeft de dichtstbijzijnde fysieke voor snelste toegang tot het netwerk. 
      
> [!NOTE]
> Aangezien DLVM is ingericht op Azure NC-serie GPU VM-instanties, moet u een van de locaties in Azure met GPU's. De locaties die GPU VM's zijn momenteel: **VS-Oost, Noord-centraal VS, Zuid-centraal VS, VS-West 2, Noord-Europa, West-Europa**. Raadpleeg voor de meest recente lijst de [Azure producten op de pagina regio](https://azure.microsoft.com/en-us/regions/services/) en zoekt u **NC-serie** onder **Compute**. 

   2. **Instellingen**: Selecteer een van de grootte van de NC-serie GPU-virtuele machine die voldoet aan uw functionele vereisten en kostenbeperkingen. Een opslagaccount maken voor uw virtuele machine.  ![dlvm-instellingen](./media/dlvm-provision-step-2.PNG)
   
   3. **Samenvatting**: Controleer of alle informatie die u hebt ingevoerd juist is.
   5. **Kopen**: klik op **kopen** starten van de inrichting. Een koppeling is met de voorwaarden van de transactie opgegeven. De virtuele machine heeft geen eventuele extra kosten afgezien van de berekening die voor de servergrootte van de die u hebt gekozen in de **grootte** stap. 

> [!NOTE]
> De inrichting duren ongeveer 10-20 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.
> 


## <a name="how-to-access-the-deep-learning-virtual-machine"></a>Over het openen van de grondige Learning virtuele Machine

### <a name="windows-edition"></a>Windows-editie
Nadat de virtuele machine is gemaakt, kunt u extern bureaublad in met behulp van de referenties van het Administrator-account die u hebt geconfigureerd in de voorgaande **basisbeginselen** sectie. 

### <a name="linux-edition"></a>Linux-editie

Nadat de virtuele machine is gemaakt, kunt u aanmelden bij het gebruik van SSH. Gebruik de accountreferenties op die u hebt gemaakt in de **basisbeginselen** sectie van stap 3 voor de interface van de shell tekst. Op de client aWindows, kunt u downloaden van een SSH clienthulpprogramma zoals [Putty](http://www.putty.org). Als u liever een grafische bureaublad (X Windows-systeem), kunt u X11 doorsturen op Putty gebruiken of de X2Go-client installeren.

> [!NOTE]
> De client X2Go beter presteert dan X11 doorsturen bij onze tests. U wordt aangeraden met behulp van de client X2Go voor een grafische interface voor het bureaublad.
> 
> 

#### <a name="installing-and-configuring-x2go-client"></a>Installeren en configureren van X2Go client
De Linux DLVM is al ingericht met X2Go server en gereed voor clientverbindingen accepteren. Voor verbinding met de grafische Linux VM-bureaublad, voer de volgende procedure op de client:

1. Download en installeer de client X2Go voor uw clientplatform van [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. De X2Go-client wordt uitgevoerd en selecteer **nieuwe sessie**. Er verschijnt een configuratievenster met meerdere tabbladen. Voer de volgende configuratieparameters:
   * **Tabblad sessie**:
     * **Host**: de hostnaam of IP-adres van uw virtuele Linux-gegevens van wetenschappelijke machine.
     * **Aanmelding**: de gebruikersnaam op de Linux-VM.
     * **SSH-poort**: 22, de standaardwaarde behoudt.
     * **Sessietype**: Wijzig de waarde in **XFCE**. De Linux DSVM ondersteunt momenteel alleen XFCE bureaublad.
   * **Tabblad Media**: U kunt uitschakelen geluid ondersteuning en de client als u hoeft niet te gebruiken.
   * **Gedeelde mappen**: als u mappen van uw clientcomputers die zijn gekoppeld aan de Linux-VM wilt, voegt u de client-machine-mappen die u wilt delen met de virtuele machine op dit tabblad.

Nadat u zich aanmeldt bij de virtuele machine met behulp van de SSH-client of XFCE grafische bureaublad via de client X2Go, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de virtuele machine. Op XFCE ziet u toepassingen menu, snelkoppelingen en bureaubladpictogrammen voor veel van de hulpprogramma's.

Als uw virtuele machine is gemaakt en ingericht, bent u klaar om te beginnen met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op deze. Er zijn start menu tegels en bureaubladpictogrammen voor veel van de hulpprogramma's. 
