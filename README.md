
## Playbooks for exercise-4


- Hostname

- name: Change hostname
  hosts: cisco
  gather_facts: false
  
  tasks:
    - name: configure top level configuration
      cisco.ios.ios_config:
         lines: hostname {{ inventory_hostname }}

    - name: save running to start-config when modified
      cisco.ios.ios_config:
        save_when: modified
- Login Banner

---
- name:  Display Cisco info
  hosts: cisco
  gather_facts: false
  
  tasks:

    - name: Configure the login banner
      cisco.ios.ios_banner:
        banner: login
        text: |
          My name is Jasher and this is my login banner
          that contains a multiline
          string
        state: present

- SNMP

- name: Config SNMP
  hosts: cisco
  gather_facts: false

  tasks:
    - name: Configure snmp
      cisco.ios.ios_config:
        commands:
          - snmp-server community ansible-public RO
          - snmp-server community ansible-private RW
          - snmp-server host 192.168.2.107 public
          
  
- Name Servers


- name: Config nameservers
  hosts: cisco
  gather_facts: false

  tasks:
    - name: Configure nameservers
      cisco.ios.ios_system:
        name_servers:
        -  8.8.8.8
        -  8.8.4.4

- Logging

---
- name: Configure buffer size
  hosts: cisco
  gather_facts: false

  tasks:
    - name: configure buffer size
      cisco.ios.ios_logging:
        dest: buffered
        size: 5000

- NTP

---
- name: NTP authentication
  hosts: cisco
  gather_facts: false
  
  tasks:
    - name: Configure NTP authentication
      cisco.ios.ios_ntp:
        key_id: 10
        auth_key: 15435A030726242723273C21181319000A
        auth: true
        state: present

- Configure IP address on interfaces

---
- name: Change IP
  hosts: myrouters
  gather_facts: false
  
  tasks:
    - name: Set new IP adress
      cisco.ios.ios_l3_interfaces:
        config:
        - name: GigabitEthernet2
          ipv4:
          - address: "{{ new_ip }}"

- Configure a routing protocol (router BGP/OSPF)

---
- name: OSPF
  hosts: cisco
  gather_facts: false

  tasks:
    - name: Configuring OSPF
      cisco.ios.ios_config:
        lines:
        - network 192.168.2.0 0.0.0.244 area 0
        parents: router ospf 1


