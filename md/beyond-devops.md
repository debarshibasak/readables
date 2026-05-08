# Beyond DevOps

It started as a joke I made to myself at 2am. What if I just… let Claude run everything?

Not "help me write a script." Not "explain this YAML." I mean the whole stack — databases, servers, the Kubernetes cluster I'd been babying for months. Hand it over. Walk away. See what happens.

It was a stupid idea. The kind you laugh off and go to bed. But laziness is a stronger force than caution, and after weeks of letting Claude write my code, my emails, and my commit messages, the muscle for "just do it yourself" had quietly atrophied. So I gave it the keys.

The first runs were embarrassing. It couldn't connect over SSH. It misread error messages. It retried the same broken command three times in a row, like a tourist hitting a vending machine that already ate their dollar. I almost killed the experiment.

Then I wrote a handful of skills — small, sharp instructions for the things it kept tripping on. And something flipped.

It started pulling system status without me asking. Surfacing what was off — a noisy pod, a slow query, a disk creeping toward full. Telling me what could be optimized and how. And the optimization itself? One Enter press away. I sat there, hands off the keyboard, watching an agent reason about my infra better than half the engineers I've worked with.

That's the part I didn't see coming. Not the failure. The competence.

It does look scary. To the pessimists: yes, a new era is dawning on us. To the optimists: no, we're not quite there yet. Classifiers inside Claude can stop the agent from dropping tables, deleting files, or killing processes — but their presence alone doesn't really earn trust. There's still a nagging feeling, late at night, that the agent could slip past its context and wreak havoc you only notice in the morning.

And yet — and this is the part I keep wrestling with — the upside is absurd. The automation ceiling here is at least 100x what you'd hit with GitHub Actions, Tines, or any of the modern automation platforms we've been pretending are the future. You describe what you want. It writes the code. It runs the code. It tells you what happened. The loop that took a sprint now fits in a paragraph.

I don't think there will be a discipline called DevOps in the next 8 to 12 months. Not gone — absorbed. The work doesn't vanish; it migrates into prose. The job title evaporates while the work multiplies.

What lies beyond DevOps is a bunch of markdown files — and a punishingly deep understanding of networks, cloud, VMs, and DevSecOps. The bar doesn't drop. It rises. The surface gets simpler while the floor gets deeper, and somewhere in that gap, the door for beginners quietly closes.

The tools are rewriting the rules while we're still reading the manual. The roles we built our careers on are dissolving in real time. And the only honest thing left to say is this: the ground is moving — and none of us know yet where it lands.
