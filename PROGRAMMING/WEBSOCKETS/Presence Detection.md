Sometimes in those software (more in the google tools) I can see who is "connected" to that page/spreadsheet... Who is working with that resource at the moment. Also is someone close the page, I can see that he is no longer working on the resource... How do they acheive this (beside websockets ?)

The feature you're describing — where you can see who is currently working on a resource (e.g., a page, spreadsheet, or document) and get updates when someone leaves — is typically implemented using a combination of techniques beyond just WebSockets. While WebSockets can be used for real-time updates, there are other methods and systems at play to manage user presence and activity efficiently.

Here's a breakdown of how such functionality can be achieved:

### 1. **Presence Detection (User Session Tracking)**:

- **Heartbeat or Ping Mechanism**:  
    Even without WebSockets, the server can periodically "ping" each client to confirm they are still active. This is often done through an HTTP request sent at regular intervals (e.g., every 30 seconds). If the client doesn't respond, the server can assume that the user is no longer actively working on the resource and update the status accordingly.
    
    **Implementation**:
    
    - Every client sends a "heartbeat" request to the server at regular intervals to confirm their presence.
    - If the server stops receiving heartbeats from a client (i.e., the client closes the page or loses connection), it marks that user as inactive.
    
    **Example**:
    
    - The client could send a `POST` request every X seconds to the server, like `POST /api/heartbeat`.
    - If the server stops receiving the heartbeat for a set time (e.g., a minute), it can mark the user as "offline" or "inactive."

### 2. **Session Management**:

- **Browser Tab Events**:  
    Browsers provide certain events like `visibilitychange` (when the tab or window visibility changes) and `beforeunload` (when the page is about to be unloaded or closed). These events can help detect when a user closes the page or navigates away.
    
    - `visibilitychange`: You can use this to check if the user has minimized or switched tabs. If the page becomes hidden or inactive, you can stop updating the status.
    - `beforeunload`: This event can be used to detect when the user is leaving the page entirely (e.g., closing the tab or navigating to a different URL).
    
    **Implementation**:
    
    - When the page visibility changes (e.g., the user switches tabs), send an update to the server to mark the user as idle or away.
    - When the page is closed or the user navigates away, send an event to the server (using the `beforeunload` event) to notify that the user is leaving.

### 3. **Collaborative Editing and Presence Management**:

- **Operational Transformation (OT) or Conflict-Free Replicated Data Types (CRDTs)**:  
    In collaborative environments (like Google Docs or Sheets), multiple users are editing the same document simultaneously. To keep track of each user’s changes and presence, OT or CRDTs are used. These algorithms allow multiple clients to make changes to a shared document concurrently without conflicts.
    
    Presence information is typically stored in a centralized data structure, and when a user makes a change or joins the document, their presence is broadcast to all other clients. The system will also keep track of when a user stops interacting with the document.
    
    **Implementation**:
    
    - Every client updates a shared presence list on the server, which contains information about who is currently editing the document and their last active timestamp.
    - When a user starts typing or makes a change, they are marked as "active."
    - If no activity is detected for a certain period (e.g., 1-2 minutes), the user is marked as "inactive."
    - This list of active users is then broadcast to all other connected clients, so everyone knows who else is currently working on the document.

### 4. **Backend Presence Management**:

- **Backend Databases and Cache**:  
    A backend database or cache (e.g., Redis) can store the current state of user presence. This is often done using a combination of session IDs and timestamps to track which users are currently active and which have been idle or disconnected.
    
    **Implementation**:
    
    - A **session store** (like Redis) is used to track the presence of users. Each time a user interacts with the document, an update is made to their session data.
    - If there’s no activity for a certain duration, their session is marked as inactive.
    - This presence information can then be queried and broadcast to all other clients connected to the same resource (document, page, etc.).

### 5. **Real-Time Updates with Polling (Fallback Option)**:

- If WebSockets aren’t being used, you can fall back to **long polling** or **short polling** for detecting changes in real-time.
- The server can periodically send updates to all connected clients, informing them of any changes in user presence (who is currently editing, who is inactive, etc.).

### Summary of the Flow:

1. **Presence Detection**: Use periodic heartbeats or browser events (`visibilitychange`, `beforeunload`) to detect when a user is active or idle.
2. **Session Management**: Track users' sessions and update their status in a centralized database or cache (like Redis).
3. **Real-Time Updates**: Push presence updates to all other users in real time when someone joins or leaves, using WebSockets or polling.
4. **Collaborative Editing**: Use algorithms like OT or CRDT to manage changes in real time while maintaining the presence of active users.

This approach, especially when combined with modern frameworks and tools, allows you to build real-time collaborative features where users can see who's actively working on a resource and get notified when someone leaves.