title=Default network address in VirtManager
date=2014-10-12
type=post
tags=blog, virtmanager
status=draft
~~~~~~
Changing default network address `192.168.122.x` in Virt Manager
--

    sudo virsh net-edit default 
    
and replace all `192.168.122.` with `192.168.200.` or other ip number.