## <a name="scenario"></a>Scenario
Een virtuele machine met één NIC is gemaakt en verbonden met een virtueel netwerk. De virtuele machine vereist drie verschillende *persoonlijke* IP-adressen en de twee *openbare* IP-adressen. De IP-adressen zijn toegewezen aan de volgende IP-configuraties:

* **IPConfig-1:** wijst een *statische* privé IP-adres en een *statische* openbaar IP-adres.
* **IPConfig-2:** wijst een *statische* privé IP-adres en een *statische* openbaar IP-adres.
* **IPConfig-3:** wijst een *statische* privé IP-adres en er is geen openbare IP-adres.
  
    ![Meerdere IP-adressen](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

De IP-configuraties zijn gekoppeld aan de NIC als de NIC wordt gemaakt en de NIC is gekoppeld aan de virtuele machine wanneer de virtuele machine wordt gemaakt. De typen van IP-adressen gebruikt voor het scenario zijn ter illustratie. U kunt elk IP-adres en de toewijzing typen dat u nodig hebt.

> [!NOTE]
> Hoewel de stappen in dit artikel worden alle IP-configuraties aan één NIC toewijst, kunt u ook meerdere IP-configuraties toewijzen aan een NIC in een VM meerdere NIC's. Lees voor meer informatie over het maken van een virtuele machine met meerdere NIC's, de [een virtuele machine maken met meerdere NIC's](../articles/virtual-network/virtual-network-deploy-multinic-arm-ps.md) artikel.