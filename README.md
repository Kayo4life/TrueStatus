# TrueStatus
A Vencodr plugin that makes it so you can show trusted people your status even when it appears offline. That's about it.<br> 
Made by 2 amigas. Client side by Kay. Server side by tjc. 
<sup>(Still salty about the server code I made which got rejected. /hj)</sup>

---

### Project Outline
- Explanation of terms
  - TrustedStatuses: A list of users who trust the current user and their true statuses.
  - UserID: The ID of the current user.
  - TrueStatus: The true status of the current user.
  - TrustedUsers: A list of other users that the current user trusts.
  - LastHeartbeat: The timestamp of the last heartbeat received from a user.
  - TimeToNextHeartbeat: The time until the client is expected to send the next heartbeat.
- Client Side
  - TrustedStatuses: User IDs mapped to true statuses to display. Receives new info and updates list on client side.
    - Check if user appears in list
      - Override Discord status
  - UserID
  - TrueStatus
  - TrustedUsers
- Client Sending / Server Receiving
  - Oauth to prevent others from tampering with a user's data.
  - Request to retrieve TrustedStatuses on startup
  - Send heartbeat
    - Sent on startup, every TimeToNextHeartbeat, and when a variable is changed.
    - Formatted as a JSON containing:
      - UserID
      - TrueStatus
      - TrustedUsers
- Server Side
  - Python, Flask
  - Contains large JSON with each user's info sourced from heartbeats
  - Append LastHeartbeat to JSON as a UNIX timestamp
  - Calculate TimeToNextHeartbeat dynamically based on server load and LastHeartbeat
  - Append TimeToNextHeartbeat into the JSON
  - If a user fails to send a heartbeat within 30 seconds of their expected TimeToNextHeartbeat, they are marked offline
- Server Sending / Client Receiving
  - Compile and send a list of TrustedStatuses when the user opens their client
  - Send new updates to TrustedStatuses as status changes to reduce server workload
  - Send updated TimeToNextHeartbeat
