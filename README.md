# ESX_CommunityService

An alternative form of punishment and social correction to jail. With this script, you can now send criminals in the central square, to provide community service by cleaning and gardening. Try to escape it and you will get your service extended!


### Requirements
* ESX
* ESX skinchanger
  * [skinchanger](https://github.com/ESX-Org/skinchanger)

## Download & Installation

### Using Git
```
cd resources
git clone https://github.com/apoiat/esx_communityservice [esx]/esx_communityservice
```

### Manually
- Download https://github.com/apoiat/esx_communityservice/archive/master.zip
- Put it in the `[esx]` directory


## Installation
- Import `esx_communityservice.sql` in your database
- Add this in your server.cfg :

```
start esx_communityservice
```
## How to apply community service.

- Use the `esx_communityservice:sendToCommunityService(target, service_count)` server trigger.
- Use the `/comserv player_id service_count` command (only admins).
- Use the `/endcomserv player_id` to finish a player's community service (only admins).



# How to add to policejob menu.

Example in `esx_policejob: client/main.lua`:

```lua
-- ADDITION [1]
{label = _U('fine'),			value = 'fine'},
{label = _U('unpaid_bills'),	value = 'unpaid_bills'},
-- add code below (don't forget to add ',' before new row)
{label = "Community Service",	value = 'communityservice'}


-- ADDITION [2]
elseif action == 'unpaid_bills' then
	OpenUnpaidBillsMenu(closestPlayer)
-- add code below
elseif action == 'communityservice' then
	SendToCommunityService(GetPlayerServerId(closestPlayer))
end


-- ADDITION [3]
-- add this function
function SendToCommunityService(player)
	ESX.UI.Menu.Open('dialog', GetCurrentResourceName(), 'Community Service Menu', {
		title = "Community Service Menu",
	}, function (data2, menu)
		local community_services_count = tonumber(data2.value)
		
		if community_services_count == nil then
			ESX.ShowNotification('Invalid services count.')
		else
			TriggerServerEvent("esx_communityservice:sendToCommunityService", player, community_services_count)
			menu.close()
		end
	end, function (data2, menu)
		menu.close()
	end)
end
```


# Legal
### License
ESX_CommunityService - A community service script for fivem servers.

Copyright (C) 2018-2019 Apostolos Iatridis

This program Is free software: you can redistribute it And/Or modify it under the terms Of the GNU General Public License As published by the Free Software Foundation, either version 3 Of the License, Or (at your option) any later version.

This program Is distributed In the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty Of MERCHANTABILITY Or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License For more details.

You should have received a copy Of the GNU General Public License along with this program. If Not, see http://www.gnu.org/licenses/.
