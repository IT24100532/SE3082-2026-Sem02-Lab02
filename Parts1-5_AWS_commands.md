# Parts 1–5: AWS EC2 commands (do these yourself and take screenshots)

These parts need your own EC2 instance and `.pem` key, so they could not be run here.
Replace `my-key.pem` with your key file and `<EC2-IP>` with your instance's public IP
(use `ubuntu@` instead of `ec2-user@` on an Ubuntu AMI).
The lab asks for screenshots in a separate Word document for each part.

## Part 1: Connect with SSH
```bash
chmod 400 my-key.pem
ssh -i my-key.pem ec2-user@<EC2-IP>
```

## Part 2: File transfers with SCP
```bash
echo "hello from my laptop" > file.txt
scp -i my-key.pem file.txt ec2-user@<EC2-IP>:~/
scp -i my-key.pem -r Part6/ ec2-user@<EC2-IP>:~/myproject/
ssh -i my-key.pem ec2-user@<EC2-IP> "echo result > ~/output.txt"
scp -i my-key.pem ec2-user@<EC2-IP>:~/output.txt ./downloaded_output.txt
```

## Part 3: SFTP
```bash
sftp -o IdentityFile=my-key.pem ec2-user@<EC2-IP>
# then, at the sftp> prompt:
#   ls
#   put file.txt
#   get output.txt
#   mkdir data
#   exit
```

## Part 4: Remote commands without logging in
```bash
ssh -i my-key.pem ec2-user@<EC2-IP> "ls -lh /home/ec2-user/"
sftp -o IdentityFile=my-key.pem ec2-user@<EC2-IP> <<< $'ls /home/ec2-user/'
```

## Part 5: rsync
```bash
rsync -avz -e "ssh -i my-key.pem" ./Part6/ ec2-user@<EC2-IP>:~/remotefolder/
```

Parts 6 and 7 can also be run on the instance after copying the folders over:
`sudo yum install gcc make -y` (Amazon Linux) or `sudo apt install gcc make -y` (Ubuntu).
