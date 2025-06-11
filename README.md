# Sharing devices with WSL

1.  **Open PowerShell with Administrative privileges.**
2.  **List Available Devices:**
    
    Execute the following command to see all available devices that Windows can recognize attached to it (not only USB devices are listed!):
    
    ```
    usbipd list
    ```
    
3.  **Identify the USB Port:**
    
    In the output, search for anything that contains a `COM<any_number>`. That will be the USB port to be shared. The output should look something like this:
    
    ```
    
    Connected:
    BUSID  VID:PID    DEVICE                                                        STATE
    2-1    1a86:7523  USB-SERIAL CH340 (COM3)                                       Not shared
    2-3    413c:2107  USB Input Device                                              Not shared
    2-4    046d:c24a  G600, USB Input Device, Virtual HID Framework (VHF) HID d...  Not shared
    2-10   06cb:00c7  Unknown device                                                Not shared
    2-14   8087:0029  Intel(R) Wireless Bluetooth(R)                                Not shared
                
    ```
    
4.  **Bind the Device:**
    
    The `BUSID` should be taken as the device you would like to share within the WSL. Run the following command:
    
    ```
    usbipd bind --busid <busid>
    ```

    The resulting output of the previous command then would be similar to the following (in case of BUSID = 2-1):

     ```
    
    Connected:
    BUSID  VID:PID    DEVICE                                                        STATE
    2-1    1a86:7523  USB-SERIAL CH340 (COM3)                                       Shared
    2-3    413c:2107  USB Input Device                                              Not shared
    2-4    046d:c24a  G600, USB Input Device, Virtual HID Framework (VHF) HID d...  Not shared
    2-10   06cb:00c7  Unknown device                                                Not shared
    2-14   8087:0029  Intel(R) Wireless Bluetooth(R)                                Not shared
                
    ```
    
6.  **Attach the Device to WSL:**
    
    It's time to definitely attach the device to the WSL! Ensure to keep a WSL window open to keep the VM active. Execute the following command:
    
    ```
    usbipd attach --wsl --busid <busid>
    ```
    
    **NOTE:** Once the device is attached to the WSL, it won't be visible anymore to Windows, but only to any distribution running WSL2.
    
7.  **Verify the Device Attachment:**
    
    Verify that the device is attached by executing:
    
    ```
    usbipd list
    ```
    
    From the WSL window, type:
    
    ```
    lsusb
    ```
    
    to verify that the USB device is listed and can be interacted with using Linux tools.
    
8.  **Detach the Device:**
    
    Once done using the device, execute the following command via PowerShell:
    
    ```
    usbipd detach --busid <busid>
    ```
