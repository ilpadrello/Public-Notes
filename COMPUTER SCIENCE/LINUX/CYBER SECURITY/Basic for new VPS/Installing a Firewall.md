## Why ?

- You specify which ports and services are accessible from the outside world.
- Reduce the "attack surface" of the machine
- Can block unauthorized attempts to connect to your server.
- You can filter based on the Ip adress, ports and protocols.
- Can help with DDOS attacks
- Log and monitor Network activity

## Which one ? 

Both `iptables` and `ufw` are powerful firewall tools, but they differ significantly in their ease of use and complexity. Here's a breakdown to help you decide:

**1. iptables:**

- **Pros:**
    - Extremely powerful and flexible.
    - Offers fine-grained control over network traffic.
    - The underlying firewall framework on most Linux systems.
- **Cons:**
    - Complex and difficult to learn, especially for beginners.
    - Rules are not persistent by default; you need to save them manually.
    - The syntax can be very cryptic.
- **Use Cases:**
    - Advanced users who need highly customized firewall configurations.
    - System administrators who require maximum control.
    - Situations where `ufw` is not available or suitable.

**2. ufw (Uncomplicated Firewall):**

- **Pros:**
    - User-friendly and easy to learn.
    - Simplified syntax for common firewall tasks.
    - Provides a more intuitive interface to `iptables`.
    - Rules are persistent by default.
- **Cons:**
    - Less flexible than `iptables` for very complex configurations.
    - It is a front end for iptables, so it is still using iptables under the hood.
- **Use Cases:**
    - Beginners and intermediate users.
    - Most common server setups.
    - Situations where simplicity and ease of use are prioritized.

**Recommendation:**

- For most users, especially those new to server administration, **`ufw` is highly recommended.** It provides a good balance of security and ease of use.
- `ufw` makes common firewall tasks, such as allowing SSH, HTTP, and HTTPS traffic, very straightforward.
- If you find that `ufw` doesn't meet your needs, you can always delve into `iptables` later.

**In summary:**

- Start with `ufw`.
- If you need advanced configuration, then use iptables.

## Start with ufw (DO NOT LOCK OUT YOURSELF)

**Install `ufw` (If Not Already Installed):**

- Although `ufw` is often pre-installed on Debian, you can ensure it's installed with:
    
    Bash
    
    ```
    sudo apt update
    sudo apt install ufw
    ```
    

**2. Allow SSH (Your Custom Port):**

- **Crucial:** Before enabling `ufw`, allow SSH traffic on the port you configured (if you changed the default). Replace `your_ssh_port` with your actual SSH port number.
    
    Bash
    
    ```
    sudo ufw allow your_ssh_port/tcp
    ```
    

**3. Allow Other Necessary Ports:**

- Allow other ports required by the services running on your VPS. Here are some common examples:
    - **HTTP (Port 80):**
        
        Bash
        
        ```
        sudo ufw allow 80/tcp
        ```
        
    - **HTTPS (Port 443):**
        
        Bash
        
        ```
        sudo ufw allow 443/tcp
        ```
        
    - **Other services:** If you're running other services (e.g., mail server, database), allow their respective ports.
        

**4. Enable `ufw`:**

- Enable the firewall:
    
    Bash
    
    ```
    sudo ufw enable
    ```
    
- You'll be prompted to confirm. Type `y` and press Enter.
    

**5. Check `ufw` Status:**

- Verify that `ufw` is active and your rules are in place:
    
    Bash
    
    ```
    sudo ufw status verbose
    ```
    
- This will display the current firewall rules.
    

**6. Default Deny (Optional but Recommended):**

- For maximum security, set the default policy to deny all incoming and outgoing traffic, and then allow only the necessary ports.
    
    Bash
    
    ```
    sudo ufw default deny incoming
    sudo ufw default deny outgoing
    ```
    
- If you use these commands, you will have to allow all outgoing connections that you need.
    

**7. Delete Rules (If Necessary):**

- To delete a rule, you can use the `delete` command:
    
    Bash
    
    ```
    sudo ufw delete allow 80/tcp
    ```
    
- You can also delete rules by number:
    
    Bash
    
    ```
    sudo ufw status numbered
    sudo ufw delete number
    ```
    

**Important Notes:**

- **Order of Rules:** The order of rules is important. `ufw` processes rules from top to bottom.
- **Logging:** You can enable logging with `sudo ufw logging on`.
- **Application Profiles:** `ufw` has application profiles for common services. You can see them with `sudo ufw app list` and allow them with `sudo ufw allow "Apache Full"`.
- **Testing:** After making changes, thoroughly test your services to ensure they're still working.
- **Lockout:** If you make a mistake, you might lock yourself out of your server. If you are using a cloud provider, use the cloud console to access the server.
- **IPv6:** If you have IPv6 enabled, you need to configure ufw for IPv6 as well
  
## Deny Outgoing... Risky manouver 

`sudo ufw default deny outgoing` can indeed lead to unexpected issues if you're not careful. Let's break down the situation and how to proceed safely:

**Understanding Your Current `ufw` Status:**

- **`Default: deny (incoming), allow (outgoing), disabled (routed)`:**
    - This means that by default, all incoming connections are blocked unless explicitly allowed (which you've done for SSH, HTTP, and HTTPS).
    - Outgoing connections are currently allowed by default. This is generally safe for most basic server operations.
    - routed connections are disabled, this is usually fine.
- **Your Allowed Ports:**
    - You've correctly allowed incoming traffic on ports 49656 (SSH), 80 (HTTP), and 443 (HTTPS), both for IPv4 and IPv6.

**Why `sudo ufw default deny outgoing` Is Risky:**

- **Potential Lockout:**
    - If you deny all outgoing traffic, your server might lose the ability to perform essential tasks like:
        - DNS resolution (to resolve domain names).
        - NTP (to synchronize time).
        - Updates (to download software packages).
        - Any outgoing connections that the services running on your server requires.
    - This could lead to service disruptions or even make your server inaccessible.
- **Troubleshooting Difficulty:**
    - Debugging network issues becomes much harder when outgoing traffic is blocked.

**How to Proceed Safely:**

1. **Understand Your Outgoing Needs:**
    
    - Before denying all outgoing traffic, carefully consider what services your server needs to access externally.
    - Common examples:
        - Package repositories (e.g., `apt.debian.org`).
        - DNS servers (e.g., 8.8.8.8, 1.1.1.1).
        - NTP servers (e.g., `pool.ntp.org`).
2. **Allow Necessary Outgoing Ports/IPs:**
    
    - Instead of a blanket `deny outgoing`, allow only the specific outgoing connections your server requires.
    - Example for DNS:
        - `sudo ufw allow out 53`
    - Example for NTP:
        - `sudo ufw allow out 123`
    - Example for outgoing http/https connections.
        - `sudo ufw allow out 80`
        - `sudo ufw allow out 443`
3. **Test Thoroughly:**
    
    - After allowing outgoing ports, test your server's functionality.
    - Try:
        - Updating your system (`sudo apt update`).
        - Pinging external hosts (`ping google.com`).
        - Accessing websites from your server (`curl google.com`).
4. **Gradual Implementation:**
    
    - If you're unsure about your outgoing needs, start by allowing only the most essential ports.
    - Then, gradually add more rules as needed, testing thoroughly after each change.
5. **Use the cloud console:**
    
    - If you do lock yourself out, remember that the cloud console is your friend.

**Recommendation:**

- It is recommended to only allow outgoing traffic that is absolutely necessary.
- If you are unsure of what outgoing traffic is needed, then it is best to leave the default outgoing policy as allow.

By following these steps, you can enhance your server's security without risking a lockout.

## Checking logs : 

First enable logs : 

```
sudo ufw logging on
```

Adjust log levels `sudo ufw logging medium` or `sudo ufw logging high` if you need more detailed information. But remember that higher logging levels generate more log data.

Use `journalctl` for logs : 

```
sudo journalctl -u ufw
sudo journalctl -fu ufw # realtime
``` 

