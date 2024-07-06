Vagrant.configure("2") do |config|
  # Define the "nexus" VM
  config.vm.define "nexus" do |nexus|
    nexus.vm.box = "centos/7" # CentOS 7 box

    # Set the number of vCPUs
    nexus.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--cpus", "2"]
    end

    # Set the amount of memory (in MiB)
    nexus.vm.provider "virtualbox" do |vb|
      vb.memory = 4096 # 4 GiB
    end

    # Configure private network with a specific IP address
    nexus.vm.network "private_network", ip: "192.168.56.41"

    # Forward ports
    nexus.vm.network "forwarded_port", guest: 22, host: 2221, protocol: "tcp", host_ip: "0.0.0.0"
    nexus.vm.network "forwarded_port", guest: 8081, host: 8081, protocol: "tcp", host_ip: "0.0.0.0"

#     # Provisioning script for Nexus
#     nexus.vm.provision "shell", inline: <<-SHELL
#       #!/bin/bash
#       sudo yum update -y
#       sudo yum install java-1.8.0-openjdk.x86_64 wget -y   
#       sudo mkdir -p /opt/nexus
#       sudo mkdir -p /tmp/nexus
#       cd /tmp/nexus/
#       NEXUSURL="https://download.sonatype.com/nexus/3/latest-unix.tar.gz"
#       wget $NEXUSURL -O nexus.tar.gz
#       sudo tar xzvf nexus.tar.gz -C /opt/nexus/
#       sudo rm -rf /tmp/nexus/nexus.tar.gz
#       sudo useradd nexus
#       sudo chown -R nexus:nexus /opt/nexus 
#       sudo tee /etc/systemd/system/nexus.service > /dev/null <<'EOF'
# [Unit]                                                                          
# Description=nexus service                                                       
# After=network.target                                                            
                                                                
# [Service]                                                                       
# Type=forking                                                                    
# LimitNOFILE=65536                                                               
# ExecStart=/opt/nexus/nexus-*/bin/nexus start                                  
# ExecStop=/opt/nexus/nexus-*/bin/nexus stop                                    
# User=nexus                                                                      
# Restart=on-abort                                                                
                                                                
# [Install]                                                                       
# WantedBy=multi-user.target                                                      
# EOF
#       echo 'run_as_user="nexus"' | sudo tee /opt/nexus/nexus-*/bin/nexus.rc > /dev/null
#       sudo systemctl daemon-reload
#       sudo systemctl start nexus
#       sudo systemctl enable nexus
#     SHELL
  end

  # Define the "sonar" VM
  config.vm.define "sonar" do |sonar|
    sonar.vm.box = "ubuntu/focal64" # Ubuntu 20.04 box

    # Set the number of vCPUs
    sonar.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--cpus", "2"]
    end

    # Set the amount of memory (in MiB)
    sonar.vm.provider "virtualbox" do |vb|
      vb.memory = 4096 # 4 GiB
    end

    # Configure private network with a specific IP address
    sonar.vm.network "private_network", ip: "192.168.56.42"

    # Forward ports
    sonar.vm.network "forwarded_port", guest: 22, host: 2223, protocol: "tcp", host_ip: "0.0.0.0"
    sonar.vm.network "forwarded_port", guest: 80, host: 8082, protocol: "tcp", host_ip: "0.0.0.0" # Port 80 forwarding added

    # # Provisioning script for SonarQube
    # sonar.vm.provision "shell", inline: <<-SHELL
    #   sudo apt-get update -y
    #   sonar.projectKey=vprofile
    #   sonar.projectName=vprofile-repo
    #   sonar.projectVersion=1.0
    #   sonar.sources=src/
    #   sonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/
    #   sonar.junit.reportsPath=target/surefire-reports/
    #   sonar.jacoco.reportsPath=target/jacoco.exec
    #   sonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
    # SHELL
  end

  # Define the "jenkins" VM
  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.box = "ubuntu/focal64"
    jenkins.vm.network "private_network", ip: "192.168.56.14"

    jenkins.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2
    end

    # Forward port 8080 for Jenkins
    jenkins.vm.network "forwarded_port", guest: 8080, host: 8080, protocol: "tcp", host_ip: "0.0.0.0"

    # # Provisioning script for Jenkins
    # jenkins.vm.provision "shell", inline: <<-SHELL
    #   #!/bin/bash
      
    #   # Update package index
    #   sudo apt update

    #   # Install Java Development Kit 11
    #   sudo apt install openjdk-11-jdk -y

    #   # Install Maven, Wget, and Unzip (optional)
    #   sudo apt install maven wget unzip -y

    #   # Add Jenkins GPG key to trusted keys
    #   curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo gpg --dearmor -o /usr/share/keyrings/jenkins-archive-keyring.gpg

    #   # Add Jenkins repository to apt sources list
    #   echo 'deb [signed-by=/usr/share/keyrings/jenkins-archive-keyring.gpg] https://pkg.jenkins.io/debian-stable binary/' | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

    #   # Update package index again to include Jenkins repository
    #   sudo apt update

    #   # Install Jenkins
    #   sudo apt install jenkins -y
    # SHELL
  end
end
