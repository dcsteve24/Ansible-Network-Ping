# Ansible-Network-Ping
A fully customizable play that will network ping (ICMP) specified ranges

This is a quick play to run on a single target that will create a list of IPs it can network ping (not ansible ping). I designed this since ansible doesnt have a module for network pinging, as their ping module checks SSH/login capabilities; and many argue that you are trying to make a firetruck do an ambulance's job. 

But I am stubborn and this became more of a "Can I do it?" project than a useability one since this can easily be scripted in bash, or done with NMAP, but may be useful for some. I have generalized the play to the point, you can run it against all and every range possible, but note, the larger the range, the signifigantly longer this will take.

Results
------------
1. The play will output the pingable IPs
2. If defaults are not changed, the same output from 1 is saved to a text file /tmp/good_ipsDATE_TIME.txt on the target

Requirements
------------
N/A

Role Variables
--------------
The following variables should not be edited as these are controlled and used throughout the play.

| Variable  | Required | Default | Description |
| ------------- | ------------- | ------------- | ------------- |
| ip_oct1_set | Yes  | True | A boolean used to determine if the user set their own octet or not |
| ip_oct2_set | Yes | True | A boolean used to determine if the user set their own octet or not |
| ip_oct3_set | Yes | True | A boolean used to determine if the user set their own octet or not |
| ip_oct4_set | Yes | True | A boolean used to determine if the user set their own octet or not |
| good_ips | Yes | [ ] | Empty array to later be used to hold all the pingable IPs |

The following variables can be edited as needed for your enviornment (optional).

| Variable  | Required | Default | Description |
| ------------- | ------------- | ------------- | ------------- |
| ip_oct1 | Yes | [ ] | The first octect in the IPs that will be looked at. Defaults to an empty array which is filled with 0-255. Can be hard set if desired; e.g. 192. If hard set, this will null out the ranges for this octet |
| ip_oct2 | Yes | [ ] | The second octect in the IPs that will be looked at. Defaults to an empty array which is filled with 0-255. Can be hard set if desired; e.g. 168. If hard set, this will null out the ranges for this octet |
| ip_oct3 | Yes | [ ] | The third octect in the IPs that will be looked at. Defaults to an empty array which is filled with 0-255. Can be hard set if desired; e.g. 100. If hard set, this will null out the ranges for this octet |
| ip_oct4 | Yes | [ ] | The first octect in the IPs that will be looked at. Defaults to an empty array which is filled with 0-255. Can be hard set if desired; e.g. 1. If hard set, this will null out the ranges for this octet |
| ip_oct1_start | Depends | 0 | Unused if ip_oct1 is hard set. If ip_oct1 is left as [ ], this controls the start of the range that's pinged |
| ip_oct1_end | Depends | 255 | Unused if ip_oct1 is hard set. If ip_oct1 is left as [ ], this controls the end of the range that's pinged |
| ip_oct2_start | Depends | 0 | Unused if ip_oct2 is hard set. If ip_oct2 is left as [ ], this controls the start of the range that's pinged |
| ip_oct2_end | Depends | 255 | Unused if ip_oct2 is hard set. If ip_oct2 is left as [ ], this controls the end of the range that's pinged |
| ip_oct3_start | Depends | 0 | Unused if ip_oct3 is hard set. If ip_oct3 is left as [ ], this controls the start of the range that's pinged |
| ip_oct3_end | Depends | 255 | Unused if ip_oct3 is hard set. If ip_oct3 is left as [ ], this controls the end of the range that's pinged |
| ip_oct4_start | Depends | 0 | Unused if ip_oct4 is hard set. If ip_oct4 is left as [ ], this controls the start of the range that's pinged |
| ip_oct4_end | Depends | 255 | Unused if ip_oct4 is hard set. If ip_oct4 is left as [ ], this controls the end of the range that's pinged |
| retry | Yes | 0 | Determines amount of times to try the same IP if it fails |
| save_path | Yes | "/tmp/good_ips{{ ansible_date_time.date }}\_{{ ansible_date_time.time }}.txt" | Save location of the pingable ips |

How to Run
--------------
This will run straight out of the box, but will take forever as it is testing every single IP possible and attempts each twice. It is strongly recommend you edit this down to the desired target.

1. Download to desired destination: i.e. /etc/ansible/playbooks
2. Change the hosts and remote_user to fit your enviornment
3. (optional) Change the variables under "Edit me as needed" for your desired results.
4. Run the play; I typically run mine with ansible-playbook -kK path_to_play.

Errors are a inherit part of this play and this play utilzes the errors to manipulate the flow. While I have no_log set in these locations, you will still see "failed". This is perfectly ok. The play will continue if it errors in the expected spots; if your play stopped, something is legitatmitely wrong. You could see failed in the following locations:
    - Task 1-4 if you hard set any of the octets
    - The task where it starts pinging IPs (any unreachable IP)

Customization
-------------- 
Edit any of the ip_oct# variables as needed and hardset them. i.e. if you want to attempt to ping all 192.168.100.X addresses; put 192 in ip_oct1, 168 in ip_oct2, 100 in ip_oct3 and leave ip_oct4 as [ ].

Similary you can do this with any octet. So if you want to try all 192.X.X.1 addresses; put 192 in ip_oct1, leave ip_oct2 and 3 alone, and put 1 in ip_oct4.

Finally you can edit the range to not look at the entire 255 block by editing the start/end variable for each octet. So to look at 192.168.100.150-250, hard set the ip_oct1-3, leave ip_oct4 as [ ], edit ip_oct4_start to be 150, and ip_oct4_end to be 250. similar approaches can be done with each octet.

Gotchas
--------------
Depending on OS, you may need to edit any '100% packet loss' lines to reflect what you actually get on a failed ping. For example: some OS report back '100.0% packet loss', which would cause this play to do unexpected actions.

Author Information
------------------
Steven Craig, 14Feb19
