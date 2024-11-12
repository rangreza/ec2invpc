terraform {
  required_version = "1.5.5"
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "public" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  map_public_ip_on_launch = true
}

resource "aws_subnet" "private" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.2.0/24"
}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main.id
}

resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
}

resource "aws_route" "public_route" {
  route_table_id         = aws_route_table.public.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.igw.id
}

resource "aws_route_table_association" "public_association" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public.id
}

resource "aws_instance" "public_instance" {
  ami           = "ami-08bf489a05e916bbd" # Replace with your desired AMI ID
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.public.id
}

resource "aws_instance" "private_instance" {
  ami           = "ami-08bf489a05e916bbd" # Replace with your desired AMI ID
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.private.id
}
