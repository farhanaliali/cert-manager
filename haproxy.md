## HaProxy Load Balancer

### installing HAPROXY

    sudo apt install haproxy

###  Configure HA proxy 


    sudo vim /etc/haproxy/haproxy.cfg

add the below line at the end of file 


    frontend haproxynode
        bind *:80
        mode tcp
        option tcplog
        stats uri /haproxy?stats
        default_backend backendnodes
    backend backendnodes
        mode tcp
        balance roundrobin
        server node1 46.4.52.94:30112 check

    frontend haproxynode_https
        bind *:443
        mode tcp
        option tcplog
        stats uri /haproxy?stats
        default_backend backendnodes_https
    backend backendnodes_https
        mode tcp
        balance roundrobin
        server node1 46.4.52.94:31941 check


Save and restart the HAProxy service 

    sudo service haproxy restart 
    