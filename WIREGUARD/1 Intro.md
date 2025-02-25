Wireguard is a modern VPN control known for simplicity, speed and security.
It is designed to be lightweight and fast, also simple and more efficient than other traditionally VPN's like OpenVPN and IPsec.

#### **1. Why WireGuard?**

- **Performance**: Faster than OpenVPN and IPsec, using modern cryptography.
- **Simplicity**: Uses a small codebase (~4,000 lines vs. hundreds of thousands in OpenVPN/IPsec).
- **Security**: Uses state-of-the-art cryptographic algorithms (ChaCha20, Poly1305, Curve25519, etc.).
- **Cross-platform**: Works on Linux, Windows, macOS, Android, iOS, and even routers.
- **Always-On Connectivity**: Uses a stateless design that allows seamless roaming between networks.

#### **2. How WireGuard Works**

- **Uses Public & Private Keys**: Similar to SSH, each device has a key pair.
- **Peer-to-Peer (No Client-Server Model)**: Any device (peer) can act as a client or server.
- **UDP-Based**: No TCP overhead, making it faster and more efficient.
- **Silent Until Needed**: WireGuard only transmits data when necessary, reducing attack surfaces.

#### **3. WireGuard Architecture**

- **Server (or Gateway)**: A device that allows other peers to connect and route traffic.
- **Clients (or Peers)**: Devices that connect to the server.
- **Configuration File (`wg0.conf`)**: Defines keys, allowed IPs, and routing rules.

#### **4. Typical Use Cases**

- Secure remote access to a home or work network.
- Connecting multiple devices into a private network.
- Bypassing internet restrictions (using a VPN gateway).
- Encrypting internet traffic over untrusted networks (e.g., public Wi-Fi).