# Linux Commands

The linux terminal is an important part of the cloud engineer's toolkit.

- Grant root access for the current terminal  
  `sudo su - `

- download package information from all configured sources.  
  `apt-get update`

- install a package  
  `apt-get install <package>`
  add `-y` to automatically answer yes to all prompts.

- ps auwx | grep <process>  
  ps - this command lists all running processes
  auwx modifies the command to list all processes with extra output  
  grep filters out the output to include only processes with <process>

- Replace value A in file `x.txt` with B:  
  `sed -i -e "s/A/B" x.txt `

- Override file with value  
  `echo "value" > <filepath>`

- append value to file  
  `echo "value" >> <filepath>`

- pings the specified address 3 times  
  `ping -c 3 <ip-address> `

- Format disk  
   `sudo mkfs.ext4 -F -E lazy_itable_init=0,\ lazy_journal_init=0,discard \ /dev/disk/by-id/google-minecraft-disk`

- Mount disk  
  `sudo mount -o discard,defaults /dev/disk/by-id/google-minecraft-disk /home/minecraft`

- create soft link  
  ln -s <source> <destination>
