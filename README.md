# Fleek

### 1. **Prepare Your Environment**
   - **System Requirements**: Ensure your server meets the basic requirements. It's recommended to use a Linux-based system (preferably Ubuntu).
   - **Install Dependencies**: Start by installing necessary packages:
     ```bash
     sudo apt-get update
     sudo apt-get install build-essential clang pkg-config libssl-dev gcc-multilib protobuf-compiler
     ```

### 2. **Install Rust**
   - **Rust Installation**: Fleek Network is built using Rust, so you need to install Rust:
     ```bash
     curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
     source $HOME/.cargo/env
     rustup update
     ```

### 3. **Clone the Fleek Network Repository**
   - **Clone the Source Code**: Download the latest version of the Fleek Network node software from GitHub:
     ```bash
     git clone -b testnet-alpha-1 https://github.com/fleek-network/lightning.git ~/fleek-network/lightning
     cd ~/fleek-network/lightning
     ```

### 4. **Build the Node**
   - **Compile the Node**: Navigate to the project directory and build the node using Rust’s package manager:
     ```bash
     cargo clean
     cargo update
     cargo +stable install --locked --path core/cli --features services
     ```
   - **Link the Executable**: To make the node command globally accessible, create a symbolic link:
     ```bash
     sudo ln -s "$HOME/.cargo/bin/lightning-node" /usr/local/bin/lgtn
     ```

### 5. **Configure and Run the Node**
   - **Node Configuration**: Before starting the node, you might need to configure it using the `lightning.toml` file. The default configuration should suffice for most cases, but adjustments can be made based on your setup.
   - **Start the Node**: Run the node using the following command:
     ```bash
     lgtn run
     ```
   - **Opt-In to Network Participation**: To participate in the network, you must opt-in:
     ```bash
     lgtn opt in
     ```

### 6. **Monitor and Maintain the Node**
   - **Systemd Service**: It's advisable to manage your node using `systemd` to ensure it restarts automatically in case of failure:
     ```bash
     [Unit]
     Description=Fleek Network Node
     After=network.target

     [Service]
     Type=simple
     User=your_username
     ExecStart=/usr/local/bin/lgtn run
     Restart=always
     RestartSec=3

     [Install]
     WantedBy=multi-user.target
     ```
     After saving the service file, enable and start the service:
     ```bash
     sudo systemctl enable fleek-node
     sudo systemctl start fleek-node
     ```

### 7. **Keep the Node Updated**
   - **Regular Updates**: Since Fleek Network is in active development, frequently pull the latest changes from the GitHub repository and rebuild the node to ensure you're running the latest version:
     ```bash
     git pull origin testnet-alpha-1
     cargo clean
     cargo update
     cargo +stable install --locked --path core/cli --features services
     ```

This setup allows you to successfully run a validator node on the Fleek Network. Make sure to keep an eye on the official [Fleek Network documentation](https://docs.fleek.network) for any updates or changes in the process.
