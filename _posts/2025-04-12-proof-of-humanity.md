# "Click All Fire Hydrants" and Other Failed Ways to Prove You're Human

One unfortunate result of the rise of AI is the proliferation of bots across the internet.  Website operators, particularly social media operators, have been playing a cat-and-mouse game with bots, and increasingly, it looks like the bots are winning.

Captchas have traditionally been our primary defence—tasks that humans could easily perform but machines supposedly couldn't. But AI has caught up. It now solves distorted text, visual puzzles, and grainy images better than most humans.

![[Pasted image 20250412181240.png]]

Adding to this challenge is the surge of LLM-powered crawlers [eating up massive amounts of website bandwidth]([FOSS infrastructure is under attack by AI companies](https://thelibre.news/foss-infrastructure-is-under-attack-by-ai-companies/)). Cloudflare recently tried throwing a "[digital corn maze]([Trapping misbehaving bots in an AI Labyrinth](https://blog.cloudflare.com/ai-labyrinth/))" at them to trap these bots. Clever! But likely just another move in the never-ending cat-and-mouse game.

We need more reliable methods to prove personhood. The next step after the captcha is verification using phone numbers or credit card details. But this creates a privacy dilemma: Sharing my number with Twitter already makes me uneasy—I'd sooner eat day-old sushi than hand it over to some rando site I found through Google.

## Can we do any better?

Here's an idea: what if we could use credentials we get from in-person verifications? Say, can we get governments and banks—who do in-person verification anyway—to vouch for you? But there would be privacy concerns there too. I don't want my banker to know what websites I use. But maybe there's a way to use that without the privacy traps.

Here's what we’d need to make this work:

1. **Issuers** (e.g. a trusted entity): issue digital certificates after in-person or document verification
2. **Provers** (you, the user): enabled to prove you hold a certificate without revealing your identity
3. **Verifiers** (websites): enabled to verify your identity without tracking you across sites. We don't want Tumblr and government to work together to figure out that mild-mannered accountant Bob from HR is the one browsing furry porn
4. **Sybil resistance**: prevent a single certificate from spawning multiple accounts. Otherwise, we're back to the bot problem

The verifiers shouldn't have to talk to the issuer since the certificate sits on the user's device and it contains everything needed to prove their personhood. 

Is this possible? It turns out it is. There have been a few so-called "[zero knowledge proof]([Zero-Knowledge Proof (ZKP) — Explained | Chainlink](https://chain.link/education/zero-knowledge-proof-zkp))" mechanisms. While they are not widely used, they seem popular in blockchain and decentralization circles.

Similar to public key cryptography, they work because of the special properties of certain mathematical operations.

## How does this work?

Honestly, I don’t know the deep math yet. At least not enough to explain without errors. But I plan to dive in. For now, [this article](https://blog.identity.foundation/cryptographic-pseudonyms) might help.

Broadly it works like this:

**Certificate generation**: An issuer uses a secret key to generate a digital certificate tied to your verified identity. You store this certificate on your device in a digital wallet.
**Signup**: When you register on a site, you use your certificate and the site’s key (aka context id) to generate a pseudonym that’s unique to that site. The same certificate will produce a different pseudonym for each site.
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
- **Corporate research**: [IBM Identity Mixer (Idemix)]([IBM/idemix: implementation of an anonymous identity stack for blockchain systems](https://github.com/IBM/idemix)), [Microsoft U-Prove](https://www.microsoft.com/en-us/research/project/u-prove/)
- **Government-backed**: [EU Digital Identity Wallet (eIDAS 2.0)](https://ec.europa.eu/digital-building-blocks/sites/display/EUDIGITALIDENTITYWALLET/About+the+initiative) 

The last one is the only one I checked in any detail. It’s expected to roll out in the next few years. From the spec sheet, it looks like it checks [all](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a2311-topic-11---pseudonyms:~:text=for%20Topic%20G.-,ZKP_04,-A%20ZKP%20scheme) [the](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a2311-topic-11---pseudonyms:~:text=Requirement%20specification-,PA_01,-A%20Wallet%20Unit) [boxes](https://eu-digital-identity-wallet.github.io/eudi-doc-architecture-and-reference-framework/latest/annexes/annex-2/annex-2-high-level-requirements/#a2311-topic-11---pseudonyms:~:text=Requirement%20specification-,PA_10,-A%20Relying%20Party). A key challenge now is how it will be integrated into the wider internet ecosystem.

I hope some of these technologies gain traction—and maybe even get baked into browsers, proving humanness quietly in the background.  
A bot-free internet? That would be something to see.
