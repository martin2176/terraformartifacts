terraform {
  required_providers {
    apigee = {
      source  = "scastria/apigee"
      version = "~> 0.1.0"
    }
  }
}


#Configure the Apigee Provider
provider "apigee" {
  username = "martin2176@yahoo.com"
  password = "abcD1234%"
  //  access_token = "Use access token instead of username/password"
  //  oauth_server = "Treat username/password as machine user and obtain access token automatically"
  organization = "martin2176-eval"
}

#Add api proxy to api product
resource "apigee_product" "weatherapis" {
  name               = "weatherapis"
  display_name       = "weatherapis"
  description        = "weatherapis"
  auto_approval_type = true
  proxies            = ["weather"]
  environments = [
    "prod"
  ]
  attributes = {
    access = "Internal only"
  }
  quota           = 20
  quota_interval  = 1
  quota_time_unit = "minute"
}

#Add api developer app and add weather api product to the app
resource "apigee_developer_app" "weatherapp" {
  name            = "weatherapp"
  developer_email = "martin2176@gmail.com"
}

resource "apigee_developer_app_credential" "weatherappcredential" {
  developer_email    = "martin2176@gmail.com"
  developer_app_name = apigee_developer_app.weatherapp.name
  consumer_key       = "gLMcKjtHsg2cLRNGZNFv9zhz45ymeseg"
  consumer_secret    = "45ymeseg"
  api_products = [
    apigee_product.weatherapis.name
  ]
}
resource "apigee_proxy" "weather" {
  name        = "weather"
  bundle      = "weather_rev6_2021_12_15.zip"
  bundle_hash = filebase64sha256("weather_rev6_2021_12_15.zip")
}
resource "apigee_proxy_deployment" "weather" {
  proxy_name       = apigee_proxy.weather.name
  environment_name = "prod"
  revision         = apigee_proxy.weather.revision
}
