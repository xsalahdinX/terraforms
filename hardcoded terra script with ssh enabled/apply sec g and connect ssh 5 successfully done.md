provider "aws" {
    profile = "default"
    region = "eu-west-3"
}

resource "aws_default_vpc" "main" {

  tags = {
    Name = "main"
  }

}
resource "aws_security_group" "salah-sec-group" {
  name        = "salah-sec-group"
  description = "Allow tls inbound traffic"
  vpc_id      = aws_default_vpc.main.id

  ingress {
    description      = "inbound rules from VPC"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }
 #it wasnt work because the sider block option i made it anywhere

  tags = {
    Name = "aws_security_group"
    instance_name = "aws_instance"
  }
}

resource "aws_instance" "ec2-instance" {
    ami = "ami-0960de83329d12f2f"
    instance_type = "t2.micro"
    security_groups = [aws_security_group.salah-sec-group.name]
    key_name = "SALAHDIN"
}

[root@client terra]# ssh -i /root/SALAHDIN.pem ec2-user@ec2-15-188-51-95.eu-west-3.compute.amazonaws.com
