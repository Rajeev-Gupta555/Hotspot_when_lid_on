# Hotspot_when_lid_on
Turns on wifi hotspot in Ubuntu, every time the lid opens.

Create a new file to handle the ACPI event. Open a terminal and run the following command to create and edit the file using a text editor:
```sh
sudo nano /etc/acpi/events/lid-open
```

In the text editor, add the following content to the lid-open file:
```sh
event=button/lid.*
action=/etc/acpi/lid-open.sh
```
Create a new script to be executed when the lid is opened. Run the following command to create and edit the script:
```sh
sudo nano /etc/acpi/lid-open.sh
```
In the text editor, add the desired commands or actions that you want to run when the lid is opened. For example:
```sh
#!/bin/bash
# Display a message when the lid is opened
DISPLAY=:0 DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus notify-send "Lid Opened" "The lid of your laptop has been opened."

sleep 10
# Check if Wi-Fi is already enabled
if [ "$(nmcli radio wifi)" == "enabled" ]; then
    echo "Wi-Fi is already enabled."
else
    # Enable Wi-Fi
    nmcli radio wifi on
    echo "Wi-Fi enabled."
fi
sleep 5
nmcli device wifi hotspot
```

Make the script executable by running the following command:
```sh
sudo chmod +x /etc/acpi/lid-open.sh
```
Restart the ACPI service to apply the changes:
```sh
sudo systemctl restart acpid
```
