This script is designed to generate route-map configurations based on interfaces found in a system database. It processes a list of system interfaces and generates configuration blocks for specific interface groups. Each group corresponds to a different community and rule number, with the ability to process multiple interfaces from each group.

The script works with interfaces categorized by keywords like MGMT, CLIENT, STUDENT, IOT, HEALTH-Z1, and HEALTH-Z2. Based on these keywords, it creates a route-map configuration and increments rule numbers for each interface in the group.


Features
Automatically processes interfaces based on predefined keywords in the interface name.
Generates route-map configurations for:
MGMT (Community: 5656:600)
CLIENT (Community: 5656:700)
STUDENT (Community: 5656:900)
IOT (Community: 5656:800)
HEALTH-Z1 (Community: 5656:200)
HEALTH-Z2 (Community: 5656:201)
Incrementing rule numbers for each matching interface within a group.


How it Works
The script loops through a list of system interfaces (DEVDB_system_interface).
It checks each interface to see if its name contains any of the following keywords:
MGMT
CLIENT
STUDENT
IOT
HEALTH-Z1
HEALTH-Z2
If a matching interface is found, a route-map block is generated, and the rule number for that group is incremented.
If no interfaces are found for any of the groups, the script will output a message indicating that no route-map configuration has been applied.
Configuration Generation
For each matching interface, the script generates a configuration block for the router's route-map as follows:

MGMT interfaces: Community 5656:600, starting rule number 100.
CLIENT interfaces: Community 5656:700, starting rule number 150.
STUDENT interfaces: Community 5656:900, starting rule number 200.
IOT interfaces: Community 5656:800, starting rule number 250.
HEALTH-Z1 interfaces: Community 5656:200, starting rule number 300.
HEALTH-Z2 interfaces: Community 5656:201, starting rule number 350.
For each new interface found in a group, the rule number increments by 1. For example, if two MGMT interfaces are found, the first will use rule number 100 and the second 101.




Example Output
For example, if the system contains the following interfaces:

MGMT-01
CLIENT-01
STUDENT-01
IOT-01
HEALTH-Z1-01
The script will generate configurations like this:

config router route-map
    edit "rm-bgp-out-hub"
        config rule
            edit 100
                set match-interface "MGMT-01"
                set set-community "5656:600"
            next
        end
    next
end

config router route-map
    edit "rm-bgp-out-hub"
        config rule
            edit 150
                set match-interface "CLIENT-01"
                set set-community "5656:700"
            next
        end
    next
end

config router route-map
    edit "rm-bgp-out-hub"
        config rule
            edit 200
                set match-interface "STUDENT-01"
                set set-community "5656:900"
            next
        end
    next
end

config router route-map
    edit "rm-bgp-out-hub"
        config rule
            edit 250
                set match-interface "IOT-01"
                set set-community "5656:800"
            next
        end
    next
end

config router route-map
    edit "rm-bgp-out-hub"
        config rule
            edit 300
                set match-interface "HEALTH-Z1-01"
                set set-community "5656:200"
            next
        end
    next
end
