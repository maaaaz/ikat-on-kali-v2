iKAT on Kali v2
================
Like me, you may have observed that the `ikat` package related to the popular kiosk attack framework [iKAT](http://ikat.ha.cked.net/) **isn't currently available on Kali v2**: [a ticket has been opened on the bug tracker](https://bugs.kali.org/view.php?id=3103) and I hope it will be fixed soon.

As it was available on Kali v1, I propose you to follow this tutorial to make it alive again.

1. Add the [old Kali v1 repository](http://docs.kali.org/general-use/kali-linux-sources-list-repositories) `deb http://old.kali.org/kali moto main non-free contrib` entry to the `/etc/apt/sources.list`. You can also manually download and install the [.deb package here](http://old.kali.org/kali/pool/non-free/i/ikat/) if you do want to edit your `sources.list` file

2. `$ apt-get update && apt-get install ikat`  

3. Remove the old Kali v1 repository from your `/etc/apt/sources.list` as it will somehow break some stuff

4. `$ cp /usr/lib/libBLT.2.5.so.8.6 /usr/lib/libBLT.2.4.so.8.5`. You mileage may vary for `libBLT.2.5.so.8.6`, the point is anyway to have a `libBLT.2.4.so.8.5` file.  

5. Create a `msfcli` file in the following folder `/usr/share/metasploit-framework/`  
    5.1 Copy and paste the following ruby snippet into `msfcli`:
    ```
    #!/usr/share/metasploit-framework/ruby
    # -*- coding: binary -*-

    args_to_pass = ["use #{ARGV[0]}", 
                    "set #{ARGV[1].split("=").join(" ")}",
                    "set #{ARGV[2].split("=").join(" ")}",
                    "set #{ARGV[3].split("=").join(" ")}",
                    "set #{ARGV[4].split("=").join(" ")}",
                    "run",
                    "quit"]
    command = "/usr/share/metasploit-framework/msfconsole -q -x \"#{args_to_pass.join(";")}\""
    puts "    [+]  #{command}"
    system(command)

    ```
    5.2 Grant the execute right to `msfcli`: `$ chmod +x /usr/share/metasploit-framework/msfcli`  

6. Start ikat: `$ ikat`

7. Select the desired interface and port. Selecting something different that `127.0.0.1` for the interface and `8080` lead to a re-generation of the browser exploits, and make use of the ruby `msfcli` script created earlier. Note that generating the exploits may take 5 minutes.

8. Visit `http://interface_adress:port/Windows/index.html` or `http://interface_adress:port/Linux/index.html` with your browser

9. Profit