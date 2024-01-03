# GEOMAP GRAFANA

Realizei a criação da visualização dos hosts do Zabbix no Grafana:

Grafana Versão: 8.4.5 (Ou qualquer um que tenha GeoMap)
Plugin Zabbix Versão: 4.2.10


Utilizado Datasource: MySQL
Format AS: Table

Query:

```
SELECT h.host, hi.location_lat AS `latitude`, hi.location_lon AS `longitude`, hy.value

FROM hosts h

                INNER JOIN host_inventory hi ON h.hostid = hi.hostid AND hi.location_lat != '' AND hi.location_lon != ''

                INNER JOIN items i ON h.hostid = i.hostid AND (i.key_ = 'icmpping' OR i.key_ LIKE 'icmpping[%')

    LEFT JOIN history_uint hy ON i.itemid = hy.itemid AND hy.clock = (SELECT MAX(clock) FROM history_uint WHERE itemid = i.itemid)

WHERE h.status = 0 AND i.status = 0
```

Explicação da Query:

Realiza um Select que pega as informações do inventory (longitude e latitude) do host e também retorna o ultimo valor coletado do item icmp ping, afim de definir os equipamentos que estão up ou down.

`
Necessário adicionar longitude e latitude do host no inventory Zabbix
`

Abaixo imagens das configurações do dashboard:



![image](https://github.com/DennisNgrox/GeoMap_Grafana/assets/81188924/c2bf1ab7-62b2-4c00-8bd1-8c00d00bd649)


Configuração do Dashboard:

![image](https://github.com/DennisNgrox/GeoMap_Grafana/assets/81188924/153b986c-f81b-4db9-9ca6-ee6b2db8596e)

Configuração das métricas:

![image](https://github.com/DennisNgrox/GeoMap_Grafana/assets/81188924/d99ff9c6-2582-497d-8b4f-d9206c0e5bef)

`Utilize a query SQL no campo que está na imagem`

