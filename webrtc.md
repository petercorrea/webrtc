# WebRTC API Cheat Sheet

WebRTC (Web Real-Time Communication) is a set of standards that enable web applications to support audio and video conferencing, file exchange, screen sharing, identity management, and interfacing with legacy telephone systems including support for sending DTMF (touch-tone dialing) signals, as well as to exchange arbitrary data between browsers without requiring an intermediary or plugins.

# Key Concepts

- Peer-to-Peer Communication: Direct connection between browsers for the exchange of data, audio, and video without the need for an intermediary server (though signaling servers are used for coordinating connections).
- Signaling: The process of coordinating communication between peers, including the exchange of information on how to connect (e.g., SDP messages) and network/firewall configurations (e.g., ICE candidates). Not part of WebRTC API but essential for establishing connections.
- NAT Traversal: Techniques facilitated by ICE, STUN, and TURN to overcome network address translation (NAT) restrictions, allowing peers to find and establish a connection with one another across the internet.

# Flow of Information

- Signaling: Peers use a signaling server (not provided by WebRTC) to exchange offer and answer messages (SDP) and ICE candidates to discover the best path for connecting.
- Connection Establishment: After exchanging signaling data, peers establish a direct connection using the ICE framework.
- Media Exchange: Peers can then exchange audio, video, or arbitrary binary data directly.

# Main Objects/Interfaces

**RTCPeerConnection**: The main interface managing the connection between local and remote peers. It handles the setup, management, and teardown of the direct connection.

- createOffer(): Initiates the creation of an SDP offer for the purpose of starting a new connection.
- createAnswer(): Creates an SDP answer in response to an offer received from a remote peer.
- setLocalDescription(): Sets the local description (offer or answer SDP).
- setRemoteDescription(): Sets the remote description (offer or answer SDP).
- addIceCandidate(): Adds a new ICE candidate to the remote peer's description.

**RTCSessionDescription**: Represents the parameters of a session. Used in signaling to exchange session information (SDP format).

**RTCIceCandidate**: Represents a candidate endpoint for WebRTC communication, used during the ICE negotiation process.

**MediaStream**: Represents a stream of media content. A stream consists of several tracks such as video or audio tracks.

- getTracks(): Returns a list of all tracks in the stream.
- addTrack(): Adds a track to the stream.
- removeTrack(): Removes a track from the stream.
**RTCTrackEvent**: Represents an event that carries a track, such as when a new track is added to a connection.
**RTCDataChannel**: Enables bidirectional data exchange between peers. Useful for text chat, file transfer, and gaming applications.
- send(): Sends data through the data channel.
- close(): Closes the data channel.

# Example Flow

Creating a Peer Connection:

    ```
    const pc = new RTCPeerConnection(iceServers);
    ```

Setting up Media Streams (assuming local media streams have been gathered):

    ```
    localStream.getTracks().forEach(track => pc.addTrack(track, localStream));
    ```
Creating an Offer:

    ```
    pc.createOffer().then(offer => pc.setLocalDescription(offer));
    ```

Exchanging Offer/Answer via Signaling Server: Not covered by WebRTC API; this involves sending the offer to the remote peer and receiving an answer, then:

    ```
    pc.setRemoteDescription(remoteDescription);
    ```

Handling ICE Candidates:

    ```
    pc.onicecandidate = event => {
    if (event.candidate) {
        // Send candidate to remote peer via signaling server
    }
    };
    ```

Receiving Media Streams:

    ```
    pc.ontrack = event => {
    // Add event.track to media element
    };
    ```

Closing the Connection:

    ```
    pc.close();
    ```
