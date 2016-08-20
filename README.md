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
2.2 "templates/haproxy.cfg.j2" will configure haproxy what you need to do with it after installation.
You can change it on your server after installation.
2.3 "2.yml" change next lines:
- Line 5: server ID. Any int, but not same as MySQL2
- Line 6: name of database which you need to replicate
- Line 7: username to replicate
- Line 8: password for username (Line 7) to replicate
- Line 9: IP-address of MySQL2
2.4 "3.yml" change next lines:
- Line 5: server ID. Any int, but not same as MySQL3
- Line 6: name of database which you need to replicate (same as MySQL2 line 6)
- Line 7: username to replicate (same as MySQL2 line 7)
- Line 8: password for username (Line 7) to replicate (same as MySQL2 line 8)
- Line 9: IP-address of MySQL1 (same as MySQL2 line 6)
2.5 "4.yml" change next lines:
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

4. Command:
After making all configuration do command: "ansible-playbook site.yml -i inventory -vv" and you will have a three servers.
