# Ansible 
# 1    Haproxy
# 2    MySQL Master-Master
# autor: Radchenko Vitalii (vitaliymichailovich@gmail.com)


1. What will you have:
1.1 One server with Haproxy load balancer
1.2 Two servers with MySQL 5.6 from repo. 
Will configure to work Master-Master replication.

2. Configuration:
2.1 In "inventory" file you should write an IP-addresses your servers:
- j1 - Haproxy
- j2 - MySQL Master 1 (MySQL1)
- j2 - MySQL Master 2 (MySQL2)
2.2 "templates/haproxy.cfg.j2" will configure haproxy to balance between two MySQL servers. 
2.3 "1.yml" change next lines:
- Line 5: IP-address of MySQL1
- Line 6: IP-address of MySQL2
2.4 "2.yml" change next lines:
- Line 5: server ID. Any int, but not same as MySQL2
- Line 6: name of database which you need to replicate
- Line 7: username to replicate
- Line 8: password for username (Line 7) to replicate
- Line 9: IP-address of MySQL2
2.5 "3.yml" change next lines:
- Line 5: server ID. Any int, but not same as MySQL3
- Line 6: name of database which you need to replicate (same as MySQL2 line 6)
- Line 7: username to replicate (same as MySQL2 line 7)
- Line 8: password for username (Line 7) to replicate (same as MySQL2 line 8)
- Line 9: IP-address of MySQL1 (same as MySQL2 line 6)
2.6 "4.yml" change next lines:
- Line 4: name of database which you need to replicate
- Line 5: username to replicate
- Line 6: password for username (Line 5) to replicate
- Line 7: IP-address of MySQL2
- Line 28: name of database which you need to replicate 
- Line 29: username to replicate 
- Line 30: password for username (Line 29) to replicate 
- Line 31: IP-address of MySQL1 

3. Files: 
3.1 "/templates/haproxy.cfg.j2" - template of haproxy configuration file
3.2 "/templates/my.cnf/j2" - templpate of MySQL configuration file
3.3 "1.yml" - install a haprohy on server j1
3.4 "2.yml" - install MySQL, and some librarys for correct work, create users for server j2
3.5 "3.yml" - install MySQL, and some librarys for correct work, create users for server j3
3.6 "4.yml" - get master status and create MySQL Master-Master replication servers j2-j3
3.7 "ansible.cfg" - ansible configuration file
3.8 "inventory" - inventory file
3.9 "site.yml" - yaml file which includ all configuration files

4. Commands:
4.1 After making all configuration do command: "ansible-playbook site.yml -i inventory -vv" and you will have a three servers.

5. Addition:
5.1 By degault HAProxy has control pannel on ip-address/haproxy server name. Access: root/root.
5.2 Command to check balancing: mysql -h 192.168.122.200 -u replicator_a -p -e "show variables like 'server_id'"
Instead of 192.168.122.200 ip address of your HAProxy.
