# paramiko_learn
Author : Ali Talebi 

<h3>Installation : </h3>

pip install paramiko

<h3> Working </h3>

import paramiko 

<h3> Create Instance for Connecting </h3>
## creating new instance 
client = paramiko.SSHClient()

## loading Know Keys 
client.load_system_host_keys()


client = paramiko.SSHClient()

## seting policy for adding unknown keys 
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

## try and except 
try : 
    client.connect('your_address_ip' , 'user_name' , 'password' ) 
    stdin , stdout , stderr = client.exec_command('ls -l')
    print(stdout.read().decode())
    ## sftp client 
    sftp = client.open_sftp()
    ## sftp upload
    sftp.put( 'local_file_path' , 'remote_file_path' )
    ## sftp download 
    sftp.get( 'remote_file_path' , 'local_file_path' )

except Exception as e : 
    print(e)


finally : 
    sftp.close()
    client.close()

print(client.__dict__)

