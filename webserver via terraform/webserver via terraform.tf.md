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
    from_port        = 0
    to_port          = 1000
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }

 egress {
    from_port        = 0
    to_port          = 1000
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }


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
    user_data =   <<-EOF
                  #!/bin/bash
                  sudo su
                  yum -y install httpd
                  echo "<p> SALAHDIN WEB SERVER TEST VIA TERRAFORM! </p>" >> /var/www/html/index.html
                  sudo systemctl enable httpd
                  sudo systemctl start httpd
                  EOF
}
