# Ansible 
    Haproxy
    MySQL Master-Master

1. Configure three servers:
1.1 One with Haproxy load balancer
1.2 Two servers with MySQL 5.6 from repo. 
Configure them that they works with Master-Master replication.

2. Configuration:
2.1 In "inventory" file you should write an IP-addresses your servers:
- j1 - Haproxy
- j2 - MySQL Master 1 (MySQL1)
- j2 - MySQL Master 2 (MySQL2)
2.2 "templates/haproxy.cfg.j2" configure haproxy what you need to do with it after installation.
You can change it on your server after installation.
2.3 "2.yml" change lines:
- Line 5: server ID. Any int, but not same as MySQL2
- Line 6: name of database which you need to replicate
- Line 7: username to replicate
- Line 8: password for username (Line 7) to replicate
- Line 9: IP-address of MySQL2
2.4 "3.yml" change lines:
- Line 5: server ID. Any int, but not same as MySQL3
- Line 6: name of database which you need to replicate (same as MySQL2 line 6)
- Line 7: username to replicate (same as MySQL2 line 7)
- Line 8: password for username (Line 7) to replicate (same as MySQL2 line 8)
- Line 9: IP-address of MySQL1 (same as MySQL2 line 6)
2.5 "4.yml" change lines: