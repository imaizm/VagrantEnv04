# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'dotenv'
Dotenv.load

Vagrant.configure("2") do |config|
  config.vm.box = "dummy"
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

  config.vm.provider :aws do |aws, override|
    # AWS認証情報
    aws.access_key_id     = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
    aws.keypair_name      = ENV['AWS_KEYPAIR_NAME']

    # Amazon Linux AMI 2015.09 (HVM), SSD Volume Type を使用
    # awsコンソール -> インスタンスの作成 -> 「ステップ 1: Amazon マシンイメージ（AMI）」より
    aws.ami = "ami-67725e57"

    aws.instance_type          = "m3.medium"
    aws.instance_ready_timeout = 120
    aws.terminate_on_shutdown  = false
    aws.security_groups = [ ENV['AWS_SECURITY_GROUP'] ]
    aws.region = "us-west-2"
    # aws.availability_zone  = "us-west-2"
    aws.tags = {
      "Name"        => "vagrant-aws",
      "Description" => "vagrant-aws test",
    }

    # VPC
    aws.subnet_id = "subnet-b8a32ef0"
    aws.associate_public_ip = true

    # eipの自動割当
    aws.elastic_ip = false

    # ssh設定
    override.ssh.username = 'centos'
    override.ssh.private_key_path = ENV['AWS_SSH_PRIVATE_KEY_PATH']

    # sudo設定(tty 許可)
    aws.user_data = <<-USER_DATA
        #!/bin/sh
        sed -i -e 's/^\\(Defaults.*requiretty\\)/#\\1/' /etc/sudoers
      USER_DATA

  end

end
