Vagrant.configure("2") do |config|
  # Máquina virtual cluster
  config.vm.define "cluster" do |cluster|
    cluster.vm.box = "debian/bullseye64"
    cluster.vm.hostname = "cluster.sistema.test"
    cluster.vm.network "private_network", ip: "192.168.57.102"
    #cluster.vm.synced_folder "./Cluster", "/home/vagrant/cluster"

    cluster.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install curl
    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
    sudo apt install -y nodejs
    sudo apt install -y npm
    mkdir cluster
    cd cluster
    nano cluster_app.js
    npm init -y
    npm install express
    nano app.js
    #aqui va el codigoconst express = require("express");
    # const app = express();
    # const port = 3000;
    # const limit = 5000000000;

    # app.get("/", (req, res) => {
    #     res.send("Hello World!");
    # });

    # app.get("/api/:n", function (req, res) {
    #     let n = parseInt(req.params.n);
    #     let count = 0;

    #     if (n > limit) n = limit;

    #     for (let i = 0; i <= n; i++) {
    #         count += i;
    #     }

    #     res.send(`Final count is ${count}`);
    # });

    # app.listen(port, () => {
    #     console.log(`App listening on port ${port}`);
    # });
    node app.js
    
#ESTO ES PARA EL PM2 CONFIG
#     module.exports = {
#     apps: [{
#         name: "cluster_app",
#         script: "cluster_app.js",
#         instances: 0,
#         exec_mode: "cluster"
#     }]
# };



    SHELL
  end
end