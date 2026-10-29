# Local patch to webrtc-sctp 0.9.1

Source: crates.io webrtc-sctp 0.9.1 (MIT / Apache-2.0; upstream licenses retained).

## Retransmission timeout

Upstream uses the RFC 4960 defaults: initial RTO 3 s, minimum RTO 1 s. Input, clipboard and
CONTROL messages travel on ordered, reliable data channels, so one lost packet holds back every
later message until its retransmission. Sparse traffic such as typing never triggers fast
retransmit, so recovery waits for the RTO. Measured against the official Windows host through a
forced TURN relay (RTT about 250 ms, about 8% of video packets lost on the first try): every lost
keystroke stalled input for over a second.

`src/timer/rtx_timer.rs`: `RTO_INITIAL` 1000 ms (RFC 9260 allows 1 s), `RTO_MIN` 400 ms. The RTO is
still SRTT + 4 x RTTVAR when that is larger. The floor stays above the 200 ms delayed-SACK timer
common to SCTP stacks, so a peer that delays its SACK does not cause spurious retransmissions.
Nothing else changed; host and viewer share the crate.
