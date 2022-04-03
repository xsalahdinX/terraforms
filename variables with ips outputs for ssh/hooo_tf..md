provider "aws" {
    profile = var.profile_type
    region = var.region_name
}

resource "aws_default_vpc" "main" {

  tags = {
    Name = var.vpc_name
  }

}
resource "aws_security_group" "security_group_name" {
  name        = var.security_group_name
  description = var.security_group_discrip
  vpc_id      = aws_default_vpc.main.id

  ingress {
    description      = var.ingress_discription
    from_port        = var.from_port_num
    to_port          = var.to_port_num
    protocol         = var.protocal_type
    cidr_blocks      = [var.cidr_block_security_port_allow]
  }


  tags = {
    Name = "aws_security_group"
    instance_name = "aws_instance"
  }
}

resource "aws_instance" "ec2-instance" {
    ami = var.ami_id
    instance_type = var.instance_type_type
    security_groups = [var.security_group_name]
    key_name = var.instance_key_name
}

output "instance_ip" {
description = "ec2 Puplic IP"
value = aws_instance.ec2-instance.public_ip
}

output "instance_ip_private" {
description = "ec2 Private IP"
value = aws_instance.ec2-instance.private_ip
}

