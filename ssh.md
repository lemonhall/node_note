It's possible that your server closes connections that are idle for too long. You can update either your client (ServerAliveInterval) or your server (ClientAliveInterval)

 ServerAliveInterval
         Sets a timeout interval in seconds after which if no data has
         been received from the server, ssh(1) will send a message through
         the encrypted channel to request a response from the server.  The
         default is 0, indicating that these messages will not be sent to
         the server.  This option applies to protocol version 2 only.

 ClientAliveInterval
         Sets a timeout interval in seconds after which if no data has
         been received from the client, sshd(8) will send a message
         through the encrypted channel to request a response from the
         client.  The default is 0, indicating that these messages will
         not be sent to the client.  This option applies to protocol
         version 2 only.
To update your server (and restart your sshd)

echo "ClientAliveInterval 60" | sudo tee -a /etc/ssh/sshd_config
Or client-side:

echo "ServerAliveInterval 60" >> ~/.ssh/config 