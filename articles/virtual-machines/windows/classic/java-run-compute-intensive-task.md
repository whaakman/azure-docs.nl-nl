---
title: Rekenintensieve Java-toepassing op een virtuele machine | Microsoft Docs
description: Informatie over het maken van een virtuele machine van Azure die wordt uitgevoerd een rekenintensieve Java-toepassing die kan worden gecontroleerd door een andere Java-toepassing.
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: ae6f2737-94c7-4569-9913-d871450c2827
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: a71efa7ff052c7c69de2b9aba3c1ed9328538e3f
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2017
---
# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Een rekenintensieve taak uitvoeren in Java op een virtuele machine
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Met Azure, kunt u een virtuele machine voor het afhandelen van rekenintensieve taken. Bijvoorbeeld: een virtuele machine verwerkt taken en resultaten ervoor zorgen dat clientcomputers of mobiele toepassingen. Na het lezen van dit artikel hebt u een goed begrip van het maken van een virtuele machine die wordt uitgevoerd een rekenintensieve Java-toepassing die kan worden gecontroleerd door een andere Java-toepassing.

Deze zelfstudie wordt ervan uitgegaan dat u weet van het maken van Java-consoletoepassingen, bibliotheken kunt importeren naar uw Java-toepassing en een Java-archief (JAR) kunt genereren. Er is geen kennis van Microsoft Azure wordt verondersteld.

U leert het volgende:

* Het maken van een virtuele machine met een Java Development Kit (JDK) al is geïnstalleerd.
* Het op afstand aan te melden bij uw virtuele machine.
* Het maken van een service bus-naamruimte.
* Het maken van een Java-toepassing die een taak rekenintensieve uitvoert.
* Het maken van een Java-toepassing de voortgang van de taak rekenintensieve bewaakt.
* Het uitvoeren van Java-toepassingen.
* Klik hier voor meer informatie over het stoppen van de Java-toepassingen.

Deze zelfstudie gebruikt het probleem Traveling verkoper voor de taak rekenintensieve. Hier volgt een voorbeeld van het uitvoeren van de taak rekenintensieve Java-toepassing.

![Traveling verkoper probleem solver][solver_output]

Hier volgt een voorbeeld van de bewaking van de taak rekenintensieve Java-toepassing.

![Traveling verkoper probleem client][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Een virtuele machine maken
1. Meld u aan bij de [klassieke Azure Portal](https://manage.windowsazure.com).
2. Klik op **nieuw**, klikt u op **Compute**, klikt u op **virtuele machine**, en klik vervolgens op **uit galerie**.
3. In de **virtuele machine-installatiekopie selecteert** dialoogvenster, **JDK 7 Windows Server 2012**.
   Houd er rekening mee dat **JDK 6 Windows Server 2012** beschikbaar is voor het geval u oudere toepassingen die nog niet klaar om te worden uitgevoerd in JDK 7 hebt.
4. Klik op **Volgende**.
5. In de **Virtuele-machineconfiguratie** in het dialoogvenster:
   1. Geef een naam voor de virtuele machine.
   2. Geef de grootte moet worden gebruikt voor de virtuele machine.
   3. Voer een naam voor de beheerder in de **gebruikersnaam** veld. Deze naam en het wachtwoord u naast voert onthouden, gebruikt u deze wanneer u extern aanmelden bij de virtuele machine.
   4. Voer een wachtwoord in de **nieuw wachtwoord** veld en opnieuw invoeren in de **bevestigen** veld. Dit is het wachtwoord van de Administrator-account.
   5. Klik op **Volgende**.
6. In de volgende **Virtuele-machineconfiguratie** in het dialoogvenster:
   1. Voor **Cloudservice**, gebruikt u de standaard **Maak een nieuwe cloudservice**.
   2. De waarde voor **Cloud service DNS-naam** uniek zijn binnen cloudapp.net. Indien nodig, wijzigt u deze waarde zodat deze Azure geeft dat deze uniek is.
   3. Geef een regio, een affiniteitsgroep of een virtueel netwerk. Geef voor deze zelfstudie een regio zoals **VS-West**.
   4. Voor **Opslagaccount**, selecteer **een automatisch gegenereerde opslagaccount gebruiken**.
   5. Voor **Beschikbaarheidsset**, selecteer **(geen)**.
   6. Klik op **Volgende**.
7. In de uiteindelijke **Virtuele-machineconfiguratie** in het dialoogvenster:
   1. Accepteer de standaardinstellingen van het eindpunt.
   2. Klik op **Voltooien**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Op afstand aan te melden bij uw virtuele machine
1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Klik op **virtuele machines**.
3. Klik op de naam van de virtuele machine die u aanmelden wilt bij.
4. Klik op **Verbinden**.
5. Reageren op de vragen naar behoefte verbinding maken met de virtuele machine. Wanneer de beheerdersnaam en het wachtwoord wordt gevraagd, gebruik de waarden die u hebt opgegeven toen u de virtuele machine hebt gemaakt.

Opmerking dat de Azure Service Bus-functionaliteit de Baltimore CyberTrust-basiscertificaat vereist moet worden geïnstalleerd als onderdeel van uw Java Runtime Environment **cacerts** opslaan. Dit certificaat wordt automatisch opgenomen in de Java Runtime Environment (JRE) die wordt gebruikt door deze zelfstudie. Als u nog geen dit certificaat in uw JRE **cacerts** opslaat, Zie [een certificaat toevoegen aan de Java CA-certificaatarchief] [ add_ca_cert] voor informatie over het toevoegen van het (evenals informatie over het weergeven van de certificaten in uw archief cacerts).

## <a name="how-to-create-a-service-bus-namespace"></a>Het maken van een service bus-naamruimte
Als u Service Bus-wachtrijen in Azure wilt gebruiken, moet u eerst een servicenaamruimte maken. Een Servicenaamruimte biedt een scoping container voor het adresseren van Service Bus-resources in uw toepassing.

Een servicenaamruimte maken:

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Klik in de linkerbenedenhoek navigatievenster van de klassieke Azure portal op **Service Bus, Access Control & opslaan in cache**.
3. Klik in het deelvenster van de linkerbovenhoek van de klassieke Azure portal, de **Service Bus** knooppunt en klik vervolgens op de **nieuw** knop.  
   ![Schermafbeelding van de service Bus-knooppunt][svc_bus_node]
4. In de **maken van een nieuwe Service-Namespace** dialoogvenster Voer een **Namespace**, en om er zeker van te zijn dat deze uniek is, klikt u op de **beschikbaarheid controleren** knop.  
   ![Maak een nieuwe Namespace-schermafbeelding][create_namespace]
5. Nadat u ervoor zorgt dat de naam van de naamruimte beschikbaar is, kiest u het land of regio waarin uw naamruimte moet worden gehost, en klik vervolgens op de **Namespace maken** knop.  
   
   De naamruimte die u hebt gemaakt, wordt vervolgens weergegeven in de klassieke Azure portal en duurt even om te activeren. Wacht totdat de status **Active** voordat u doorgaat met de volgende stap.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>De standaard Standaardbeheerreferenties voor de naamruimte ophalen
U moet beheerreferenties voor de naamruimte beheerbewerkingen, zoals het maken van een wachtrij op de nieuwe naamruimte wilt uitvoeren.

1. Klik in het navigatiedeelvenster links op de **Service Bus** knooppunt om de lijst met beschikbare naamruimten weer te geven.
   ![Schermafbeelding met beschikbare naamruimten][avail_namespaces]
2. Selecteer de naamruimte die u zojuist hebt gemaakt in de lijst weergegeven.
   ![Lijst met Namespace-schermafbeelding][namespace_list]
3. De rechter **eigenschappen** deelvenster bevat de eigenschappen voor de nieuwe naamruimte.
   ![Schermopname van eigenschappen deelvenster][properties_pane]
4. De **standaard sleutel** wordt verborgen. Klik op de **weergave** knop om de beveiligingsreferenties weer te geven.
   ![Standaard-sleutel-schermafbeelding][default_key]
5. Noteer de **standaard verlener** en de **standaard sleutel** zoals u kunt deze gegevens hieronder bewerkingen uitvoeren met de naamruimte.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Het maken van een Java-toepassing die een taak rekenintensieve uitvoert
1. Op uw ontwikkelcomputer (die hoeft niet te worden van de virtuele machine die u hebt gemaakt), downloaden de [Azure SDK voor Java](https://azure.microsoft.com/develop/java/).
2. Maak een Java-consoletoepassing met behulp van de voorbeeldcode aan het einde van deze sectie. In deze zelfstudie gebruiken we **TSPSolver.java** als de Java-bestandsnaam. Wijzigt de **uw\_service\_bus\_naamruimte**, **uw\_service\_bus\_eigenaar**, en **uw\_service\_bus\_sleutel** tijdelijke aanduidingen voor het gebruik van uw servicebus **naamruimte**, **standaard verlener** en **standaard sleutel** waarden, respectievelijk.
3. Na de codering, de toepassing naar een uitvoerbare Java-archief (JAR) exporteren en de vereiste bibliotheken van het pakket in de gegenereerde JAR. In deze zelfstudie gebruiken we **TSPSolver.jar** als de gegenereerde naam van de JAR.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Het maken van een Java-toepassing de voortgang van de taak rekenintensieve bewaakt
1. Maak een Java-consoletoepassing met behulp van de voorbeeldcode aan het einde van deze sectie op uw ontwikkelcomputer. In deze zelfstudie gebruiken we **TSPClient.java** als de Java-bestandsnaam. Zoals eerder besproken, wijzig de **uw\_service\_bus\_naamruimte**, **uw\_service\_bus\_eigenaar**, en **uw\_service\_bus\_sleutel** tijdelijke aanduidingen voor het gebruik van uw servicebus **naamruimte**, **standaard verlener** en **standaard sleutel** waarden, respectievelijk.
2. De toepassing naar een uitvoerbare JAR exporteren en de vereiste bibliotheken van het pakket in de gegenereerde JAR. In deze zelfstudie gebruiken we **TSPClient.jar** als de gegenereerde naam van de JAR.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Het uitvoeren van Java-toepassingen
Voer de toepassing rekenintensieve eerst naar de wachtrij op en klik vervolgens maken om het probleem op reis Saleseman die de huidige beste route wordt toegevoegd aan de service bus-wachtrij te verhelpen. Terwijl de rekenintensieve toepassing wordt uitgevoerd (of later), de client als u wilt weergeven van resultaten van de service bus-wachtrij wordt uitgevoerd.

### <a name="to-run-the-compute-intensive-application"></a>De rekenintensieve toepassing uit te voeren
1. Meld u bij uw virtuele machine.
2. Maak een map waar u uw toepassing wordt uitgevoerd. Bijvoorbeeld: **c:\TSP**.
3. Kopiëren **TSPSolver.jar** naar **c:\TSP**,
4. Maak een bestand met de naam **c:\TSP\cities.txt** met de volgende inhoud.
   
        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33
5. Wijzig de mappen in c:\TSP bij een opdrachtprompt.
6. Zorg ervoor dat de JRE bin-map in de omgevingsvariabele PATH.
7. U moet de service bus-wachtrij voordat u het aantal permutaties TSP-solver maken. Voer de volgende opdracht voor het maken van de service bus-wachtrij.
   
        java -jar TSPSolver.jar createqueue
8. Nu dat de wachtrij is gemaakt, kunt u het aantal permutaties TSP-solver kunt uitvoeren. Voer bijvoorbeeld de volgende opdracht om uit te voeren de solver voor 8 steden.
   
        java -jar TSPSolver.jar 8
   
   Als u niet een getal opgeeft, wordt deze uitgevoerd voor 10 steden. Als de solver vindt huidige kortste routes, deze wordt toe te voegen aan de wachtrij.

> [!NOTE]
> Hoe hoger het getal dat u, hoe langer opgeeft de solver wordt uitgevoerd. Bijvoorbeeld, actief 14 steden kunnen enkele minuten duren, en wordt uitgevoerd voor 15 steden kan enkele uren duren. 16 meerdere plaatsen worden verhoogd, kan dit leiden tot dagen van de runtime (uiteindelijk weken, maanden en jaren). Dit komt door de snelle toename van het aantal permutaties geëvalueerd door de solver als het aantal steden toeneemt.
> 
> 

### <a name="how-to-run-the-monitoring-client-application"></a>Het uitvoeren van de bewaking clienttoepassing
1. Meld u bij de computer waar u de clienttoepassing wordt uitgevoerd. Dit hoeft niet dezelfde computer uitgevoerd worden de **TSPSolver** toepassing, maar het kan zijn.
2. Maak een map waar u uw toepassing wordt uitgevoerd. Bijvoorbeeld: **c:\TSP**.
3. Kopiëren **TSPClient.jar** naar **c:\TSP**,
4. Zorg ervoor dat de JRE bin-map in de omgevingsvariabele PATH.
5. Wijzig de mappen in c:\TSP bij een opdrachtprompt.
6. Voer de volgende opdracht.
   
        java -jar TSPClient.jar
   
    Geef eventueel het aantal minuten in de slaapstand bij het controleren van de wachtrij door door te geven in een opdrachtregelargument. De standaardperiode slaapstand voor het controleren van de wachtrij is 3 minuten, die wordt gebruikt als er geen opdrachtregelargument wordt doorgegeven aan **TSPClient**. Als u met een andere waarde voor de slaap-interval wilt, bijvoorbeeld één minuut, voer de volgende opdracht.
   
        java -jar TSPClient.jar 1
   
    De client wordt uitgevoerd totdat deze een wachtrijbericht van 'Voltooi' ziet. Houd er rekening mee dat als u meerdere exemplaren van de solver uitvoert zonder dat de client worden uitgevoerd, moet u mogelijk meerdere keren volledig leegmaken van de wachtrij voor de client uitvoeren. U kunt ook de wachtrij verwijderen en opnieuw maken. De wachtrij wilt verwijderen, voert u de volgende **TSPSolver** (geen **TSPClient**) opdracht.
   
        java -jar TSPSolver.jar deletequeue
   
    De solver wordt uitgevoerd totdat het klaar is met het onderzoeken van alle routes.

## <a name="how-to-stop-the-java-applications"></a>Het beëindigen van de Java-toepassingen
Voor zowel het solver clienttoepassingen, drukt u op **Ctrl + C** om af te sluiten als u wilt beëindigen voordat de normale voltooiing.

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
