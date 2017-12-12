# Firewalls

## Terminology

Let's start with some terminology. We often hear the words **egress filtering** and **ingress** in connection to talk about firewalls and routers.

**Egress filtering**

This basically means that we are filtering outgoing traffic. So egress filtering ensures that malicious, or just prohibited, traffic is not allowed to leave the network. Of course egress filtering then is the enemy of the hacker.

**Ingress filtering**

This describes filtering traffic coming into the device.  This is far more common than egress filtering as generally there shouldn't be anything but wanted traffic entering the network.  Setting up a bind-shell inherently comes into contact with this issue.

