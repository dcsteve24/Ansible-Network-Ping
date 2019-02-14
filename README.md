# Ansible-Network-Ping
A fully customizable script that will network ping (ICMP) specified ranges

This is a quick play to run on a single target that will create a list of IPs it can network ping (not ansible ping). I designed this since ansible doesnt have a module for network pinging, as their ping module checks SSH/login capabilities; and many argue that you are trying to make a firetruck do an ambulance's job. 

But I am stubborn and this became more of a "Can I do it?" project than a useability one since this can easily be scripted in bash, or done with NMAP, but may be useful for some. I have generalized the play to the point, you can run it against all and every range possible, but note, the larger the range, the signifigantly longer this will take.

Results
------------
1. The play will output the pingable IPs
2. If defaults are not changed, the same output from 1 is saved to a text file /tmp/good_ipsDATE_TIME.txt on the target

Requirements
------------
An account that can SSH into the target

Role Variables
--------------


How to Run
--------------
This will run straight out of the box, but will take forever as it is testing every single IP possible and attempts each twice. It is strongly recommend you edit this down to the desired target.

1. Download to desired destination: i.e. /etc/ansible/playbooks
2. (optional) Change the variables under "Edit me as needed" for your desired results.
3. Run the play; I typically run mine with ansible-playbook -kK path_to_play.

Author Information
------------------
Steven Craig, 14Feb19
