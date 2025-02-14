Vagrant.configure("2") do |config|
  # Máquina virtual cluster
  config.vm.define "cluster" do |cluster|
    cluster.vm.box = "debian/bullseye64"
    cluster.vm.hostname = "cluster.sistema.test"
    cluster.vm.network "private_network", ip: "192.168.57.102"

    cluster.vm.provision "shell", inline: <<-SHELL
    sudo apt update && sudo apt install -y nodejs npm

    node -v && npm -v

    mkdir -p /home/vagrant/cluster_app
    cd /home/vagrant/cluster_app

    npm init -y

    npm install express

    sudo apt update && sudo apt install -y nodejs npm

    node -v && npm -v

    mkdir -p /home/vagrant/cluster_app
    cd /home/vagrant/cluster_app

    npm init -y

    npm install express

    echo 'const express = require("express");
    const cluster = require("cluster");
    const os = require("os");

    const totalCPUs = os.cpus().length;
    const port = 3000;

    if (cluster.isMaster) {
        console.log(`Master ${process.pid} está corriendo`);
        for (let i = 0; i < totalCPUs; i++) {
            cluster.fork();
        }
        cluster.on("exit", (worker) => {
            console.log(`Worker ${worker.process.pid} murió, creando otro...`);
            cluster.fork();
        });
    } else {
        const app = express();
        app.get("/", (req, res) => {
            res.send(`Hola desde Worker ${process.pid}`);
        });
        app.listen(port, "0.0.0.0", () => {
            console.log(`Worker ${process.pid} escuchando en puerto ${port}`);
        });
    }' > cluster_app.js

    echo 'module.exports = {
        apps: [{
            name: "cluster_app",
            script: "cluster_app.js",
            instances: 0,
            exec_mode: "cluster"
        }]
    };' > ecosystem.config.js

    npm install -g pm2
    pm2 start ecosystem.config.js
    pm2 save
    pm2 startup systemd

    SHELL
  end
end