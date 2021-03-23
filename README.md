# x728-ups-auto-shutdown

The ups.py script allow the [Raspberry Pi X728 UPS](https://geekworm.com/products/raspberry-pi-x728-max-5-1v-8a-18650-ups-power-management-board)
X728 V1.3 UPS hat to auto shutdown (ASD) your raspbery pi.

This project can provide these features:
- Listen to the change of power supply event (Power supply lost or Power supply recovered)
- Detect power supply failure also when the PI starts
- Keep checking the power left every 60 seconds (configurable) ONLY when power supply lost and shutdown eventually when the batteries have 10% power left (configurable).
- Stop power left checking loop gracefully after the power supply recovers
- Put the log to /var/log/syslog

## Setup
1. Install Python3, enable RTC and ic2 accoeding to [official guidelines](https://github.com/geekworm-com/x728/blob/master/x728.sh)

```
# Install Python 3 and dependencies
sudo apt install python3-pip
sudo apt-get -y install python3-rpi.gpio
sudo pip install smbus

#X728 RTC setting up
sudo sed -i '$ i rtc-ds1307' /etc/modules
sudo sed -i '$ i echo ds1307 0x68 > /sys/class/i2c-adapter/i2c-1/new_device' /etc/rc.local
sudo sed -i '$ i hwclock -s' /etc/rc.local
sudo sed -i '$ i #Start power management on boot' /etc/rc.local
```
2. Put the file to somewhere. e.g. /opt/ups.py
3. Set file permission
```
sudo chmod +x /opt/ups.py
```
4. Run the script on boot by crontab
```
# Must be run with root user, since shutdown commands will be called
sudo su
crontab - e
# Add the following line to the crontab
@reboot python3 /opt/ups.py
```
5. Reboot your PI

You can check the log in /var/log/syslog file for app started, app error, power supply event change, etc.

You can test the program by keeping plugin and eject the power supply cable connected to the UPS, and you can find how robust the program is ~.

This is based on the sample script in [official guidelines](https://github.com/geekworm-com/x728/blob/master/x728.sh)
