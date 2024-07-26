# WebServerConfiguration
Tools to setup pretty secure webserver

`
# Réinitialiser les règles existantes
sudo iptables -F
sudo iptables -t nat -F
sudo iptables -t mangle -F
sudo iptables -X

# Définir les politiques par défaut
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Chaîne INPUT
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -p tcp -m multiport --dports 8080,4443 -j ACCEPT
sudo iptables -A INPUT -p tcp -s ROUTER_ADRESS --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp -s INTERNAL_VPN_ADRESS --dport 22 -j ACCEPT
sudo iptables -A INPUT -s 192.168.1.0/24 -j DROP
sudo iptables -A INPUT -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

# Chaîne FORWARD
sudo iptables -P FORWARD DROP

# Chaîne OUTPUT
sudo iptables -A OUTPUT -o lo -j ACCEPT
sudo iptables -A OUTPUT -d 192.168.1.0/24 -j DROP
sudo iptables -A OUTPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
`
