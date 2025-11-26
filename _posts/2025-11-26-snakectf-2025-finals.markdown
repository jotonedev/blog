---
title:  "SnakeCTF 2025 Finals - my experience"
date:   2025-11-26 12:19:00 +0100
image: /assets/images/posts/snakectf_2025_finals_01.avif
---

Hi everyone!

Last week I took part in the SnakeCTF 2025 Finals as one of the organizers (_not the team [organizers](https://ctftime.org/team/42934)_).
It took place in Lignano Sabbiadoro, in Italy, from November 20 to November 23.
It consisted of two sub-events: the main jeopardy CTF event and the [Real World CTF](https://snakectf.org/signup#real-world-ctf).
The Real World CTF is different from a classic CTF, because it requires finding boxes disseminated around the city and hacking them.

I developed one challenge for the main event: `microrealm` ([writeup](https://github.com/MadrHacks/snakeCTF2025Finals-writeups/tree/main/web/microrealm)).
It focused on hijacking a microservices cluster to bypass the authentication service and log in as administrator to get the flag.
I was incredibly proud to have developed it using Go and [Fx](https://github.com/uber-go/fx).

Additionally, I created another challenge for the Real World event and contributed to the creation of another 4.
The one I created was named `llm-in-a-box` ([code](https://github.com/jotonedev/llm-in-a-box)); it consisted of an assistant box based on an open-weight LLM where, using only your voice, you had to prompt-inject it to get the flag.
Creating it allowed me to learn how products like the [Nest Mini](https://store.google.com/it/magazine/compare_speakers?hl=it) and similar devices work, and how they could be vulnerable to such attacks.

Overall, SnakeCTF 2025 Finals was a fantastic experience. I am proud of the challenges I created and the opportunity to share my work and contribute to such a high-quality event. I’m already looking forward to future editions.

If you want to learn more about the event, please check out our [website](https://snakectf.org/).
You can find the scoreboard for the event [here](https://ctftime.org/event/2994).

![Group photo with all the participants](/assets/images/posts/snakectf_2025_finals_02.avif)
