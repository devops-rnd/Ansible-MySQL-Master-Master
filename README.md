Installing and setting up a MySQL Master-Master replication and HAProxy for balancing a MySQL querys
autor: Radchenko Vitalii (vitaliymichailovich@gmail.com)

What will you have?
One server with Haproxy load balancer.
Two MySQL servers with Master-Master replication.

Configuration.
  1 In "inventory" file you should write an IP-addresses your servers:
    j1 - Haproxy
    j2 - MySQL Master 1 (MySQL1)
    j3 - MySQL Master 2 (MySQL2)
  2 "templates/haproxy.cfg.j2" will configure haproxy to balance between two MySQL servers. 
  3 "1.yml" change next lines:
    Line 5: IP-address of MySQL1
    Line 6: IP-address of MySQL2
  4 "2.yml" change next lines:
    Line 5: server ID. Any int, but not same as MySQL2
    Line 6: name of database which you need to replicate
    Line 7: username to replicate
    Line 8: password for username (Line 7) to replicate
    Line 9: IP-address of MySQL2
  5 "3.yml" change next lines:
    Line 5: server ID. Any int, but not same as MySQL3
    Line 6: name of database which you need to replicate (same as MySQL2 line 6)
    Line 7: username to replicate (same as MySQL2 line 7)
    Line 8: password for username (Line 7) to replicate (same as MySQL2 line 8)
    Line 9: IP-address of MySQL1 (same as MySQL2 line 6)
  6 "4.yml" change next lines:
    Line 4: name of database which you need to replicate
    Line 5: username to replicate
    Line 6: password for username (Line 5) to replicate
    Line 7: IP-address of MySQL2
    Line 28: name of database which you need to replicate 
    Line 29: username to replicate 
    Line 30: password for username (Line 29) to replicate 
    Line 31: IP-address of MySQL1 

Files: 
  1 "/templates/haproxy.cfg.j2" - template of haproxy configuration file
  2 "/templates/my.cnf/j2" - templpate of MySQL configuration file
  3 "1.yml" - install a haprohy on server j1
  4 "2.yml" - install MySQL, and some librarys for correct work, create users for server j2
  5 "3.yml" - install MySQL, and some librarys for correct work, create users for server j3
  6 "4.yml" - get master status and create MySQL Master-Master replication servers j2-j3
  7 "ansible.cfg" - ansible configuration file
  8 "inventory" - inventory file
  9 "site.yml" - yaml file which includ all configuration files

Commands:
  After making all configuration do command: "ansible-playbook site.yml -i inventory -vv" and you will have a three servers.

Addition:
  1 By degault HAProxy has control pannel on ip-address/haproxy server name. Access: root/root.
  2 Command to check balancing: mysql -h 192.168.122.200 -u replicator_a -p -e "show variables like 'server_id'"
  Instead of 192.168.122.200 ip address of your HAProxy.
  
Testing:
  Successfully tested on KVM with Centos 7.
