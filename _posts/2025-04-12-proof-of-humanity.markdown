---
layout: post
title:  "\"Click All Fire Hydrants\" and Other Failed Ways to Prove You're Human"
date:   2025-04-12 19:43:09 +0200
categories: tech
---

Among the many unfortunate consequences of AI is the proliferation of bots across the internet. Website operators, particularly social media platforms, have been engaged in a cat-and-mouse game with bots, and increasingly, it looks like the bots are winning.

Captchas have traditionally been our primary defence—tasks that humans could easily perform but machines supposedly couldn't. But AI has caught up. It now solves distorted text, visual puzzles, and grainy images better than most humans.

{:refdef: style="text-align: center;"}
![](/assets/bear-garbage.png){: width="300" }
{: refdef}

Adding to this challenge is the surge of LLM-powered crawlers [eating up massive amounts of website bandwidth](https://thelibre.news/foss-infrastructure-is-under-attack-by-ai-companies/). Cloudflare recently tried throwing a "[digital corn maze](https://blog.cloudflare.com/ai-labyrinth/)" at them—clever, but likely just another move in the never-ending cat-and-mouse game.

We need more reliable methods to prove personhood. The next step after captchas is verification using phone numbers or credit card details. But this creates a privacy dilemma: Sharing my number with Twitter already makes me uneasy—I'd sooner eat day-old sushi than hand it over to some rando site I found through Google.

## Can we do any better?

Here's an idea: what if we could use credentials from in-person verifications? Could governments and banks—who do in-person verification anyway—vouch for you? But there would be privacy concerns there too. I don't want my banker to know what websites I use. But maybe there's a way to do this without the privacy traps.

Here's what we’d need to make this work:

1. **Issuers** (a trusted entity, eg. government, bank etc): issue digital certificates after in-person or document verification
2. **Provers** (you, the user): enabled to prove you hold a certificate without revealing your identity
3. **Verifiers** (websites): enabled to verify your identity without tracking you across sites. We don't want Tumblr and the government to work together to figure out that mild-mannered accountant Bob from HR is the one browsing furry porn
4. **Sybil resistance**: prevent a single certificate from spawning multiple accounts. Otherwise, we're back to the bot problem. This property is aptly named after a [character with multiple personality disorder](https://en.wikipedia.org/wiki/Sybil_attack).

The verifiers shouldn't have to talk to the issuer since the certificate sits on the user's device and it contains everything needed to prove their personhood. 

Is this possible? It turns out it is, with "[zero knowledge proofs](https://chain.link/education/zero-knowledge-proof-zkp)". While ZKP is not widely used, it seem getting popular in blockchain and decentralization circles. Identify verification is one of its many use cases.

Similar to public key cryptography, it works because of the special properties of certain mathematical operations.

## How does this work?

Honestly, I don’t know the deep math yet—at least not enough to confidently explain it. But I plan to dive in. For now, [this article](https://blog.identity.foundation/cryptographic-pseudonyms) might help.

Broadly it works like this:

**Certificate generation**: An issuer uses a secret key to generate a digital certificate tied to your verified identity. You store this certificate on your device in a digital wallet.  
  
**Signup**: When you register on a site, or visit it for the first time, you use your certificate and the site’s key (aka context id) to generate a pseudonym that’s unique to that site. The same certificate will produce a different pseudonym for each site.  
  
**Login**: You prove you're the same person behind the pseudonym—without revealing your identity or the certificate itself—by performing a mathematical proof that only someone holding the certificate could complete.  
  
The site knows you’re a real person, but not who you are. And it can’t link your activity across other sites.

The math ensures that the real identity cannot be derived from a pseudonym or set of pseudonyms. Since different websites get different pseudonyms for the same user, they cannot correlate their activity with this.

## Why is this not in use yet?

Wide adoption for this requires a lot:

1. You need users to go get certificates
2. You need trusted entities willing to give out certificates
3. Web services need to make changes to their authentication stack

The big players—Google, Facebook, etc.—make money by tracking users. They aren’t eager to hand us more privacy tools. But if bot spam keeps getting worse, we might finally see some pressure to change.

Some initiatives are already exploring this space. A quick search surfaced these:

- **Blockchain-based**: Hyperledger Indy, [Sovrin](https://sovrin.org/)
- **Corporate research**: [IBM Identity Mixer (Idemix)](https://github.com/IBM/idemix), [Microsoft U-Prove](https://www.microsoft.com/en-us/research/project/u-prove/)
- **Government-backed**: [EU Digital Identity Wallet (eIDAS 2.0)](https://ec.europa.eu/digital-building-blocks/sites/display/EUDIGITALIDENTITYWALLET/About+the+initiative) 

The last one is the only one I checked in any detail. It’s expected to roll out in the next few years. From the spec sheet, it looks like it checks [all](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a2311-topic-11---pseudonyms:~:text=for%20Topic%20G.-,ZKP_04,-A%20ZKP%20scheme) [the](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a2311-topic-11---pseudonyms:~:text=Requirement%20specification-,PA_01,-A%20Wallet%20Unit) [boxes](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a2311-topic-11---pseudonyms:~:text=Requirement%20specification-,PA_10,-A%20Relying%20Party). A key challenge now is how it will be integrated into the wider internet ecosystem.

I hope some of these technologies gain traction—and maybe even get baked into browsers, proving humanness quietly in the background.  
A bot-free internet? *That* would be something to see.
