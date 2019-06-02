# ESX_CommunityService

Uma forma alternativa de punição e correção social para a cadeia.
Com este script, agora você pode enviar criminosos na praça central para fornecer serviços comunitários através de limpeza e jardinagem.
Tente escapar e você terá seu serviço estendido!


### Requisitos
* ESX
* ESX skinchanger
  * [skinchanger](https://github.com/ESX-Brasil/skinchanger)

## Download e Instalação

### Usando o Git
```
cd resources
git clone https://github.com/ESX-Brasil/esx_communityservice [esx]/esx_communityservice
```

### Manually
- Download https://github.com/ESX-Brasil/esx_communityservice/archive/master.zip
- Coloque-o no diretório `[esx]`


## Instalação
- Importe `esx_communityservice.sql` em seu banco de dados
- Adicione isto em seu server.cfg:

```
start esx_communityservice
```
## Como aplicar o serviço comunitário.

- Use o gatilho do servidor `esx_communityservice: sendToCommunityService (target, service_count)`.
- Use o comando `/comserv player_id service_count` (somente admins).
- Use o `/endcomserv player_id` para terminar o serviço da comunidade de um jogador (somente admins).



# Como adicionar ao menu policial.

Exemplo em `esx_policejob: client/main.lua`:

```lua
-- ADIÇÃO [1]
{label = _U('fine'),			value = 'fine'},
{label = _U('unpaid_bills'),	value = 'unpaid_bills'},
-- adicione código abaixo (não se esqueça de adicionar ',' antes da nova linha)
{label = "Serviço comunitário",	value = 'communityservice'}


-- ADIÇÃO [2]
elseif action == 'unpaid_bills' then
	OpenUnpaidBillsMenu(closestPlayer)
-- adicione o código abaixo
elseif action == 'communityservice' then
	SendToCommunityService(GetPlayerServerId(closestPlayer))
end


-- ADIÇÃO [3]
-- adicionar esta função
function SendToCommunityService(player)
	ESX.UI.Menu.Open('dialog', GetCurrentResourceName(), 'Community Service Menu', {
		title = "Menu de serviço comunitário",
	}, function (data2, menu)
		local community_services_count = tonumber(data2.value)

		if community_services_count == nil then
			ESX.ShowNotification('Contagem de serviços inválidos.')
		else
			TriggerServerEvent("esx_communityservice:sendToCommunityService", player, community_services_count)
			menu.close()
		end
	end, function (data2, menu)
		menu.close()
	end)
end
```
#Creditos

- [Ap. Iatridis](https://github.com/apoiat) - Desenvolvedor do client_scripts
- [Renildo Marcio](https://github.com/psycodeliccircus) - Tradução e implementações

# Legal
### License
ESX_CommunityService - from ESX Brasil
Copyright (C) 2018-2019 Apostolos Iatridis

This program Is free software: you can redistribute it And/Or modify it under the terms Of the GNU General Public License As published by the Free Software Foundation, either version 3 Of the License, Or (at your option) any later version.

This program Is distributed In the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty Of MERCHANTABILITY Or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License For more details.

You should have received a copy Of the GNU General Public License along with this program. If Not, see http://www.gnu.org/licenses/.
