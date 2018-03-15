# -*- mode: ruby -*-
# vi: set ft=ruby :

app_servers = {
    :slave2 => 'en0: Wi-Fi (AirPort)',
    :slave3 => 'en0: Wi-Fi (AirPort)'
}

Vagrant.configure("2") do |config|
    config.vm.box = "centos7"

    config.vm.define :jenkins_master do |jenkins_master_config|
        jenkins_master_config.vm.network :public_network, bridge: "en0: Wi-Fi (AirPort)"
        config.vm.network :forwarded_port, guest: 8080, host: 8090
        config.vm.synced_folder "./app1_home", "/vagrant_data"
        config.vm.provision "shell", inline: <<-SHELL
            sudo yum install wget curl -y
            sudo service firewalld stop
            sudo chkconfig firewalld off
            wget http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-7/v7.0.85/bin/apache-tomcat-7.0.85.tar.gz
            sudo tar -xvf apache-tomcat-7.0.85.tar.gz -C /vagrant_data/
            echo "export JAVA_HOME=/vagrant_data/jdk1.8.0_161" >> /home/vagrant/.bashrc
            echo "export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar" >> /home/vagrant/.bashrc
            echo "export PATH=$JAVA_HOME/bin:$PATH" >> /home/vagrant/.bashrc
            sudo wget -P /vagrant_data/apache-tomcat-7.0.85/webapps http://mirrors.jenkins-ci.org/war/latest/jenkins.war
            yum install git -y
            echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC1Na5icG6aB/9PahR8H9zX8Y6ogUqWAcp9r5vMCY8MDQ9boz9d/EWAEgs+mLyX97NQ2CgOnRtgYWqSfiyDwS8xfk04m4s+YlK9VYrb5yXu7oe3jH1J8CkE+PaOw3WKpHUxItCKbMDiZ6fC73HwZV5aaDyskoWVejp9qJiAtb+ROSrZI6T5HbTuDMsIrdOT7ix23j60DRTxqQk/4neGIjl7Ys2SsUZxS0Lrsirs3uQHA+DfnX3MSkn91gW8PvVSnp/FF4SqxNQdCzFe8ZAWKkTJtEkFTgtJK/GbJTrzpkUCBf04KeqrGLvMxe9Rr1mz3CE5aLUBp914hjVg88Hrpmv7 StephenChou@StephendeAir.lan" >> /home/vagrant/.ssh/authorized_keys
         SHELL
        config.vm.provider :virtualbox do |vb|
            vb.name = "jenkins_master"
            vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
            vb.customize ["modifyvm", :id, "--memory", "1024"]
            vb.customize ["modifyvm", :id, "--cpus", "2"]
            vb.cpus = 2
        end
    end

    app_servers.each do |app_server_name, app_server_ip|
        config.vm.synced_folder "./app2_home", "/vagrant_data"
        config.vm.provision "shell", inline: <<-SHELL
            sudo yum install wget curl -y
            sudo service firewalld stop
            sudo chkconfig firewalld off
            echo "export JAVA_HOME=/vagrant_data/jdk1.8.0_161" >> /home/vagrant/.bashrc
            echo "export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar" >> /home/vagrant/.bashrc
            echo "export PATH=$JAVA_HOME/bin:$PATH" >> /home/vagrant/.bashrc
            yum install git -y
            echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC1Na5icG6aB/9PahR8H9zX8Y6ogUqWAcp9r5vMCY8MDQ9boz9d/EWAEgs+mLyX97NQ2CgOnRtgYWqSfiyDwS8xfk04m4s+YlK9VYrb5yXu7oe3jH1J8CkE+PaOw3WKpHUxItCKbMDiZ6fC73HwZV5aaDyskoWVejp9qJiAtb+ROSrZI6T5HbTuDMsIrdOT7ix23j60DRTxqQk/4neGIjl7Ys2SsUZxS0Lrsirs3uQHA+DfnX3MSkn91gW8PvVSnp/FF4SqxNQdCzFe8ZAWKkTJtEkFTgtJK/GbJTrzpkUCBf04KeqrGLvMxe9Rr1mz3CE5aLUBp914hjVg88Hrpmv7 StephenChou@StephendeAir.lan" >> /home/vagrant/.ssh/authorized_keys
         SHELL
        config.vm.define app_server_name do |app_config|
            app_config.vm.hostname = "#{app_server_name.to_s}.vagrant.internal"
            app_config.vm.network :public_network, bridge: app_server_ip
            app_config.vm.provider "virtualbox" do |vb|
                vb.name = app_server_name.to_s
            end
        end
    end
end