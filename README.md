# OSM-Server
These following things change the language in OSM Server setup in Local system.
To Setup the OSM Server on Local system i Use following link:-
https://www.linuxbabe.com/ubuntu/openstreetmap-tile-server-ubuntu-18-04-osm

In the 3rd Step of setup we have to download the Openstreet Carto using following Command

git clone https://github.com/gravitystorm/openstreetmap-carto.git

We have to change some funtions in this Carto file to change the local language of map into other languange.

Example:- Below Example I am changing the all world Local's Language into English.

1.) After Download the Openstreet Carto find the following file in the folder
    openstreetmap-carto.lua
   
2.) Open this file and find  these Three Functions
    1.) function filter_tags_node (keyvalues, numberofkeys)
  
    2.) function filter_basic_tags_rel (keyvalues, numberofkeys)
    
    3.) function filter_tags_way (keyvalues, numberofkeys)
    
To Change the Local Language to English these Follwing editings are required in all 3 Funtions.
 
1.) -- Filtering on nodes
function filter_tags_node (keyvalues, numberofkeys)
    if keyvalues["name"] then
        keyvalues["name"]=keyvalues["name:en"]
    end
    return filter_tags_generic(keyvalues)
end

  2.) -- Filtering on relations
function filter_basic_tags_rel (keyvalues, numberofkeys)
    -- Filter out objects that are filtered out by filter_tags_generic
    if keyvalues["name"] then
        keyvalues["name"]=keyvalues["name:en"]
    end
    local filter, keyvalues = filter_tags_generic(keyvalues)
    if filter == 1 then
        return 1, keyvalues
    end

    -- Filter out all relations except route, multipolygon and boundary relations
    if ((keyvalues["type"] ~= "route") and (keyvalues["type"] ~= "multipolygon") and (keyvalues["type"] ~= "boundary")) then
        return 1, keyvalues
    end

    return 0, keyvalues
end

  3.) -- Filtering on ways
function filter_tags_way (keyvalues, numberofkeys)
    local filter = 0  -- Will object be filtered out?
    local polygon = 0 -- Will object be treated as polygon?
    if keyvalues["name"] then
        keyvalues["name"]=keyvalues["name:en"]
    end
    
    -- Filter out objects that are filtered out by filter_tags_generic
    filter, keyvalues = filter_tags_generic(keyvalues)
    if filter == 1 then
        return filter, keyvalues, polygon, roads
    end

    polygon = isarea(keyvalues)

    -- Add z_order column
    keyvalues["z_order"] = z_order(keyvalues)

    return filter, keyvalues, polygon, roads(keyvalues)
end

(Screenshot of Changing Local Language will be add Soon)
