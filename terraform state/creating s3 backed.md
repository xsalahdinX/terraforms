
terraform {
  backend "s3" {
    encrypt = true   
    bucket = "salahdin-bucket"
    dynamodb_table = "terraform-state-dynamo"
    key    = "terraform.tfstate"
    region = "eu-west-3"
  }
}
