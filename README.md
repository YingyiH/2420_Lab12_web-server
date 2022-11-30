# 2420_Lab12_web-server
2420_Lab12_web-server
Student: Yingyi He
Student ID: A01230375

## Step1: Install Nginx web server
- Use `sudo apt update` and `sudo apt upgrade` at first.

<img width="288" alt="Screenshot 2022-11-23 at 6 03 15 PM" src="https://user-images.githubusercontent.com/100324443/204765230-587440b6-0984-4476-9a2b-e21b890fee40.png">
<img width="662" alt="Screenshot 2022-11-23 at 6 05 17 PM" src="https://user-images.githubusercontent.com/100324443/204765453-d7bd9de0-b6a7-4c96-8831-1fbb10987ca3.png">

- Use `sudo apt install nginx` to install nginx package.

<img width="646" alt="Screenshot 2022-11-23 at 6 06 20 PM" src="https://user-images.githubusercontent.com/100324443/204765627-c2ad64f6-2e56-493d-b5d2-2700ddb0db05.png">

- Use `cd /etc/nginx` and `ls` check if nginx package installed successfully.

<img width="969" alt="Screenshot 2022-11-23 at 1 55 13 AM" src="https://user-images.githubusercontent.com/100324443/204765770-06f29fba-be68-4110-b0c5-809a428baedf.png">

## Step2: Create html file in `/var/www/<IP Address>` directory
```
<!DOCTYPE html>
<html lang="en">
<html>
    <head>
				<meta charset="UTF-8" />
        <title>Example Site for 2420</title>
    </head>
    <body>
        <h1>Success!</h1>
        <h2 style="color: red;">All your internets are belong to us!</h2>
    </body>
</html>
```
- Create a html file in host specific directory:

<img width="496" alt="Untitled" src="https://user-images.githubusercontent.com/100324443/204765937-fa1a39d6-d3c2-48d4-8768-9a909e8e5be0.png">

## Step3: Create a Nginx server block
- Use `cd /etc/nginx/sites-available/` to get into the folder.
- Use `sudo vim **<IP Address>**` to create and write the server:

<img width="556" alt="Screenshot 2022-11-23 at 6 09 31 PM" src="https://user-images.githubusercontent.com/100324443/204766118-ac60a6c5-6732-4c8b-80e7-355d6123fdab.png">

In the “143.110.239.183” file:
```                                                                                                               
server {
        listen 80;
        listen [::]:80;

        root /var/www/143.110.239.183/html;
        index index.html;

        server_name 143.110.239.183;

        location / {
                try_files $uri $uri/ =404;
        }
}

```
- Create a soft link to the new server block in sites-enabled:
with this command line:
```
sudo ln -s /etc/nginx/sites-available/<IP Address> /etc/nginx/sites-enabled/
```
- Use `sudo nginx -t` to test your Nginx configuration:

<img width="1028" alt="Screenshot 2022-11-23 at 9 24 13 PM" src="https://user-images.githubusercontent.com/100324443/204766556-99f67db0-758f-4f4c-b369-260805216160.png">

## Step4: Upload file from local host to server
- Use `mkdir` to create a backup_files directory:
(backup_files directory works as PC host download directory)

<img width="310" alt="Screenshot 2022-11-23 at 8 15 14 PM" src="https://user-images.githubusercontent.com/100324443/204766776-42f27893-3927-49e9-953d-86315424c608.png">

- Connect the local host to the server “web-one”:
- Use `mkdir` to create a .ssh directory in web-one user home page
  - Create an “authorized_key” in .ssh directory in web-one
  
<img width="896" alt="Screenshot 2022-11-23 at 9 06 38 PM" src="https://user-images.githubusercontent.com/100324443/204766949-76596749-9733-4ec3-a9cf-d7cdd6f0a5ee.png">

  - Use vim to add “web-one” ssh public key to “authorized_key” file
  
<img width="1466" alt="Screenshot 2022-11-23 at 9 07 08 PM" src="https://user-images.githubusercontent.com/100324443/204767100-7f74d5dd-5070-4579-b8bc-20b7923ab3f4.png">

- Get into the file location in local host
- Use `sftp` to connect the local host and the server “web-one”

<img width="592" alt="Screenshot 2022-11-23 at 9 11 56 PM" src="https://user-images.githubusercontent.com/100324443/204767273-72cce54e-5b54-4d73-b5c9-8096eef6b347.png">

- Use `put` to copy the `index.html` to server “web-one”

<img width="591" alt="Screenshot 2022-11-23 at 9 12 43 PM" src="https://user-images.githubusercontent.com/100324443/204767396-9759408e-0ed1-48b3-b1ce-370b27551362.png">

- Use `mv` to move the `index.html` to the correct location:

<img width="549" alt="Screenshot 2022-11-23 at 9 18 38 PM" src="https://user-images.githubusercontent.com/100324443/204767569-efd4c664-e25f-4a92-be1a-d96e01bb51cc.png">

## Step5: Restart server
- Use `sudo systemctl restart nginx` to restart the Nginx server:

<img width="625" alt="Untitled" src="https://user-images.githubusercontent.com/100324443/204768031-06d63ba8-6446-4a0f-92d4-17ea8a1335e1.png">

## Step6: Check if the IP Address is served
- Use `http://<IP Address>` as the web address to access the html file

<img width="500" alt="Screenshot 2022-11-23 at 6 22 44 PM" src="https://user-images.githubusercontent.com/100324443/204768234-9f558528-e169-4307-bf29-4944b998a0a9.png">

## Step7: Set up the firewall
- Use `sudo ufw status` to check ufw status and use `sudo ufw app list` to list them

<img width="306" alt="Untitled" src="https://user-images.githubusercontent.com/100324443/204768590-f0a42ed3-e2bd-4ec2-ba32-3fa52a44003d.png">

- Allow incoming http and ssh

<img width="319" alt="Screenshot 2022-11-23 at 9 47 46 PM" src="https://user-images.githubusercontent.com/100324443/204768651-ec73335a-3fef-45be-a00d-1858d8aefae5.png">

## Step8: Enable the firewall

<img width="634" alt="Screenshot 2022-11-23 at 9 52 44 PM" src="https://user-images.githubusercontent.com/100324443/204768752-09cc0441-5bd1-4716-87c3-a8d9978a5677.png">

- Check if still can access the website by IP address

<img width="354" alt="Screenshot 2022-11-23 at 9 53 33 PM" src="https://user-images.githubusercontent.com/100324443/204768867-5771eb9e-3cde-4244-bd9c-d9214ee63e4e.png">










