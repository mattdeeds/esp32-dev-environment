# esp32-dev-environment
 Vagrant development environment for ESP32 MCUs using ESP-IDF 

## Requirements

* [VirtualBox 5.x](https://www.virtualbox.org/wiki/Downloads)
* [VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](https://www.vagrantup.com/downloads.html)

## Usage

Clone this repository:

	git clone https://github.com/mattdeeds/esp32-dev-environment.git

Refer to the 'Connecting USB device' section to make sure your esp32 dev board will connect to the virtual machine.

To start the VM, open a terminal in the `esp32-dev-environment` directory with the Vagrantfile and run:

    vagrant up

The setup process will install esp-idf tools for esp32, esp32s2, esp32s3, esp32c3, esp32c2 and add them to the PATH.

It my take a few minutes for the setup process to run.

Then, you can log onto the virtual machine by running:

    vagrant ssh

Once in the virtual machine, you can run these commands to get started with the 'hello_world' example:

    cd esp
    get_idf
    cp -r esp-idf/examples/get-started/hello_world .        
    cd hello_world
    idf.py set-target esp32c3
    idf.py menuconfig
    idf.py build

You can leave the virtual machine by running: 

    logout

You can destroy the virtual machine by running: 

    vagrant destroy

### Connecting a USB device 

You may need to modify the Vagrant file where it specifies which USB device to connect to the virtual machine.
Get the USB device information by running: 

    VBoxManage list usbhost

In the Vagrant file, replace the name, vendorid, and productid with the information of your usb device:

    vb.customize ['usbfilter', 'add', '0', '--target', :id, '--name', 'USB JTAG/serial debug unit', '--vendorid', '0x303a', '--productid', '0x1001']

### Issues
My intent was to use this for programming my esp32c3 dev board over USB without the use of a serial to USB IC. This functionality works well directly on the host machine without the virtual machine in the way. There seems to be some sort of issue with USB timing with esptool when trying to program the board from within the vagrant virtual machine.

## Kudos and inspiration

* pichenettes' [mutable-dev-environment](https://github.com/pichenettes/mutable-dev-environment)
* larryli's [esp-idf-template pull request](https://github.com/espressif/esp-idf-template/pull/10/files)
* Adafruit's [ARM Toolchain for Vagrant](https://github.com/adafruit/ARM-toolchain-vagrant)
* Novation's [LaunchPad pro](https://github.com/dvhdr/launchpad-pro)
