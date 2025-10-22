#Delete Stale Configurations
sed -i '/GatewayPorts/d' /etc/ssh/sshd_config
sed -i '/AllowTcpForwarding/d' /etc/ssh/sshd_config
sed -i '/AllowStreamLocalForwarding/d' /etc/ssh/sshd_config

#Allow any computer in the world to connect to forwarded port
echo "GatewayPorts yes" >> /etc/ssh/sshd_config

#Enable Port Forwarding 
echo "AllowTcpForwarding remote" >> /etc/ssh/sshd_config

#Do not forward unix domain sockets
echo "AllowStreamLocalForwarding no" >> /etc/ssh/sshd_config

#Restart SSH Daemon
sudo systemctl restart ssh.service
