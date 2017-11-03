## <a name="scenario"></a>Scenario
Dit document om beter te laten zien hoe udr's maken, gebruikt het onderstaande scenario.

![BESCHRIJVING VAN AFBEELDING](./media/virtual-network-create-udr-scenario-include/figure1.png)

In dit scenario maakt u een UDR voor de *Front end subnet* en een andere UDR voor de *subnet voor Back-end* , zoals hieronder wordt beschreven: 

* **UDR FrontEnd**. De front-end UDR worden toegepast op de *FrontEnd* subnet, en bevatten één route:    
  * **RouteToBackend**. Deze route voor al het verkeer wordt verzonden naar het back-end-subnet aan de **FW1** virtuele machine.
* **UDR-back-end**. De back-end UDR wordt toegepast op de *back-end* subnet, en bevatten één route:    
  * **RouteToFrontend**. Deze route voor al het verkeer wordt verzonden aan het front-end-subnet moet de **FW1** virtuele machine.

De combinatie van deze routes zorgt ervoor dat alle het verkeer naar uit één subnet worden doorgestuurd naar de **FW1** virtuele machine, die wordt gebruikt als een virtueel apparaat. U moet ook schakelt u doorsturen via IP voor die virtuele machine, zodat het verkeer dat is bestemd voor andere virtuele machines kan ontvangen.

