# Guide to get your egpu working in linux
This is a very small guide of what I did to setup my egpu.

**My Hardware**

- eGPU Enclosure: [Razer Core X - Thunderbolt™ 3 eGPU](https://www.razer.com/gaming-egpus/razer-core-x)
- eGPU: [ROG Strix GeForce® RTX 2080 SUPER™ OC edition 8GB GDDR6](https://rog.asus.com/graphics-cards/graphics-cards/rog-strix/rog-strix-rtx2080s-o8g-gaming-model/)
- Notebook: [Intel® NUC 12 Pro Kit RNUC12WSHI50001 (has an Intel® Iris® Xe Graphics Chip)](https://www.intel.com/content/www/us/en/products/sku/121626/intel-nuc-12-pro-kit-nuc12wshi5/specifications.html)

**My Software**

- Ubuntu 22.04.2 LTS
- akmod-nvidia and xorg-x11-drv-nvidia-cuda (version 4.4.0)


## PreConditions

- setup the appropriate driver for the gpu's you are using
- make sure your desktop environment is using xorg - https://www.x.org/wiki/ Will probably be true for most distros
- you might want blacklist your old driver, I did it with the nouveau driver

    > some of the things going well in silverblue, add kernel arguments. You need to reboot for this to take effect
    > 
    > sudo rpm-ostree kargs --append=rd.driver.blacklist=nouveau --append=modprobe.blacklist=nouveau --append=nvidia-drm.modeset=1


- ensure thunderbolt authorization works properly
- you should understand basic linux things, for example bash or systemd

## Setup
You only need to put 4 files at the right locations and you are basically done.

- https://raw.githubusercontent.com/phstudy/egpu-setup/master/egpu to /usr/local/bin/egpu
- https://raw.githubusercontent.com/phstudy/egpu-setup/master/egpu.service to /etc/systemd/system/egpu.service
- https://raw.githubusercontent.com/phstudy/egpu-setup/master/xorg.conf.egpu to /etc/X11/xorg.conf.egpu
- https://raw.githubusercontent.com/phstudy/egpu-setup/master/xorg.conf.internal to /etc/X11/xorg.conf.internal

Then run some commands

    # make the file executable

    sudo chmod +x /usr/local/bin/egpu

    # enable egpu.service

    sudo systemctl daemon-reload

    sudo systemctl enable egpu.service

## You are done

If both of your xorg config files are right, you should now be able to switch to your egpu using:

    /usr/local/bin/egpu start

If you are done using it fire a stop command, unplug the egpu only after you have X on your internal screen again

    /usr/local/bin/egpu stop

## Attention

These are my configs, the might not work for you.

- **xorg.conf.internal** should basically contain what you had before you started using an egpu
- **xorg.conf.egpu** file could be useful for you, find out the Device ID with
        
        lspci | grep VGA

        nvidia-smi -L # in case you have nvidia gpus as i do
