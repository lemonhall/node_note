1、
yum install ppp
cd /usr/local/src
wget http://poptop.sourceforge.net/yum/stable/packages/pptpd-1.3.4-2.el6.x86_64.rpm
rpm -Uhv pptpd-1.3.4-2.el6.x86_64.rpm

Step 2. Edit IP setttings in /etc/pptpd.conf

vi /etc/pptpd.conf

localip 192.168.0.1
remoteip 192.168.0.101-200

Step 3. Add user account in/etc/ppp/chap-secrets (assign username and password)

vi /etc/ppp/chap-secrets

usernameForuser1 *  setpassword1here  *

usernameForuser2 *  setpassword2here  *

Step 4. Optional settings in /etc/ppp/options.pptpd

vi /etc/ppp/options.pptpd

ms-dns 8.8.8.8
ms-dns 4.4.4.4

Step 5. Enable network forwarding in /etc/sysctl.conf
vi /etc/sysctl.conf

net.ipv4.ip_forward = 1

use the following command to apply the change:

sysctl -p


Step 7. Start PPTP VPN server

If your OS is CentOS or Redhat, using the following command:

service pptpd restart

To start PPTP Daemon automatically when rebooting next time, use command:

chkconfig pptpd on