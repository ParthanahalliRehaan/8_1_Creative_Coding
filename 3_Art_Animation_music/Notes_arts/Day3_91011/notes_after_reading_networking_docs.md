## 🌐 Godot Multiplayer Networking Notes

### 1. High-Level Multiplayer API
- Godot provides a **High-Level Multiplayer API** built on top of the low-level networking system.
- It uses **Remote Procedure Calls (RPCs)** to synchronize nodes and functions across clients and servers.
- Key classes:  
  - `SceneTree` → manages multiplayer state.  
  - `MultiplayerAPI` → handles connections, peers, and RPCs.

---

### 2. Server & Client Setup
- One instance acts as the **server/host**.  
- Other instances connect as **clients**.  
- Use `create_server(port)` and `create_client(ip, port)` to establish connections.

---

### 3. Port Forwarding
- To allow external players to connect, you must **forward the UDP port** your game uses.  
- Without port forwarding, only devices on the same local network can connect.  
- Configure this in your router settings:
  - Forward the chosen **UDP port** (e.g., 7777).  
  - Ensure firewall rules allow traffic on that port.

---

### 4. RPC & Synchronization
- **`rpc()`** → calls a function on remote peers.  
- **`rpc_id(peer_id, "function")`** → calls on a specific peer.  
- **`remotesync` keyword** → ensures state consistency across server and clients.  
- Example:
  ```gdscript
  @rpc("any_peer", "reliable")
  func spawn_player(position):
      # Spawns player at given position
  ```

---

### 5. Common Practices
- Decide which node is **authority** (usually the server).  
- Keep sensitive logic (e.g., score validation, anti-cheat) on the server.  
- Use **reliable RPCs** for critical data (like health, inventory).  
- Use **unreliable RPCs** for fast updates (like player movement).

---

### 6. Debugging & Testing
- Test locally with multiple instances of the game.  
- Use different ports for multiple servers.  
- Check router/firewall settings if external connections fail.

---

👉 In short: **Godot’s high-level multiplayer makes RPCs easy, but you must forward the UDP port on your router so external players can join your server.**
