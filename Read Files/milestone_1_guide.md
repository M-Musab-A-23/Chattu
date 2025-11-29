Milestone 1: Two-Person Video Call Implementation Guide

Based on your slide, this milestone requires three key technical achievements:

Minimal Interface Design (Seamless joining)

Reliable Communication (High-quality AV)

WebRTC Implementation (Peer-to-Peer logic)

1. Minimal Interface Design (Seamless Joining)

To achieve the "Seamless joining" mentioned in your slide, we must remove friction.

Problem: Copy-pasting long ID strings is annoying.

Solution: Use URL Parameters.

User A starts a call.

App generates a shareable link: myapp.com?call=USER_A_ID.

User B clicks the link and the app auto-connects without them needing to press "Join".

2. Reliable Communication (High Quality)

To satisfy the "Reliable Communication" requirement, we need to handle network hiccups and enforce quality.

Audio/Video Constraints

Don't just ask for video; ask for good video.

const constraints = {
video: {
width: { ideal: 1280 }, // HD Resolution
height: { ideal: 720 },
facingMode: "user"
},
audio: {
echoCancellation: true, // Vital for "Reliable" audio
noiseSuppression: true,
autoGainControl: true
}
};

Connection Guards

WebRTC can be fragile. You need "Connection Guards" in your code:

on('close'): Detect if the other person closed the tab.

on('error'): specific handling for peer-unavailable (wrong ID) or network errors.

Buffer Checks: Ensure the local stream is fully active before answering a call to prevent black screens.

3. WebRTC Architecture (The Logic)

This is the modern flow for your specific requirement:

Initialize: new Peer() connects to the signaling server (the "phone book").

Get Media: navigator.mediaDevices.getUserMedia(constraints).

The Handshake:

Caller: peer.call(remoteId, localStream)

Answerer: peer.on('call', (call) => call.answer(localStream))

Render: Attach the stream object to a <video> element's srcObject property.

Risk Mitigation for this Milestone

Risk

Mitigation Strategy

Howling Feedback

Mute the local video element (<video muted />).

Black Screen

Use playsInline and autoPlay attributes on video tags (crucial for mobile support).

Network Lag

If video freezes, implementing a "Restart ICE" button is a good fallback feature.
