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


ویژگی‌ها و متدهای مهم paramiko.Transport

1. بررسی فعال بودن اتصال


ssh_client.get_transport().is_active()

مقدار True برمی‌گرداند اگر اتصال SSH هنوز فعال باشد.

اگر اتصال قطع شده باشد یا مشکل داشته باشد، مقدار False خواهد بود.

2. بررسی احراز هویت کامل (Authentication Complete)
ssh_client.get_transport().is_authenticated()
مقدار True برمی‌گرداند اگر فرآیند احراز هویت با موفقیت انجام شده باشد.

اگر احراز هویت انجام نشده باشد یا شکست خورده باشد، مقدار False برمی‌گرداند.

ssh_client.get_transport().close()

اتصال SSH را به صورت امن می‌بندد.

4. دریافت مشخصات سرور و کلاینت
5. 
مشخصات کلاینت:
print(ssh_client.get_transport().get_client_version())

print(ssh_client.get_transport().get_client_version())

5. دریافت وضعیت لایه‌های ارتباطی

برای مشاهده جزئیات رمزنگاری و الگوریتم‌ها:

print(ssh_client.get_transport().get_security_options())








