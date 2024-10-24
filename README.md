# MAC Address Spoofing with systemd

This repository provides a simple setup for spoofing the MAC address of a network interface on Linux systems using `systemd` services. It automates the process of changing the MAC address at boot or when a network interface is brought up.

## Features

- **Automated MAC Address Spoofing:** Automatically changes the MAC address of a specified network interface at boot or whenever the interface is activated.
- **Customizable MAC Addresses:** Define your own static or random MAC address for each interface.
- **systemd Integration:** Uses `systemd` for smooth integration with system services and easy management.
- **Lightweight and Efficient:** Minimal resource consumption, ideal for devices with limited resources, such as Raspberry Pi or other Linux-based systems.

## Requirements

- Linux system with `systemd` (most modern Linux distributions).
- Root or sudo privileges to configure network interfaces.

## Usage

### Setup

1. Clone the repository to your local machine:
   ```bash
   git clone https://github.com/OddRefrigerator/MAC-adress-Spoofing-systemd.git
   cd MAC-adress-Spoofing-systemd
   ```

2. Customize the `spoof-mac@.service` file to specify the desired network interface and MAC address:

   - To spoof the MAC address of `eth0` with a random address:
     ```bash
     sudo systemctl enable spoof-mac@eth0.service
     ```

   - To spoof the MAC address of `wlan0` with a specific address (e.g., `02:00:00:00:00:01`), edit the corresponding configuration file and enable the service:
     ```bash
     sudo systemctl enable spoof-mac@wlan0.service
     ```

3. If you want to specify a static MAC address, you can modify the service file and set the desired address. Open `spoof-mac@.service` and add the following under `[Service]`:

   ```bash
   ExecStart=/usr/bin/ip link set dev %i address 02:00:00:00:00:01
   ```

4. Reload systemd to apply the changes:
   ```bash
   sudo systemctl daemon-reload
   ```

### Starting the Service

To start the MAC address spoofing service manually, run the following command for the specific interface:

```bash
sudo systemctl start spoof-mac@eth0.service
```

To verify that the MAC address has been changed:

```bash
ip link show eth0
```

### Random MAC Address Generation

By default, the service can be set to generate a random MAC address every time it is triggered. You can use the following script within the service:

```bash
ExecStart=/usr/bin/macchanger -r %i
```

This will generate a new random MAC address every time the interface is brought up.

## Example Configuration

Here’s an example of enabling MAC address spoofing on a wireless interface (`wlan0`), using a random MAC address:

1. Enable the service:
   ```bash
   sudo systemctl enable spoof-mac@wlan0.service
   ```

2. Start the service:
   ```bash
   sudo systemctl start spoof-mac@wlan0.service
   ```

3. Check the status:
   ```bash
   systemctl status spoof-mac@wlan0.service
   ```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! If you'd like to contribute, please fork the repository and create a pull request with a description of your changes.

### Steps for Contribution

1. Fork the repository.
2. Create a new branch:
   ```bash
   git checkout -b feature-name
   ```
3. Commit your changes:
   ```bash
   git commit -m "Description of feature"
   ```
4. Push the branch:
   ```bash
   git push origin feature-name
   ```
5. Open a pull request with a description of the changes.

## Author

This project was created by [OddRefrigerator](https://github.com/OddRefrigerator).

## Contact

For any questions or issues, feel free to open an issue on GitHub or contact the repository owner.
