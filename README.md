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




متد open_channel()

متد open_channel() به شما اجازه می‌دهد تا یک کانال جدید در یک اتصال SSH باز کنید که می‌تواند برای انتقال داده‌ها، اجرای دستورات، ارسال ورودی، دریافت خروجی و سایر ارتباطات مورد استفاده قرار گیرد.

ساختار کلی متد:

channel = transport.open_channel(kind, host=None, port=None, environment=None)

kind: نوع کانال. معمولاً این مقدار یکی از موارد زیر است:

"session": برای اجرای دستورات و تعامل با سرور.

"forwarded-tcpip": برای فورواردینگ پورت SSH.

"direct-tcpip": برای کانال‌های ارتباطی TCP مستقیم.

host و port (اختیاری): در صورتی که نوع کانال، ارتباط شبکه‌ای باشد، این مقادیر به مقصدی که می‌خواهید با آن ارتباط برقرار کنید اشاره دارند.

environment (اختیاری): برای تنظیم متغیرهای محیطی برای کانال.

مثال استفاده از open_channel() برای اجرای دستور در سرور

فرض کنید می‌خواهید یک دستور SSH روی سرور از طریق کانال اجرا کنید:



import paramiko

hostname = "example.com"
username = "your_username"
password = "your_password"

:

    try:

        # ایجاد SSHClient
        ssh_client = paramiko.SSHClient()
        ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
        
        # اتصال به سرور
        ssh_client.connect(hostname, username=username, password=password)
        
        # گرفتن شیء Transport
        transport = ssh_client.get_transport()
    
        # باز کردن کانال برای اجرای دستور
        channel = transport.open_channel("session")
        
        # اجرای دستور روی سرور
        channel.exec_command("echo 'Hello, World!'")
        
        # دریافت خروجی دستور
        output = channel.recv(1024).decode("utf-8")
        print(f"Command output: {output}")
        
        # بستن کانال
        channel.close()



    except Exception as e:
    
        print(f"An error occurred: {e}")
        
    finally:
    
        # بستن اتصال SSH
        if ssh_client:
            ssh_client.close()
توضیح کد:
ابتدا اتصال SSH به سرور برقرار می‌شود.

متد open_channel("session") یک کانال جدید به نام "session" ایجاد می‌کند که می‌توان از آن برای ارسال دستورات به سرور استفاده کرد.

دستور echo 'Hello, World!' روی سرور اجرا می‌شود.

خروجی دستور دریافت می‌شود و چاپ می‌شود.

کانال و اتصال SSH بسته می‌شوند.

تفاوت کانال‌ها و متد exec_command()

متد exec_command() به طور خودکار یک کانال "session" را برای شما باز می‌کند و دستور را اجرا می‌کند.

در حالی که open_channel() به شما این امکان را می‌دهد که کانال‌ها را به صورت دستی باز کرده و بیشتر کنترل کنید (مثلاً برای تنظیمات خاص یا کانال‌های متنوع).

مثال دیگر: استفاده از کانال برای فوروارد کردن پورت

در صورتی که بخواهید از open_channel() برای فوروارد کردن پورت SSH استفاده کنید، می‌توانید از نوع کانال "forwarded-tcpip" استفاده کنید.

:

    import paramiko
    
    hostname = "example.com"
    username = "your_username"
    password = "your_password"
    local_port = 8080
    remote_host = "localhost"
    remote_port = 80
    
    try:
        # ایجاد SSHClient
        ssh_client = paramiko.SSHClient()
        ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
        
        # اتصال به سرور
        ssh_client.connect(hostname, username=username, password=password)
        
        # گرفتن شیء Transport
        transport = ssh_client.get_transport()
    
        # باز کردن کانال فوروارد TCP
        channel = transport.open_channel("forwarded-tcpip", remote_host, remote_port, None, local_port)
        
        print(f"Port forwarding established from localhost:{local_port} to {remote_host}:{remote_port}")
        
        # می‌توانید حالا از local_port به عنوان یک پروکسی برای اتصال به remote_host:remote_port استفاده کنید.
        
        # برای بستن کانال و اتصال:
        channel.close()
    
    except Exception as e:
        print(f"An error occurred: {e}")
    finally:
        # بستن اتصال SSH
        if ssh_client:
            ssh_client.close()
    نتیجه‌گیری
متد open_channel() امکان باز کردن کانال‌های مختلف برای انتقال داده‌ها، اجرای دستورات، فوروارد کردن پورت‌ها و دیگر عملیات‌های SSH را فراهم می‌کند.

این متد معمولاً برای مواردی که به کنترل دقیق‌تری روی ارتباط SSH نیاز دارید، کاربرد دارد.

در حالی که exec_command() برای اجرای دستورات SSH به صورت خودکار و ساده‌تر استفاده می‌شود، open_channel() به شما این امکان را می‌دهد که کانال‌ها را دستی ایجاد کرده و مدیریت کنید.






